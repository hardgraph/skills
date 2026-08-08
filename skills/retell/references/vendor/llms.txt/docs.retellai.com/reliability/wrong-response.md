> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Debug wrong response

> Fix Retell agents that give incorrect responses — switch to a more capable LLM, simplify prompt structure, add finetune examples, and review function calls.

<Steps>
  <Step title="Check Model Capability">
    If your agent isn't following instructions correctly, especially with longer or complex prompts:

    1. Check if you're using a lightweight model (e.g., 4.1-mini)
    2. Switch to a more capable model like `gpt-4.1`

    <Frame>
      <img height="700" src="https://mintcdn.com/retellai/a1LftRqc_k-5TDA7/reliability/images/llm_selector.png?fit=max&auto=format&n=a1LftRqc_k-5TDA7&q=85&s=b9062afe00f089f0339f5cc4c1fd8ccd" data-path="reliability/images/llm_selector.png" />
    </Frame>
  </Step>

  <Step title="Review Prompt Structure">
    If the issue persists:

    1. Check if your prompt structure is too complex
    2. Follow [prompt engineering guide](https://www.promptingguide.ai/)
    3. Break down complex tasks into clear, sequential steps
    4. Add explicit transition conditions between different steps
  </Step>
</Steps>
