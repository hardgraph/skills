# Logging

Copy for LLM

The library logs useful debug information to the `apify_client` logger whenever it sends requests to the Apify API. You can configure this logger to print debug information to the standard output by adding a handler:

```
import logging



# Configure the Apify client logger

apify_client_logger = logging.getLogger('apify_client')

apify_client_logger.setLevel(logging.DEBUG)

apify_client_logger.addHandler(logging.StreamHandler())
```

The log records include additional properties, provided via the extra argument, which can be helpful for debugging. Some of these properties are:

* `attempt` - Number of retry attempts for the request.
* `status_code` - HTTP status code of the response. Only present on records about a request's outcome.
* `url` - URL of the API endpoint being called.
* `client_method` - Method name of the client that initiated the request.
* `resource_id` - Identifier of the resource being accessed.

To display these additional properties in the log output, you need to use a custom log formatter. Reference only the properties present on every record, since a `%`-style formatter raises an error for records that lack a referenced property. Here's a basic example:

```
import logging



# Create a custom logging formatter. Reference only the properties present

# on every record. Properties attached to some records only, such as

# `status_code`, would raise an error for the records that lack them.

formatter = logging.Formatter(

    '%(asctime)s - %(name)s - %(levelname)s - %(message)s - '

    '%(client_method)s - %(attempt)s - %(url)s'

)



handler = logging.StreamHandler()

handler.setFormatter(formatter)



# Configure the Apify client logger to use the custom formatter

apify_client_logger = logging.getLogger('apify_client')

apify_client_logger.setLevel(logging.DEBUG)

apify_client_logger.addHandler(handler)
```

For more information on creating and using custom log formatters, refer to the official Python [logging documentation](https://docs.python.org/3/howto/logging.html#formatters).
