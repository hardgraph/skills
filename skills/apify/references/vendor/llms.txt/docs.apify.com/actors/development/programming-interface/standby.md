---
title: Standby mode
url: https://docs.apify.com/actors/development/programming-interface/standby.md
parents:
  - [Apify documentation](https://docs.apify.com/llms.txt)
  - [Actors](https://docs.apify.com/actors.md)
  - [Development](https://docs.apify.com/actors/development.md)
  - [Programming interface](https://docs.apify.com/actors/development/programming-interface.md)
previous: [Metamorph](https://docs.apify.com/actors/development/programming-interface/metamorph.md)
next: [Deployment](https://docs.apify.com/actors/development/deployment.md)
---

> ## Documentation index
> Fetch the complete documentation index at: https://docs.apify.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Standby mode

Traditional Actors are designed to run a single task and then stop. They're mostly intended for batch jobs, such as when you need to perform a large scrape or data processing task. However, in some applications, waiting for an Actor to start is not an option. Actor Standby mode solves this problem by letting you have the Actor ready in the background, waiting for the incoming HTTP requests. In a sense, the Actor behaves like a real-time web server or standard API server.

## Developing Actors using Standby mode

<!-- -->

The best way to start developing Standby Actors is to use the predefined templates in the [Console UI](https://console.apify.com/actors/templates) or in [CLI](https://docs.apify.com/cli/) via `apify create`. The templates contain minimal code to get you up to speed for development in JavaScript, TypeScript or Python. Standby mode will automatically be enabled with default settings.

If you already have an existing Actor, or you just want to tweak the configuration of Standby mode, you can head to the Settings tab of your Actor, where the Actor Standby settings are located. ![Standby for creators](/assets/images/standby-creators-a4633d8d11b7d7b016ddb197cd838b53.png)

Actors using Standby mode must run a HTTP server listening on a specific port. The user requests will then be proxied to the HTTP server. You can use any of the existing [HTTP request methods](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods) like GET, POST, PUT, DELETE, etc. You can pass the input via [HTTP request query string](https://en.wikipedia.org/wiki/Query_string) or via [HTTP request body](https://developer.mozilla.org/en-US/docs/Web/HTTP/Messages#body).

Sometimes, you want the HTTP server to listen on a specific port and cannot change it yourself. You can use `ACTOR_WEB_SERVER_PORT` environment variable to override the port so that Actor Standby will work with your code.

You can get the port using the Actor configuration available in Apify SDK. See example below with a simple Actor using Standby mode.

**JavaScript**


```js
import http from 'http';

import { Actor } from 'apify';



await Actor.init();



const server = http.createServer((req, res) => {

    res.writeHead(200, { 'Content-Type': 'text/plain' });

    res.end('Hello from Actor Standby!\n');

});



server.listen(Actor.config.get('containerPort'));
```


**Python**


```python
from http.server import HTTPServer, SimpleHTTPRequestHandler

from apify import Actor



class GetHandler(SimpleHTTPRequestHandler):

    def do_GET(self):

        self.send_response(200)

        self.end_headers()

        self.wfile.write(b'Hello from Actor Standby!')



async def main() -> None:

    async with Actor:

        with HTTPServer(('', Actor.configuration.web_server_port), GetHandler) as http_server:

            http_server.serve_forever()
```


Please make sure to describe your Actors, their endpoints, and the schema for their inputs and outputs in your README.

### Readiness probe

Before Actor standby runs are ready to serve requests, the Apify platform checks the web server's readiness using a readiness probe. The platform sends a GET request to the path `/` with a header `x-apify-container-server-readiness-probe`. If the header is present in the request, you can perform an early return with a simple response to prevent wasting resources.

Return a response

You must return a response; otherwise, the Actor run will never be marked as ready and won't process requests.

See example code below that distinguishes between "normal" and "readiness probe" requests.

**JavaScript**


```js
import http from 'http';

import { Actor } from 'apify';



await Actor.init();



const server = http.createServer((req, res) => {

    res.writeHead(200, { 'Content-Type': 'text/plain' });

    if (req.headers['x-apify-container-server-readiness-probe']) {

        console.log('Readiness probe');

        res.end('Hello, readiness probe!\n');

    } else {

        console.log('Normal request');

        res.end('Hello from Actor Standby!\n');

    }

});



server.listen(Actor.config.get('standbyPort'));
```


**Python**


```python
from http.server import HTTPServer, SimpleHTTPRequestHandler

from apify import Actor



class GetHandler(SimpleHTTPRequestHandler):

    def do_GET(self) -> None:

        self.send_response(200)

        self.end_headers()

        if self.headers['x-apify-container-server-readiness-probe']:

            print('Readiness probe')

            self.wfile.write(b'Hello, readiness probe!')

        else:

            print('Normal request')

            self.wfile.write(b'Hello, normal request!')



async def main() -> None:

    async with Actor:

        with HTTPServer(('', Actor.configuration.standby_port), GetHandler) as http_server:

            http_server.serve_forever()
```


## Determining an Actor is started in Standby

Actors that support Actor Standby can still be started in standard mode, for example from the Console or via the API. To find out in which mode was the Actor started, you can read the `metaOrigin` option in `Actor.config`, or the `APIFY_META_ORIGIN` environment variable in case you're not using the Apify SDK. If it is equal to `STANDBY`, the Actor was started in Standby mode, otherwise it was started in standard mode.

**JavaScript**


```js
import { Actor } from 'apify';



await Actor.init();



if (Actor.config.get('metaOrigin') === 'STANDBY') {

    // Start your Standby server here

} else {

    // Perform the standard Actor operations here

}
```


**Python**


```python
from apify import Actor



async def main() -> None:

    async with Actor:

        if Actor.configuration.meta_origin == 'STANDBY':

            # Start your Standby server here

        else:

            # Perform the standard Actor operations here
```


## Timeouts

When you send a request to an Actor in Standby mode, the total timeout for receiving the first response is *5 minutes*. Before the platform forwards the request to a specific Actor run, it performs a *run selection* process to determine the specific Actor run that will handle it. This process has internal timeout of *2 minutes*.

## Getting the URL of the Standby Actor

The URL is exposed as an environment variable `ACTOR_STANDBY_URL`. You can also use `Actor.config`, where the `standbyUrl` option is available.

## Monetization of Actors in Standby mode

You can monetize Standby Actors just like any other Actor.

For best results with Standby workflows, use the [pay-per-event pricing model](https://docs.apify.com/actors/publishing/monetize/pay-per-event.md). In this model, users cover both the platform usage costs of their runs, as well as the event costs.
