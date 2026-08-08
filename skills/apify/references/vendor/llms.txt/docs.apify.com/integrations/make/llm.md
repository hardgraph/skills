---
title: Make - LLMs Actor integration
url: https://docs.apify.com/integrations/make/llm.md
parents:
  - [Apify documentation](https://docs.apify.com/llms.txt)
  - [Integrations](https://docs.apify.com/integrations.md)
  - [Workflows and notifications](https://docs.apify.com/integrations/workflows-and-notifications.md)
  - [Make](https://docs.apify.com/integrations/make.md)
previous: [E-commerce](https://docs.apify.com/integrations/make/e-commerce.md)
next: [n8n](https://docs.apify.com/integrations/n8n.md)
---

> ## Documentation index
> Fetch the complete documentation index at: https://docs.apify.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Make - LLMs Actor integration

Use the Apify Scraper for LLMs module in [Make](https://www.make.com/) to query Google Search, scrape the top results, and return clean Markdown for AI assistants and RAG pipelines.

Help keep this page up to date

This integration uses a third-party service. If you find outdated content, please [submit an issue on GitHub](https://github.com/apify/apify-docs/issues).

## Apify Scraper for LLMs

Apify Scraper for LLMs from [Apify](https://apify.com) is a web browsing module for OpenAI Assistants, RAG pipelines, and AI agents. It can query Google Search, scrape the top results, and return page content as Markdown for downstream AI processing.

To use these modules, you need an [Apify account](https://console.apify.com) and an [API token](https://docs.apify.com/integrations/api#api-token). You can find your token in Apify Console under **Settings > API & Integrations**. After connecting, you can automate content extraction and integrate results into your AI workflows.

## Connect Apify Scraper for LLMs

1. Create an account at [Apify](https://console.apify.com/). You can sign up using your email, Gmail, or GitHub account.

   ![Make interface showing API token field and connection name field for Apify integration setup](/assets/images/rag-signup-e6bc62ebf159bbe6d4b19eaffb677af8.png)

2. To connect your Apify account to Make, you can use an OAuth connection (recommended) or an Apify API token. To get the token, go to **[Settings > API & Integrations](https://console.apify.com/settings/integrations)** in Apify Console.

   ![Apify Console sign-up page with email, Gmail, and GitHub sign-up options](/assets/images/apify-console-token-for-make-cf75dbeb5effdcab9bc204cee94cdb6a.png)

3. Find your token under **Personal API tokens**. You can also create a new token with custom permissions by clicking **+ Create a new token**.

4. Click the **Copy** icon to copy your API token, then return to your Make scenario.

   ![Apify Console Settings page showing Personal API tokens section with token management options](/assets/images/Apify_token_on_Make-78f67b559503d92cffb17e5abffd18d2.png)

5. In Make, click **Add** to open the **Create a connection** dialog of the chosen Apify Scraper module.

6. In the **API token** field, paste your token, provide a clear **Connection name**, and click **Save**.

   ![Make connection dialog with completed API token and connection name fields for Apify Scraper module](/assets/images/apify-token-for-module-on-make-fb8870f28951892d4f031eb2899e3a3f.png)

Once connected, you can build workflows that search the web, extract content, and pass it to your AI applications.

## Apify Scraper for LLMs modules

After connecting the app, you can use two modules to search and extract content.

### Standard Settings module

Use Standard Settings to quickly search the web and extract content with optimized defaults. This is ideal for AI agents that need to answer questions or gather information from multiple sources.

The module supports two modes:

* *Search mode* (keywords)

  * Queries Google Search with your keywords (supports advanced operators)
  * Retrieves the top N organic results
  * Loads each result and extracts the main content
  * Returns Markdown-formatted content

* *Direct URL mode* (URL)

  * Navigates to a specific URL
  * Extracts page content
  * Skips Google Search

#### How it works

When you provide keywords, the module runs Google Search, parses the results, and collects organic URLs. For content extraction, it loads pages, waits for dynamic content to render, removes clutter, extracts the main content, and converts it to Markdown. Finally, it generates output by combining content, adding metadata and sources, and formatting everything for AI consumption.

#### Output data

Standard Settings output (shortened)


```json
{

  "query": "web browser for RAG pipelines -site:reddit.com",

  "crawl": {

    "httpStatusCode": 200,

    "httpStatusMessage": "OK",

    "loadedAt": "2025-06-30T10:15:23.456Z",

    "uniqueKey": "https://example.com/article",

    "requestStatus": "handled"

  },

  "searchResult": {

    "title": "Building RAG Pipelines with Web Browsers",

    "description": "Integrate web browsing into your RAG pipeline for real-time retrieval.",

    "url": "https://example.com/article",

    "resultType": "organic",

    "rank": 1

  },

  "metadata": {

    "title": "Building RAG Pipelines with Web Browsers",

    "description": "Add web browsing to RAG systems",

    "languageCode": "en",

    "url": "https://example.com/article"

  },

  "markdown": "# Building RAG Pipelines with Web Browsers\n\n..."

}
```


#### Configuration (Standard Settings)

* *Search query*: Google Search keywords or a direct URL
* *Maximum results*: Number of top search results to process (default: 3)
* *Output formats*: Markdown, text, or HTML
* *Remove cookie warnings*: Dismiss cookie consent dialogs
* *Debug mode*: Enable extraction diagnostics

### Advanced Settings module

Advanced Settings give you full control over search and extraction. Use it for complex sites or production RAG pipelines.

#### Key features

* *Advanced search options*: full Google operator support
* *Flexible crawling tools*: browser-based (Playwright) or HTTP-based (Cheerio)
* *Proxy configuration*: handle geo-restrictions and rate limits
* *Granular content control*: include, remove, and click selectors
* *Dynamic content handling*: wait strategies for JavaScript rendering
* *Multiple output formats*: Markdown, HTML, or text
* *Request management*: timeouts, retries, and concurrency

#### Configuration options

* *Search*: query, max results (1–100), SERP proxy group, SERP retries
* *Scraping*: tool (browser-playwright, raw-http), HTML transformer, selectors (remove/keep/click), expand clickable elements
* *Requests*: timeouts, retries, dynamic content wait
* *Proxy*: use Apify Proxy, proxy groups, countries
* *Output*: formats, save HTML/Markdown, debug mode, save screenshots

#### Output data

Advanced Settings output (shortened)


```json
{

  "query": "advanced RAG implementation strategies",

  "crawl": {

    "httpStatusCode": 200,

    "httpStatusMessage": "OK",

    "loadedUrl": "https://ai-research.com/rag-strategies",

    "loadedTime": "2025-06-30T10:45:12.789Z",

    "referrerUrl": "https://www.google.com/search?q=advanced+RAG+implementation+strategies",

    "uniqueKey": "https://ai-research.com/rag-strategies",

    "requestStatus": "handled",

    "depth": 0

  },

  "searchResult": {

    "title": "Advanced RAG Implementation: A Complete Guide",

    "description": "Cutting-edge strategies for RAG systems.",

    "url": "https://ai-research.com/rag-strategies",

    "resultType": "organic",

    "rank": 1

  },

  "metadata": {

    "canonicalUrl": "https://ai-research.com/rag-strategies",

    "title": "Advanced RAG Implementation: A Complete Guide | AI Research",

    "description": "Vector DBs, chunking, and optimization techniques.",

    "languageCode": "en"

  },

  "markdown": "# Advanced RAG Implementation: A Complete Guide\n\n...",

  "debug": {

    "extractorUsed": "readableText",

    "elementsRemoved": 47,

    "elementsClicked": 3

  }

}
```


### Use cases

* Quick information retrieval for AI assistants
* General web search integration and Q\&A
* Production RAG pipelines that need reliability
* Extracting content from JavaScript-heavy sites
* Building specialized knowledge bases and research workflows

### Best practices

To get the best search results, use specific keywords and operators, and exclude unwanted domains with `-site:`. For better performance, use HTTP mode for static sites and only switch to browser mode when necessary. You can also tune concurrency settings based on your needs. To maintain content quality, remove non-content elements, choose the right HTML transformer, and enable debug mode when troubleshooting. Finally, ensure reliable operation by setting appropriate timeouts and retries, and monitoring HTTP status codes for errors.

## Other scrapers available

Looking for more scrapers? You can access any of thousands of our scrapers on Apify Store by using the [general Apify connections on Make](https://www.make.com/en/integrations/apify).
