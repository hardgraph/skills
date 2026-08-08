> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Function Calling Overview

> Function calling lets Retell single or multi-prompt agents take real actions — transfer calls, end calls, book appointments, send SMS, and call external APIs.

## Introduction

Function calling transforms your AI agent from a conversational interface into an action-oriented assistant. By connecting your agent to functions, you enable it to interact with external systems, manage call flows, and perform real-world tasks.

## Common Use Cases

Function calling enables your agent to:

### Call Management

* **Transfer calls**: Route to human agents or other departments
* **End calls**: Gracefully terminate conversations
* **Send DTMF tones**: Navigate phone menus

### Business Operations

* **Book appointments**: Integrate with scheduling systems
* **Check availability**: Query calendars or inventory
* **Process orders**: Create, modify, or cancel orders

### Data Integration

* **Retrieve information**: Pull data from CRMs, databases, or APIs
* **Update records**: Modify customer information or case details
* **Send notifications**: Trigger emails, SMS, or push notifications

## How Function Calling Works

### The Process

1. **Agent Decision**: Based on the conversation context, the LLM determines when a function is needed
2. **Parameter Extraction**: The agent extracts required information from the conversation
3. **Function Execution**: Retell calls your function with the extracted parameters
4. **Response Handling**: The function result is processed and incorporated into the conversation

### Technical Overview

Function calling uses structured JSON to communicate between the LLM and your systems:

```json theme={"dark"}
{
  "function": "book_appointment",
  "arguments": {
    "date": "2024-03-15",
    "time": "14:00",
    "customer_name": "John Smith"
  }
}
```

### Additional Resources

* [OpenAI's Function Calling Guide](https://platform.openai.com/docs/guides/function-calling): Technical deep dive
* [Function Calling Tutorial](https://semaphoreci.com/blog/function-calling): Step-by-step implementation guide

## Configuring Tool Calls

### Dashboard Configuration

Access and manage tools in the agent detail page under the "Functions" section.

<Frame>
  <img height="700" src="https://mintcdn.com/retellai/M9QYKZE4hbt00HfL/images/multi-prompt/functions.png?fit=max&auto=format&n=M9QYKZE4hbt00HfL&q=85&s=0c6fcd4ff45d3ebc43bef2fb728caed9" alt="Functions section in dashboard" data-path="images/multi-prompt/functions.png" />
</Frame>

## Available Function Types

### 1. Pre-built Functions

Retell provides ready-to-use functions for common scenarios:

| Function                                                      | Purpose                           | Common Use Cases                             |
| ------------------------------------------------------------- | --------------------------------- | -------------------------------------------- |
| [**End Call**](/build/single-multi-prompt/end-call)           | Terminate conversation gracefully | After completing tasks, when caller requests |
| [**Transfer Call**](/build/single-multi-prompt/transfer-call) | Route to human agents             | Escalation, department routing               |
| [**Press Digits**](/build/single-multi-prompt/press-digit)    | Send DTMF tones                   | Navigate IVR systems, enter codes            |
| [**Check Availability**](/build/check-availability)           | Query available time slots        | Appointment scheduling                       |
| [**Book Calendar**](/build/book-calendar)                     | Create calendar events            | Confirm appointments, meetings               |
| [**Send SMS**](/build/single-multi-prompt/send-sms)           | Send text messages                | Confirmations, follow-ups                    |

### 2. Custom Functions

Create your own functions to:

* **Integrate with your APIs**: Connect to CRM, ERP, or custom systems
* **Execute business logic**: Validate data, calculate prices, check inventory
* **Trigger workflows**: Start processes in other systems

#### Custom Function Features

* **Flexible parameters**: Define any input structure
* **Async execution**: Run in background without blocking conversation
* **Error handling**: Graceful fallbacks for failures
* **Response control**: Customize what the agent says during/after execution

[Learn more about custom functions →](/build/single-multi-prompt/custom-function)

### 3. Code Tool

Run JavaScript code directly in Retell's sandbox without setting up an external server. Useful for:

* **Data transformation**: Format, combine, or compute values from dynamic variables
* **Simple API calls**: Fetch data using the built-in `fetch()` function
* **Logic and calculations**: Implement conditional logic, math, or string operations

[Learn more about Code Tool →](/build/single-multi-prompt/code-tool)

## Video Tutorial

<iframe width="360" height="200" src="https://www.youtube.com/embed/0iGAFUYhuoE?si=_IYtfQNmb1haneif" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen />

### Additional Resources

* [Community Templates](https://docs.google.com/document/d/1hx6hdTEjAR4y4xXZ7RLMH2byQNVW1ABxC8S4FwvTx_Y/edit?tab=t.0#heading=h.wf5bktkelope): Real-world function examples
* [API Reference](/api-references/create-agent): Technical documentation
* [Best Practices](/build/prompt-engineering-guide): Writing effective function prompts
