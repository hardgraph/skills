---
title: Creating Actors
url: https://docs.apify.com/academy/getting-started/creating-actors.md
parents:
  - [Apify documentation](https://docs.apify.com/llms.txt)
  - [Apify Academy](https://docs.apify.com/academy.md)
  - [Getting started](https://docs.apify.com/academy/getting-started.md)
previous: [Actors](https://docs.apify.com/academy/getting-started/actors.md)
next: [Inputs & outputs](https://docs.apify.com/academy/getting-started/inputs-outputs.md)
---

> ## Documentation index
> Fetch the complete documentation index at: https://docs.apify.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Creating Actors

**This lesson offers hands-on experience in building and running Actors in Apify Console using a template. By the end of it, you will be able to build and run your first Actor using an Actor template.**

***

You can create an Actor in several ways. In this tutorial, we'll focus on the easiest method: selecting an Actor template. You don't need to install any special software, and you can complete the entire process directly in Apify Console using your Apify account.

## Choose the source

To choose the source of your new Actor, go to Apify Console > [Actors](https://console.apify.com/actors), and select **Develop new**.

You have two options:

* Create an Actor from existing source code by using Git providers or pushing the code from your local machine using Apify CLI.
* Create an Actor from a template.

## Create Actor from existing source code

If you already have your code hosted by a Git provider, you can use it to create an Actor by linking the repository. If you use GitHub, you can use the [GitHub integration](https://docs.apify.com/integrations/github.md) to create an Actor from your public or private repository. You can also use GitLab, Bitbucket or other Git providers or external repositories. To see all available options, select **Import from Git**.

You can also push your existing code from your local machine using [Apify CLI](https://docs.apify.com/cli). This option is useful when you develop your code locally and then you want to push it to Apify Console to run the code as an Actor in the cloud. For details, see [Quick start](https://docs.apify.com/cli/docs/quick-start) in the Apify CLI docs.

## Create Actor from a template

Python, JavaScript, and TypeScript have several template options that you can use. To see all available templates, select **Browse all templates**.

In this tutorial, let's choose the **One-Page HTML Scraper with Cheerio** template.

You will end up on a template detail page where you can see all the important information about the template - description, included features, used technologies, and what is the use-case of this template. More importantly, there is a code preview and also instructions for how the code works.

![JavaScript template detail page](/assets/images/create-actor-template-detail-page-8ff37bb2c50a5756663f61ffca76a010.png)

### Use templates in Web IDE

By clicking **Use this template** button you will create the Actor in Apify Console and you will be moved to the **Code** tab with the [Web IDE](https://docs.apify.com/actors/development/quick-start/web-ide.md) where you can see the code of the template and start editing it.

Web IDE

The Web IDE is a great tool for developing your Actor directly in Apify Console without the need to install or use any other software.

![Web IDE](/assets/images/create-actor-web-ide-53857177e9d96389456c6d0e5feff72a.png)

### Use templates locally

If you want to use the template locally, you can again use our [Apify CLI](https://docs.apify.com/cli) to download the template to your local machine.

Local development

Creating an Actor from a template locally is a great option if you want to develop your code using your local environment and IDE and then push the final solution back to Apify Console.

When you click on the **Use locally** button, you'll be presented with instructions on how to create an Actor from this template in your local environment.

With the Apify CLI installed, you can run the following commands in your terminal:


```shell
apify create my-actor -t getting_started_node
```



```shell
cd my-actor

apify run
```


![Use the template locally](/assets/images/create-actor-template-locally-b4d9caaebe286c60cbc29017f02ab3d4.png)

## Start with scraping single page

This template is a great starting point for web scraping as it extracts data from a single website. It uses [Axios](https://axios-http.com/docs/intro) for downloading the page content and [Cheerio](https://cheerio.js.org/) for parsing the HTML from the content.

Let's see what's inside the **Start with JavaScript** template. The main logic of the template lives in the `src/main.js` file.


```js
// Axios - Promise based HTTP client for the browser and node.js (Read more at https://axios-http.com/docs/intro).

import { Actor } from 'apify';

import axios from 'axios';

// Cheerio - The fast, flexible & elegant library for parsing and manipulating HTML and XML (Read more at https://cheerio.js.org/).

import * as cheerio from 'cheerio';

// Apify SDK - toolkit for building Apify Actors (Read more at https://docs.apify.com/sdk/js/).



// The init() call configures the Actor for its environment. It's recommended to start every Actor with an init().

await Actor.init();



// Structure of input is defined in input_schema.json

const input = await Actor.getInput();

const { url } = input;



// Fetch the HTML content of the page.

const response = await axios.get(url);



// Parse the downloaded HTML with Cheerio to enable data extraction.

const $ = cheerio.load(response.data);



// Extract all headings from the page (tag name and text).

const headings = [];

$('h1, h2, h3, h4, h5, h6').each((i, element) => {

    const headingObject = {

        level: $(element).prop('tagName').toLowerCase(),

        text: $(element).text(),

    };

    console.log('Extracted heading', headingObject);

    headings.push(headingObject);

});



// Save headings to Dataset - a table-like storage.

await Actor.pushData(headings);



// Gracefully exit the Actor process. It's recommended to quit all Actors with an exit().

await Actor.exit();
```


The Actor takes the `url` from the input and then:

1. Sends a request to the URL.
2. Downloads the page's HTML content.
3. Extracts headings (H1 - H6) from the page.
4. Stores the extracted data.

The extracted data is stored in the [Dataset](https://docs.apify.com/storage/dataset.md) where you can preview it and download it. We'll show how to do that later in Run the Actor section.

Customize template

Feel free to play around with the code and add some more features to it. For example, you can extract all the links from the page or extract all the images or completely change the logic of this template.

Keep in mind that this template uses [input schema](https://docs.apify.com/academy/deploying-your-code/input-schema.md) defined in the `.actor/input_schema.json` file and linked to the `.actor/actor.json`. If you want to change the input schema, you need to change it in those files as well.

Learn more about the Actor input and output [in the next page](https://docs.apify.com/academy/getting-started/inputs-outputs.md).

## Build the Actor

In order to run the Actor, you need to [build](https://docs.apify.com/actors/development/builds-and-runs/builds.md) it first. Click on the **Build** button at the bottom of the page or **Build now** button right under the code editor.

![Build the Actor](/assets/images/build-actor-5aaefc12ec3684c08bd92818b88e3576.png)

After you've clicked the **Build** button, it'll take around 5–10 seconds to complete the build. You'll know it's finished when you see a green **Start** button.

![Start button](data:image/png;base64,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)

## Fill the input

And now we are ready to run the Actor. But before we do that, let's give the Actor some input by going to the `Input` tab.

The input tab is where you can provide the Actor with some meaningful input. In this case, we'll be providing the Actor with a URL to scrape. For now, we'll use the prefilled value of [Apify website](https://apify.com/) (`https://apify.com/`).

You can change the website you want to extract the data from by changing the URL in the input field.

![Input tab](/assets/images/actor-input-tab-93256e980a452661e0a608910bddecb1.png)

## Run the Actor

Once you have provided the Actor with some URL you want to extract the data from, click **Start** button and wait a few seconds. You should see the Actor run logs in the **Last run** tab.

![Actor run logs](/assets/images/actor-run-1c928e9040dac9112be91f2bfbfde02f.png)

After the Actor finishes, you can preview or download the extracted data by clicking on the **Export X results** button.

![Export results](/assets/images/actor-run-dataset-a27223a2b496df661e18f8e311c9bfc4.png)

And that's it! You've just created your first Actor and extracted data from a website 🎉.

## Next up

We've created an Actor, but how can we give it more complex inputs and make it do stuff based on these inputs? This is exactly what we'll be discussing in the [next lesson](https://docs.apify.com/academy/getting-started/inputs-outputs.md)'s activity.
