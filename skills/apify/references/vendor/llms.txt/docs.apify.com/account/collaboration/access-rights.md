---
title: Grant access rights
url: https://docs.apify.com/account/collaboration/access-rights.md
parents:
  - [Apify documentation](https://docs.apify.com/llms.txt)
  - [Account](https://docs.apify.com/account.md)
  - [Collaboration](https://docs.apify.com/account/collaboration.md)
previous: [General resource access](https://docs.apify.com/account/collaboration/general-resource-access.md)
next: [Organization account](https://docs.apify.com/account/collaboration/organization.md)
---

> ## Documentation index
> Fetch the complete documentation index at: https://docs.apify.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Grant access rights

You can securely share your resources with others by using a granular permissions system. Such resources include Actors, tasks, key-value stores, datasets, and request queues.

For example, you can let your colleague run an [Actor](https://docs.apify.com/actors.md) or view a [dataset](https://docs.apify.com/storage/dataset.md) but not modify it. You can also grant permission to update an Actor and build a new version. [Storages](https://docs.apify.com/storage.md) are sharable in the same way as a **read** permission or a combination of both **read** and **write** permissions.

## Grant access to resources

To share an Actor or a task:

1. In Apify Console, go to the Actor or task page.
2. From the menu, select **Share**.
3. Under **Invite**, add the user ID, email, or username of a person you want to share the resource with and select **Add user**.
4. From the dropdown, select the permissions to grant.

![Actor page in Apify Console with the menu highlighted](/assets/images/share-actor-fe45aaad4477a4504b55efd00ac040bc.svg)

To share a key-value store, request queue, or a dataset:

1. In Apify Console, in the left-side menu, go to **Storage**.
2. Use tabs to switch between datasets, key-value stores, and request queues.
3. From the table, select the resource you want to share.
4. Under **Invite**, add the user ID, email, or username of a person you want to share the resource with and select **Add user**.
5. From the dropdown, select the permissions to grant.

## Transfer an Actor between accounts

To transfer your Actor to another Apify account, [contact support](http://apify.com/contact). The transfer keeps the Actor's reviews and usage statistics.

Note that the Actor's URL in Apify Store changes, as it includes the account ID. The Actor's URL in Apify Console stays the same.
