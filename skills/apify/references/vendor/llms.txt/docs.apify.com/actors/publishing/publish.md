---
title: Publish your Actor
url: https://docs.apify.com/actors/publishing/publish.md
parents:
  - [Apify documentation](https://docs.apify.com/llms.txt)
  - [Actors](https://docs.apify.com/actors.md)
  - [Publishing and monetization](https://docs.apify.com/actors/publishing.md)
children:
  - [Create an Actor README](https://docs.apify.com/actors/publishing/actor-readme.md)
previous: [Publishing and monetization](https://docs.apify.com/actors/publishing.md)
next: [Create an Actor README](https://docs.apify.com/actors/publishing/actor-readme.md)
---

> ## Documentation index
> Fetch the complete documentation index at: https://docs.apify.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Publish your Actor

By publishing your Actor, you make it available to the public on [Apify Store](https://apify.com/store). Publishing turns your Actor into a product with its own dedicated page, where users can read its documentation, run it, and review it.

## Before you start

Before you publish your Actor, update its README file. The README becomes your Actor's public detail page on Apify Store. It's where users can learn about your Actor and how to use it.

For details, see [Create an Actor README](https://docs.apify.com/actors/publishing/actor-readme.md).

## Make your Actor public

To make your Actor available on Apify Store:

1. Log in to [Apify Console](https://console.apify.com).
2. In the left-side panel, go to **Development** > **My Actors**.
3. From the table, select the Actor you want to publish.
4. Go to the **Publication** tab.

Complete all required fields in the following sections:

1. In the **Display information** section, add an Actor logo and [description](https://docs.apify.com/academy/actor-marketing-playbook/actor-basics/actor-description.md).

2. In the **Monetization** section, [set up monetization](https://docs.apify.com/actors/publishing/monetize.md) for your Actor.

3. In the **Sample output** section, add a sample output for your Actor. If the [input schema](https://docs.apify.com/actors/development/actor-definition/input-schema.md) already includes the sample output, it's added automatically.

4. In the **Output schema** section, define the [output schema](https://docs.apify.com/actors/development/actor-definition/output-schema.md) of your Actor. Optionally, define also the following schemas:

   <!-- -->

   * [Dataset schema](https://docs.apify.com/storage/dataset-schema.md)
   * [Key-value store schema](https://docs.apify.com/storage/key-value-store-schema.md)
   * [Live-view web server OpenAPI schema](https://docs.apify.com/actors/development/actor-definition/web-server-schema.md)

5. In **Actor permissions**, define the [permission level](https://docs.apify.com/actors/development/permissions.md) that your Actor requires.

Once all sections are marked as completed, select **Publish on Store**.

## Verify the publication

To verify that your Actor has been published:

1. Go to the [Apify Store](https://apify.com/store).
2. Search for your Actor's name.
3. Select your Actor's card and review its dedicated page.

## Publish Actor's source code

By default, the source code of your Actor is hidden from the public.

By making the source files and non-secret [environment variables](https://docs.apify.com/actors/development/programming-interface/environment-variables.md) of your Actor publicly visible, you make your Actor open source and eligible for payouts through the [Apify Open Source Fair Share program](https://apify.com/partners/open-source-fair-share).

To publish your Actor's source code:

1. Log in to [Apify Console](https://console.apify.com).
2. In the left-side panel, go to **Development** > **My Actors**.
3. From the table, select the Actor whose source code you want to publish.
4. Go to the **Publication** tab.
5. In **Display information**, uncheck **Hide source files from Actor detail**.

You can't publish the source code of an Actor that you build from a private repository. Secret environment variables are never exposed.
