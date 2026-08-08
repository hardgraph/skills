> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/deploy/host-n8n/configure-n8n/basic-configuration/configuration-examples/specify-custom-nodes-location.md).

# Specify custom nodes location

Every user can add custom nodes that get loaded by n8n on startup. The default location is in the subfolder `.n8n/custom` of the user who started n8n.

You can define more folders with an environment variable:

```bash
export N8N_CUSTOM_EXTENSIONS="/home/jim/n8n/custom-nodes;/data/n8n/nodes"
```

Refer to [Environment variables reference](/deploy/host-n8n/configure-n8n/basic-configuration/use-environment-variables/nodes.md) for more information on this variable.
