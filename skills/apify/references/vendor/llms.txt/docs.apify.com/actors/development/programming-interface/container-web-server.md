---
title: Container web server
url: https://docs.apify.com/actors/development/programming-interface/container-web-server.md
parents:
  - [Apify documentation](https://docs.apify.com/llms.txt)
  - [Actors](https://docs.apify.com/actors.md)
  - [Development](https://docs.apify.com/actors/development.md)
  - [Programming interface](https://docs.apify.com/actors/development/programming-interface.md)
previous: [System events](https://docs.apify.com/actors/development/programming-interface/system-events.md)
next: [Metamorph](https://docs.apify.com/actors/development/programming-interface/metamorph.md)
---

> ## Documentation index
> Fetch the complete documentation index at: https://docs.apify.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Container web server

***

Each Actor run is assigned a unique URL (e.g. `kmdo7wpzlshygi.runs.apify.net`) that allows HTTP access to an optional web server running inside the Actor's Docker container. This feature enhances your Actor's capabilities by enabling external communication.

Using Actors as an API

The container web server provides a way how to connect to one specific Actor run. To enable using your Actor as an API, with a pre-defined hostname, load balancing and autoscaling, check out [Actor Standby](https://docs.apify.com/actors/development/programming-interface/standby.md).

## Access the container URL

You can find the container URL in three locations:

* In the web application, on the Actor run details page as the **Container URL** field.
* In the API as the `containerUrl` property of the [Run object](https://docs.apify.com/api/v2/actor-run-get.md).
* In the Actor run's container as the `ACTOR_WEB_SERVER_URL` environment variable.

## Set up the web server

The web server inside the container must listen on the port specified by the `ACTOR_WEB_SERVER_PORT` environment variable (typically: *4321*). To use a different port:

1. Go to your Actor version configuration

2. Define the `ACTOR_WEB_SERVER_PORT` environment variable with your desired port number.

Check out [Custom environment variables](https://docs.apify.com/actors/development/programming-interface/environment-variables.md) for more details.

## Example: Start a simple web server

**JavaScript**

Here's how to start a basic web server in your Actor using Express.js:


```js
// npm install express

import { Actor } from 'apify';

import express from 'express';



await Actor.init();



const app = express();

const port = process.env.ACTOR_WEB_SERVER_PORT;



app.get('/', (req, res) => {

    res.send('Hello world from Express app!');

});



app.listen(port, () => console.log(`Web server is listening

    and can be accessed at

    ${process.env.ACTOR_WEB_SERVER_URL}!`));



// Let the Actor run for an hour

await new Promise((r) => setTimeout(r, 60 * 60 * 1000));



await Actor.exit();
```


**Python**

Here's how to start a basic web server in your Actor using Flask:


```python
# pip install flask

import asyncio

import os

from apify import Actor

from apify_shared.consts import ActorEnvVars

from flask import Flask



async def main():

    async with Actor:

        # Create a Flask app

        app = Flask(__name__)



        # Define a route

        @app.route('/')

        def hello_world():

            return 'Hello world from Flask app!'



        # Log the public URL

        url = os.environ.get(ActorEnvVars.WEB_SERVER_URL)

        Actor.log.info(f'Web server is listening and can be accessed at {url}')



        # Start the web server

        port = os.environ.get(ActorEnvVars.WEB_SERVER_PORT)

        app.run(host='0.0.0.0', port=port)
```
