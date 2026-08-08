---
title: Deploying your code to Apify
url: https://docs.apify.com/academy/deploying-your-code.md
parents:
  - [Apify documentation](https://docs.apify.com/llms.txt)
  - [Apify Academy](https://docs.apify.com/academy.md)
children:
  - [Managing Actor inputs and outputs](https://docs.apify.com/academy/deploying-your-code/inputs-outputs.md)
  - [How to write Actor input schema](https://docs.apify.com/academy/deploying-your-code/input-schema.md)
  - [Creating dataset schema](https://docs.apify.com/academy/deploying-your-code/dataset-schema.md)
  - [Creating Actor Dockerfile](https://docs.apify.com/academy/deploying-your-code/docker-file.md)
  - [Publishing your Actor](https://docs.apify.com/academy/deploying-your-code/deploying.md)
previous: [Apify client](https://docs.apify.com/academy/getting-started/apify-client.md)
next: [Managing Actor inputs and outputs](https://docs.apify.com/academy/deploying-your-code/inputs-outputs.md)
---

> ## Documentation index
> Fetch the complete documentation index at: https://docs.apify.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Deploying your code to Apify

**In this course learn how to take an existing project of yours and deploy it to the Apify platform as an Actor.**

***

This section discusses how to use your newfound knowledge of the Apify platform and Actors from the [Getting started](https://docs.apify.com/academy/getting-started.md) section to deploy your existing project's code to the Apify platform as an Actor. Any program running in a Docker container can become an Apify Actor.

Apify provides detailed guidance on how to deploy Node.js and Python programs as Actors, but apart from that you're not limited in what programming language you choose for your scraper.

![Supported languages](/assets/images/supported-languages-2b3aced02908c1def900dbace072201a.jpg)

Here are a few examples of Actors in other languages:

* [Rust Actor](https://apify.com/lukaskrivka/rust-actor-example)
* [Go Actor](https://apify.com/jirimoravcik/go-actor-example)
* [Julia Actor](https://apify.com/jirimoravcik/julia-actor-example)

## The "Actorization" workflow

Follow these main steps to turn a piece of code into an Actor:

1. Handle [accepting inputs and writing outputs](https://docs.apify.com/academy/deploying-your-code/inputs-outputs.md).
2. Create the [.actor directory](https://docs.apify.com/actors/development/actor-definition.md) and an `actor.json` configuration file.
3. Create an [input schema](https://docs.apify.com/academy/deploying-your-code/input-schema.md) *(optional)*.
4. Add a [Dockerfile](https://docs.apify.com/academy/deploying-your-code/docker-file.md).
5. [Deploy](https://docs.apify.com/academy/deploying-your-code/deploying.md) to the Apify platform!

## Our example project

For this section, we'll be turning this example project into an Actor:

**JavaScript**


```js
// index.js

const addAllNumbers = (...nums) => nums.reduce((total, curr) => total + curr, 0);



console.log(addAllNumbers(1, 2, 3, 4)); // -> 10
```


**Python**


```py
# index.py

def add_all_numbers (nums):

    total = 0



    for num in nums:

        total += num



    return total



print(add_all_numbers([1, 2, 3, 4])) # -> 10
```


Language examples

For all lessons in this section, we'll have examples for both Node.js and Python so that you can follow along in either language.

## Next up

[Next lesson](https://docs.apify.com/academy/deploying-your-code/inputs-outputs.md), we'll be learning how to accept input into our Actor as well as deliver output.
