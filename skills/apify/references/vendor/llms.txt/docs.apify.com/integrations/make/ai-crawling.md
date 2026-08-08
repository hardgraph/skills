---
title: Make - AI crawling Actor integration
url: https://docs.apify.com/integrations/make/ai-crawling.md
parents:
  - [Apify documentation](https://docs.apify.com/llms.txt)
  - [Integrations](https://docs.apify.com/integrations.md)
  - [Workflows and notifications](https://docs.apify.com/integrations/workflows-and-notifications.md)
  - [Make](https://docs.apify.com/integrations/make.md)
previous: [Make](https://docs.apify.com/integrations/make.md)
next: [E-commerce](https://docs.apify.com/integrations/make/e-commerce.md)
---

> ## Documentation index
> Fetch the complete documentation index at: https://docs.apify.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Make - AI crawling Actor integration

Use the Apify Scraper for AI Crawling module in [Make](https://www.make.com/) to extract clean Markdown from websites and feed AI models, RAG pipelines, or LLM frameworks at scale.

Help keep this page up to date

This integration uses a third-party service. If you find outdated content, please [submit an issue on GitHub](https://github.com/apify/apify-docs/issues).

## Apify Scraper for AI Crawling

Apify Scraper for AI Crawling from [Apify](https://apify.com/) lets you extract text content from websites to feed AI models, LLM applications, vector databases, or Retrieval Augmented Generation (RAG) pipelines. It supports rich formatting using Markdown, cleans the HTML of irrelevant elements, downloads linked files, and integrates with AI ecosystems like LangChain, LlamaIndex, and other LLM frameworks.

To use these modules, you need an [Apify account](https://console.apify.com) and an [API token](https://docs.apify.com/integrations/api#api-token). You can find your token in the [Apify Console](https://console.apify.com/) under **Settings > API & Integrations**. After connecting, you can automate content extraction at scale and incorporate the results into your AI workflows.

## Connect Apify Scraper for AI Crawling

1. Create an account at [Apify](https://console.apify.com/). You can sign up using your email, Gmail, or GitHub account.

   ![Sign up page](/assets/images/wcc-signup-05f272efdc2e70fddd89ff59d8600031.png)

2. To connect your Apify account to Make, you can use an OAuth connection (recommended) or an Apify API token. To get the Apify API token, navigate to **[Settings > API & Integrations](https://console.apify.com/settings/integrations)** in Apify Console.

   ![Apify Console token for Make.png](/assets/images/apify-console-token-for-make-cf75dbeb5effdcab9bc204cee94cdb6a.png)

3. Find your token under **Personal API tokens** section. You can also create a new API token with multiple customizable permissions by clicking on **+ Create a new token**.

4. Click the **Copy** icon next to your API token to copy it to your clipboard. Then, return to your Make scenario interface.

   ![Apify token on Make.png](/assets/images/Apify_token_on_Make-78f67b559503d92cffb17e5abffd18d2.png)

5. In Make, click **Add** to open the **Create a connection** dialog of the chosen Apify Scraper module.

6. In the **API token** field, paste the API token you copied from Apify. Provide a clear **Connection name**, and click **Save**.

   ![Make API token](/assets/images/apify-token-for-module-on-make-6f80f8f08cdad0946d3bb7130ab2d087.png)

Once connected, you can build workflows to automate website extraction and integrate results into your AI applications.

## Apify Scraper for Website Content modules

After connecting the app, you can use one of the two modules as native scrapers to extract website content.

### Standard Settings module

The Standard Settings module is a streamlined component of the Website Content Crawler that allows you to quickly extract content from websites using optimized default settings. This module is perfect for extracting content from blogs, documentation sites, knowledge bases, or any text-rich website to feed into AI models.

#### How it works

The crawler starts with one or more **Start URLs** you provide, typically the top-level URL of a documentation site, blog, or knowledge base. It then:

* Crawls these start URLs
* Finds links to other pages on the site
* Recursively crawls those pages as long as their URL is under the start URL
* Respects URL patterns for inclusion/exclusion
* Automatically skips duplicate pages with the same canonical URL
* Provides various settings to customize crawling behavior (crawler type, max pages, depth, concurrency, etc.)

Once a web page is loaded, the Actor processes its HTML to ensure quality content extraction:

* Waits for dynamic content to load if using a headless browser
* Can scroll to a certain height to ensure all page content is loaded
* Can expand clickable elements to reveal hidden content
* Removes DOM nodes matching specific CSS selectors (like navigation, headers, footers)
* Optionally keeps only content matching specific CSS selectors
* Removes cookie warnings using browser extensions
* Transforms the page using the selected HTML transformer to extract the main content

#### Output data

For each crawled web page, you'll receive:

* *Page metadata*: URL, title, description, canonical URL
* *Cleaned text content*: The main article content with irrelevant elements removed
* *Markdown formatting*: Structured content with headers, lists, links, and other formatting preserved
* *Crawl information*: Loaded URL, referrer URL, timestamp, HTTP status
* *Optional file downloads*: PDFs, DOCs, and other linked documents

Sample output (shortened)


```json
{

  "url": "https://docs.apify.com/academy/scraping-basics-javascript",

  "crawl": {

    "loadedUrl": "https://docs.apify.com/academy/scraping-basics-javascript",

    "loadedTime": "2025-04-22T14:33:20.514Z",

    "referrerUrl": "https://docs.apify.com/academy",

    "depth": 1,

    "httpStatusCode": 200

  },

  "metadata": {

    "canonicalUrl": "https://docs.apify.com/academy/scraping-basics-javascript",

    "title": "Web scraping basics for JavaScript devs | Apify Documentation",

    "description": "Learn how to use JavaScript to extract information from websites in this practical course, starting from the absolute basics.",

    "languageCode": "en",

    "markdown": "# Web scraping basics for JavaScript devs\n\nWelcome to our comprehensive web scraping tutorial for beginners. This guide will take you through the fundamentals of extracting data from websites, with practical examples and exercises.\n\n## What is web scraping?\n\nWeb scraping is the process of extracting data from websites. It involves making HTTP requests to web servers, downloading HTML pages, and parsing them to extract the desired information.\n\n## Why learn web scraping?\n\n- **Data collection**: Gather information for research, analysis, or business intelligence\n- **Automation**: Save time by automating repetitive data collection tasks\n- **Integration**: Connect web data with your applications or databases\n- **Monitoring**: Track changes on websites automatically\n\n## Getting started\n\nTo begin web scraping, you'll need to understand the basics of HTML, CSS selectors, and HTTP. This tutorial will guide you through these concepts step by step.\n\n...",

    "text": "Web scraping basics for JavaScript devs\n\nWelcome to our comprehensive web scraping tutorial for beginners. This guide will take you through the fundamentals of extracting data from websites, with practical examples and exercises.\n\nWhat is web scraping?\n\nWeb scraping is the process of extracting data from websites. It involves making HTTP requests to web servers, downloading HTML pages, and parsing them to extract the desired information.\n\nWhy learn web scraping?\n\n- Data collection: Gather information for research, analysis, or business intelligence\n- Automation: Save time by automating repetitive data collection tasks\n- Integration: Connect web data with your applications or databases\n- Monitoring: Track changes on websites automatically\n\nGetting started\n\nTo begin web scraping, you'll need to understand the basics of HTML, CSS selectors, and HTTP. This tutorial will guide you through these concepts step by step.\n\n..."

  }

}
```


### Advanced Settings module

The Advanced Settings module provides complete control over the content extraction process, allowing you to fine-tune every aspect of the crawling and transformation pipeline. This module is ideal for complex websites, JavaScript-heavy applications, or when you need precise control over content extraction.

#### Key features

* *Multiple Crawler Options*: Choose between headless browsers (Playwright) or faster HTTP clients (Cheerio)
* *Custom Content Selection*: Specify exactly which elements to keep or remove
* *Advanced Navigation Control*: Set crawling depth, scope, and URL patterns
* *Dynamic Content Handling*: Wait for JavaScript-rendered content to load
* *Interactive Element Support*: Click expandable sections to reveal hidden content
* *Multiple Output Formats*: Save content as Markdown, HTML, or plain text
* *Proxy Configuration*: Use proxies to handle geo-restrictions or avoid IP blocks
* *Content Transformation Options*: Multiple algorithms for optimal content extraction

#### How it works

The Advanced Settings module provides granular control over the entire crawling process:

1. *Crawler Selection*: Choose from Playwright (Firefox/Chrome), or Cheerio based on website complexity
2. *URL Management*: Define precise scoping with include/exclude URL patterns
3. *DOM Manipulation*: Control which HTML elements to keep or remove
4. *Content Transformation*: Apply specialized algorithms for content extraction
5. *Output Formatting*: Select from multiple formats for AI model compatibility

#### Configuration options

Advanced Settings offers numerous configuration options, including:

* *Crawler Type*: Select the rendering engine (browser or HTTP client)
* *Content Extraction Algorithm*: Choose from multiple HTML transformers
* *Element Selectors*: Specify which elements to keep, remove, or click
* *URL Patterns*: Define URL inclusion/exclusion patterns with glob syntax
* *Crawling Parameters*: Set concurrency, depth, timeouts, and retries
* *Proxy Configuration*: Configure proxy settings for robust crawling
* *Output Options*: Select content formats and storage options

#### Output data

In addition to the standard output fields, Advanced Settings provides:

* *Multiple Format Options*: Content in Markdown, HTML, or plain text
* *Debug Information*: Detailed extraction diagnostics and snapshots
* *HTML Transformations*: Results from different content extraction algorithms
* *File Storage Options*: Flexible storage for HTML, screenshots, or downloaded files

## Other scrapers available

Looking for more scrapers? You can access any of thousands of our scrapers on Apify Store by using the [general Apify connections on Make](https://www.make.com/en/integrations/apify).
