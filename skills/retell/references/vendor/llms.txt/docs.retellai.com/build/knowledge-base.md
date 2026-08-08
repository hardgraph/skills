> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Knowledge base setup and retrieval tuning

> Attach URLs, documents, and custom text to a Retell agent, then tune retrieval with query-rewrite instructions so it answers calls with accurate context.

## Overview

Knowledge bases are a collection of sources of information that your agent can access to retrieve relevant information during the call, that can provide additional context to the conversation. It can greatly improve the quality of the responses and the overall experience, especially in cases where there is a lot of information available (too long for putting in the prompt), but having the information is essential for the agent to respond correctly. This feature is quite useful for use cases like support, helpdesk, FAQ, etc.

Supported sources:

* Website content (via URLs)
* Documents (supported formats: .bmp, .csv, .doc, .docx, .eml, .epub, .heic, .html, .jpeg, .md, .msg, .odt, .org, .p7s, .pdf, .png, .ppt, .pptx, .rst, .rtf, .tiff, .txt, .tsv, .xls, .xlsx, .xml)
* Custom text snippets

### How it works

You can create knowledge bases, and link them to your agents. When a knowledge base is linked to an agent, the agent will always try to retrieve information from the knowledge base before responding. There's no need to change your prompt for it to trigger, as it will be done automatically, for every response generation.

During the creation of knowledge bases, it will chunk the source, embed them and store into a vector database.

During the call, when the agent is about to respond, it will use the transcript so far (prompt is not included) to find the most relevant chunks from the knowledge base, and feed them to the LLM as context.

Before searching, Retell condenses the recent conversation into a short, standalone search query and uses that query to find the most relevant chunks. You can steer how this query is built with a Knowledge Base Instruction (see the "Configure Knowledge Base Instruction" step below).

### Auto-refreshing and auto-crawling

You can enable auto-refreshing and/or auto-crawling for URL sources in your knowledge base.

* Auto-refreshing: When enabled, the system re-fetches all URLs in the knowledge base every 24 hours to ensure the latest content is reflected. If a URL can't be re-fetched, or the refresh reaches its time limit before reaching every source, the existing content for those sources is kept unchanged so the knowledge base stays usable. The detail view reports how many sources were affected.

* Auto-crawling: You can enable this feature for specific URL paths. The system will automatically crawl all pages under each path every 24 hours, excluding any URLs you’ve added to the exclusion list. All pages found—except those explicitly excluded—will be stored.

### Limits

Each knowledge base has the following limits:

* URL: at max 500 URLs.
* Auto-Crawling URL Paths: at max 200 exclusion URLs for each auto-crawling path, and at max 500 exclusion URLs per knowledge base.
* Text: at max 50 text snippets
* File: at max 25 files, with each file at max 50MB. For CSV, TSV, XLS, and XLSX, the row limit is 1000 rows and the column limit is 50 columns.

You can create multiple knowledge bases to overcome these limits. An agent can have more than one knowledge base linked to it.

## Best Practices

* Prefer `.md` (Markdown) files over `.txt`. Well-structured Markdown is chunked and retrieved more accurately.
  * Use clear, descriptive headings and keep each `##` section focused and reasonably short. If a `##` becomes long, split it into multiple `##`/`###` sections.
  * Write short paragraphs and lists to separate concepts; avoid walls of text.
  * For tabular or image-heavy content, retrieval may be less reliable; consider adding explanatory text so related information stays in the same chunk.
* Group related information within the same section so chunks remain cohesive and relevant.
* Avoid ambiguity and use specificity in references. Include names, dates, units, and avoid ambiguous pronouns like `it` or `this`, because prior chunks may not be present.
* Use more granular paths for auto-crawling instead of broad paths with many exclusion URLs. This improves crawl performance and helps avoid hitting the exclusion URL limits.
* Use the knowledge base to supply supporting information, not agent instructions or prompts. Put instructions in the agent's prompt.

## Use Knowledge Base

