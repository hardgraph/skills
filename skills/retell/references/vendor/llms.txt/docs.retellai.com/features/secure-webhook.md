> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Secure the webhook

> Verify Retell AI webhook signatures with the raw request body, X-Retell-Signature header, API key, SDK helpers, and IP allowlisting.

You can use the `X-Retell-Signature` header together with your Retell API Key to verify the webhook comes from Retell AI, not from a malicious third party. We have provided a verify function in our SDKs to help you with this.

<img src="https://mintcdn.com/retellai/_FxJdxv7mQoPqyGs/images/webhook-key.jpeg?fit=max&auto=format&n=_FxJdxv7mQoPqyGs&q=85&s=1bfb4353e0bbaff6399ccea1e06237ca" width="1036" height="274" data-path="images/webhook-key.jpeg" />

<Note>Only the API key that has a webhook badge next to it can be used to verify the webhook.</Note>

You can also check and allowlist Retell IP addresses: `100.20.5.228`.

The following code snippets demonstrate how to verify and handle the webhook in Node.js and Python.

### Install the SDK

Install the corresponding Python or Node.js SDK:

* [Node.js](/get-started/sdk)
* [Python](/get-started/sdk)

### Sample Code

<CodeGroup>
  ```typescript Node.js theme={"dark"}
  // Install the SDK: https://docs.retellai.com/get-started/sdk
  import { Retell } from "retell-sdk";
  import express from "express";

  const app = express();
  // Use raw body for signature verification, not JSON.stringify(req.body).
  app.use(express.raw({ type: "application/json" }));

  app.post("/webhook", async (req, res) => {
    const rawBody = req.body.toString("utf-8");
    const signature = req.headers["x-retell-signature"];

    if (
      typeof signature !== "string" ||
      !(await Retell.verify(rawBody, process.env.RETELL_API_KEY, signature))
    ) {
      console.error("Invalid signature");
      return res.status(401).send("Unauthorized");
    }

    const { event, call } = JSON.parse(rawBody);
    // process the webhook

    // Acknowledge the receipt of the event
    res.status(204).send();
  });
  ```

  ```Python Python theme={"dark"}
  # Install the SDK: https://docs.retellai.com/get-started/sdk
  from fastapi import FastAPI, Request
  from fastapi.responses import JSONResponse
  from retell import Retell

  retell = Retell(api_key=os.environ["RETELL_API_KEY"])

  @app.post("/webhook")
  async def handle_webhook(request: Request):
      try:
          # Use raw body for signature verification, not json.dumps(request.json()).
          raw_body = (await request.body()).decode("utf-8")
          valid_signature = retell.verify(
              raw_body,
              api_key=str(os.environ["RETELL_API_KEY"]),
              signature=str(request.headers.get("X-Retell-Signature")),
          )
          if not valid_signature:
              print("Received Unauthorized")
              return JSONResponse(status_code=401, content={"message": "Unauthorized"})

          post_data = json.loads(raw_body)
          # process the webhook

          return JSONResponse(status_code=204)
      except Exception as err:
          print(f"Error in webhook: {err}")
          return JSONResponse(
              status_code=500, content={"message": "Internal Server Error"}
          )
  ```
</CodeGroup>

## Verify Without SDK

If you're using a language without an official Retell SDK, you can verify the webhook signature manually. The signature uses HMAC-SHA256.

### How the Signature Works

Every webhook request includes an `X-Retell-Signature` header in the format:

```
v={timestamp},d={hex_digest}
```

* `v` is the Unix timestamp in milliseconds when the webhook was sent.
* `d` is the HMAC-SHA256 hex digest of the raw request body concatenated with the timestamp.

### Verification Steps

1. Extract the `X-Retell-Signature` header from the request.
2. Parse the timestamp (`v`) and digest (`d`) from the header using the pattern `v=(\d+),d=(.*)`.
3. Check that the timestamp is within **5 minutes** of the current time (to prevent replay attacks).
4. Compute `HMAC-SHA256(raw_body + timestamp, api_key)` where `+` is string concatenation.
5. Compare the computed hex digest with the `d` value from the header. If they match, the webhook is authentic.

<Warning>
  You must use the **raw request body** string for verification, not a re-serialized version from parsed JSON. Re-serializing may change whitespace or key ordering, which will cause verification to fail.
