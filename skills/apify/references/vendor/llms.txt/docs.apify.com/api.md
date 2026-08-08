---
title: Apify API documentation
url: https://docs.apify.com/api.md
parents:
  - [Apify documentation](https://docs.apify.com/llms.txt)
---

> ## Documentation index
> Fetch the complete documentation index at: https://docs.apify.com/llms.txt
> Use this file to discover all available pages before exploring further.

Skip to main content

https://docs.apify.com

https://discord.com/invite/jyEM2PRvMU[Get started](https://console.apify.com)

[Get started](https://docs.apify.com/get-started)[Actors](https://docs.apify.com/actors)[Storage](https://docs.apify.com/storage)[Proxy](https://docs.apify.com/proxy)[Account](https://docs.apify.com/account)[Integrations](https://docs.apify.com/integrations)[Security](https://docs.apify.com/security)

[Academy](https://docs.apify.com/academy)

[API](https://docs.apify.com/api)

* [API reference](https://docs.apify.com/api/v2)
* [Client for JavaScript](https://docs.apify.com/api/client/js/docs)
* [Client for Python](https://docs.apify.com/api/client/python/docs)
* [Client for Go](https://github.com/apify/apify-client-go)
* [Client for PHP](https://github.com/apify/apify-client-php)
* [Client for Java](https://github.com/apify/apify-client-java)
* [Client for .NET](https://github.com/apify/apify-client-dotnet)
* [Client for Rust](https://github.com/apify/apify-client-rust)

[SDK](https://docs.apify.com/sdk)

* [SDK for JavaScript](https://docs.apify.com/sdk/js/docs/overview)
* [SDK for Python](https://docs.apify.com/sdk/python/docs/overview)

[CLI](https://docs.apify.com/cli)

* [Installation](https://docs.apify.com/cli/docs/installation)
* [Quick start](https://docs.apify.com/cli/docs/quick-start)
* [Command reference](https://docs.apify.com/cli/docs/reference)

[MCP](https://docs.apify.com/integrations/mcp)

# Apify API documentation

Learn how to use the [Apify platform](https://docs.apify.com/) programmatically.

## REST API

The Apify API is built around HTTP REST, uses predictable resource-oriented URLs, returns JSON-encoded responses, and uses standard HTTP response codes, authentication, and verbs.

[View API reference](https://docs.apify.com/api/v2.md)

cURL


```bash
# Prepare Actor input and run it synchronously

echo '{ "searchStringsArray": ["Apify"] }' |

curl -X POST -d @- \

  -H 'Content-Type: application/json' \

  -H 'Authorization: Bearer <YOUR_API_TOKEN>' \

  -L 'https://api.apify.com/v2/actors/compass~crawler-google-places/run-sync-get-dataset-items'
```


## OpenAPI schema

You can download the complete OpenAPI schema of the Apify API in the

<!-- -->

[YAML](https://docs.apify.com/api/openapi.yaml) or

<!-- -->

[JSON](https://docs.apify.com/api/openapi.json) formats. The source code is also

<!-- -->

[available on GitHub](https://github.com/apify/apify-docs/tree/master/apify-api/openapi).

## API clients

The client libraries are a more convenient way to interact with the Apify platform than the HTTP REST API.

##### ![](/img/javascript-40x40.svg)![](/img/javascript-40x40.svg)JavaScript

##### ![](/img/python-40x40.svg)![](/img/python-40x40.svg)Python

### JavaScript API client

For web browser, JavaScript/TypeScript applications, Node.js, Deno, or Bun.[Star](https://github.com/apify/apify-client-js)

[Get started](https://docs.apify.com/api/client/js/docs)[View reference](https://docs.apify.com/api/client/js/reference)


```bash
npm install apify-client
```



```javascript
// Easily run Actors, await them to finish using the convenient .call() method, and retrieve results from the resulting dataset.

const { ApifyClient } = require('apify-client');



const client = new ApifyClient({

    token: 'MY-APIFY-TOKEN',

});



// Starts an actor and waits for it to finish.

const { defaultDatasetId } = await client.actor('john-doe/my-cool-actor').call();



// Fetches results from the actor's dataset.

const { items } = await client.dataset(defaultDatasetId).listItems();
```


## Experimental API clients

Clients for other languages are official Apify projects, but they are experimental: they are generated automatically from the OpenAPI schema. Review the code before you rely on them in production, and report issues on their repositories.

##### Go

##### PHP

##### Java

##### .NET

##### Rust

### Go API client

Client for Go 1.23 or newer, built almost entirely on the standard library.

[Get started](https://github.com/apify/apify-client-go)[View reference](https://github.com/apify/apify-client-go/blob/master/docs/README.md)


```bash
go get github.com/apify/apify-client-go
```



```go
package main



import (

    "context"

    "fmt"

    "log"



    apify "github.com/apify/apify-client-go"

)



func main() {

    client := apify.NewClient(apify.WithToken("MY-APIFY-TOKEN"))

    ctx := context.Background()



    // Starts an Actor and waits for it to finish.

    run, err := client.Actor("john-doe/my-cool-actor").Call(ctx, nil, apify.ActorStartOptions{}, nil)

    if err != nil {

        log.Fatal(err)

    }



    // Fetches results from the Actor's dataset.

    page, err := client.Dataset(run.DefaultDatasetID).ListItems(ctx, apify.DatasetListItemsOptions{})

    if err != nil {

        log.Fatal(err)

    }

    fmt.Printf("Got %d items\n", page.Total)

}
```


## Related articles

https://blog.apify.com/web-scraping-with-client-side-vanilla-javascript/

[Web scraping with client-side Vanilla JavaScript](https://blog.apify.com/web-scraping-with-client-side-vanilla-javascript/)

[Read more](https://blog.apify.com/web-scraping-with-client-side-vanilla-javascript/)

https://blog.apify.com/apify-python-api-client/

[Apify ❤️ Python, so we’re releasing a Python API client](https://blog.apify.com/apify-python-api-client/)

[Read more](https://blog.apify.com/apify-python-api-client/)

https://blog.apify.com/api-for-dummies/

[API for dummies](https://blog.apify.com/api-for-dummies/)

[Read more](https://blog.apify.com/api-for-dummies/)

Reference

* [API Reference](https://docs.apify.com/api/v2)
* [SDK for JavaScript](https://docs.apify.com/sdk/js/docs/overview)
* [SDK for Python](https://docs.apify.com/sdk/python/docs/overview)
* [Client for JavaScript](https://docs.apify.com/api/client/js/docs)
* [Client for Python](https://docs.apify.com/api/client/python/docs)
* [CLI](https://docs.apify.com/cli/docs)

Open source

* [Crawlee](https://crawlee.dev)
* [Fingerprint Suite](https://github.com/apify/fingerprint-suite)
* [impit](https://github.com/apify/impit)
* [MCP CLI](https://github.com/apify/mcpc)
* [Actor whitepaper](https://whitepaper.actor)
* [proxy-chain](https://github.com/apify/proxy-chain)

Security

* [Trust Center](https://trust.apify.com)

Community

* [Discord](https://discord.com/invite/jyEM2PRvMU)
* [X](https://x.com/apify)
* [YouTube](https://www.youtube.com/c/Apify)
* [GitHub](https://github.com/apify)

For AI

* [llms.txt](https://docs.apify.com/llms.txt)
* [llms-full.txt](https://docs.apify.com/llms-full.txt)
* [OpenAPI (JSON)](https://docs.apify.com/api/openapi.json)
* [OpenAPI (YAML)](https://docs.apify.com/api/openapi.yaml)

https://apify.com