<Steps>
  <Step title="Access Knowledge Base Settings">
    1. Navigate to your dashboard
    2. Select the "Knowledge Base" tab
    3. Click the "Add" button in the top-right corner

    <Frame>
      <img height="700" src="https://mintcdn.com/retellai/rxvYffEkEJPRL1KD/images/knowledge-base/kb_1.png?fit=max&auto=format&n=rxvYffEkEJPRL1KD&q=85&s=974ecc2568c9daa93a127192903985ca" alt="Knowledge Base dashboard view" data-path="images/knowledge-base/kb_1.png" />
    </Frame>
  </Step>

  <Step title="Create Knowledge Base Items">
    Choose from three types of knowledge sources:

    1. **URL**: Import content from web pages
       * Supports single pages or entire websites
       * Automatically updates when content changes

    2. **File**: Upload documents
       * Supported formats: PDF, TXT, DOCX, etc.
       * Maximum file size: 50MB

    3. **Text**: Add custom content
       * Paste or type direct information
       * Ideal for specific instructions or data

    <Frame>
      <img height="700" src="https://mintcdn.com/retellai/M9QYKZE4hbt00HfL/images/knowledge-base/kb_2.png?fit=max&auto=format&n=M9QYKZE4hbt00HfL&q=85&s=1da9a44858f03c1e9191af5958e8edf9" alt="Adding knowledge base items" data-path="images/knowledge-base/kb_2.png" />
    </Frame>
  </Step>

  <Step title="Enable auto-crawling">
    Auto-crawling can be enabled for individual URL paths. URLs that are not selected will be added to the exclusion list and excluded from the knowledge base.

    You can edit auto-crawling paths to add or remove URLs from the exclusion list. Previously crawled URLs (displayed in bold) can be excluded, and URLs currently on the exclusion list can be reinstated.

    <Frame>
      <img height="700" src="https://mintcdn.com/retellai/rxvYffEkEJPRL1KD/images/knowledge-base/auto-crawling.png?fit=max&auto=format&n=rxvYffEkEJPRL1KD&q=85&s=61c357b664e835ca7d4ff717f2330b20" alt="Adding an auto-crawling path" data-path="images/knowledge-base/auto-crawling.png" />
    </Frame>

    <Frame>
      <img height="700" src="https://mintcdn.com/retellai/rxvYffEkEJPRL1KD/images/knowledge-base/edit-path.png?fit=max&auto=format&n=rxvYffEkEJPRL1KD&q=85&s=1b77966facefdb4ffa8f84e955c9fb3b" alt="Editing an auto-crawling path" data-path="images/knowledge-base/edit-path.png" />
    </Frame>

    <Frame>
      <img height="700" src="https://mintcdn.com/retellai/rxvYffEkEJPRL1KD/images/knowledge-base/edit-path-detail.png?fit=max&auto=format&n=rxvYffEkEJPRL1KD&q=85&s=3baa775e95cb3b2e6007a5669ef7a810" alt="Editing the exclusion list" data-path="images/knowledge-base/edit-path-detail.png" />
    </Frame>
  </Step>

  <Step title="Verify Added Items">
    After adding items, they will appear in your knowledge base list. You can:

    * View all added items
    * Edit existing items
    * Delete items when no longer needed

    <Frame>
      <img height="700" src="https://mintcdn.com/retellai/M9QYKZE4hbt00HfL/images/knowledge-base/kb_3.png?fit=max&auto=format&n=M9QYKZE4hbt00HfL&q=85&s=5f5d1360d08fcb8d673eab1f10f3e90a" alt="Knowledge base items list" data-path="images/knowledge-base/kb_3.png" />
    </Frame>
  </Step>

  <Step title="Connect to Your Agent">
    1. Open the agent editor
    2. Locate the "Knowledge Base" section
    3. Select the knowledge base items you want to use

    <Frame>
      <img height="700" src="https://mintcdn.com/retellai/M9QYKZE4hbt00HfL/images/knowledge-base/kb_4.png?fit=max&auto=format&n=M9QYKZE4hbt00HfL&q=85&s=afe2ceb218350d06914c9d053c9ec939" alt="Connecting knowledge base to agent" data-path="images/knowledge-base/kb_4.png" />
    </Frame>
  </Step>

  <Step title="Configure Knowledge Base Settings">
    After connecting a knowledge base to your agent, you can configure how many chunks are retrieved via "Adjust KB Retrieval Chunks and Similarity".

    * Chunks to retrieve: The max number of chunks to retrieve from the knowledge base, range 1-10. Default: 3.
    * Similarity Threshold: Adjust how strict the system is when matching chunks to the context. A higher setting gives you fewer, but more similar, matches. Default: 0.6.

    Note: Increasing "Chunks to retrieve" gives the LLM more context, but also increases the prompt length and can interfere with generation quality. For most cases, the recommended settings are 3 chunks and a 0.60 similarity threshold.

    <Frame>
      <img height="400" src="https://mintcdn.com/retellai/bQa8HG9t8oKhk9Vc/images/knowledge-base/kb_5.png?fit=max&auto=format&n=bQa8HG9t8oKhk9Vc&q=85&s=bb733e15ab2217a894c78212904b8abe" alt="Adjust knowledge base retrieval chunks and similarity settings" data-path="images/knowledge-base/kb_5.png" />
    </Frame>
  </Step>

  <Step title="Setup Node Level Knowledge Base">
    For Conversation Flow Agent, you can configure knowledge base at both Conversation Node level and Subagent Node level. Node-level knowledge base will be combined with the agent-level knowledge base to provide more accurate context for a specific topic.

    1. Open the agent editor
    2. Click the Conversation Node or Subagent Node you want to configure.
    3. Similar to agent level knowledge base config above, you can select and add the knowledge base items you want to use.

    <Frame>
      <img height="700" src="https://mintcdn.com/retellai/bhoR2p9MQ_lRE5vG/images/knowledge-base/node-level-kb.png?fit=max&auto=format&n=bhoR2p9MQ_lRE5vG&q=85&s=a2c2412f394cbfdd7cd0f6cb0f385a15" alt="Config node level knowledge base" data-path="images/knowledge-base/node-level-kb.png" />
    </Frame>
  </Step>

  <Step title="Configure Knowledge Base Instruction">
    Before searching your knowledge base, Retell condenses the recent conversation into a short search query. The **Knowledge Base Instruction** lets you give that step extra guidance, so the search focuses on what matters for your use case. This setting is optional.

    To set it, open the knowledge base's **Advanced Settings**, select **Configure Knowledge Base Instruction**, and enter the specific focus areas for retrieval (see the example below).

    * It shapes the search query; it does not filter or rewrite the retrieved chunks. Use "Chunks to retrieve" and "Similarity Threshold" above to control how much content is returned.
    * Keep it short and specific (up to 500 characters).
    * For most cases you can leave it empty, as the default query building already works well. Add an instruction only when retrieval keeps focusing on the wrong topic or misses context you expect.

    For Conversation Flow agents, you can set this per Conversation Node or Subagent Node. A node-level instruction overrides the flow-level instruction while that node is active.

    <Frame>
      <img height="400" src="https://mintcdn.com/retellai/pDB2uO7-3qiNGowN/images/knowledge-base/kb-instruction.png?fit=max&auto=format&n=pDB2uO7-3qiNGowN&q=85&s=fc4564a92470634e4624555e89b8c423" alt="Configure Knowledge Base Instruction dialog with focus areas for retrieval" data-path="images/knowledge-base/kb-instruction.png" />
    </Frame>
  </Step>
