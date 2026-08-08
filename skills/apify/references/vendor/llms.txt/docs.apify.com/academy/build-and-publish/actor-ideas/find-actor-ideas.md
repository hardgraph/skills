---
title: Find ideas for new Actors
url: https://docs.apify.com/academy/build-and-publish/actor-ideas/find-actor-ideas.md
parents:
  - [Apify documentation](https://docs.apify.com/llms.txt)
  - [Apify Academy](https://docs.apify.com/academy.md)
previous: [Why publish Actors on Apify](https://docs.apify.com/academy/build-and-publish/why.md)
next: [Validate your Actor idea](https://docs.apify.com/academy/build-and-publish/actor-ideas/actor-validation.md)
---

> ## Documentation index
> Fetch the complete documentation index at: https://docs.apify.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Find ideas for new Actors

Learn what kind of software tools are suitable to be packaged and published as Actors on Apify, and where you can find inspiration what to build.

***

## What can you build as an Actor

[Actors](https://docs.apify.com/actors) are a new way for building serverless micro-apps, which are easy to develop, share, integrate, and build upon.

They are useful for backend automation jobs, which users set up, integrate into their workflow, and let run in the background, rather than consumer-facing applications that users need to interact with.

Actors can run in two modes:

* In *batch mode*, they take a well-defined input, perform a job, and produce a well-defined output. This is useful for longer-running operations, such as web crawling or data processing.
* In *standby mode*, they run as a web server at a specific public URL. This is useful for request-response style applications, such as APIs or MCP servers.

### Web scrapers and crawlers

This is the most common type of Actors on [Apify Store](https://apify.com/store). These Actors navigate websites, collect information from web pages, and store structured data in datasets for further processing.

Examples:

* **Website-specific scrapers** ([Amazon Product Scraper](https://apify.com/junglee/amazon-crawler), [LinkedIn Profile Scraper](https://apify.com/curious_coder/linkedin-profile-scraper))
* **Search engines** ([Google Search Results](https://apify.com/apify/google-search-scraper), [Bing Search](https://apify.com/curious_coder/bing-search-scraper))
* **Social media** ([X/Twitter Scraper](https://apify.com/apidojo/twitter-scraper-lite), [Instagram Scraper](https://apify.com/apify/instagram-scraper))
* **E-commerce data** ([Shopify Store Scraper](https://apify.com/autofacts/shopify), [eBay Price Monitor](https://apify.com/dtrungtin/ebay-items-scraper))
* **General-purpose crawlers** ([Web Scraper](https://apify.com/apify/web-scraper), [Website Content Crawler](https://apify.com/apify/website-content-crawler))

### SaaS API wrappers

These Actors wrap existing SaaS services as Actors to make them accessible through the Apify platform and its many integrations, potentially with additional services built on top.

Example Actors:

* [OpenRouter](https://apify.com/apify/openrouter)
* [Parsera](https://apify.com/parsera-labs/parsera)
* [Super Scraper API](https://apify.com/apify/super-scraper-api)

For inspiration, see:

* [RapidAPI Hub](https://rapidapi.com/hub)
* [Google Cloud APIs](https://cloud.google.com/apis)
* [{API}Market](https://api.market/)
* [openapi](https://openapi.com/products)
* [n8n Workflow Automation Templates](https://n8n.io/workflows/)
* [Make Template gallery](https://www.make.com/en/templates)

### Open-source libraries

Many open-source automation or data processing tools do not have a presence in the cloud, and need to be installed locally in "just five easy steps". Wrap those tools as Actors and make it easy for users to try and integrate those tools.

Examples:

* [Sherlock](https://apify.com/misceres/sherlock)
* [Docling](https://apify.com/vancura/docling)
* [Monolith](https://apify.com/snshn/monolith)
* [Crawl4AI](https://apify.com/janbuchar/crawl4ai)

For inspiration, check out the [Open-source category](https://apify.com/store/categories/open-source) in Apify Store, or the following list:

GitHub projects potentially suitable for turning into Actors

* [https://github.com/bytedance/Dolphin](https://github.com/bytedance/Dolphin)
* [https://github.com/google/langextract](https://github.com/google/langextract)
* [https://github.com/ytdl-org/youtube-dl](https://github.com/ytdl-org/youtube-dl)
* [https://github.com/virattt/ai-hedge-fund](https://github.com/virattt/ai-hedge-fund)
* [https://github.com/jamesturk/scrapeghost/](https://github.com/jamesturk/scrapeghost/)
* [https://github.com/idosal/git-mcp](https://github.com/idosal/git-mcp)
* [https://github.com/browser-use/browser-use](https://github.com/browser-use/browser-use)
* [https://github.com/browserbase/stagehand](https://github.com/browserbase/stagehand)
* [https://github.com/BuilderIO/gpt-crawler](https://github.com/BuilderIO/gpt-crawler)
* [https://github.com/errata-ai/vale](https://github.com/errata-ai/vale)
* [https://github.com/scrapybara/scrapybara-demos](https://github.com/scrapybara/scrapybara-demos)
* [https://github.com/David-patrick-chuks/Riona-AI-Agent](https://github.com/David-patrick-chuks/Riona-AI-Agent)
* [https://github.com/projectdiscovery/katana](https://github.com/projectdiscovery/katana)
* [https://github.com/exa-labs/company-researcher](https://github.com/exa-labs/company-researcher)
* [https://github.com/Janix-ai/mcp-validator](https://github.com/Janix-ai/mcp-validator)
* [https://github.com/JoshuaC215/agent-service-toolkit](https://github.com/JoshuaC215/agent-service-toolkit)
* [https://github.com/dequelabs/axe-core](https://github.com/dequelabs/axe-core)
* [https://github.com/janreges/siteone-crawler](https://github.com/janreges/siteone-crawler)
* [https://github.com/eugeneyan/news-agents](https://github.com/eugeneyan/news-agents)
* [https://github.com/askui/askui](https://github.com/askui/askui)
* [https://github.com/Shubhamsaboo/awesome-llm-apps](https://github.com/Shubhamsaboo/awesome-llm-apps)
* [https://github.com/TheAgenticAI/TheAgenticBrowser](https://github.com/TheAgenticAI/TheAgenticBrowser)
* [https://github.com/zcaceres/markdownify-mcp](https://github.com/zcaceres/markdownify-mcp)

Open Source Fair Share

Developers of open-source Actors can earn passive affiliate income through Apify's [Open source fair share](https://apify.com/partners/open-source-fair-share) program to help them support their projects.

### MCP servers and tools for AI

[Model Context Protocol (MCP)](https://modelcontextprotocol.io/docs/getting-started/intro) lets AI agents interact with external tools and data sources. Many MCP servers are still stand-alone packages that need to be installed locally, which is both inefficient and insecure, or require an external service account. Publishing these packages as Actors makes the MCP servers remote and accessible through the Apify platform and ecosystem, including the new agentic payments protocols.

Examples:

* [Playwright MCP Server](https://apify.com/jiri.spilka/playwright-mcp-server)
* [Browserbase MCP Server](https://apify.com/agentify/browserbase-mcp-server/api/mcp)
* [Firecrawl MCP Server](https://apify.com/agentify/firecrawl-mcp-server)
* [Brave Search MCP Server](https://apify.com/agentify/brave-search-mcp-server)

For more inspiration, check out the [MCP server category](https://apify.com/store/categories/mcp-servers) in Apify Store.

### AI agents

Build Actors that use LLMs to perform complex tasks autonomously. These Actors can navigate websites, make decisions, and complete multistep workflows. The Actor can either run the agent directly or just wrap an existing agent running elsewhere and call it using an API.

Secure execution

Actors are cloud-based sandboxes that can securely run any AI-generated code. As LLMs are [generally better](https://www.anthropic.com/engineering/code-execution-with-mcp) at generating code than selecting tools, one can use Actors to generate task-specific code and expose it using Actor interface to other apps and agents.

For inspiration, see:

* [Agents category](https://apify.com/store/categories/agents) in Apify Store
* [agent.ai marketplace](https://agent.ai/)
* [AI Agents Directory](https://aiagentsdirectory.com/landscape)
* [Awesome AI Agents by E2B](https://github.com/e2b-dev/awesome-ai-agents)
* [n8n Workflow Automation Templates](https://n8n.io/workflows/)

### Other

Any repetitive job matching the following criteria might be suitable for turning into an Actor:

* The job is better to be run in the background in the cloud and forgotten.
* The task is isolated and can be described and delegated to another person.
* There are at least a few hundred people in the world dealing with this problem.

If you look closely, you'll start seeing opportunities for new Actors everywhere. Be creative!

## Use the Actor ideas page

The [Actor ideas](https://apify.com/ideas) page is where you can find inspiration for new Actors sourced from the Apify community.

### Browse and claim ideas

1. *Visit* [apify.com/ideas](https://apify.com/ideas) to find ideas that interest you. Look for ideas that align with your skills.

2. *Select an Actor idea*: Review the details and requirements. Check the status - if it's marked **Open to develop**, you can start building.

3. *Build your Actor*: Develop your Actor based on the idea. You don't need to notify Apify during development.

4. *Prepare for launch*: Ensure your Actor meets quality standards and has a comprehensive README with installation instructions, usage details, and examples.

5. *Publish your Actor*: Deploy your Actor on Apify Store and make it live.

6. *Monitor and optimize*: Track your Actor's performance and user feedback. Make improvements to keep your Actor current.

### Submit your own ideas

You can submit your own Actor ideas through [this form](https://apify.com/ideas?modal=ACTOR_IDEA). Please provide clear details about what the tool should do and how it should work.

## Find ideas from other sources

Beyond the [Actor ideas](https://apify.com/ideas) page, you can find new Actor ideas through:

* SEO tools: Discover relevant search terms people use to find solutions
* Your experience: Draw from problems you've encountered in your work
* Community discussions: Browse Reddit, Twitter, Stack Overflow, and forums for user pain points
* Competitor analysis: Research existing tools and identify gaps
* Other portals, such as [Indiehackers](https://www.indiehackers.com/), [Superframeworks](https://superframeworks.beehiiv.com/t/startup-ideas), etc.

Once you get one, learn how to [validate your Actor idea](https://docs.apify.com/academy/build-and-publish/actor-ideas/actor-validation.md).
