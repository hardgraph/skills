# Running a web server

Copy for LLM

In this guide, you'll learn how to run a web server inside your Apify Actor. This is useful for monitoring Actor progress, creating custom APIs, or serving content during the Actor run.

## Introduction[](#introduction)

Each Actor run on the Apify platform is assigned a unique hard-to-guess URL (for example `https://8segt5i81sokzm.runs.apify.net`), which enables HTTP access to an optional web server running inside the Actor run's container.

The URL is available in the following places:

* In [Apify Console](https://docs.apify.com/platform/console), on the Actor run details page as the **Container URL** field.
* In the API as the `containerUrl` property of the [Run object](https://docs.apify.com/api/v2/actor-run-get).
* In the Actor as the `Actor.configuration.web_server_url` property.

The web server running inside the container must listen at the port defined by the `Actor.configuration.web_server_port` property. When running Actors locally, the port defaults to `4321`, so the web server will be accessible at `http://localhost:4321`.

## Using the standard library[](#using-the-standard-library)

The following example shows how to start a simple web server in your Actor, which will respond to every GET request with the number of items that the Actor has processed so far:

[Run on](https://console.apify.com/actors/HH9rhkFXiZbheuq1V?runConfig=eyJ1IjoiRWdQdHczb2VqNlRhRHQ1cW4iLCJ2IjoxfQ.eyJpbnB1dCI6IntcImNvZGVcIjpcImltcG9ydCBhc3luY2lvXFxuZnJvbSBodHRwLnNlcnZlciBpbXBvcnQgQmFzZUhUVFBSZXF1ZXN0SGFuZGxlciwgVGhyZWFkaW5nSFRUUFNlcnZlclxcblxcbmZyb20gYXBpZnkgaW1wb3J0IEFjdG9yXFxuXFxucHJvY2Vzc2VkX2l0ZW1zID0gMFxcbmh0dHBfc2VydmVyID0gTm9uZVxcblxcblxcbmNsYXNzIFJlcXVlc3RIYW5kbGVyKEJhc2VIVFRQUmVxdWVzdEhhbmRsZXIpOlxcbiAgICBcXFwiXFxcIlxcXCJBIGhhbmRsZXIgdGhhdCBwcmludHMgdGhlIG51bWJlciBvZiBwcm9jZXNzZWQgaXRlbXMgb24gZXZlcnkgR0VUIHJlcXVlc3QuXFxcIlxcXCJcXFwiXFxuXFxuICAgIGRlZiBkb19HRVQoc2VsZikgLT4gTm9uZTpcXG4gICAgICAgIHNlbGYubG9nX3JlcXVlc3QoKVxcbiAgICAgICAgc2VsZi5zZW5kX3Jlc3BvbnNlKDIwMClcXG4gICAgICAgIHNlbGYuZW5kX2hlYWRlcnMoKVxcbiAgICAgICAgc2VsZi53ZmlsZS53cml0ZShieXRlcyhmJ1Byb2Nlc3NlZCBpdGVtczoge3Byb2Nlc3NlZF9pdGVtc30nLCBlbmNvZGluZz0ndXRmLTgnKSlcXG5cXG5cXG5kZWYgcnVuX3NlcnZlcigpIC0-IE5vbmU6XFxuICAgIFxcXCJcXFwiXFxcIlN0YXJ0IHRoZSBIVFRQIHNlcnZlciBhbmQga2VlcCBhIHJlZmVyZW5jZSB0byBpdC5cXFwiXFxcIlxcXCJcXG4gICAgZ2xvYmFsIGh0dHBfc2VydmVyXFxuICAgIHdpdGggVGhyZWFkaW5nSFRUUFNlcnZlcihcXG4gICAgICAgICgnJywgQWN0b3IuY29uZmlndXJhdGlvbi53ZWJfc2VydmVyX3BvcnQpLCBSZXF1ZXN0SGFuZGxlclxcbiAgICApIGFzIHNlcnZlcjpcXG4gICAgICAgIEFjdG9yLmxvZy5pbmZvKGYnU2VydmVyIHJ1bm5pbmcgb24ge0FjdG9yLmNvbmZpZ3VyYXRpb24ud2ViX3NlcnZlcl9wb3J0fScpXFxuICAgICAgICBodHRwX3NlcnZlciA9IHNlcnZlclxcbiAgICAgICAgc2VydmVyLnNlcnZlX2ZvcmV2ZXIoKVxcblxcblxcbmFzeW5jIGRlZiBtYWluKCkgLT4gTm9uZTpcXG4gICAgZ2xvYmFsIHByb2Nlc3NlZF9pdGVtc1xcbiAgICBhc3luYyB3aXRoIEFjdG9yOlxcbiAgICAgICAgIyBTdGFydCB0aGUgSFRUUCBzZXJ2ZXIgaW4gYSBzZXBhcmF0ZSB0aHJlYWQuXFxuICAgICAgICBydW5fc2VydmVyX3Rhc2sgPSBhc3luY2lvLmdldF9ydW5uaW5nX2xvb3AoKS5ydW5faW5fZXhlY3V0b3IoTm9uZSwgcnVuX3NlcnZlcilcXG5cXG4gICAgICAgICMgU2ltdWxhdGUgZG9pbmcgc29tZSB3b3JrLlxcbiAgICAgICAgZm9yIF8gaW4gcmFuZ2UoMTAwKTpcXG4gICAgICAgICAgICBhd2FpdCBhc3luY2lvLnNsZWVwKDEpXFxuICAgICAgICAgICAgcHJvY2Vzc2VkX2l0ZW1zICs9IDFcXG4gICAgICAgICAgICBBY3Rvci5sb2cuaW5mbyhmJ1Byb2Nlc3NlZCBpdGVtczoge3Byb2Nlc3NlZF9pdGVtc30nKVxcblxcbiAgICAgICAgaWYgaHR0cF9zZXJ2ZXIgaXMgTm9uZTpcXG4gICAgICAgICAgICByYWlzZSBSdW50aW1lRXJyb3IoJ0hUVFAgc2VydmVyIG5vdCBzdGFydGVkJylcXG5cXG4gICAgICAgICMgU2lnbmFsIHRoZSBzZXJ2ZXIgdG8gc2h1dCBkb3duIGFuZCB3YWl0LlxcbiAgICAgICAgaHR0cF9zZXJ2ZXIuc2h1dGRvd24oKVxcbiAgICAgICAgYXdhaXQgcnVuX3NlcnZlcl90YXNrXFxuXFxuXFxuaWYgX19uYW1lX18gPT0gJ19fbWFpbl9fJzpcXG4gICAgYXN5bmNpby5ydW4obWFpbigpKVxcblwifSIsIm9wdGlvbnMiOnsiYnVpbGQiOiJsYXRlc3QiLCJjb250ZW50VHlwZSI6ImFwcGxpY2F0aW9uL2pzb247IGNoYXJzZXQ9dXRmLTgiLCJtZW1vcnkiOjEwMjQsInRpbWVvdXQiOjE4MH19.a693gUp4a_VmrsUIeO1sDx338jUrQp8CP0Q3vBC6zGI\&asrc=run_on_apify)

```
import asyncio

from http.server import BaseHTTPRequestHandler, ThreadingHTTPServer



from apify import Actor



processed_items = 0

http_server = None





class RequestHandler(BaseHTTPRequestHandler):

    """A handler that prints the number of processed items on every GET request."""



    def do_GET(self) -> None:

        self.log_request()

        self.send_response(200)

        self.end_headers()

        self.wfile.write(bytes(f'Processed items: {processed_items}', encoding='utf-8'))





def run_server() -> None:

    """Start the HTTP server and keep a reference to it."""

    global http_server

    with ThreadingHTTPServer(

        ('', Actor.configuration.web_server_port), RequestHandler

    ) as server:

        Actor.log.info(f'Server running on {Actor.configuration.web_server_port}')

        http_server = server

        server.serve_forever()





async def main() -> None:

    global processed_items

    async with Actor:

        # Start the HTTP server in a separate thread.

        run_server_task = asyncio.get_running_loop().run_in_executor(None, run_server)



        # Simulate doing some work.

        for _ in range(100):

            await asyncio.sleep(1)

            processed_items += 1

            Actor.log.info(f'Processed items: {processed_items}')



        if http_server is None:

            raise RuntimeError('HTTP server not started')



        # Signal the server to shut down and wait.

        http_server.shutdown()

        await run_server_task





if __name__ == '__main__':

    asyncio.run(main())
```

## Using FastAPI[](#using-fastapi)

The example relies only on Python's standard library, which keeps it dependency-free but leaves you handling requests by hand. For anything beyond a single endpoint, a web framework such as [FastAPI](https://fastapi.tiangolo.com/) is a better fit. It gives you routing, request parsing, and automatic JSON responses, and is served by an ASGI server like [uvicorn](https://www.uvicorn.org/).

Install both, for example by adding them to your `requirements.txt`:

```
fastapi

uvicorn[standard]
```

The following Actor serves the same processed-items counter as before, but through a FastAPI endpoint. The key difference is that uvicorn runs inside the Actor's event loop as a background task, bound to `Actor.configuration.web_server_port`. The platform then routes the container URL to it:

[Run on](https://console.apify.com/actors/HH9rhkFXiZbheuq1V?runConfig=eyJ1IjoiRWdQdHczb2VqNlRhRHQ1cW4iLCJ2IjoxfQ.eyJpbnB1dCI6IntcImNvZGVcIjpcImltcG9ydCBhc3luY2lvXFxuXFxuaW1wb3J0IHV2aWNvcm5cXG5mcm9tIGZhc3RhcGkgaW1wb3J0IEZhc3RBUElcXG5cXG5mcm9tIGFwaWZ5IGltcG9ydCBBY3RvclxcblxcbiMgQ291bnRlciB0aGUgc2VydmVyIHJlcG9ydHMgYW5kIHRoZSBBY3RvciB1cGRhdGVzLlxcbnByb2Nlc3NlZF9pdGVtcyA9IDBcXG5cXG4jIEZhc3RBUEkgYXBwIHdpdGggYSBzaW5nbGUgZW5kcG9pbnQuXFxuYXBwID0gRmFzdEFQSSgpXFxuXFxuXFxuQGFwcC5nZXQoJy8nKVxcbmFzeW5jIGRlZiBpbmRleCgpIC0-IGRpY3Rbc3RyLCBpbnRdOlxcbiAgICBcXFwiXFxcIlxcXCJSZXNwb25kIHRvIGV2ZXJ5IEdFVCByZXF1ZXN0IHdpdGggdGhlIG51bWJlciBvZiBwcm9jZXNzZWQgaXRlbXMuXFxcIlxcXCJcXFwiXFxuICAgIHJldHVybiB7J3Byb2Nlc3NlZF9pdGVtcyc6IHByb2Nlc3NlZF9pdGVtc31cXG5cXG5cXG5hc3luYyBkZWYgbWFpbigpIC0-IE5vbmU6XFxuICAgIGdsb2JhbCBwcm9jZXNzZWRfaXRlbXNcXG4gICAgYXN5bmMgd2l0aCBBY3RvcjpcXG4gICAgICAgICMgU2VydmUgdGhlIGFwcCBvbiB0aGUgcGxhdGZvcm0ncyB3ZWIgc2VydmVyIHBvcnQuIEJpbmRpbmcgdG8gMC4wLjAuMFxcbiAgICAgICAgIyBtYWtlcyBpdCByZWFjaGFibGUgdGhyb3VnaCB0aGUgY29udGFpbmVyIFVSTC5cXG4gICAgICAgIGNvbmZpZyA9IHV2aWNvcm4uQ29uZmlnKFxcbiAgICAgICAgICAgIGFwcCxcXG4gICAgICAgICAgICBob3N0PScwLjAuMC4wJywgICMgbm9xYTogUzEwNFxcbiAgICAgICAgICAgIHBvcnQ9QWN0b3IuY29uZmlndXJhdGlvbi53ZWJfc2VydmVyX3BvcnQsXFxuICAgICAgICApXFxuICAgICAgICBzZXJ2ZXIgPSB1dmljb3JuLlNlcnZlcihjb25maWcpXFxuXFxuICAgICAgICAjIFJ1biB0aGUgc2VydmVyIGluIHRoZSBiYWNrZ3JvdW5kLlxcbiAgICAgICAgc2VydmVyX3Rhc2sgPSBhc3luY2lvLmNyZWF0ZV90YXNrKHNlcnZlci5zZXJ2ZSgpKVxcbiAgICAgICAgQWN0b3IubG9nLmluZm8oZidTZXJ2ZXIgcnVubmluZyBhdCB7QWN0b3IuY29uZmlndXJhdGlvbi53ZWJfc2VydmVyX3VybH0nKVxcblxcbiAgICAgICAgIyBTaW11bGF0ZSB3b3JrLCB1cGRhdGluZyB0aGUgcmVwb3J0ZWQgY291bnRlci5cXG4gICAgICAgIGZvciBfIGluIHJhbmdlKDEwMCk6XFxuICAgICAgICAgICAgYXdhaXQgYXN5bmNpby5zbGVlcCgxKVxcbiAgICAgICAgICAgIHByb2Nlc3NlZF9pdGVtcyArPSAxXFxuICAgICAgICAgICAgQWN0b3IubG9nLmluZm8oZidQcm9jZXNzZWQgaXRlbXM6IHtwcm9jZXNzZWRfaXRlbXN9JylcXG5cXG4gICAgICAgICMgU2lnbmFsIHRoZSBzZXJ2ZXIgdG8gc2h1dCBkb3duIGFuZCB3YWl0LlxcbiAgICAgICAgc2VydmVyLnNob3VsZF9leGl0ID0gVHJ1ZVxcbiAgICAgICAgYXdhaXQgc2VydmVyX3Rhc2tcXG5cXG5cXG5pZiBfX25hbWVfXyA9PSAnX19tYWluX18nOlxcbiAgICBhc3luY2lvLnJ1bihtYWluKCkpXFxuXCJ9Iiwib3B0aW9ucyI6eyJidWlsZCI6ImxhdGVzdCIsImNvbnRlbnRUeXBlIjoiYXBwbGljYXRpb24vanNvbjsgY2hhcnNldD11dGYtOCIsIm1lbW9yeSI6MTAyNCwidGltZW91dCI6MTgwfX0.iRX2E2Dmq1DV-dOFRAOUVMxu4q-k9hj7jBXldKBvfSc\&asrc=run_on_apify)

```
import asyncio



import uvicorn

from fastapi import FastAPI



from apify import Actor



# Counter the server reports and the Actor updates.

processed_items = 0



# FastAPI app with a single endpoint.

app = FastAPI()





@app.get('/')

async def index() -> dict[str, int]:

    """Respond to every GET request with the number of processed items."""

    return {'processed_items': processed_items}





async def main() -> None:

    global processed_items

    async with Actor:

        # Serve the app on the platform's web server port. Binding to 0.0.0.0

        # makes it reachable through the container URL.

        config = uvicorn.Config(

            app,

            host='0.0.0.0',  # noqa: S104

            port=Actor.configuration.web_server_port,

        )

        server = uvicorn.Server(config)



        # Run the server in the background.

        server_task = asyncio.create_task(server.serve())

        Actor.log.info(f'Server running at {Actor.configuration.web_server_url}')



        # Simulate work, updating the reported counter.

        for _ in range(100):

            await asyncio.sleep(1)

            processed_items += 1

            Actor.log.info(f'Processed items: {processed_items}')



        # Signal the server to shut down and wait.

        server.should_exit = True

        await server_task





if __name__ == '__main__':

    asyncio.run(main())
```

Note that:

* `uvicorn.Server(...).serve()` is a coroutine. It runs as an `asyncio` task alongside the Actor's own work instead of blocking it. Setting `server.should_exit = True` triggers a graceful shutdown once the work is done.
* The server binds to `0.0.0.0` (all interfaces) rather than `localhost`. This makes it reachable through the container URL, not only from inside the container.
* The same pattern powers an [Actor Standby](#exposing-it-over-standby) service. Swap the one-off work loop for an Actor that keeps serving requests.

## Exposing it over Standby[](#exposing-it-over-standby)

The example runs a web server for the duration of a single Actor run. With [Actor Standby](https://docs.apify.com/platform/actors/development/programming-interface/standby), you can instead expose your Actor as an always-ready HTTP API: the platform keeps the Actor running in the background and routes incoming HTTP requests to the web server inside it, spinning up additional instances as the load grows.

From the SDK's perspective, a Standby Actor is built the same way as the web server above. You start an HTTP server listening on the port from `Actor.configuration.web_server_port`. The difference is operational: instead of doing its work once and exiting, a Standby Actor stays up and serves requests. This makes it a good fit for low-latency, on-demand use cases, such as serving scraped data or acting as a microservice.

To enable Standby, set `usesStandbyMode` in the Actor's `.actor/actor.json`:

```
{

    "actorSpecification": 1,

    "name": "my-standby-server",

    "usesStandbyMode": true

}
```

Deploy the Actor with `apify push`. Once it's running, a client reaches the web server at the Actor's Standby URL, passing an [Apify API token](https://console.apify.com/account/integrations) as a bearer token:

```
curl "https://me--my-standby-server.apify.actor" \

    -H "Authorization: Bearer <YOUR_APIFY_API_TOKEN>"
```

To get started, use the [Standby Python template](https://apify.com/templates/python-standby). For details on enabling Standby, request routing, and readiness probes, see the [Actor Standby documentation](https://docs.apify.com/platform/actors/development/programming-interface/standby).

## Conclusion[](#conclusion)

In this guide, you learned how to run a web server inside your Apify Actor. By leveraging the container URL and port provided by the platform, you can expose HTTP endpoints for monitoring, reporting, or serving content during Actor execution. If you have questions or need assistance, feel free to reach out on our [GitHub](https://github.com/apify/apify-sdk-python) or join our [Discord community](https://discord.com/invite/jyEM2PRvMU).

## Additional resources[](#additional-resources)

* [Apify templates: Standby Python project](https://apify.com/templates/python-standby)
