> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Compare agent versions

> Compare any two versions of a Retell agent side by side to review prompt, voice, tool, and flow changes before publishing or auditing past modifications.

You can compare any two versions of an agent to see exactly what changed between them. This is useful for reviewing changes before publishing, auditing past modifications, or understanding the differences between versions.

## How to Access Version Comparison

There are two ways to open the version comparison modal:

1. **From Version History**: Click the clock icon to open version history, hover over a version, and click the "Compare versions" button. This will compare the selected version against the current draft.

2. **From Publish Modal**: When publishing a new version, click the "Compare" button in the publish modal to see what's changing between the last published version and your current draft.

<img src="https://mintcdn.com/retellai/8NtMj_-iY-w_Wddx/images/version/version-comparison-modal.png?fit=max&auto=format&n=8NtMj_-iY-w_Wddx&q=85&s=70e9a66fbbee56959c100feb34688dd9" width="1252" height="990" data-path="images/version/version-comparison-modal.png" />

## Comparison Modes

The version comparison feature offers two different view modes to help you understand changes:

### Standard View (JSON Diff)

The standard view shows a traditional diff view with the full JSON configuration of both versions side by side.

<img src="https://mintcdn.com/retellai/8NtMj_-iY-w_Wddx/images/version/standard-diff.png?fit=max&auto=format&n=8NtMj_-iY-w_Wddx&q=85&s=4d8f0d1c3eebcbadd5f3f268e9de658e" width="2068" height="2420" data-path="images/version/standard-diff.png" />

**Features:**

* **Split view**: Shows the two versions in separate columns for easy comparison
* **Syntax highlighting**: JSON syntax is color-coded for readability
* **Show parent fields**: Toggle this option to see the parent JSON structure containing changes, making it easier to understand the context of modifications

### Semantic Diff

The semantic diff view provides a more human-readable summary of changes, organizing them by field.

<img src="https://mintcdn.com/retellai/8NtMj_-iY-w_Wddx/images/version/semantic-diff.png?fit=max&auto=format&n=8NtMj_-iY-w_Wddx&q=85&s=52fea9eaf2e15f508fd2e18adb043710" width="2072" height="2432" data-path="images/version/semantic-diff.png" />

**Features:**

* **Change summary**: Shows a count of additions, removals, and modifications at the top
* **Categorized by change type**: Changes are categorized as added (+), removed (−), or changed (\~)
* **Field paths**: Each change shows the full path to the modified field (e.g., `response_engine.prompt`)
* **Expandable details**: Click on complex values to expand and see the full content
* **Word-level diffs**: For long text changes, highlights the specific words that were added or removed

<Tip>
  Use **Standard View** when you need to see the complete configuration context or verify exact JSON structure. Use **Semantic Diff** when you want a quick, scannable summary of what changed.
</Tip>

## What Gets Compared

When comparing versions, the modal shows differences across all components of your agent:

* **Agent configuration**: Basic agent settings and metadata
* **Response Engine**: For single/multi prompt agents, shows Retell LLM changes. For conversation flow agents, shows flow configuration changes
* **Related entities**: Any linked knowledge bases, functions, or other configurations
