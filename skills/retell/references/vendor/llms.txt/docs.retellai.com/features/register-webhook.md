> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Register and set up a Retell webhook

> Register a Retell webhook to receive real-time call_started, call_ended, and call_analyzed events, with Node.js, Python, and signature verification code.

<Steps>
  <Step title="Create your server endpoint">
    Set up an HTTP or HTTPS endpoint function that can accept webhook requests with a POST method.

    Example endpoint:

    <CodeGroup>
      ```typescript Node.js theme={"dark"}
      // install the sdk: https://docs.retellai.com/get-started/sdk
      import { Retell } from "retell-sdk";
      import express, { Request, Response } from "express";

      const app = express();
      app.use(express.json());

      app.post("/webhook", (req: Request, res: Response) => {
        const {event, call} = req.body;
        switch (event) {
          case "call_started":
            console.log("Call started event received", call.call_id);
            break;
          case "call_ended":
            console.log("Call ended event received", call.call_id);
            break;
          case "call_analyzed":
            console.log("Call analyzed event received", call.call_id);
            break;
          default:
            console.log("Received an unknown event:", event);
        }
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
              post_data = await request.json()
              if post_data["event"] == "call_started":
                  print("Call started event", post_data["call"]["call_id"])
              elif post_data["event"] == "call_ended":
                  print("Call ended event", post_data["call"]["call_id"])
              elif post_data["event"] == "call_analyzed":
                  print("Call analyzed event", post_data["call"]["call_id"])
              else:
                  print("Unknown event", post_data["event"])
              return JSONResponse(status_code=204)
          except Exception as err:
              print(f"Error in webhook: {err}")
              return JSONResponse(
                  status_code=500, content={"message": "Internal Server Error"}
              )
      ```
    </CodeGroup>
  </Step>

  <Step title="Test your endpoint locally">
    Before going live, test your application integration locally. For example, host the endpoint on `localhost:8080/webhook` and test with Postman:

    <Frame>
      <img height="700" src="https://mintcdn.com/retellai/_FxJdxv7mQoPqyGs/images/webhook_postman.png?fit=max&auto=format&n=_FxJdxv7mQoPqyGs&q=85&s=f6f0c37648926e503685ec7de48c4c30" alt="Testing webhook with postman" data-path="images/webhook_postman.png" />
    </Frame>

    Test using this CURL command:

    ```bash theme={"dark"}
    curl --location 'localhost:8080/webhook' \
    --header 'Content-Type: application/json' \
    --data '{
      "event": "call_ended",
      "call": {
        "call_type": "phone_call",
        "from_number": "+12137771234",
        "to_number": "+12137771235",
        "direction": "inbound",
        "call_id": "Jabr9TXYYJHfvl6Syypi88rdAHYHmcq6",
        "agent_id": "oBeDLoLOeuAbiuaMFXRtDOLriTJ5tSxD",
        "call_status": "registered",
        "metadata": {},
        "retell_llm_dynamic_variables": {
          "customer_name": "John Doe"
        },
        "start_timestamp": 1714608475945,
        "end_timestamp": 1714608491736,
        "disconnection_reason": "user_hangup",
        "transcript": "...",
        "opt_out_sensitive_data_storage": false
      }
    }'
    ```
  </Step>

  <Step title="Make your local endpoint online">
    Deploy your endpoint using [Ngrok](https://ngrok.com/docs/getting-started/):

    1. Install ngrok:

    ```bash theme={"dark"}
    brew install ngrok/ngrok/ngrok
    ```

    2. Start ngrok:

    ```bash theme={"dark"}
    ngrok http http://localhost:8080
    ```

    You'll see a console UI like this:

    ```
    ngrok                                                                   (Ctrl+C to quit)

    Session Status                online
    Account                       inconshreveable (Plan: Free)
    Version                       3.0.0
    Region                        United States (us)
    Latency                       78ms
    Web Interface                 http://127.0.0.1:4040
    Forwarding                    https://84c5df474.ngrok-free.dev -> http://localhost:8080
    ```

    Your webhook endpoint will be `https://84c5df474.ngrok-free.dev/webhook`
  </Step>

  <Step title="Register your webhook endpoint">
    You have two options:

    **Option 1: Register an account level webhook**
    Set up through the system settings' webhooks tab for events related to any agent under your account.

    <Frame>
      <img height="200" src="https://mintcdn.com/retellai/lFgxod3Z4ow1Jkgv/images/webhook.png?fit=max&auto=format&n=lFgxod3Z4ow1Jkgv&q=85&s=ad640e6141ba063dd7f6d33d43f36435" alt="Account-level webhook URL configuration in the dashboard settings" data-path="images/webhook.png" />
    </Frame>

    **Option 2: Register an agent level webhook**
    Set up through the dashboard's agent detail page. Note: If set, account level webhooks will not be triggered for that agent.

    <Frame>
      <img height="200" src="https://mintcdn.com/retellai/zL2HeUqUnagEN9eK/images/agent_webhook.png?fit=max&auto=format&n=zL2HeUqUnagEN9eK&q=85&s=a33cfdbda36a3647d60d73d05e0c7330" alt="Agent-level webhook URL configuration in the agent detail page" data-path="images/agent_webhook.png" />
    </Frame>

    The webhook URL field accepts [dynamic variables](/build/dynamic-variables). Type `{{` to open a list of available variables, or wrap a name in double curly braces yourself (for example `https://example.com/webhook?customer={{customer_name}}`). Each variable is replaced with its value per call before the event is delivered.

    Use the **Test** button to send a sample request to your endpoint. If your URL uses variables whose values are only known during a live call (such as contact or CRM fields), the **Test** button is disabled and a tooltip explains why — the webhook will still work on real calls, but it can't be tested here because there are no values to substitute yet. Variables that Retell can resolve ahead of time, such as system variables and your version's environment variables, can be tested normally.
  </Step>

  <Step title="Verify your webhook endpoint">
    Start a web call in the dashboard to verify the webhook is triggered correctly.
  </Step>
</Steps>
