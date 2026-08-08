---
title: Airbyte integration
url: https://docs.apify.com/integrations/airbyte.md
parents:
  - [Apify documentation](https://docs.apify.com/llms.txt)
  - [Integrations](https://docs.apify.com/integrations.md)
  - [Data and storage](https://docs.apify.com/integrations/data-and-storage.md)
previous: [Data and storage](https://docs.apify.com/integrations/data-and-storage.md)
next: [Airtable](https://docs.apify.com/integrations/airtable.md)
---

> ## Documentation index
> Fetch the complete documentation index at: https://docs.apify.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Airbyte integration

Airbyte is an open-source data integration platform that allows you to move your data between different sources and destinations using pre-built connectors, which are maintained either by Airbyte itself or by its community. One of these connectors is the Apify Dataset connector, which makes it simple to move data from Apify datasets to any supported destination.

To use Airbyte's Apify connector you need to:

* Have an Apify account.
* Have an Airbyte account.

Help keep this page up to date

This integration uses a third-party service. If you find outdated content, please [submit an issue on GitHub](https://github.com/apify/apify-docs/issues).

## Set up Apify connector in Airbyte

Once you have all the necessary accounts set up, you need to set up the Apify connector. To do so, you will need to navigate to **Sources** tab in Airbyte and select **Apify Dataset**

![Airbyte sources tab](/assets/images/airbyte-sources-7915e8b8c9b5959862c7c52c1505067f.png)

You will need to provide a **dataset ID** and your Apify API Token. You can find both of these in [Apify Console](https://console.apify.com).

![Airbyte source setup](/assets/images/airbyte-source-setup-8c9f9311148dad47f6c80bdbfe9cf3f1.png)

To find your **dataset ID**, you need to navigate to the **Storage** tab in Apify Console. Copy it and paste it in Airbyte.

![Datasets in app](/assets/images/datasets-app-2249b1a36efd9e35b15c68ae64f99ac7.png)

To find your Apify API token, you need to navigate to the **Settings** tab and select **API & Integrations**. Copy it and paste it in the relevant field in Airbyte.

![Integrations token](/assets/images/apify-integrations-token-a480c4034e9658f9989b7c661ee0fad5.png)

And that's it! You now have Apify datasets set up as a Source, and you can use Airbyte to transfer your datasets to one of the available destinations.

To learn more about how to setup a Connection, visit [Airbyte's documentation](https://docs.airbyte.com/using-airbyte/getting-started/set-up-a-connection)
