> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/administer/readme.md).

# Administer

Administer n8n by controlling access, securing credentials, managing changes, and monitoring activity.

This section helps you run n8n securely and reliably as usage grows.

{% hint style="info" %}
Enterprise teams often spend more time in this section. SSO, directory integration, change control, and centralized logging become more important at scale. Many features covered here are also useful outside Enterprise, including user management basics, credential security, and operational monitoring.
{% endhint %}

### A typical administration workflow

{% stepper %}
{% step %}

### Control access

Decide who can sign in, what they can do, and how work is organized. Start with [Manage users and access](/administer/manage-users-and-access.md).
{% endstep %}

{% step %}

### Protect secrets

Store and share credentials safely. Use [Manage credentials](/administer/manage-credentials.md) to reduce secret sprawl.
{% endstep %}

{% step %}

### Move changes safely

Use Git-backed workflows to promote changes between environments. See [Use source control and environments](/administer/use-source-control-and-environments.md).
{% endstep %}

{% step %}

### Monitor activity

Track usage and send signals to your logging tools. Use [Observe and log](/administer/observe-and-log.md) to improve visibility.
{% endstep %}
{% endstepper %}
