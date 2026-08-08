> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Code tool

> Run JavaScript as a tool in a Retell single- or multi-prompt agent. Read dynamic variables and metadata, make HTTP requests, and store structured results.

Code Tool lets a single- or multi-prompt agent run JavaScript in Retell's
sandbox. The LLM decides when to call the tool from its name, description, and
conversation context.

For example, a lending agent can call a Code Tool to calculate a repayment
amount from values already stored in dynamic variables, then use the returned
amount in its next response.

If you use a conversation flow agent, see [Code node](/build/conversation-flow/code-node).

## When to use Code Tool

|                       | Code Tool                                           | Custom function                                              |
| --------------------- | --------------------------------------------------- | ------------------------------------------------------------ |
| **Runs**              | JavaScript in Retell's sandbox                      | An HTTP request to your server                               |
| **Trigger**           | The LLM decides when to call it                     | The LLM decides when to call it                              |
| **Inputs**            | Dynamic variables and call or chat metadata         | LLM-supplied parameters, dynamic variables, and request data |
| **Best for**          | Formatting, calculations, and low-risk HTTP lookups | Authenticated integrations and production workflows          |
| **Maximum code size** | 20,000 characters                                   | Not applicable                                               |

<Note>
  Code Tool doesn't accept LLM-supplied parameters. The sandbox receives only the
  current `dv` and `metadata` objects. If the LLM must compose arguments when it
  calls the tool, use a [custom function](/build/single-multi-prompt/custom-function).
</Note>

<Warning>
  Use Code Tool only for logic that can run without secrets. Dynamic variables
  and metadata are stored in plaintext with the call or chat record. For
  authentication, secret management, internal systems, or production writes, use
  a custom function hosted on your backend.
</Warning>

## Create a Code Tool

