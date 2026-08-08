---
title: Google Drive integration
url: https://docs.apify.com/integrations/drive.md
parents:
  - [Apify documentation](https://docs.apify.com/llms.txt)
  - [Integrations](https://docs.apify.com/integrations.md)
  - [Data and storage](https://docs.apify.com/integrations/data-and-storage.md)
previous: [Airtable](https://docs.apify.com/integrations/airtable.md)
next: [HubSpot](https://docs.apify.com/integrations/hubspot.md)
---

> ## Documentation index
> Fetch the complete documentation index at: https://docs.apify.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Google Drive integration

Save Apify Actor run results directly to Google Drive. Set up the integration on an Actor or saved task to automatically upload files after each successful run.

Help keep this page up to date

This integration uses a third-party service. If you find outdated content, please [submit an issue on GitHub](https://github.com/apify/apify-docs/issues).

## Get started

To use the Apify integration for Google Drive, you will need:

* An [Apify account](https://console.apify.com/).
* A Google account

## Set up Google Drive integration

1. Head over to the **Integrations** tab of your Actor or saved task and click on the **Upload results to GDrive** integration.

   ![Google Drive integration](/assets/images/google-integrations-add-a4a8cc6b90c1593b629ecf9c5d750b18.png)

2. Click on **Connect with Google** button and select the account with which you want to use the integration.

   ![Google Drive integration](/assets/images/google-integrations-connect-drive-836e2e2e4618baefb146659112e6bb4a.png)

3. Set up the integration details. You can choose the **Filename** and **Format** , which can make use of available variables. The file will be uploaded to your Google Drive account to `Apify Uploads` folder. By default, the integration is triggered by successful runs only.

   ![Google Drive integration](/assets/images/google-integrations-details-drive-a5ac7880e4d742e2cefe11efaa3e247f.png)

4. Click on **Save** & enable the integration.

Once this is done, run your Actor to test whether the integration is working.

You can manage your connected accounts at **[Settings > API & Integrations](https://console.apify.com/settings/integrations)**.

![Google Drive integration](/assets/images/google-integrations-accounts-95c33e6e7c658a29a5b87f4a4c65a653.png)