</Warning>

### Sample Code

<CodeGroup>
  ```go Go theme={"dark"}
  package main

  import (
  	"crypto/hmac"
  	"crypto/sha256"
  	"encoding/hex"
  	"fmt"
  	"io"
  	"math"
  	"net/http"
  	"os"
  	"regexp"
  	"strconv"
  	"time"
  )

  func verifyWebhook(rawBody string, apiKey string, signature string) bool {
  	re := regexp.MustCompile(`v=(\d+),d=(.*)`)
  	matches := re.FindStringSubmatch(signature)
  	if len(matches) != 3 {
  		return false
  	}

  	timestamp, err := strconv.ParseInt(matches[1], 10, 64)
  	if err != nil {
  		return false
  	}
  	digest := matches[2]

  	// Check timestamp is within 5 minutes
  	now := time.Now().UnixMilli()
  	if math.Abs(float64(now-timestamp)) > 5*60*1000 {
  		return false
  	}

  	// Compute HMAC-SHA256 and use constant-time comparison
  	mac := hmac.New(sha256.New, []byte(apiKey))
  	mac.Write([]byte(rawBody + matches[1]))
  	expectedMAC, _ := hex.DecodeString(digest)

  	return hmac.Equal(mac.Sum(nil), expectedMAC)
  }

  func webhookHandler(w http.ResponseWriter, r *http.Request) {
  	body, _ := io.ReadAll(r.Body)
  	rawBody := string(body)
  	signature := r.Header.Get("X-Retell-Signature")

  	if !verifyWebhook(rawBody, os.Getenv("RETELL_API_KEY"), signature) {
  		http.Error(w, "Unauthorized", http.StatusUnauthorized)
  		return
  	}

  	// Process the webhook
  	fmt.Println("Webhook verified successfully")
  	w.WriteHeader(http.StatusNoContent)
  }

  func main() {
  	http.HandleFunc("/webhook", webhookHandler)
  	http.ListenAndServe(":8080", nil)
  }
  ```

  ```ruby Ruby theme={"dark"}
  require "openssl"
  require "sinatra"
  require "json"

  API_KEY = ENV["RETELL_API_KEY"]

  def verify_webhook(raw_body, api_key, signature)
    match = signature.match(/v=(\d+),d=(.*)/)
    return false unless match

    timestamp = match[1]
    digest = match[2]

    # Check timestamp is within 5 minutes
    now = (Time.now.to_f * 1000).to_i
    return false if (now - timestamp.to_i).abs > 5 * 60 * 1000

    # Compute HMAC-SHA256 and use constant-time comparison
    expected = OpenSSL::HMAC.hexdigest("SHA256", api_key, raw_body + timestamp)
    OpenSSL.secure_compare(expected, digest)
  end

  post "/webhook" do
    raw_body = request.body.read
    signature = request.env["HTTP_X_RETELL_SIGNATURE"]

    unless verify_webhook(raw_body, API_KEY, signature)
      halt 401, "Unauthorized"
    end

    # Process the webhook
    status 204
  end
  ```

  ```php PHP theme={"dark"}
  <?php
  $apiKey = getenv("RETELL_API_KEY");
  $rawBody = file_get_contents("php://input");
  $signature = $_SERVER["HTTP_X_RETELL_SIGNATURE"] ?? "";

  function verifyWebhook(string $rawBody, string $apiKey, string $signature): bool {
      if (!preg_match('/v=(\d+),d=(.*)/', $signature, $matches)) {
          return false;
      }

      $timestamp = $matches[1];
      $digest = $matches[2];

      // Check timestamp is within 5 minutes
      $now = round(microtime(true) * 1000);
      if (abs($now - intval($timestamp)) > 5 * 60 * 1000) {
          return false;
      }

      // Compute HMAC-SHA256 and use constant-time comparison
      $expected = hash_hmac("sha256", $rawBody . $timestamp, $apiKey);
      return hash_equals($expected, $digest);
  }

  if (!verifyWebhook($rawBody, $apiKey, $signature)) {
      http_response_code(401);
      echo "Unauthorized";
      exit;
  }

  // Process the webhook
  $data = json_decode($rawBody, true);
  http_response_code(204);
  ?>
  ```
</CodeGroup>
