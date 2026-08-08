> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/deploy/host-n8n/community-edition-features.md).

# Compare editions

Every self-hosted installation runs on the same underlying product. You can stick with the basic Community edition for free, indefinitely. Alternatively, register to unlock extra features for free, or subscribe to a paid plan to get a license key that unlocks the features for that plan.

* **Community edition**: the free edition. This is what n8n runs without a license key.
* **Registered Community edition**: the free Community edition with extra features unlocked by registering your email. Still free.
* **Business** and **Enterprise** plans: paid plans. Subscribing gives you a license key that unlocks the features for that plan.

{% hint style="info" %}
**Plan, edition, or license?**

An **edition** is the variant of the self-hosted software you run: Community, registered Community, Business, or Enterprise. A **plan** is the paid subscription tier you buy, for self-hosted or [n8n Cloud](/deploy/use-n8n-cloud/start-your-free-trial.md). When you subscribe to a paid plan, you get a **license** key that unlocks the features for that plan. Registering a free Community edition also uses a license key. See the [glossary](/key-concept-glossary.md#edition-n8n) for definitions.
{% endhint %}

{% hint style="info" %}
**The pricing page is the source of truth**

Exact features per plan and edition can change. For the current breakdown, see the [pricing page](https://n8n.io/pricing/). This page explains what the plans and editions are and the main differences between them, not the full feature list.
{% endhint %}

## Community edition <a href="#community-edition" id="community-edition"></a>

The Community edition includes almost the complete feature set of n8n. It doesn't include these features:

* [Custom Variables](/build/code-in-n8n/define-custom-variables.md)
* [Environments](/administer/use-source-control-and-environments.md)
* [External secrets](/administer/manage-credentials/use-external-secret-stores.md)
* [External storage for binary data](/deploy/host-n8n/configure-n8n/scaling/use-external-storage.md)
* [Log streaming](/administer/observe-and-log/stream-logs-to-external-systems.md) ([Logging](/deploy/host-n8n/keep-n8n-running/set-up-logging.md) *is* included)
* [Multi-main mode](/deploy/host-n8n/configure-n8n/scaling/enable-queue-mode.md#multi-main-setup) ([Queue mode](/deploy/host-n8n/configure-n8n/scaling/enable-queue-mode.md) *is* included)
* [Projects](/administer/manage-users-and-access/set-permissions-and-roles-rbac/organize-work-in-projects.md)
* SSO ([SAML](/deploy/host-n8n/configure-n8n/security/configure-sso.md), [LDAP](/administer/manage-users-and-access/verify-user-identity/connect-ldap.md))
* Sharing ([workflows](/build/manage-workflows/share-with-others.md), [credentials](/administer/manage-credentials/share-credentials-securely.md)) (Only the instance owner and the user who creates them can access workflows and credentials)
* [Version control using Git](/administer/use-source-control-and-environments.md)

These features are available on the Business and Enterprise plans. Some are also available on the Starter, Pro, and Enterprise n8n Cloud plans. See [pricing](https://n8n.io/pricing/) for the current breakdown.

## Registered Community edition <a href="#registered-community-edition" id="registered-community-edition"></a>

Register your Community edition with your email to receive a free license key. It unlocks these features for the Community edition:

* Folders: Organize your workflows into tidy folders
* [Debug in editor](/build/understand-workflows/understand-executions/debug-executions.md): Copy and pin[^1] execution data when working on a workflow
* [Custom execution data](/build/understand-workflows/understand-executions/customize-executions-data.md): Save, find, and annotate execution metadata

To register a new Community edition instance, select the option during your initial account creation.

To register an existing Community edition instance:

1. Select the **three dots icon** <img src="/files/cswZmvezHpwEHCDZa9sT" alt="three dots icon" data-size="line"> in the lower-left corner.
2. Select **Settings** and then **Usage and plan**.
3. Select **Unlock** to enter your email and then select **Send me a free license key**.
4. Check your email for the account you entered.

Once you have a license key, activate it by selecting the button in the license email, or by going to **Options > Settings > Usage and plan** and selecting **Enter activation key**.

## Business and Enterprise plans <a href="#business-and-enterprise-editions" id="business-and-enterprise-editions"></a>

The Business and Enterprise plans unlock the paid features that the Community edition doesn't include, such as SSO, environments, external secrets, and log streaming. The Enterprise plan adds the most advanced features for security, scaling, and governance.

When you subscribe to a paid plan, you get a license key that unlocks the features for that plan. Add the key to your instance to activate it. For how to get and activate a key, see [Manage your license](/deploy/host-n8n/configure-n8n/manage-your-license.md). For the features included in each plan, see the [pricing page](https://n8n.io/pricing/).

## Which plan or edition is right for you <a href="#which-edition-is-right-for-you" id="which-edition-is-right-for-you"></a>

* Use the **Community edition** to self-host n8n for free with almost the complete feature set.
* **Register** your Community edition (free) to also get folders, debug in editor, and custom execution data.
* Subscribe to a **Business** or **Enterprise** plan when you need paid features like SSO, environments, projects, or external secrets. If you'd rather not manage infrastructure at all, consider [n8n Cloud](/deploy/use-n8n-cloud/start-your-free-trial.md) instead.

[^1]: Data pinning allows you to temporarily freeze the output data of a node during workflow development. This allows you to develop workflows with predictable data without making repeated requests to external services. Production workflows ignore pinned data and request new data on each execution.
