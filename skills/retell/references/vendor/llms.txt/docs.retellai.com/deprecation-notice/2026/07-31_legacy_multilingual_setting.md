> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Multilingual agent locale array required (07/31/2026)

> On 07/31/2026, the scalar "multi" value of the agent language field is removed in favor of an explicit locale array for multilingual Retell agents.

## Legacy multilingual setting is deprecated

The legacy scalar value `"multi"` of the agent `language` field is deprecated in favor of the explicit locale-array form (e.g. `["en-US","es-ES"]`). The array form is supported on the same `language` field and lets you pick the exact set of languages your agent should handle, which improves accuracy compared to the static 10-language `"multi"` set.

**Affected APIs:**

* [Create Agent](/api-references/create-agent)
* [Update Agent](/api-references/update-agent)
* [Create Chat Agent](/api-references/create-chat-agent)
* [Update Chat Agent](/api-references/update-chat-agent)

**What's changing on 07/31/2026:**

1. The scalar `"multi"` value of `language` will be rejected by the Create Agent, Update Agent, Create Chat Agent, and Update Chat Agent endpoints.
2. The dashboard's legacy **Multilingual** toggle will be removed.
3. Existing agents that still have `language: "multi"` will be **automatically migrated by Retell** to the equivalent locale array (`["en-US", "es-ES", "fr-FR", "de-DE", "hi-IN", "ru-RU", "pt-PT", "ja-JP", "it-IT", "nl-NL"]`). No action is required on your existing agents.

**Action required for API integrators:**

If your client code constructs agent payloads with `"language": "multi"`, update it to send the locale array form before 07/31/2026. After that date, requests using the scalar `"multi"` value will be rejected. For better accuracy, narrow the array down to the languages your agent actually needs — see the accuracy trade-offs in [Configure a multilingual agent](/agent/multilingual).

<CodeGroup>
  ```json Before theme={"dark"}
  {
    "language": "multi"
  }
  ```

  ```json After theme={"dark"}
  {
    "language": ["en-US", "es-ES", "fr-FR", "de-DE", "hi-IN", "ru-RU", "pt-PT", "ja-JP", "it-IT", "nl-NL"]
  }
  ```
</CodeGroup>

Also update any code that reads agent configurations and branches on `language === "multi"`, since after the automatic migration the field will return the array form instead.

**Effective date:** After 07/31/2026, agents created or updated with `language: "multi"` will be rejected. Existing agents are migrated automatically on or before this date.