<Steps>
  <Step title="Add a Code Tool">
    In the agent's **Functions** section, select **+ Add**, then select
    **Code**.

    <Frame caption="Code in the function-type menu.">
      <img src="https://mintcdn.com/retellai/9xto-Or-KLZnnbLr/images/cf/add-code-tool.png?fit=max&auto=format&n=9xto-Or-KLZnnbLr&q=85&s=392f0cafd7a3e574d6058240c6dfc3b3" alt="Function-type menu with Code below Extract Dynamic Variable and above Custom Function." width="307" height="379" data-path="images/cf/add-code-tool.png" />
    </Frame>
  </Step>

  <Step title="Set the name and description">
    Use a unique name with 1–64 letters, numbers, underscores, or dashes. Spaces
    aren't allowed. The description can contain up to 1,024 characters and
    should tell the LLM exactly when to call the tool.

    For example:

    * **Name:** `calculate_daily_repayment`
    * **Description:** `Calculate the daily repayment amount after fin_amount, tenure_days, and interest_rate are available as dynamic variables.`
  </Step>

  <Step title="Write JavaScript">
    Read input values from `dv` or `metadata` and return a JSON-serializable
    value. Retell serializes the return value to JSON, sends it to the LLM as
    the tool result, and extracts any response variables from it (next step).

    ```javascript theme={"dark"}
    const principal = Number(dv.fin_amount);
    const days = Number(dv.tenure_days);
    const annualRate = Number(dv.interest_rate) / 100;

    if (![principal, days, annualRate].every(Number.isFinite) || days <= 0) {
      return { ok: false, error: "Missing or invalid repayment inputs" };
    }

    const interest = principal * annualRate * (days / 365);
    const dailyRepayment = (principal + interest) / days;

    return {
      ok: true,
      daily_repayment: dailyRepayment.toFixed(2)
    };
    ```
  </Step>

  <Step title="Store fields as variables">
    Under **Store Fields as Variables**, map a dynamic-variable name to a path
    in the returned value. For the example above, map `daily_repayment` to
    `daily_repayment`.

    See [Store response variables](#store-response-variables) for nested and
    array paths.
  </Step>

  <Step title="Configure execution feedback">
    Under **During Execution Feedback**, turn on **Talk while waiting** to use a
    generated prompt or static sentence, or turn on **Play typing sound**.
    **Talk After Action Completed** is on by default so the LLM responds after
    the result arrives.
  </Step>

  <Step title="Update the agent prompt">
    Tell the LLM when the required dynamic variables are ready and when to call
    the tool.

    ```text theme={"dark"}
    After fin_amount, tenure_days, and interest_rate are available as dynamic
    variables, call calculate_daily_repayment before quoting a repayment amount.
    ```
  </Step>

  <Step title="Test the code">
    Use **Dynamic Variables** in the editor to add test values, then select
    **Run Code**. The result and any `console.log()` output appear below the
    editor.

    Test values stay in the dashboard and don't change the live agent. The
    dashboard test doesn't populate `metadata`, so metadata-dependent code must
    handle an empty object during testing.
  </Step>
</Steps>

## JavaScript environment

The code runs inside a QuickJS sandbox. Top-level `await` is supported.

### `dv`: dynamic variables

Read [dynamic variables](/build/dynamic-variables) as properties of `dv`. All
values in `dv` are strings, including values stored by earlier tools.

```javascript theme={"dark"}
const customerName = dv.customer_name;
const orderTotal = Number(dv.order_total);
const callerNumber = dv.user_number;
```

Use `dv.user_number`, not `{{user_number}}`, inside JavaScript. Runtime values
such as `call_id`, `chat_id`, `session_type`, `direction`, `user_number`, and
`agent_number` are available when they apply to the current session.

<Note>
  Dynamic variables are available through `dv` inside JavaScript. `{{...}}`
  template substitution isn't available inside code.
</Note>

Code Tool has no parameter schema. If the tool needs values mentioned by the
caller, extract them into dynamic variables first with an
[Extract Dynamic Variable tool](/build/single-multi-prompt/extract-dv) or return
them as response variables from an earlier tool. Because the LLM selects tools,
it can skip a separate extraction tool. Use a custom function when arguments must be composed
at call time, or a conversation flow when the extraction and calculation must
run in a fixed sequence.

### `metadata`: call or chat metadata

Read metadata passed when you create the [phone call](/api-references/create-phone-call)
or [chat](/api-references/create-chat).

```javascript theme={"dark"}
const customerId = metadata.customer_id;
const priority = metadata.priority_level;
```

Metadata values can use JSON types. Unlike values in `dv`, they aren't limited
to strings.

### `fetch(url, options)`: HTTP requests

`fetch()` supports HTTP and HTTPS URLs, request methods, string-valued headers,
and request bodies. The returned response supports `status`, `ok`,
`statusText`, `url`, `headers.get()`, `text()`, and `json()`.

```javascript theme={"dark"}
const zip = encodeURIComponent(dv.zip_code);
const response = await fetch(`https://api.zippopotam.us/us/${zip}`);

if (!response.ok) {
  return { found: false, status: response.status };
}

const location = await response.json();
return {
  found: true,
  city: location.places[0]["place name"],
  state: location.places[0]["state abbreviation"]
};
```

The sandbox blocks non-HTTP protocols and selected local, private, and platform
addresses. It doesn't provide the complete browser Fetch API.

### `console.log()`: test output

Use `console.log()` while testing. Logs appear in **Run Code** results. They
aren't added to the live tool result or shown as Code Tool logs in call logs.

```javascript theme={"dark"}
console.log("Customer:", dv.customer_name);
```

### Runtime limits

| Limit                    | Value                               |
| ------------------------ | ----------------------------------- |
| Code size                | 20,000 characters                   |
| Timeout                  | 5–60 seconds; 30 seconds by default |
| Result sent to the agent | 15,000 characters by default        |
| Automatic retries        | None                                |

The sandbox provides common JavaScript built-ins such as `Array`, `Date`,
`JSON`, `Map`, `Math`, `Promise`, `RegExp`, and `Set`. Node.js modules, browser
DOM APIs, `require`, `import`, and `Intl` aren't available.

## Store response variables

Response variables copy fields from the full return value into dynamic
variables. For this return value:

```javascript theme={"dark"}
return {
  status: "confirmed",
  order: {
    id: "ord_123",
    items: [{ name: "Replacement filter" }]
  }
};
```

configure these mappings:

| Dynamic-variable name | Path to value         | Stored value         |
| --------------------- | --------------------- | -------------------- |
| `order_status`        | `status`              | `confirmed`          |
| `order_id`            | `order.id`            | `ord_123`            |
| `first_item_name`     | `order.items[0].name` | `Replacement filter` |

Stored values are strings. Objects and arrays are stored as JSON strings. If a
path doesn't exist or resolves to `null`, Retell skips that variable without
failing the execution.

Response-variable extraction uses the full return value, even when the result
sent to the agent is capped at 15,000 characters.

## Configuration

| Setting                         | Default    | Behavior                                                                                         |
| ------------------------------- | ---------- | ------------------------------------------------------------------------------------------------ |
| **Timeout**                     | 30 seconds | Stops execution after 5–60 seconds.                                                              |
| **Talk while waiting**          | Off        | Speaks one generated prompt or static sentence when the tool starts.                             |
| **Play typing sound**           | Off        | Plays a typing sound while the tool runs.                                                        |
| **Talk After Action Completed** | On         | Calls the LLM after the result arrives so the agent can respond. Turn it off to finish silently. |

If several tools run in sequence, enable **Talk while waiting** on only the tool
that should speak. Otherwise, the caller can hear multiple waiting messages.

## Use Code Tool safely

* Don't put API keys, credentials, or sensitive tokens in code, `dv`, or
  `metadata`.
* Use `fetch()` for low-risk HTTP reads. Put authenticated requests and
  state-changing operations behind a custom function on your backend.
* Don't make critical routing or financial decisions depend on an unvalidated
  return value. Handle missing and invalid `dv` values explicitly.

## FAQ

<AccordionGroup>
  <Accordion title="Can the LLM pass arguments directly to Code Tool?">
    No. Code Tool doesn't expose a parameter schema and receives only `dv` and `metadata`. Use a custom function when the LLM must fill arguments at call time. If you chain an Extract Dynamic Variable tool before Code Tool, the LLM can still skip that first tool; use a conversation flow when the sequence must be deterministic.
  </Accordion>

  <Accordion title="Why does the agent say the waiting message twice?">
    Each tool with **Talk while waiting** enabled can speak its own message. If an extraction tool and Code Tool run in sequence, enable the setting on only one of them or give them distinct messages.
  </Accordion>

  <Accordion title="Can I use npm packages or browser APIs?">
    No. The sandbox doesn't provide `require`, `import`, Node.js modules, browser DOM APIs, or `Intl`. Use the available JavaScript built-ins and `fetch()`.
  </Accordion>

  <Accordion title="What happens if the code times out or throws an error?">
    Retell marks the execution as failed, sends the error to the LLM, doesn't extract response variables, and doesn't retry automatically. Use `try/catch` for expected errors and return a structured fallback such as `{ "ok": false, "error": "lookup failed" }`.
  </Accordion>

  <Accordion title="Can I use async/await?">
    Yes. Top-level `await` is supported, including for `fetch()` calls.
  </Accordion>

  <Accordion title="Can I write dynamic variables from inside the code?">
    Not by assigning to `dv`. The sandbox receives a copy of `dv`, so changes you make to it inside the code don't persist. To write a value back for later turns, return it and map it under **Store Fields as Variables**.
  </Accordion>

  <Accordion title="Is there a limit on the result size?">
    Retell sends up to 15,000 characters to the agent by default. Response-variable extraction uses the full return value before that result is capped.
  </Accordion>
</AccordionGroup>
