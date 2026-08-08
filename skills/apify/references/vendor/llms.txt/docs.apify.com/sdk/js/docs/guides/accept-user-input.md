# Accept user input

Copy for LLM

This example accepts and logs user input:

```
import { Actor } from 'apify';



await Actor.init();



const input = await Actor.getInput();

console.log(input);



await Actor.exit();
```

To provide the actor with input, create a `INPUT.json` file inside the "default" key-value store:

```
{PROJECT_FOLDER}/storage/key_value_stores/default/INPUT.json
```

Anything in this file will be available to the actor when it runs.

To learn about other ways to provide an actor with input, refer to the [Apify Platform Documentation](https://apify.com/docs/actor#run).
