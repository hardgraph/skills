# Actor lifecycle

Copy for LLM

This guide explains how an **Apify Actor** starts, runs, and shuts down, describing the complete Actor lifecycle. For information about the core concepts such as Actors, the [Apify Console](https://docs.apify.com/platform/console), storages, and events, check out the [Apify platform documentation](https://docs.apify.com/platform).

## Actor initialization[](#actor-initialization)

During initialization, the SDK prepares all the components required to integrate with the Apify platform. It loads configuration from environment variables, initializes access to platform storages such as the [key-value store, dataset, and request queue](https://docs.apify.com/platform/storage), sets up event handling for [platform events](https://docs.apify.com/platform/integrations/webhooks/events), and configures logging.

The recommended approach in Python is to use the global [`Actor`](https://docs.apify.com/sdk/python/sdk/python/reference/class/Actor.md) class as an asynchronous context manager. This approach automatically manages setup and teardown and keeps your code concise. When entering the context, the SDK loads configuration and initializes clients lazily—for example, a dataset is opened only when it is first accessed. If the Actor runs on the Apify platform, it also begins listening for platform events.

When the Actor exits, either normally or due to an exception, the SDK performs a graceful shutdown. It persists the final Actor state, stops event handling, and sets the terminal exit code together with the [status message](https://docs.apify.com/platform/actors/development/programming-interface/status-messages).

* Actor class with context manager
* Actor class with manual init/exit

[Run on](https://console.apify.com/actors/HH9rhkFXiZbheuq1V?runConfig=eyJ1IjoiRWdQdHczb2VqNlRhRHQ1cW4iLCJ2IjoxfQ.eyJpbnB1dCI6IntcImNvZGVcIjpcImltcG9ydCBhc3luY2lvXFxuXFxuZnJvbSBhcGlmeSBpbXBvcnQgQWN0b3JcXG5cXG5cXG5hc3luYyBkZWYgbWFpbigpIC0-IE5vbmU6XFxuICAgIGFzeW5jIHdpdGggQWN0b3I6XFxuICAgICAgICAjIEdldCBpbnB1dFxcbiAgICAgICAgYWN0b3JfaW5wdXQgPSBhd2FpdCBBY3Rvci5nZXRfaW5wdXQoKVxcbiAgICAgICAgQWN0b3IubG9nLmluZm8oJ0FjdG9yIGlucHV0OiAlcycsIGFjdG9yX2lucHV0KVxcblxcbiAgICAgICAgIyBZb3VyIEFjdG9yIGxvZ2ljIGhlcmVcXG4gICAgICAgIGRhdGEgPSB7J21lc3NhZ2UnOiAnSGVsbG8gZnJvbSBBY3RvciEnLCAnaW5wdXQnOiBhY3Rvcl9pbnB1dH1cXG4gICAgICAgIGF3YWl0IEFjdG9yLnB1c2hfZGF0YShkYXRhKVxcblxcbiAgICAgICAgIyBTZXQgc3RhdHVzIG1lc3NhZ2VcXG4gICAgICAgIGF3YWl0IEFjdG9yLnNldF9zdGF0dXNfbWVzc2FnZSgnQWN0b3IgY29tcGxldGVkIHN1Y2Nlc3NmdWxseScpXFxuXFxuXFxuaWYgX19uYW1lX18gPT0gJ19fbWFpbl9fJzpcXG4gICAgYXN5bmNpby5ydW4obWFpbigpKVxcblwifSIsIm9wdGlvbnMiOnsiYnVpbGQiOiJsYXRlc3QiLCJjb250ZW50VHlwZSI6ImFwcGxpY2F0aW9uL2pzb247IGNoYXJzZXQ9dXRmLTgiLCJtZW1vcnkiOjEwMjQsInRpbWVvdXQiOjE4MH19.dfnZ3EM-6xRFUTvMWT1esehu2-lA2PZJmoLBc7BBtgQ\&asrc=run_on_apify)

```
import asyncio



from apify import Actor





async def main() -> None:

    async with Actor:

        # Get input

        actor_input = await Actor.get_input()

        Actor.log.info('Actor input: %s', actor_input)



        # Your Actor logic here

        data = {'message': 'Hello from Actor!', 'input': actor_input}

        await Actor.push_data(data)



        # Set status message

        await Actor.set_status_message('Actor completed successfully')





if __name__ == '__main__':

    asyncio.run(main())
```

[Run on](https://console.apify.com/actors/HH9rhkFXiZbheuq1V?runConfig=eyJ1IjoiRWdQdHczb2VqNlRhRHQ1cW4iLCJ2IjoxfQ.eyJpbnB1dCI6IntcImNvZGVcIjpcImltcG9ydCBhc3luY2lvXFxuXFxuZnJvbSBhcGlmeSBpbXBvcnQgQWN0b3JcXG5cXG5cXG5hc3luYyBkZWYgbWFpbigpIC0-IE5vbmU6XFxuICAgIGF3YWl0IEFjdG9yLmluaXQoKVxcblxcbiAgICB0cnk6XFxuICAgICAgICAjIEdldCBpbnB1dFxcbiAgICAgICAgYWN0b3JfaW5wdXQgPSBhd2FpdCBBY3Rvci5nZXRfaW5wdXQoKVxcbiAgICAgICAgQWN0b3IubG9nLmluZm8oJ0FjdG9yIGlucHV0OiAlcycsIGFjdG9yX2lucHV0KVxcblxcbiAgICAgICAgIyBZb3VyIEFjdG9yIGxvZ2ljIGhlcmVcXG4gICAgICAgIGRhdGEgPSB7J21lc3NhZ2UnOiAnSGVsbG8gZnJvbSBBY3RvciEnLCAnaW5wdXQnOiBhY3Rvcl9pbnB1dH1cXG4gICAgICAgIGF3YWl0IEFjdG9yLnB1c2hfZGF0YShkYXRhKVxcblxcbiAgICAgICAgIyBTZXQgc3RhdHVzIG1lc3NhZ2VcXG4gICAgICAgIGF3YWl0IEFjdG9yLnNldF9zdGF0dXNfbWVzc2FnZSgnQWN0b3IgY29tcGxldGVkIHN1Y2Nlc3NmdWxseScpXFxuXFxuICAgIGZpbmFsbHk6XFxuICAgICAgICBhd2FpdCBBY3Rvci5leGl0KClcXG5cXG5cXG5pZiBfX25hbWVfXyA9PSAnX19tYWluX18nOlxcbiAgICBhc3luY2lvLnJ1bihtYWluKCkpXFxuXCJ9Iiwib3B0aW9ucyI6eyJidWlsZCI6ImxhdGVzdCIsImNvbnRlbnRUeXBlIjoiYXBwbGljYXRpb24vanNvbjsgY2hhcnNldD11dGYtOCIsIm1lbW9yeSI6MTAyNCwidGltZW91dCI6MTgwfX0.fJ1NUD4sv032YWP2SK67VLTacs5vHpqMgqr5Gr3-c3o\&asrc=run_on_apify)

```
import asyncio



from apify import Actor





async def main() -> None:

    await Actor.init()



    try:

        # Get input

        actor_input = await Actor.get_input()

        Actor.log.info('Actor input: %s', actor_input)



        # Your Actor logic here

        data = {'message': 'Hello from Actor!', 'input': actor_input}

        await Actor.push_data(data)



        # Set status message

        await Actor.set_status_message('Actor completed successfully')



    finally:

        await Actor.exit()





if __name__ == '__main__':

    asyncio.run(main())
```

You can also create an [`Actor`](https://docs.apify.com/sdk/python/sdk/python/reference/class/Actor.md) instance directly. This does not change its capabilities but allows you to specify optional parameters during initialization. The key parameters are:

* `configuration` — a custom [`Configuration`](https://docs.apify.com/sdk/python/sdk/python/reference/class/Configuration.md) instance to control storage paths, API URLs, and other settings.
* `configure_logging` — whether to set up default logging configuration (default `True`). Set to `False` if you configure logging yourself.
* `exit_process` — whether the Actor calls `sys.exit()` when the context manager exits. Defaults to `True`, except in IPython and Scrapy environments.
* `event_listeners_timeout` — maximum time to wait for Actor event listeners to complete before exiting.
* `cleanup_timeout` — maximum time to wait for cleanup tasks to finish (default 30 seconds).

The choice between using a context manager or manual initialization depends on how much control you require over the Actor's startup and shutdown sequence.

* Actor instance with context manager
* Actor instance with manual init/exit

[Run on](https://console.apify.com/actors/HH9rhkFXiZbheuq1V?runConfig=eyJ1IjoiRWdQdHczb2VqNlRhRHQ1cW4iLCJ2IjoxfQ.eyJpbnB1dCI6IntcImNvZGVcIjpcImltcG9ydCBhc3luY2lvXFxuZnJvbSBkYXRldGltZSBpbXBvcnQgdGltZWRlbHRhXFxuXFxuZnJvbSBhcGlmeSBpbXBvcnQgQWN0b3JcXG5cXG5cXG5hc3luYyBkZWYgbWFpbigpIC0-IE5vbmU6XFxuICAgIGFjdG9yID0gQWN0b3IoXFxuICAgICAgICBldmVudF9saXN0ZW5lcnNfdGltZW91dD10aW1lZGVsdGEoc2Vjb25kcz0zMCksXFxuICAgICAgICBjbGVhbnVwX3RpbWVvdXQ9dGltZWRlbHRhKHNlY29uZHM9MzApLFxcbiAgICApXFxuXFxuICAgIGFzeW5jIHdpdGggYWN0b3I6XFxuICAgICAgICAjIEdldCBpbnB1dFxcbiAgICAgICAgYWN0b3JfaW5wdXQgPSBhd2FpdCBhY3Rvci5nZXRfaW5wdXQoKVxcbiAgICAgICAgYWN0b3IubG9nLmluZm8oJ0FjdG9yIGlucHV0OiAlcycsIGFjdG9yX2lucHV0KVxcblxcbiAgICAgICAgIyBZb3VyIEFjdG9yIGxvZ2ljIGhlcmVcXG4gICAgICAgIGRhdGEgPSB7J21lc3NhZ2UnOiAnSGVsbG8gZnJvbSBBY3RvciBpbnN0YW5jZSEnLCAnaW5wdXQnOiBhY3Rvcl9pbnB1dH1cXG4gICAgICAgIGF3YWl0IGFjdG9yLnB1c2hfZGF0YShkYXRhKVxcblxcbiAgICAgICAgIyBTZXQgc3RhdHVzIG1lc3NhZ2VcXG4gICAgICAgIGF3YWl0IGFjdG9yLnNldF9zdGF0dXNfbWVzc2FnZSgnQWN0b3IgY29tcGxldGVkIHN1Y2Nlc3NmdWxseScpXFxuXFxuXFxuaWYgX19uYW1lX18gPT0gJ19fbWFpbl9fJzpcXG4gICAgYXN5bmNpby5ydW4obWFpbigpKVxcblwifSIsIm9wdGlvbnMiOnsiYnVpbGQiOiJsYXRlc3QiLCJjb250ZW50VHlwZSI6ImFwcGxpY2F0aW9uL2pzb247IGNoYXJzZXQ9dXRmLTgiLCJtZW1vcnkiOjEwMjQsInRpbWVvdXQiOjE4MH19.cA8xIbeHNMk88004brHmb9qQGAwxdwuD-BrHZzRAU1A\&asrc=run_on_apify)

```
import asyncio

from datetime import timedelta



from apify import Actor





async def main() -> None:

    actor = Actor(

        event_listeners_timeout=timedelta(seconds=30),

        cleanup_timeout=timedelta(seconds=30),

    )



    async with actor:

        # Get input

        actor_input = await actor.get_input()

        actor.log.info('Actor input: %s', actor_input)



        # Your Actor logic here

        data = {'message': 'Hello from Actor instance!', 'input': actor_input}

        await actor.push_data(data)



        # Set status message

        await actor.set_status_message('Actor completed successfully')





if __name__ == '__main__':

    asyncio.run(main())
```

[Run on](https://console.apify.com/actors/HH9rhkFXiZbheuq1V?runConfig=eyJ1IjoiRWdQdHczb2VqNlRhRHQ1cW4iLCJ2IjoxfQ.eyJpbnB1dCI6IntcImNvZGVcIjpcImltcG9ydCBhc3luY2lvXFxuZnJvbSBkYXRldGltZSBpbXBvcnQgdGltZWRlbHRhXFxuXFxuZnJvbSBhcGlmeSBpbXBvcnQgQWN0b3JcXG5cXG5cXG5hc3luYyBkZWYgbWFpbigpIC0-IE5vbmU6XFxuICAgIGFjdG9yID0gQWN0b3IoXFxuICAgICAgICBldmVudF9saXN0ZW5lcnNfdGltZW91dD10aW1lZGVsdGEoc2Vjb25kcz0zMCksXFxuICAgICAgICBjbGVhbnVwX3RpbWVvdXQ9dGltZWRlbHRhKHNlY29uZHM9MzApLFxcbiAgICApXFxuXFxuICAgIGF3YWl0IGFjdG9yLmluaXQoKVxcblxcbiAgICB0cnk6XFxuICAgICAgICAjIEdldCBpbnB1dFxcbiAgICAgICAgYWN0b3JfaW5wdXQgPSBhd2FpdCBhY3Rvci5nZXRfaW5wdXQoKVxcbiAgICAgICAgYWN0b3IubG9nLmluZm8oJ0FjdG9yIGlucHV0OiAlcycsIGFjdG9yX2lucHV0KVxcblxcbiAgICAgICAgIyBZb3VyIEFjdG9yIGxvZ2ljIGhlcmVcXG4gICAgICAgIGRhdGEgPSB7J21lc3NhZ2UnOiAnSGVsbG8gZnJvbSBBY3RvciEnLCAnaW5wdXQnOiBhY3Rvcl9pbnB1dH1cXG4gICAgICAgIGF3YWl0IGFjdG9yLnB1c2hfZGF0YShkYXRhKVxcblxcbiAgICAgICAgIyBTZXQgc3RhdHVzIG1lc3NhZ2VcXG4gICAgICAgIGF3YWl0IGFjdG9yLnNldF9zdGF0dXNfbWVzc2FnZSgnQWN0b3IgY29tcGxldGVkIHN1Y2Nlc3NmdWxseScpXFxuXFxuICAgIGZpbmFsbHk6XFxuICAgICAgICBhd2FpdCBhY3Rvci5leGl0KClcXG5cXG5cXG5pZiBfX25hbWVfXyA9PSAnX19tYWluX18nOlxcbiAgICBhc3luY2lvLnJ1bihtYWluKCkpXFxuXCJ9Iiwib3B0aW9ucyI6eyJidWlsZCI6ImxhdGVzdCIsImNvbnRlbnRUeXBlIjoiYXBwbGljYXRpb24vanNvbjsgY2hhcnNldD11dGYtOCIsIm1lbW9yeSI6MTAyNCwidGltZW91dCI6MTgwfX0.DleIoDi4ZIZlj9zzVKVeoxceUOz4n7wr4QNv0DJlb7E\&asrc=run_on_apify)

```
import asyncio

from datetime import timedelta



from apify import Actor





async def main() -> None:

    actor = Actor(

        event_listeners_timeout=timedelta(seconds=30),

        cleanup_timeout=timedelta(seconds=30),

    )



    await actor.init()



    try:

        # Get input

        actor_input = await actor.get_input()

        actor.log.info('Actor input: %s', actor_input)



        # Your Actor logic here

        data = {'message': 'Hello from Actor!', 'input': actor_input}

        await actor.push_data(data)



        # Set status message

        await actor.set_status_message('Actor completed successfully')



    finally:

        await actor.exit()





if __name__ == '__main__':

    asyncio.run(main())
```

## Error handling[](#error-handling)

Good error handling lets your Actor fail fast on critical errors, retry transient issues safely, and keep data consistent. Normally you rely on the `async with Actor:` block—if it finishes, the run succeeds (exit code 0); if an unhandled exception occurs, the run fails (exit code 91).

The SDK provides helper methods for explicit control:

* [`Actor.exit`](https://docs.apify.com/sdk/python/sdk/python/reference/class/Actor.md#exit) - terminates the run successfully (default exit code 0).
* [`Actor.fail`](https://docs.apify.com/sdk/python/sdk/python/reference/class/Actor.md#fail) - marks the run as failed (default exit code 1).

Any non-zero exit code is treated as a `FAILED` run. You rarely need to call these methods directly unless you want to perform a controlled shutdown or customize the exit behavior.

Catch exceptions only when necessary - for example, to retry network timeouts or map specific errors to exit codes. Keep retry loops bounded with backoff and re-raise once exhausted. Make your processing idempotent so that restarts don't corrupt results. Both [`Actor.exit`](https://docs.apify.com/sdk/python/sdk/python/reference/class/Actor.md#exit) and [`Actor.fail`](https://docs.apify.com/sdk/python/sdk/python/reference/class/Actor.md#fail) perform the same cleanup, so complete any long-running persistence before calling them.

Below is a minimal context-manager example where an unhandled exception automatically fails the run, followed by a manual pattern giving you more control.

[Run on](https://console.apify.com/actors/HH9rhkFXiZbheuq1V?runConfig=eyJ1IjoiRWdQdHczb2VqNlRhRHQ1cW4iLCJ2IjoxfQ.eyJpbnB1dCI6IntcImNvZGVcIjpcImltcG9ydCBhc3luY2lvXFxuXFxuZnJvbSBhcGlmeSBpbXBvcnQgQWN0b3JcXG5cXG5cXG5hc3luYyBkZWYgbWFpbigpIC0-IE5vbmU6XFxuICAgIGFzeW5jIHdpdGggQWN0b3I6XFxuICAgICAgICAjIEFueSB1bmhhbmRsZWQgZXhjZXB0aW9uIHRyaWdnZXJzIEFjdG9yLmZhaWwoKSBhdXRvbWF0aWNhbGx5XFxuICAgICAgICByYWlzZSBSdW50aW1lRXJyb3IoJ0Jvb20nKVxcblxcblxcbmlmIF9fbmFtZV9fID09ICdfX21haW5fXyc6XFxuICAgIGFzeW5jaW8ucnVuKG1haW4oKSlcXG5cIn0iLCJvcHRpb25zIjp7ImJ1aWxkIjoibGF0ZXN0IiwiY29udGVudFR5cGUiOiJhcHBsaWNhdGlvbi9qc29uOyBjaGFyc2V0PXV0Zi04IiwibWVtb3J5IjoxMDI0LCJ0aW1lb3V0IjoxODB9fQ.tsWHKk9HL-cEP63gyZO5gCvcfsjYgA4J7JPEYEnKwyg\&asrc=run_on_apify)

```
import asyncio



from apify import Actor





async def main() -> None:

    async with Actor:

        # Any unhandled exception triggers Actor.fail() automatically

        raise RuntimeError('Boom')





if __name__ == '__main__':

    asyncio.run(main())
```

If you need explicit control over exit codes or status messages, you can manage the Actor manually using [`Actor.init`](https://docs.apify.com/sdk/python/sdk/python/reference/class/Actor.md#init), [`Actor.exit`](https://docs.apify.com/sdk/python/sdk/python/reference/class/Actor.md#exit), and [`Actor.fail`](https://docs.apify.com/sdk/python/sdk/python/reference/class/Actor.md#fail).

[Run on](https://console.apify.com/actors/HH9rhkFXiZbheuq1V?runConfig=eyJ1IjoiRWdQdHczb2VqNlRhRHQ1cW4iLCJ2IjoxfQ.eyJpbnB1dCI6IntcImNvZGVcIjpcImltcG9ydCBhc3luY2lvXFxuaW1wb3J0IHJhbmRvbVxcblxcbmZyb20gYXBpZnkgaW1wb3J0IEFjdG9yXFxuXFxuXFxuYXN5bmMgZGVmIGRvX3dvcmsoKSAtPiBOb25lOlxcbiAgICAjIFNpbXVsYXRlIHJhbmRvbSBvdXRjb21lczogc3VjY2VzcyBvciBvbmUgb2YgdHdvIGV4Y2VwdGlvbiB0eXBlcy5cXG4gICAgb3V0Y29tZSA9IHJhbmRvbS5yYW5kb20oKVxcblxcbiAgICBpZiBvdXRjb21lIDwgMC4zMzpcXG4gICAgICAgIHJhaXNlIFZhbHVlRXJyb3IoJ0ludmFsaWQgaW5wdXQgZGF0YSBlbmNvdW50ZXJlZCcpXFxuICAgIGlmIG91dGNvbWUgPCAwLjY2OlxcbiAgICAgICAgcmFpc2UgUnVudGltZUVycm9yKCdVbmV4cGVjdGVkIHJ1bnRpbWUgZmFpbHVyZScpXFxuXFxuICAgICMgU2ltdWxhdGUgc3VjY2Vzc2Z1bCB3b3JrXFxuICAgIEFjdG9yLmxvZy5pbmZvKCdXb3JrIGNvbXBsZXRlZCBzdWNjZXNzZnVsbHknKVxcblxcblxcbmFzeW5jIGRlZiBtYWluKCkgLT4gTm9uZTpcXG4gICAgYXdhaXQgQWN0b3IuaW5pdCgpXFxuICAgIHRyeTpcXG4gICAgICAgIGF3YWl0IGRvX3dvcmsoKVxcbiAgICBleGNlcHQgVmFsdWVFcnJvciBhcyBleGM6ICAjIFNwZWNpZmljIGVycm9yIG1hcHBpbmcgZXhhbXBsZVxcbiAgICAgICAgYXdhaXQgQWN0b3IuZmFpbChleGl0X2NvZGU9MTAsIGV4Y2VwdGlvbj1leGMpXFxuICAgIGV4Y2VwdCBFeGNlcHRpb24gYXMgZXhjOiAgIyBDYXRjaC1hbGwgZm9yIHVuZXhwZWN0ZWQgZXJyb3JzXFxuICAgICAgICBhd2FpdCBBY3Rvci5mYWlsKGV4aXRfY29kZT05MSwgZXhjZXB0aW9uPWV4YylcXG4gICAgZWxzZTpcXG4gICAgICAgIGF3YWl0IEFjdG9yLmV4aXQoc3RhdHVzX21lc3NhZ2U9J0FjdG9yIGNvbXBsZXRlZCBzdWNjZXNzZnVsbHknKVxcblxcblxcbmlmIF9fbmFtZV9fID09ICdfX21haW5fXyc6XFxuICAgIGFzeW5jaW8ucnVuKG1haW4oKSlcXG5cIn0iLCJvcHRpb25zIjp7ImJ1aWxkIjoibGF0ZXN0IiwiY29udGVudFR5cGUiOiJhcHBsaWNhdGlvbi9qc29uOyBjaGFyc2V0PXV0Zi04IiwibWVtb3J5IjoxMDI0LCJ0aW1lb3V0IjoxODB9fQ.v-eAp4iKOiRP5t_jeg4MaXzUONdmxs_7xM5uZAErtj8\&asrc=run_on_apify)

```
import asyncio

import random



from apify import Actor





async def do_work() -> None:

    # Simulate random outcomes: success or one of two exception types.

    outcome = random.random()



    if outcome < 0.33:

        raise ValueError('Invalid input data encountered')

    if outcome < 0.66:

        raise RuntimeError('Unexpected runtime failure')



    # Simulate successful work

    Actor.log.info('Work completed successfully')





async def main() -> None:

    await Actor.init()

    try:

        await do_work()

    except ValueError as exc:  # Specific error mapping example

        await Actor.fail(exit_code=10, exception=exc)

    except Exception as exc:  # Catch-all for unexpected errors

        await Actor.fail(exit_code=91, exception=exc)

    else:

        await Actor.exit(status_message='Actor completed successfully')





if __name__ == '__main__':

    asyncio.run(main())
```

## Reboot[](#reboot)

Rebooting (available on the Apify platform only) instructs the platform worker to restart your Actor from the beginning of its execution. Use this mechanism only for transient conditions that are likely to resolve after a fresh start — for example, rotating a blocked proxy pool or recovering from a stuck browser environment.

Before triggering a reboot, persist any essential state externally (e.g., to the key-value store or dataset), as all in-memory data is lost after reboot. The example below tracks a reboot counter in the default key-value store and allows at most three restarts before exiting normally.

[Run on](https://console.apify.com/actors/HH9rhkFXiZbheuq1V?runConfig=eyJ1IjoiRWdQdHczb2VqNlRhRHQ1cW4iLCJ2IjoxfQ.eyJpbnB1dCI6IntcImNvZGVcIjpcImltcG9ydCBhc3luY2lvXFxuXFxuZnJvbSBhcGlmeSBpbXBvcnQgQWN0b3JcXG5cXG5cXG5hc3luYyBkZWYgbWFpbigpIC0-IE5vbmU6XFxuICAgIGFzeW5jIHdpdGggQWN0b3I6XFxuICAgICAgICAjIFVzZSB0aGUgS1ZTIHRvIHBlcnNpc3QgYSBzaW1wbGUgcmVib290IGNvdW50ZXIgYWNyb3NzIHJlc3RhcnRzLlxcbiAgICAgICAga3ZzID0gYXdhaXQgQWN0b3Iub3Blbl9rZXlfdmFsdWVfc3RvcmUoKVxcbiAgICAgICAgcmVib290X2NvdW50ZXIgPSBhd2FpdCBrdnMuZ2V0X3ZhbHVlKCdyZWJvb3RfY291bnRlcicsIDApXFxuXFxuICAgICAgICAjIExpbWl0IHRoZSBudW1iZXIgb2YgcmVib290cyB0byBhdm9pZCBpbmZpbml0ZSBsb29wcy5cXG4gICAgICAgIGlmIHJlYm9vdF9jb3VudGVyIDwgMzpcXG4gICAgICAgICAgICBhd2FpdCBrdnMuc2V0X3ZhbHVlKCdyZWJvb3RfY291bnRlcicsIHJlYm9vdF9jb3VudGVyICsgMSlcXG4gICAgICAgICAgICBBY3Rvci5sb2cuaW5mbyhmJ1JlYm9vdCBhdHRlbXB0IHtyZWJvb3RfY291bnRlciArIDF9LzMnKVxcbiAgICAgICAgICAgICMgVHJpZ2dlciBhIHBsYXRmb3JtIHJlYm9vdDsgYWZ0ZXIgcmVzdGFydCB0aGUgY29kZSBydW5zIGZyb20gdGhlIGJlZ2lubmluZy5cXG4gICAgICAgICAgICBhd2FpdCBBY3Rvci5yZWJvb3QoKVxcblxcbiAgICAgICAgQWN0b3IubG9nLmluZm8oJ1JlYm9vdCBsaW1pdCByZWFjaGVkLCBmaW5pc2hpbmcgcnVuJylcXG5cXG5cXG5pZiBfX25hbWVfXyA9PSAnX19tYWluX18nOlxcbiAgICBhc3luY2lvLnJ1bihtYWluKCkpXFxuXCJ9Iiwib3B0aW9ucyI6eyJidWlsZCI6ImxhdGVzdCIsImNvbnRlbnRUeXBlIjoiYXBwbGljYXRpb24vanNvbjsgY2hhcnNldD11dGYtOCIsIm1lbW9yeSI6MTAyNCwidGltZW91dCI6MTgwfX0.zkUzAz4PtCnAuT1nWbo-0HRIvzObMn-69-lX2N96_G0\&asrc=run_on_apify)

```
import asyncio



from apify import Actor





async def main() -> None:

    async with Actor:

        # Use the KVS to persist a simple reboot counter across restarts.

        kvs = await Actor.open_key_value_store()

        reboot_counter = await kvs.get_value('reboot_counter', 0)



        # Limit the number of reboots to avoid infinite loops.

        if reboot_counter < 3:

            await kvs.set_value('reboot_counter', reboot_counter + 1)

            Actor.log.info(f'Reboot attempt {reboot_counter + 1}/3')

            # Trigger a platform reboot; after restart the code runs from the beginning.

            await Actor.reboot()



        Actor.log.info('Reboot limit reached, finishing run')





if __name__ == '__main__':

    asyncio.run(main())
```

## Status message[](#status-message)

[Status messages](https://docs.apify.com/platform/actors/development/programming-interface/status-messages) are lightweight, human-readable progress indicators displayed with the Actor run in Apify Console. Use them to communicate high-level phases or milestones, such as "Fetching list", "Processed 120/500 pages", or "Uploading results". When running locally, the SDK logs the status message instead of sending it to the platform.

Update the status only when the user's understanding of progress changes - avoid frequent updates for every processed item. Detailed information should go to logs or storages (dataset, key-value store) instead.

[Run on](https://console.apify.com/actors/HH9rhkFXiZbheuq1V?runConfig=eyJ1IjoiRWdQdHczb2VqNlRhRHQ1cW4iLCJ2IjoxfQ.eyJpbnB1dCI6IntcImNvZGVcIjpcImltcG9ydCBhc3luY2lvXFxuXFxuZnJvbSBhcGlmeSBpbXBvcnQgQWN0b3JcXG5cXG5cXG5hc3luYyBkZWYgbWFpbigpIC0-IE5vbmU6XFxuICAgIGFzeW5jIHdpdGggQWN0b3I6XFxuICAgICAgICBhd2FpdCBBY3Rvci5zZXRfc3RhdHVzX21lc3NhZ2UoJ0hlcmUgd2UgZ28hJylcXG4gICAgICAgICMgRG8gc29tZSB3b3JrLi4uXFxuICAgICAgICBhd2FpdCBhc3luY2lvLnNsZWVwKDMpXFxuICAgICAgICBhd2FpdCBBY3Rvci5zZXRfc3RhdHVzX21lc3NhZ2UoJ1NvIGZhciBzbyBnb29kLi4uJylcXG4gICAgICAgIGF3YWl0IGFzeW5jaW8uc2xlZXAoMylcXG4gICAgICAgICMgRG8gc29tZSBtb3JlIHdvcmsuLi5cXG4gICAgICAgIGF3YWl0IEFjdG9yLnNldF9zdGF0dXNfbWVzc2FnZSgnU3RlYWR5IGFzIHNoZSBnb2VzLi4uJylcXG4gICAgICAgIGF3YWl0IGFzeW5jaW8uc2xlZXAoMylcXG4gICAgICAgICMgRG8gZXZlbiBtb3JlIHdvcmsuLi5cXG4gICAgICAgIGF3YWl0IEFjdG9yLnNldF9zdGF0dXNfbWVzc2FnZSgnQWxtb3N0IHRoZXJlLi4uJylcXG4gICAgICAgIGF3YWl0IGFzeW5jaW8uc2xlZXAoMylcXG4gICAgICAgICMgRmluaXNoIHRoZSBqb2JcXG4gICAgICAgIGF3YWl0IEFjdG9yLnNldF9zdGF0dXNfbWVzc2FnZSgnUGhldyEgVGhhdCB3YXMgbm90IHRoYXQgaGFyZCEnKVxcblxcblxcbmlmIF9fbmFtZV9fID09ICdfX21haW5fXyc6XFxuICAgIGFzeW5jaW8ucnVuKG1haW4oKSlcXG5cIn0iLCJvcHRpb25zIjp7ImJ1aWxkIjoibGF0ZXN0IiwiY29udGVudFR5cGUiOiJhcHBsaWNhdGlvbi9qc29uOyBjaGFyc2V0PXV0Zi04IiwibWVtb3J5IjoxMDI0LCJ0aW1lb3V0IjoxODB9fQ.iN59lPqRVtg82YPXPHxgaCtSPbwp_m94VdcyUb8nT5Q\&asrc=run_on_apify)

```
import asyncio



from apify import Actor





async def main() -> None:

    async with Actor:

        await Actor.set_status_message('Here we go!')

        # Do some work...

        await asyncio.sleep(3)

        await Actor.set_status_message('So far so good...')

        await asyncio.sleep(3)

        # Do some more work...

        await Actor.set_status_message('Steady as she goes...')

        await asyncio.sleep(3)

        # Do even more work...

        await Actor.set_status_message('Almost there...')

        await asyncio.sleep(3)

        # Finish the job

        await Actor.set_status_message('Phew! That was not that hard!')





if __name__ == '__main__':

    asyncio.run(main())
```
