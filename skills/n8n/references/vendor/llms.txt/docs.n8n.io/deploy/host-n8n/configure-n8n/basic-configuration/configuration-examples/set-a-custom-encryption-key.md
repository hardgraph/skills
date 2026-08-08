> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/deploy/host-n8n/configure-n8n/basic-configuration/configuration-examples/set-a-custom-encryption-key.md).

# Set a custom encryption key

n8n creates a random encryption key automatically on the first launch and saves it in the `~/.n8n` folder. n8n uses that key to encrypt the credentials before they get saved to the database. If the key isn't yet in the settings file, you can set it using an environment variable, so that n8n uses your custom key instead of generating a new one.

In [queue mode](/deploy/host-n8n/configure-n8n/scaling/enable-queue-mode.md), you must specify the encryption key environment variable for all workers.

```bash
export N8N_ENCRYPTION_KEY=<SOME RANDOM STRING>
```

Refer to [Environment variables reference](/deploy/host-n8n/configure-n8n/basic-configuration/use-environment-variables/deployment.md) for more information on this variable.