</Steps>

Your agent will now use this information when responding to queries related to the added content. Retrieved knowledge base content will be appended to the LLM prompt under the header **## Related Knowledge Base Contexts**.

## FAQ

<AccordionGroup>
  <Accordion title="Do I need to change my prompt to use knowledge base?">
    No, you don't need to change your prompt. It will be used automatically when added to an agent.
  </Accordion>

  <Accordion title="How can I prevent the LLM from generating information not found in the knowledge base?">
    You can add the following prompt to the agent:

    ```
    Only answer using the information in ## Related Knowledge Base Contexts.
    If ## Related Knowledge Base Contexts is missing or does not contain relevant information, respond:
    "There is no related information in knowledge base."
    ```
  </Accordion>

  <Accordion title="The agent response is not correct, what should I do?">
    Please check the knowledge base documents to see if the information is correct. And check the format of the source as suggested above, markdown format with clear paragraphs is recommended.
  </Accordion>

  <Accordion title="Will this add a long latency to the call?">
    We've optimized Knowledge Base retrieval latency for real time use case, so it should generally be under 100ms of latency impact.
  </Accordion>

  <Accordion title="Is there a way to check what's getting retrieved from the knowledge base?">
    We're working on adding this feature to expose this information soon.
  </Accordion>

  <Accordion title="Will the knowledge base name influence the retrieval?">
    No, the name is for display purpose only. It's not included in the retrieval process.
  </Accordion>

  <Accordion title="What happens if a source fails to process?">
    A source that fails to process doesn't block the others. If only some sources fail, the knowledge base still finishes and works with the sources that succeeded. Its detail view shows **Some knowledge base sources could not be processed:** followed by the sources that failed.

    If the whole knowledge base fails to process, its status shows **Failed** and the detail view shows **Failed to process the knowledge base:** with the error details.

    To clear the notice, select **Ignore**. To fix a failing source, re-add it after resolving the cause, for example an unreachable URL or a file that exceeds the supported format or size limits.
  </Accordion>

  <Accordion title="What happens if an auto-refresh can't finish?">
    A refresh never discards content it can't update. If a URL fails to re-fetch, its existing content is kept, and the detail view shows **Refresh completed with N errors. Existing content was retained for sources that could not be refreshed.** If the refresh reaches its time limit before reaching every source, the remaining sources are also left unchanged, and the detail view shows **Refresh reached its time limit after processing X of Y sources. The remaining sources were retained unchanged.**

    Both cases run again automatically on the next 24-hour refresh, so a transient timeout or a slow site usually resolves on its own. Select **Ignore** to clear the notice.
  </Accordion>

  <Accordion title="What does the Knowledge Base Instruction (focus areas) setting do?">
    Before each retrieval, Retell turns the recent conversation into a short search query. The Knowledge Base Instruction is extra guidance for building that query, for example telling it which topics or terms to prioritize. It influences what is searched for, not how results are filtered or ranked. It's optional, limited to 500 characters, and can be set per node for Conversation Flow agents. See the "Configure Knowledge Base Instruction" step above.
  </Accordion>
</AccordionGroup>

## Pricing

* Knowledge base creation:
  * First 10 knowledge bases are free.
  * Additional ones are billed at \$8 / month per knowledge base.
* Using knowledge base:
  * \$0.005 per minute of calls that have knowledge base enabled.
  * It does not matter if your agent is using 1 or many knowledge bases, the billing is the same.
