> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Opt in to secure URL

> Opt in to secure URLs so Retell-generated call recording and log links automatically expire after 24 hours, preventing unauthorized access if a link leaks.

# Secure URL

By default, the URLs we generate for call recordings and logs do not expire, allowing you to easily share the links with other people.
However, if security is a concern and you want to prevent unauthorized access in case the URL is leaked, you can opt in for secure URLs.
Secure URLs automatically expire 24 hours after they are generated, providing an additional layer of security.

## How to Opt In

You can opt in to secure URLs at any time:

1. Navigate to your agent
2. Toggle the "Opt In Secure URL" switch

<Frame>
  <img height="700" src="https://mintcdn.com/retellai/M9QYKZE4hbt00HfL/images/opt_in_secure_url.png?fit=max&auto=format&n=M9QYKZE4hbt00HfL&q=85&s=ffb9fb13081c5e9217e242fca334ca61" alt="Privacy settings showing the Opt In Secure URL toggle" data-path="images/opt_in_secure_url.png" />
</Frame>

## What Happens When You Opt In

* Every time you request the URLs of your call's recording and log, we will generate a URL with a signature that will expire 24 hours after it is generated.
* Accessing the resource using the URL after 24 hours will be denied.
* Files created before secure URLs were enabled will still be generated without signatures.

## What Happens When You Opt Out After Opt In

* Files created while secure URLs were enabled will continue to generate signed URLs with 24-hour expiration whenever you request their URLs, even after you opt out.
* Only files created after you opt out will generate non-expiring URLs.
