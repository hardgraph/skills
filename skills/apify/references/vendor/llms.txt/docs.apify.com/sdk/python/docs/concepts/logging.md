# Logging

Copy for LLM

The Apify SDK logs through Python's standard [`logging`](https://docs.python.org/3/library/logging.html) module, using the `apify` logger. Configuring log levels and formatting helps you debug Actors locally and monitor them on the platform.

## Automatic configuration[](#automatic-configuration)

When you create an Actor from an Apify-provided template, either in [Apify Console](https://docs.apify.com/platform/console) or through the Apify CLI, you do not have to configure the logger yourself. The template already contains initialization code for the logger, which sets the logger level to `DEBUG` and the log formatter to [`ActorLogFormatter`](https://docs.apify.com/sdk/python/sdk/python/reference/class/ActorLogFormatter.md).

## Manual configuration[](#manual-configuration)

If you're not using an Apify template, or you want to override what it sets up, you can configure the log level and the log formatting yourself.

### Configuring the log level[](#configuring-the-log-level)

In Python's default behavior, if you don't configure the logger otherwise, only logs with level `WARNING` or higher are printed out to the standard output, without any formatting. To also have logs with `DEBUG` and `INFO` level printed out, you need to call the [`Logger.setLevel`](https://docs.python.org/3/library/logging.html#logging.Logger.setLevel) method on the logger, with the desired minimum level as an argument.

### Configuring the log formatting[](#configuring-the-log-formatting)

By default, only the log message is printed out to the output, without any formatting. To have a nicer output, with the log level printed in color, the messages nicely aligned, and extra log fields printed out, you can use the [`ActorLogFormatter`](https://docs.apify.com/sdk/python/sdk/python/reference/class/ActorLogFormatter.md) class from the `apify.log` module.

### Example log configuration[](#example-log-configuration)

To configure and test the logger, you can use this snippet:

[Run on](https://console.apify.com/actors/HH9rhkFXiZbheuq1V?runConfig=eyJ1IjoiRWdQdHczb2VqNlRhRHQ1cW4iLCJ2IjoxfQ.eyJpbnB1dCI6IntcImNvZGVcIjpcImltcG9ydCBhc3luY2lvXFxuaW1wb3J0IGxvZ2dpbmdcXG5cXG5mcm9tIGFwaWZ5LmxvZyBpbXBvcnQgQWN0b3JMb2dGb3JtYXR0ZXJcXG5cXG5cXG5hc3luYyBkZWYgbWFpbigpIC0-IE5vbmU6XFxuICAgIGhhbmRsZXIgPSBsb2dnaW5nLlN0cmVhbUhhbmRsZXIoKVxcbiAgICBoYW5kbGVyLnNldEZvcm1hdHRlcihBY3RvckxvZ0Zvcm1hdHRlcigpKVxcblxcbiAgICBhcGlmeV9sb2dnZXIgPSBsb2dnaW5nLmdldExvZ2dlcignYXBpZnknKVxcbiAgICBhcGlmeV9sb2dnZXIuc2V0TGV2ZWwobG9nZ2luZy5ERUJVRylcXG4gICAgYXBpZnlfbG9nZ2VyLmFkZEhhbmRsZXIoaGFuZGxlcilcXG5cXG5cXG5pZiBfX25hbWVfXyA9PSAnX19tYWluX18nOlxcbiAgICBhc3luY2lvLnJ1bihtYWluKCkpXFxuXCJ9Iiwib3B0aW9ucyI6eyJidWlsZCI6ImxhdGVzdCIsImNvbnRlbnRUeXBlIjoiYXBwbGljYXRpb24vanNvbjsgY2hhcnNldD11dGYtOCIsIm1lbW9yeSI6MTAyNCwidGltZW91dCI6MTgwfX0.MfFDLunt1JWpP7of6buQNuF8-CFxp71MvoBpEm2u_QE\&asrc=run_on_apify)

```
import asyncio

import logging



from apify.log import ActorLogFormatter





async def main() -> None:

    handler = logging.StreamHandler()

    handler.setFormatter(ActorLogFormatter())



    apify_logger = logging.getLogger('apify')

    apify_logger.setLevel(logging.DEBUG)

    apify_logger.addHandler(handler)





if __name__ == '__main__':

    asyncio.run(main())
```

This configuration will cause all levels of messages to be printed to the standard output, with some pretty formatting.

## Logger usage[](#logger-usage)

Here you can see how all the log levels would look like.

You can use the `extra` argument for all log levels, it's not specific to the warning level. When you use `Logger.exception`, there is no need to pass the Exception object to the log manually, it will automatically infer it from the current execution context and print the exception details.

[Run on](https://console.apify.com/actors/HH9rhkFXiZbheuq1V?runConfig=eyJ1IjoiRWdQdHczb2VqNlRhRHQ1cW4iLCJ2IjoxfQ.eyJpbnB1dCI6IntcImNvZGVcIjpcImltcG9ydCBhc3luY2lvXFxuaW1wb3J0IGxvZ2dpbmdcXG5cXG5mcm9tIGFwaWZ5IGltcG9ydCBBY3RvclxcbmZyb20gYXBpZnkubG9nIGltcG9ydCBBY3RvckxvZ0Zvcm1hdHRlclxcblxcblxcbmFzeW5jIGRlZiBtYWluKCkgLT4gTm9uZTpcXG4gICAgaGFuZGxlciA9IGxvZ2dpbmcuU3RyZWFtSGFuZGxlcigpXFxuICAgIGhhbmRsZXIuc2V0Rm9ybWF0dGVyKEFjdG9yTG9nRm9ybWF0dGVyKCkpXFxuXFxuICAgIGFwaWZ5X2xvZ2dlciA9IGxvZ2dpbmcuZ2V0TG9nZ2VyKCdhcGlmeScpXFxuICAgIGFwaWZ5X2xvZ2dlci5zZXRMZXZlbChsb2dnaW5nLkRFQlVHKVxcbiAgICBhcGlmeV9sb2dnZXIuYWRkSGFuZGxlcihoYW5kbGVyKVxcblxcbiAgICBhc3luYyB3aXRoIEFjdG9yOlxcbiAgICAgICAgQWN0b3IubG9nLmRlYnVnKCdUaGlzIGlzIGEgZGVidWcgbWVzc2FnZScpXFxuICAgICAgICBBY3Rvci5sb2cuaW5mbygnVGhpcyBpcyBhbiBpbmZvIG1lc3NhZ2UnKVxcbiAgICAgICAgQWN0b3IubG9nLndhcm5pbmcoJ1RoaXMgaXMgYSB3YXJuaW5nIG1lc3NhZ2UnLCBleHRyYT17J3JlYXNvbic6ICdCYWQgQWN0b3IhJ30pXFxuICAgICAgICBBY3Rvci5sb2cuZXJyb3IoJ1RoaXMgaXMgYW4gZXJyb3IgbWVzc2FnZScpXFxuICAgICAgICB0cnk6XFxuICAgICAgICAgICAgcmFpc2UgUnVudGltZUVycm9yKCdPdWNoIScpXFxuICAgICAgICBleGNlcHQgUnVudGltZUVycm9yOlxcbiAgICAgICAgICAgIEFjdG9yLmxvZy5leGNlcHRpb24oJ1RoaXMgaXMgYW4gZXhjZXB0aW9uYWwgbWVzc2FnZScpXFxuXFxuXFxuaWYgX19uYW1lX18gPT0gJ19fbWFpbl9fJzpcXG4gICAgYXN5bmNpby5ydW4obWFpbigpKVxcblwifSIsIm9wdGlvbnMiOnsiYnVpbGQiOiJsYXRlc3QiLCJjb250ZW50VHlwZSI6ImFwcGxpY2F0aW9uL2pzb247IGNoYXJzZXQ9dXRmLTgiLCJtZW1vcnkiOjEwMjQsInRpbWVvdXQiOjE4MH19.pxlvXBG8Z0KC9eG3NJHUqqacB5VspvLZgtpG5XrzvKQ\&asrc=run_on_apify)

```
import asyncio

import logging



from apify import Actor

from apify.log import ActorLogFormatter





async def main() -> None:

    handler = logging.StreamHandler()

    handler.setFormatter(ActorLogFormatter())



    apify_logger = logging.getLogger('apify')

    apify_logger.setLevel(logging.DEBUG)

    apify_logger.addHandler(handler)



    async with Actor:

        Actor.log.debug('This is a debug message')

        Actor.log.info('This is an info message')

        Actor.log.warning('This is a warning message', extra={'reason': 'Bad Actor!'})

        Actor.log.error('This is an error message')

        try:

            raise RuntimeError('Ouch!')

        except RuntimeError:

            Actor.log.exception('This is an exceptional message')





if __name__ == '__main__':

    asyncio.run(main())
```

Result:

```
DEBUG This is a debug message
INFO  This is an info message
WARN  This is a warning message ({"reason": "Bad Actor!"})
ERROR This is an error message
ERROR This is an exceptional message
      Traceback (most recent call last):
        File "main.py", line 6, in <module>
          raise RuntimeError('Ouch!')
      RuntimeError: Ouch!
```

## Redirect logs from other Actor runs[](#redirect-logs-from-other-actor-runs)

In some situations, one Actor is going to start one or more other Actors and wait for them to finish and produce some results. In such cases, you might want to redirect the logs and status messages of the started Actors runs back to the parent Actor run, so that you can see the progress of the started Actors' runs in the parent Actor's logs. This guide will show possibilities on how to do it.

### Redirecting logs from Actor.call[](#redirecting-logs-from-actorcall)

Typical use case for log redirection is to call another Actor using the [`Actor.call`](https://docs.apify.com/sdk/python/sdk/python/reference/class/Actor.md#call) method. This method has an optional `logger` argument, which is by default set to the `default` literal. This means that the logs of the called Actor will be automatically redirected to the parent Actor's logs with default formatting and filtering. If you set the `logger` argument to `None`, then no log redirection happens. The third option is to pass your own `Logger` instance with the possibility to define your own formatter, filter, and handler. Below you can see those three possible ways of log redirection when starting another Actor run through [`Actor.call`](https://docs.apify.com/sdk/python/sdk/python/reference/class/Actor.md#call).

[Run on](https://console.apify.com/actors/HH9rhkFXiZbheuq1V?runConfig=eyJ1IjoiRWdQdHczb2VqNlRhRHQ1cW4iLCJ2IjoxfQ.eyJpbnB1dCI6IntcImNvZGVcIjpcImltcG9ydCBhc3luY2lvXFxuaW1wb3J0IGxvZ2dpbmdcXG5cXG5mcm9tIGFwaWZ5IGltcG9ydCBBY3RvclxcblxcblxcbmFzeW5jIGRlZiBtYWluKCkgLT4gTm9uZTpcXG4gICAgYXN5bmMgd2l0aCBBY3RvcjpcXG4gICAgICAgICMgRGVmYXVsdCByZWRpcmVjdCBsb2dnZXJcXG4gICAgICAgIGF3YWl0IEFjdG9yLmNhbGwoYWN0b3JfaWQ9J3NvbWVfYWN0b3JfaWQnKVxcbiAgICAgICAgIyBObyByZWRpcmVjdCBsb2dnZXJcXG4gICAgICAgIGF3YWl0IEFjdG9yLmNhbGwoYWN0b3JfaWQ9J3NvbWVfYWN0b3JfaWQnLCBsb2dnZXI9Tm9uZSlcXG4gICAgICAgICMgQ3VzdG9tIHJlZGlyZWN0IGxvZ2dlclxcbiAgICAgICAgYXdhaXQgQWN0b3IuY2FsbChcXG4gICAgICAgICAgICBhY3Rvcl9pZD0nc29tZV9hY3Rvcl9pZCcsXFxuICAgICAgICAgICAgbG9nZ2VyPWxvZ2dpbmcuZ2V0TG9nZ2VyKCdjdXN0b21fbG9nZ2VyJyksXFxuICAgICAgICApXFxuXFxuXFxuaWYgX19uYW1lX18gPT0gJ19fbWFpbl9fJzpcXG4gICAgYXN5bmNpby5ydW4obWFpbigpKVxcblwifSIsIm9wdGlvbnMiOnsiYnVpbGQiOiJsYXRlc3QiLCJjb250ZW50VHlwZSI6ImFwcGxpY2F0aW9uL2pzb247IGNoYXJzZXQ9dXRmLTgiLCJtZW1vcnkiOjEwMjQsInRpbWVvdXQiOjE4MH19.NoXpDq_L9HTzDtUMDK4Nfml60wZDW_3spjdN6NOAmgg\&asrc=run_on_apify)

```
import asyncio

import logging



from apify import Actor





async def main() -> None:

    async with Actor:

        # Default redirect logger

        await Actor.call(actor_id='some_actor_id')

        # No redirect logger

        await Actor.call(actor_id='some_actor_id', logger=None)

        # Custom redirect logger

        await Actor.call(

            actor_id='some_actor_id',

            logger=logging.getLogger('custom_logger'),

        )





if __name__ == '__main__':

    asyncio.run(main())
```

Each default redirect logger log entry will have a specific format. After the timestamp, it will contain cyan colored text that will contain the redirect information: the other Actor's name and the run ID. The rest of the log message will be printed in the same manner as the parent Actor's logger is configured.

The log redirection can be deep, meaning that if the other Actor also starts another Actor and is redirecting logs from it, then in the top-level Actor, you can see it as well. See the following example screenshot of the Apify log console when one Actor recursively starts itself (there are 2 levels of recursion in the example).

![Console with redirected logs](/sdk/python/assets/images/redirected_logs_example-56d852dcd17849fecc65a2eb72cab7e3.webp "Example of console with redirected logs from recursively started Actor.")

### Redirecting logs from already running Actor run[](#redirecting-logs-from-already-running-actor-run)

In some cases, you might want to connect to an already running Actor run and redirect its logs to your current Actor run. This can be done using the [`Actor.apify_client`](https://docs.apify.com/sdk/python/sdk/python/reference/class/Actor.md#apify_client) and getting the streamed log from a specific Actor run. You can then use it as a context manager, and the log redirection will be active in the context, or you can control the log redirection manually by explicitly calling `start` and `stop` methods.

You can further decide whether you want to redirect just new logs of the ongoing Actor run, or if you also want to redirect historical logs from that Actor's run, so all logs it has produced since it was started. Both options are shown in the example code below.

[Run on](https://console.apify.com/actors/HH9rhkFXiZbheuq1V?runConfig=eyJ1IjoiRWdQdHczb2VqNlRhRHQ1cW4iLCJ2IjoxfQ.eyJpbnB1dCI6IntcImNvZGVcIjpcImltcG9ydCBhc3luY2lvXFxuXFxuZnJvbSBhcGlmeSBpbXBvcnQgQWN0b3JcXG5cXG5cXG5hc3luYyBkZWYgbWFpbigpIC0-IE5vbmU6XFxuICAgIGFzeW5jIHdpdGggQWN0b3I6XFxuICAgICAgICAjIExpZmVjeWNsZSBvZiByZWRpcmVjdGVkIGxvZ3MgaXMgaGFuZGxlZCBieSB0aGUgY29udGV4dCBtYW5hZ2VyLlxcbiAgICAgICAgYXN5bmMgd2l0aCBhd2FpdCBBY3Rvci5hcGlmeV9jbGllbnQucnVuKCdzb21lX2FjdG9yX2lkJykuZ2V0X3N0cmVhbWVkX2xvZyhcXG4gICAgICAgICAgICAjIFJlZGlyZWN0IGFsbCBsb2dzIGZyb20gdGhlIHN0YXJ0IG9mIHRoYXQgcnVuLCBldmVuIHRoZSBsb2dzIGZyb20gcGFzdC5cXG4gICAgICAgICAgICBmcm9tX3N0YXJ0PVRydWVcXG4gICAgICAgICk6XFxuICAgICAgICAgICAgYXdhaXQgYXN5bmNpby5zbGVlcCg1KVxcbiAgICAgICAgICAgICMgTG9nZ2luZyB3aWxsIHN0b3Agb3V0IG9mIGNvbnRleHRcXG5cXG4gICAgICAgICMgTGlmZWN5Y2xlIG9mIHJlZGlyZWN0ZWQgbG9ncyBjYW4gYmUgaGFuZGxlZCBtYW51YWxseS5cXG4gICAgICAgIHN0cmVhbWVkX2xvZyA9IGF3YWl0IEFjdG9yLmFwaWZ5X2NsaWVudC5ydW4oJ3NvbWVfaWQnKS5nZXRfc3RyZWFtZWRfbG9nKFxcbiAgICAgICAgICAgICMgRG8gbm90IHJlZGlyZWN0IGhpc3RvcmljYWwgbG9ncyBmcm9tIHRoaXMgYWN0b3IgcnVuLlxcbiAgICAgICAgICAgICMgUmVkaXJlY3Qgb25seSBuZXcgbG9ncyBmcm9tIG5vdyBvbi5cXG4gICAgICAgICAgICBmcm9tX3N0YXJ0PUZhbHNlXFxuICAgICAgICApXFxuICAgICAgICBzdHJlYW1lZF9sb2cuc3RhcnQoKVxcbiAgICAgICAgYXdhaXQgYXN5bmNpby5zbGVlcCg1KVxcbiAgICAgICAgYXdhaXQgc3RyZWFtZWRfbG9nLnN0b3AoKVxcblxcblxcbmlmIF9fbmFtZV9fID09ICdfX21haW5fXyc6XFxuICAgIGFzeW5jaW8ucnVuKG1haW4oKSlcXG5cIn0iLCJvcHRpb25zIjp7ImJ1aWxkIjoibGF0ZXN0IiwiY29udGVudFR5cGUiOiJhcHBsaWNhdGlvbi9qc29uOyBjaGFyc2V0PXV0Zi04IiwibWVtb3J5IjoxMDI0LCJ0aW1lb3V0IjoxODB9fQ.sRNip8tvY3OtlKpufo1NahKaoRYXvkki1hoFEjVMmFM\&asrc=run_on_apify)

```
import asyncio



from apify import Actor





async def main() -> None:

    async with Actor:

        # Lifecycle of redirected logs is handled by the context manager.

        async with await Actor.apify_client.run('some_actor_id').get_streamed_log(

            # Redirect all logs from the start of that run, even the logs from past.

            from_start=True

        ):

            await asyncio.sleep(5)

            # Logging will stop out of context



        # Lifecycle of redirected logs can be handled manually.

        streamed_log = await Actor.apify_client.run('some_id').get_streamed_log(

            # Do not redirect historical logs from this actor run.

            # Redirect only new logs from now on.

            from_start=False

        )

        streamed_log.start()

        await asyncio.sleep(5)

        await streamed_log.stop()





if __name__ == '__main__':

    asyncio.run(main())
```
