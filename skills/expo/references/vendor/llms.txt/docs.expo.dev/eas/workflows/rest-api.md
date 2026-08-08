---
modificationDate: May 25, 2026
title: Workflows REST API
description: Trigger a workflow and check its status from your own systems with the EAS REST API.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/eas/workflows/rest-api/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/eas/workflows/rest-api/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: EAS > EAS Workflows
Pages in this section:
- [Introduction](https://docs.expo.dev/eas/workflows/introduction.md)
- [Get started](https://docs.expo.dev/eas/workflows/get-started.md)
- [Pre-packaged jobs](https://docs.expo.dev/eas/workflows/pre-packaged-jobs.md)
- [Syntax](https://docs.expo.dev/eas/workflows/syntax.md)
- [Environment variables](https://docs.expo.dev/eas/workflows/environment.md)
- [Automating EAS CLI commands](https://docs.expo.dev/eas/workflows/automating-eas-cli.md)
- [REST API](https://docs.expo.dev/eas/workflows/rest-api.md) (this page)
- [Troubleshooting](https://docs.expo.dev/eas/workflows/troubleshooting.md)
- [Limitations](https://docs.expo.dev/eas/workflows/limitations.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Workflows REST API

Trigger a workflow and check its status from your own systems with the EAS REST API.

The EAS REST API exposes endpoints for triggering a workflow and reading run details. All endpoints live under `https://api.expo.dev`. Requests and responses are JSON.

## Authentication

Both endpoints require an EAS access token, sent as a bearer token in the `Authorization` header.

For production integrations, create a [robot user](/accounts/programmatic-access.md#robot-users-and-access-tokens) on the account that owns the project and give it a scoped role. The token then represents the robot user, not a person. You can revoke it without affecting any user. A [personal access token](/accounts/programmatic-access.md#personal-access-tokens) also works for one-off scripts.

```text
Authorization: Bearer <EXPO_TOKEN>
Content-Type: application/json
```

## Trigger a workflow

```text
POST /v2/workflows/dispatch
```

Resolves the workflow file on the given Git ref, validates `inputs` against the `workflow_dispatch` schema declared in the workflow, and enqueues a new run.

### Request body

| Field | Type | Required | Description |
| --- | --- | --- | --- |
| `appId` | string (UUID) | Yes | The EAS project ID. Find it on the project page or in [app config](/workflow/configuration.md) under `extra.eas.projectId`. |
| `gitRef` | string | Yes | A branch name (`main`), tag (`v1.0.0`), commit SHA, or fully qualified ref (`refs/heads/main`, `refs/tags/v1.0.0`). |
| `fileName` | string | Yes | The workflow file name only, such as **deploy.yml**. Do not include the **.eas/workflows/** prefix or any other path segments. |
| `inputs` | object | No | Values for inputs declared under `on.workflow_dispatch.inputs` in the workflow file. Validated against the declared schema. |

### Response

Returns `200 OK` with the new workflow run ID and a link to the run in the dashboard:

```json
{
  "data": {
    "id": "019d9d17-013a-7e05-89aa-4aa83ff68c32",
    "url": "https://expo.dev/accounts/acme/projects/example/workflows/019d9d17-013a-7e05-89aa-4aa83ff68c32"
  }
}
```

### Example

```sh
curl -X POST "https://api.expo.dev/v2/workflows/dispatch" \
-H "Authorization: Bearer $EXPO_TOKEN" \
-H "Content-Type: application/json" \
-d '{
"appId": "a415eac6-231a-4b38-b481-3255a59f13b8",
"gitRef": "main",
"fileName": "deploy.yml",
"inputs": { "environment": "production" }
}'
```

### Errors

| Status | Reason |
| --- | --- |
| 400 | The request body fails schema validation, or `inputs` do not match the workflow's declared schema. |
| 403 | The token does not have access to the given `appId`. |
| 404 | The Git ref does not exist in the linked repository, or the workflow file is not found on that ref. |

## Get a workflow run

```text
GET /v2/workflows/runs/:workflowRunId
```

Returns the workflow run and the jobs it contains. Use this endpoint to poll for completion after triggering a run, or to render a run's details in your own UI.

### Response

```json
{
  "data": {
    "id": "019d9d17-013a-7e05-89aa-4aa83ff68c32",
    "status": "in-progress",
    "url": "https://expo.dev/accounts/acme/projects/example/workflows/019d9d17-013a-7e05-89aa-4aa83ff68c32",
    "gitCommitHash": "564b61ebdd403d28b5dc616a12ce160b91585b5b",
    "gitCommitMessage": "Add home screen",
    "requestedGitRef": "refs/heads/main",
    "triggerEventType": "MANUAL",
    "createdAt": "2026-05-22T15:04:11.000Z",
    "updatedAt": "2026-05-22T15:05:42.000Z",
    "jobs": [
      {
        "id": "019d9d17-1a3f-7c10-bdee-5f2811a9d6ad",
        "key": "build_ios",
        "name": "Build iOS",
        "type": "build",
        "status": "in-progress",
        "requiredJobKeys": [],
        "environment": "production",
        "outputs": {},
        "errors": [],
        "buildId": "f9609423-5072-4ea2-a0a5-c345eedf2c2a",
        "submissionId": null,
        "createdAt": "2026-05-22T15:04:11.000Z",
        "updatedAt": "2026-05-22T15:04:42.000Z"
      }
    ]
  }
}
```

The run's `status` is one of:

| Value | Meaning |
| --- | --- |
| `new` | The run is queued and has not started yet. |
| `in-progress` | At least one job is running. |
| `action-required` | The run is paused and waiting on a manual action, such as approval. |
| `success` | The run finished and every job succeeded. Terminal. |
| `failure` | The run finished with at least one failed job. Terminal. |
| `canceled` | The run was canceled before it finished. Terminal. |

Each entry in `jobs` includes its own `status`, one of:

| Value | Meaning |
| --- | --- |
| `new` | The job is queued. |
| `in-progress` | The job is running. |
| `action-required` | The job is paused and waiting on a manual action, such as approval. |
| `pending-cancel` | A cancellation has been requested but the job has not stopped yet. |
| `success` | The job succeeded. Terminal. |
| `failure` | The job failed. Terminal. |
| `canceled` | The job was canceled. Terminal. |
| `skipped` | The job was skipped because a required upstream job did not succeed. |

`outputs` contains any values the job sets with `outputs:` in the workflow file. Any referenced secrets appear as placeholders in the response. Jobs of `type: build` include `buildId`, and jobs of `type: submission` include `submissionId`. Each ID links to the underlying EAS Build or EAS Submit record.

### Example

Save the following as a shell script (or paste into your terminal) to trigger a run and poll until it reaches a terminal status:

```bash
RUN_ID=$(curl -s -X POST "https://api.expo.dev/v2/workflows/dispatch" \
  -H "Authorization: Bearer $EXPO_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"appId":"...","gitRef":"main","fileName":"deploy.yml"}' \
  | jq -r '.data.id')

while true; do
  STATUS=$(curl -s "https://api.expo.dev/v2/workflows/runs/$RUN_ID" \
    -H "Authorization: Bearer $EXPO_TOKEN" \
    | jq -r '.data.status')
  echo "status: $STATUS"
  case "$STATUS" in
    success|failure|canceled) break ;;
  esac
  sleep 10
done
```

### Errors

| Status | Reason |
| --- | --- |
| 400 | `workflowRunId` is not a valid UUID. |
| 403 | The token does not have access to the run's project. |
| 404 | No workflow run exists with that ID. |

## Next step

[Workflow syntax](/eas/workflows/syntax.md) — Reference for the workflow YAML file, including the workflow_dispatch trigger and input schema.
