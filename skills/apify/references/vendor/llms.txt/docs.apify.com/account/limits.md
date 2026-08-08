---
title: Limits
url: https://docs.apify.com/account/limits.md
parents:
  - [Apify documentation](https://docs.apify.com/llms.txt)
  - [Account](https://docs.apify.com/account.md)
previous: [Billing](https://docs.apify.com/account/billing.md)
next: [Account settings](https://docs.apify.com/account/settings.md)
---

> ## Documentation index
> Fetch the complete documentation index at: https://docs.apify.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Limits

The tables below demonstrate the Apify platform's default resource limits. For API limits such as rate limits and max payload size, see the [API documentation](https://docs.apify.com/api/v2.md#rate-limiting).

## Actor runtime limits

| Description                                 | Limit for plan        |           |            |            |
| ------------------------------------------- | --------------------- | --------- | ---------- | ---------- |
|                                             | Free                  | Starter   | Scale      | Business   |
| Build memory size                           | 4,096 MB              |           |            |            |
| Run minimum memory                          | 128 MB                | 128 MB    |            |            |
| Run maximum memory                          | 16,384 MB             | 32,768 MB |            |            |
| Maximum combined memory of all running jobs | 16,384 MB             | 65,536 MB | 262,144 MB | 524,288 MB |
| Build timeout                               | 1800 secs             |           |            |            |
| Build/run disk size                         | 2× job memory limit   |           |            |            |
| Memory per CPU core                         | 4,096 MB              |           |            |            |
| Maximum log size                            | 10,485,760 characters |           |            |            |
| Maximum number of metamorphs                | 10 metamorphs per run |           |            |            |

## Apify platform limits

| Description                                                            | Limit for plan |         |       |          |
| ---------------------------------------------------------------------- | -------------- | ------- | ----- | -------- |
|                                                                        | Free           | Starter | Scale | Business |
| Maximum number of dataset columns for tabular formats (XLSX, CSV, ...) | 2000 columns   |         |       |          |
| Maximum size of Actor input schema                                     | 500 kB         |         |       |          |
| Maximum number of Actors per user                                      | 500            |         |       |          |
| Maximum number of tasks per user                                       | 5000           |         |       |          |
| Maximum number of schedules per user                                   | 100            |         |       |          |
| Maximum number of webhooks per user                                    | 100            |         |       |          |
| Maximum number of Actors per schedule                                  | 10             |         |       |          |
| Maximum number of tasks per schedule                                   | 10             |         |       |          |
| Maximum number of concurrent Actor runs per user                       | 25             | 32      | 128   | 256      |

## Usage limit

To protect you from accidental overspending, the Apify platform introduces usage limits based on your billing plan. For details, see [Limits](https://docs.apify.com/account/billing.md#limits).

## Increase limits

On paid accounts, there's an option to increase the Actor runtime limits and Apify platform limits. For details, [contact support](http://apify.com/contact).
