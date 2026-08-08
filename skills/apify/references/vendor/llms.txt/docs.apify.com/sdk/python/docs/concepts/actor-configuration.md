# Actor configuration

Copy for LLM

The [`Actor`](https://docs.apify.com/sdk/python/sdk/python/reference/class/Actor.md) class is configured through the [`Configuration`](https://docs.apify.com/sdk/python/sdk/python/reference/class/Configuration.md) class, which reads its settings from environment variables. When running on the Apify platform or through the Apify CLI, configuration is automatic — manual setup is only needed for custom requirements.

If you need some special configuration, you can adjust it either through the `Configuration` class directly, or by setting [environment variables](https://docs.apify.com/platform/actors/development/programming-interface/environment-variables) when running the Actor locally.

To see the full list of configuration options, check the `Configuration` class or the list of environment variables that the Actor understands.

## Configuring from code[](#configuring-from-code)

This will cause the Actor to persist its state every 10 seconds:

[Run on](https://console.apify.com/actors/HH9rhkFXiZbheuq1V?runConfig=eyJ1IjoiRWdQdHczb2VqNlRhRHQ1cW4iLCJ2IjoxfQ.eyJpbnB1dCI6IntcImNvZGVcIjpcImltcG9ydCBhc3luY2lvXFxuZnJvbSBkYXRldGltZSBpbXBvcnQgdGltZWRlbHRhXFxuXFxuZnJvbSBhcGlmeSBpbXBvcnQgQWN0b3IsIENvbmZpZ3VyYXRpb24sIEV2ZW50XFxuXFxuXFxuYXN5bmMgZGVmIG1haW4oKSAtPiBOb25lOlxcbiAgICBjb25maWd1cmF0aW9uID0gQ29uZmlndXJhdGlvbihcXG4gICAgICAgIHBlcnNpc3Rfc3RhdGVfaW50ZXJ2YWw9dGltZWRlbHRhKHNlY29uZHM9MTApXFxuICAgICAgICAjIFNldCBvdGhlciBjb25maWd1cmF0aW9uIG9wdGlvbnMgaGVyZSBhcyBuZWVkZWQuXFxuICAgIClcXG5cXG4gICAgYXN5bmMgd2l0aCBBY3Rvcihjb25maWd1cmF0aW9uPWNvbmZpZ3VyYXRpb24pOlxcbiAgICAgICAgIyBEZWZpbmUgYSBoYW5kbGVyIHRoYXQgd2lsbCBiZSBjYWxsZWQgZm9yIGV2ZXJ5IHBlcnNpc3Qgc3RhdGUgZXZlbnQuXFxuICAgICAgICBhc3luYyBkZWYgc2F2ZV9zdGF0ZSgpIC0-IE5vbmU6XFxuICAgICAgICAgICAgYXdhaXQgQWN0b3Iuc2V0X3ZhbHVlKCdTVEFURScsICdIZWxsbywgd29ybGQhJylcXG5cXG4gICAgICAgICMgVGhlIHNhdmVfc3RhdGUgaGFuZGxlciB3aWxsIGJlIGNhbGxlZCBldmVyeSAxMCBzZWNvbmRzIG5vdy5cXG4gICAgICAgIEFjdG9yLm9uKEV2ZW50LlBFUlNJU1RfU1RBVEUsIHNhdmVfc3RhdGUpXFxuXFxuXFxuaWYgX19uYW1lX18gPT0gJ19fbWFpbl9fJzpcXG4gICAgYXN5bmNpby5ydW4obWFpbigpKVxcblwifSIsIm9wdGlvbnMiOnsiYnVpbGQiOiJsYXRlc3QiLCJjb250ZW50VHlwZSI6ImFwcGxpY2F0aW9uL2pzb247IGNoYXJzZXQ9dXRmLTgiLCJtZW1vcnkiOjEwMjQsInRpbWVvdXQiOjE4MH19.jrbhGRjqB5tlMvKvW2uCKjDF8hWmIkTe9A28UQseo78\&asrc=run_on_apify)

```
import asyncio

from datetime import timedelta



from apify import Actor, Configuration, Event





async def main() -> None:

    configuration = Configuration(

        persist_state_interval=timedelta(seconds=10)

        # Set other configuration options here as needed.

    )



    async with Actor(configuration=configuration):

        # Define a handler that will be called for every persist state event.

        async def save_state() -> None:

            await Actor.set_value('STATE', 'Hello, world!')



        # The save_state handler will be called every 10 seconds now.

        Actor.on(Event.PERSIST_STATE, save_state)





if __name__ == '__main__':

    asyncio.run(main())
```

## Configuring via environment variables[](#configuring-via-environment-variables)

All configuration options can also be set via environment variables. Most options are read from an environment variable named after the option in uppercase. Many options also accept several aliases, commonly with an `APIFY_`, `ACTOR_`, or `CRAWLEE_` prefix. For the full list of configuration options, see the [`Configuration`](https://docs.apify.com/sdk/python/sdk/python/reference/class/Configuration.md) API reference.

For example, this Actor run will keep the contents of its local storages instead of purging them on start:

```
APIFY_PURGE_ON_START=0 apify run
```

### Commonly used options[](#commonly-used-options)

The following table lists a few options you are most likely to set yourself. When running on the Apify platform or via the Apify CLI, the platform-related options are populated automatically.

| Option                   | Environment variable                  | Default       | Description                                                                       |
| ------------------------ | ------------------------------------- | ------------- | --------------------------------------------------------------------------------- |
| `token`                  | `APIFY_TOKEN`                         | `None`        | API token used to authenticate calls to the Apify API.                            |
| `proxy_password`         | `APIFY_PROXY_PASSWORD`                | `None`        | Password for [Apify Proxy](https://docs.apify.com/proxy).                         |
| `purge_on_start`         | `APIFY_PURGE_ON_START`                | `True`        | Whether to purge local storages when the Actor starts.                            |
| `persist_state_interval` | `APIFY_PERSIST_STATE_INTERVAL_MILLIS` | `1 min`       | How often the `PERSIST_STATE` event is emitted (the variable is in milliseconds). |
| `log_level`              | `APIFY_LOG_LEVEL`                     | `'INFO'`      | Minimum severity of log messages that are printed.                                |
| `headless`               | `APIFY_HEADLESS`                      | `True`        | Whether to run browsers in headless mode.                                         |
| `storage_dir`            | `APIFY_LOCAL_STORAGE_DIR`             | `'./storage'` | Directory holding local storages when running outside the platform.               |
| `is_at_home`             | `APIFY_IS_AT_HOME`                    | `False`       | Set by the platform. `True` when the Actor runs on Apify.                         |

## Reading the runtime environment[](#reading-the-runtime-environment)

The [`Actor.get_env`](https://docs.apify.com/sdk/python/sdk/python/reference/class/Actor.md#get_env) method returns a dictionary with all `APIFY_*` environment variables parsed into their typed values. This is useful for inspecting the Actor's runtime context, such as the Actor ID, run ID, or default storage IDs. Variables that are not set or are invalid will have a value of `None`.

[Run on](https://console.apify.com/actors/HH9rhkFXiZbheuq1V?runConfig=eyJ1IjoiRWdQdHczb2VqNlRhRHQ1cW4iLCJ2IjoxfQ.eyJpbnB1dCI6IntcImNvZGVcIjpcImltcG9ydCBhc3luY2lvXFxuXFxuZnJvbSBhcGlmeSBpbXBvcnQgQWN0b3JcXG5cXG5cXG5hc3luYyBkZWYgbWFpbigpIC0-IE5vbmU6XFxuICAgIGFzeW5jIHdpdGggQWN0b3I6XFxuICAgICAgICBlbnYgPSBBY3Rvci5nZXRfZW52KClcXG5cXG4gICAgICAgIEFjdG9yLmxvZy5pbmZvKGYnQWN0b3IgSUQ6IHtlbnYuZ2V0KFxcXCJpZFxcXCIpfScpXFxuICAgICAgICBBY3Rvci5sb2cuaW5mbyhmJ1J1biBJRDoge2Vudi5nZXQoXFxcInJ1bl9pZFxcXCIpfScpXFxuICAgICAgICBBY3Rvci5sb2cuaW5mbyhmJ0RlZmF1bHQgZGF0YXNldCBJRDoge2Vudi5nZXQoXFxcImRlZmF1bHRfZGF0YXNldF9pZFxcXCIpfScpXFxuICAgICAgICBBY3Rvci5sb2cuaW5mbyhmJ0RlZmF1bHQgS1ZTIElEOiB7ZW52LmdldChcXFwiZGVmYXVsdF9rZXlfdmFsdWVfc3RvcmVfaWRcXFwiKX0nKVxcblxcblxcbmlmIF9fbmFtZV9fID09ICdfX21haW5fXyc6XFxuICAgIGFzeW5jaW8ucnVuKG1haW4oKSlcXG5cIn0iLCJvcHRpb25zIjp7ImJ1aWxkIjoibGF0ZXN0IiwiY29udGVudFR5cGUiOiJhcHBsaWNhdGlvbi9qc29uOyBjaGFyc2V0PXV0Zi04IiwibWVtb3J5IjoxMDI0LCJ0aW1lb3V0IjoxODB9fQ.C2j6zCMhvZUqXp2YXTtsbAwm8U48Shg9gmOrIyG1Sas\&asrc=run_on_apify)

```
import asyncio



from apify import Actor





async def main() -> None:

    async with Actor:

        env = Actor.get_env()



        Actor.log.info(f'Actor ID: {env.get("id")}')

        Actor.log.info(f'Run ID: {env.get("run_id")}')

        Actor.log.info(f'Default dataset ID: {env.get("default_dataset_id")}')

        Actor.log.info(f'Default KVS ID: {env.get("default_key_value_store_id")}')





if __name__ == '__main__':

    asyncio.run(main())
```

## Platform detection[](#platform-detection)

The [`Actor.is_at_home`](https://docs.apify.com/sdk/python/sdk/python/reference/class/Actor.md#is_at_home) method returns `True` when the Actor is running on the Apify platform, and `False` when running locally. This is useful for branching behavior based on the environment, such as using different storage backends or skipping proxy configuration during local development.

[Run on](https://console.apify.com/actors/HH9rhkFXiZbheuq1V?runConfig=eyJ1IjoiRWdQdHczb2VqNlRhRHQ1cW4iLCJ2IjoxfQ.eyJpbnB1dCI6IntcImNvZGVcIjpcImltcG9ydCBhc3luY2lvXFxuXFxuZnJvbSBhcGlmeSBpbXBvcnQgQWN0b3JcXG5cXG5cXG5hc3luYyBkZWYgbWFpbigpIC0-IE5vbmU6XFxuICAgIGFzeW5jIHdpdGggQWN0b3I6XFxuICAgICAgICBpZiBBY3Rvci5pc19hdF9ob21lKCk6XFxuICAgICAgICAgICAgQWN0b3IubG9nLmluZm8oJ1J1bm5pbmcgb24gdGhlIEFwaWZ5IHBsYXRmb3JtJylcXG4gICAgICAgIGVsc2U6XFxuICAgICAgICAgICAgQWN0b3IubG9nLmluZm8oJ1J1bm5pbmcgbG9jYWxseScpXFxuXFxuXFxuaWYgX19uYW1lX18gPT0gJ19fbWFpbl9fJzpcXG4gICAgYXN5bmNpby5ydW4obWFpbigpKVxcblwifSIsIm9wdGlvbnMiOnsiYnVpbGQiOiJsYXRlc3QiLCJjb250ZW50VHlwZSI6ImFwcGxpY2F0aW9uL2pzb247IGNoYXJzZXQ9dXRmLTgiLCJtZW1vcnkiOjEwMjQsInRpbWVvdXQiOjE4MH19.IsUHQSbR0NWsr1SeDFtwTGeJ-YldhDyC8KlALqdusss\&asrc=run_on_apify)

```
import asyncio



from apify import Actor





async def main() -> None:

    async with Actor:

        if Actor.is_at_home():

            Actor.log.info('Running on the Apify platform')

        else:

            Actor.log.info('Running locally')





if __name__ == '__main__':

    asyncio.run(main())
```
