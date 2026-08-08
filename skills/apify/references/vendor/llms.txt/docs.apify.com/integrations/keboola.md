---
title: Keboola integration
url: https://docs.apify.com/integrations/keboola.md
parents:
  - [Apify documentation](https://docs.apify.com/llms.txt)
  - [Integrations](https://docs.apify.com/integrations.md)
  - [Data and storage](https://docs.apify.com/integrations/data-and-storage.md)
previous: [HubSpot](https://docs.apify.com/integrations/hubspot.md)
next: [Snowflake](https://docs.apify.com/integrations/snowflake.md)
---

> ## Documentation index
> Fetch the complete documentation index at: https://docs.apify.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Keboola integration

With Apify integration for [Keboola](https://www.keboola.com/), you can extract data from various sources using your Apify Actors and load it into Keboola for further processing, transformation, and integration with other platforms.

The Keboola integration allows you to run your Actors, fetch items from datasets, and retrieve results, all within the Keboola platform.

Help keep this page up to date

This integration uses a third-party service. If you find outdated content, please [submit an issue on GitHub](https://github.com/apify/apify-docs/issues).

## Connect Apify with Keboola

To use the Apify integration on Keboola, you will need to:

* Have an [Apify account](https://console.apify.com/).
* Have a [Keboola account](https://www.keboola.com/).

### Step 1: Create a new Data Source in Keboola

Once your Keboola account is ready and you are logged in, navigate to the **Components** section in the top menu and click the **Add Component** button.

![Keboola dashboard](/assets/images/keboola-dashboard-9c1f255d99ade9239aa323f485051ae3.png)

In the list of available Components, find and select the **Apify** from Data Sources and click on the **Create configuration** button.

![Keboola component search](/assets/images/keboola-components-e902b830867152bff6fa84fd58fbfdea.png)

Provide a name and description for your configuration, then click the **Create Configuration** button.

![Keboola configuration setup](/assets/images/keboola-create-configuration-6abf700cb8fb021e53980e9dcc9e840c.png)

### Step 2: Configure the Apify Data Source

With the new configuration created, you can now configure the data source to retrieve the needed data. Click on the **Configure Component** button to begin the setup process.

![Keboola component configuration](/assets/images/keboola-configure-component-5837e3c680c9e2afbfc913d22b178a38.png)

#### Choose an action

In the next step, you can choose the action you want to perform:

* **Run Actor**: This action runs the selected Actor, waits until it finishes, and then pushes all items from the default dataset to Keboola Storage.
* **Retrieve dataset items from the last Actor run** - This action takes the dataset of a specific Actor's last run.
* **Run Task** - This action runs the selected task, waits until it finishes, and then pushes all items from the default dataset to Keboola Storage.
* **Retrieve items from the last task run** - This action takes the dataset of a specific task last run.
* **Retrieve items from Dataset**: This action takes the dataset ID or dataset name and retrieves all items from that dataset.

![Keboola component action setup ](/assets/images/keboola-component-setup-c75d4f496be06dcb7056fc84c181183e.png)

#### Authentication

After selecting the action, you will need to provide your Apify API credentials. You can find these credentials on your Apify account page by navigating to **Settings > API & Integrations** and copying them into the provided form.

![Keboola API authentication](/assets/images/keboola-setup-api-token-2fa67938e05c98a15521b914d8c08d34.png)

#### Specifications

In the specifications step, you can set up various options for your Actor run:

* **Actor**: Select the Actor you want to run from your Apify account.
* **Input Table**: Choose a table from the Keboola platform to be sent to the Actor as input data.
* **Output field**: Comma-separated list of fields to be picked from the dataset.
* **Memory**: Adjust the memory settings if needed (the default values can be kept).
* **Build**: Adjust if you want to run a specific build of an Actor. Tag or number of the build to run.
* **Actor Input**: Pass any JSON data as input to the Actor.

Once you have filled in all the necessary options, click the **Save** button to save your configuration.

![Keboola component specification setup](/assets/images/keboola-setup-specification-2bff78a68a69f8c899b29632806362c0.png)

### Step 3: Run the configured Data Source

After your data source has been configured, you can run it by clicking the **Run** button in the upper-right corner of your configuration.

![Keboola run configured component](/assets/images/keboola-run-component-6b03974ffc7beeaec3e8349470c1a208.png)

You can monitor the progress of your run in the job detail section on the right-hand side of the page.

Once the run finishes successfully, you can find the results by following the link in the Storage Stats section of the job detail page.

## Next steps

With your data now in Keboola, you can integrate it with dozens of other services that Keboola supports. Check out the [complete list of integrations](https://www.keboola.com/product/integrations) to explore your options.

You can set up a writer for a selected service using Keboola Writer or create [orchestrations](https://help.keboola.com/orchestrator/) to transform, merge, or split your data.

Keboola Helper

In Apify Store, you'll find the [Keboola Input Mapping Actor](https://apify.com/drobnikj/keboola-input-mapping), designed to streamline the integration between Apify and Keboola. This helper Actor parses the input table received from the Apify <> Keboola Data Source and maps the data into the required input format for another task or Actor within the Keboola platform.

If you have any questions or need assistance, feel free to contact us at [info@apify.com](mailto:info@apify.com), through our live chat, or in our [developer community on Discord](https://discord.com/invite/jyEM2PRvMU).
