> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# MCP node in conversation flow

> Add an MCP node to a Retell conversation flow to call remote MCP server tools during a live call, with custom headers and authentication.

The MCP node is used to call tools on your MCP server. It's not intended for having a conversation with the user, but the agent can still talk while in this node if needed. For single- and multi-prompt agents, connect an MCP server through the response engine instead — see [MCP tools for single and multi-prompt agents](/build/single-multi-prompt/mcp).

<Frame>
  <img width="250px" src="https://mintcdn.com/retellai/32uO5g9DswfoJ9j7/images/cf/mcp/node.png?fit=max&auto=format&n=32uO5g9DswfoJ9j7&q=85&s=e0bfeacf317232bd2956dcd06f8190a7" data-path="images/cf/mcp/node.png" />
</Frame>

## Add MCP server

<Steps>
  <Step title="Add MCP server">
    To create an MCP node, we first need to add an MCP server.

    <Frame>
      <img src="https://mintcdn.com/retellai/zL2HeUqUnagEN9eK/images/cf/mcp/add_mcp.png?fit=max&auto=format&n=zL2HeUqUnagEN9eK&q=85&s=6dee07c5d03c2be78acfce15d7d8e724" width="734" height="589" data-path="images/cf/mcp/add_mcp.png" />
    </Frame>
  </Step>

  <Step title="Set request headers (optional)">
    You can define custom headers to include with the request Retell sends to your MCP Server.

    <Frame>
      <img height="200" src="https://mintcdn.com/retellai/32uO5g9DswfoJ9j7/images/custom-function/headers.png?fit=max&auto=format&n=32uO5g9DswfoJ9j7&q=85&s=44295c732912fcb1c3f29259e3f59609" data-path="images/custom-function/headers.png" />
    </Frame>
  </Step>

  <Step title="Set query parameters (optional)">
    You can define query parameters to include in the request URL that Retell appends to your MCP Server Endpoint.

    <Frame>
      <img height="200" src="https://mintcdn.com/retellai/YMPW7mFNipo6shGp/images/custom-function/query-params.png?fit=max&auto=format&n=YMPW7mFNipo6shGp&q=85&s=0a0ccd469644d04aa87c086a8dc68bfb" data-path="images/custom-function/query-params.png" />
    </Frame>
  </Step>

  <Step title="Add MCP Tool">
    Select MCP Tool

    <Frame>
      <img src="https://mintcdn.com/retellai/32uO5g9DswfoJ9j7/images/cf/mcp/add_tool.png?fit=max&auto=format&n=32uO5g9DswfoJ9j7&q=85&s=ad49026f6f2a42a5e14a5abcd82e0cfa" width="725" height="365" data-path="images/cf/mcp/add_tool.png" />
    </Frame>
  </Step>

  <Step title="Set response variables (optional)">
    Extract values from the MCP tool response and save them as <strong>dynamic variables</strong> for use later in the conversation.

    For example, you can extract a user’s name from the response and reference it later using <code>\{\{user\_name}}</code>.

    Example response body

    ```javascript theme={"dark"}
    {
    "properties": {
        "user": {
        "name": "John Doe",
        "age": 26
        }
    }
    }
    ```

    <Frame>
      <img height="200" src="https://mintcdn.com/retellai/32uO5g9DswfoJ9j7/images/custom-function/response-variables.png?fit=max&auto=format&n=32uO5g9DswfoJ9j7&q=85&s=389a898c0ab3705d3a83acb61a3a4634" data-path="images/custom-function/response-variables.png" />
    </Frame>
  </Step>
</Steps>

## Node Settings

* **Global Node**: read more at [Global Node](/build/conversation-flow/global-node)
* **Fine-tuning Examples**: Can finetune transition. Read more at [Finetune Examples](/build/conversation-flow/finetune-examples)
