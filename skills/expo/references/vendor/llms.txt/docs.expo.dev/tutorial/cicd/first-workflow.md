---
modificationDate: July 08, 2026
title: 'Run your first EAS Workflows job'
description: Learn basics of EAS Workflows by creating workflow files, jobs, triggers, and chain jobs together for an Expo app.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/tutorial/cicd/first-workflow/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/tutorial/cicd/first-workflow/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Learn > CI/CD tutorial
Pages in this section:
- [Introduction](https://docs.expo.dev/tutorial/cicd/introduction.md)
- [First EAS Workflows job](https://docs.expo.dev/tutorial/cicd/first-workflow.md) (this page)
- [Development builds](https://docs.expo.dev/tutorial/cicd/development-builds.md)
- [Preview builds](https://docs.expo.dev/tutorial/cicd/preview-builds.md)
- [E2E tests](https://docs.expo.dev/tutorial/cicd/e2e-tests.md)
- [Production deployments](https://docs.expo.dev/tutorial/cicd/production.md)
- [Tag-based releases](https://docs.expo.dev/tutorial/cicd/tag-based-releases.md)
- [Web deployments](https://docs.expo.dev/tutorial/cicd/web-deployments.md)
- [Next steps](https://docs.expo.dev/tutorial/cicd/next-steps.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Run your first EAS Workflows job

Learn basics of EAS Workflows by creating workflow files, jobs, triggers, and chain jobs together for an Expo app.

In this chapter, we create our first EAS Workflows job.

## Learning outcomes

-   Create and run a custom workflow that prints a message to the EAS dashboard
-   Set up an automatic trigger so every push to `main` runs the workflow
-   Chain multiple jobs together so one job's output feeds the next
-   Read workflow logs and job graphs in the EAS dashboard

## How workflow files work

Workflow files are YAML files stored in our project's **.eas/workflows/** directory. Each file defines the jobs to run and the steps each job follows.

```
A workflow file contains a name, trigger, and jobs. Jobs are either pre-packaged (using type field) or custom (using steps with run commands).
```

A workflow file contains one or more jobs and an optional trigger. When it runs, EAS reads the file, provisions the environment, and executes the jobs in that environment.

## Types of jobs in EAS Workflows

EAS Workflows has two types of jobs:

-   **Pre-packaged job:** This job uses the `type` field. It tells EAS which operation to run, like `build`, `submit`, or `update`. EAS then runs the pre-configured steps for that operation.
-   **Custom job:** This type of job uses `steps` instead of the `type` field. We write our own commands using `run` for each step. A custom job allows us to control what happens when a workflow runs and when to run a specific command.

## Create our first workflow

Let's create our first workflow using a custom job and run it on EAS.

### Create the workflow directory

Create the **.eas/workflows/** directory at the root of our Expo project. EAS looks for workflow files only in this location.

```sh
mkdir -p .eas/workflows
```

### Add a hello.yml file

Inside **.eas/workflows/**, create a file named **hello.yml**. This file defines our first workflow using a single custom job. The job runs a shell command that prints `"Hello, Workflows"`:

```yaml
name: Hello

jobs:
  greet:
    steps:
      - run: echo "Hello, Workflows" # Any shell command works here
```

The above workflow uses the following syntax:

-   `name` is the workflow's name as shown on the EAS dashboard.
-   `jobs` contains one or more jobs to run. Each job has a unique ID (in this case, `greet`).
-   `steps` is a sequence of tasks the job executes in order. A job with `steps` but no `type` field is a custom job.
-   `run` is the shell command to execute in a step. Here, it runs `echo` to print a message. Any shell command works.

### Manually run the workflow

To run the workflow on the EAS dashboard, run the following command from a terminal window:

```sh
eas workflow:run .eas/workflows/hello.yml
```

The `eas workflow:run` command uploads the workflow file to EAS. It then runs any job defined inside the file and the EAS CLI prints a URL to the EAS dashboard where we can watch the job.

### View the job logs

Open the URL from the previous step in a browser window. On the workflow page, we can see the `greet` job and its status. Once the job completes, expand the step output to see the logged message: `Hello, Workflows`.

The EAS dashboard is where we view and debug any workflow job. The **Workflow graph** shows what triggered our job and the job's unique ID.

Switch to the **Workflow file** tab. This is the workflow file we wrote in Step 2, preserved with the run.

> **Tip:** View all of our project's workflows and filter them by status or type on the [Workflows page](https://expo.dev/accounts/%5Baccount%5D/projects/%5Bproject%5D/workflows).

## Automatically run a workflow

Running `eas workflow:run` from the terminal works for a one-off, but most teams want workflows that fire automatically on GitHub events. Automatic triggers mean no one has to remember to run a workflow. The workflow's configuration lives in the repository alongside the code it builds.

> **Note:** A trigger is a rule that tells a CI/CD pipeline when to start. It defines the event (such as a code push or a pull request) that causes the pipeline to run automatically instead of requiring manual execution.

We define a trigger with the `on` key in the workflow file. EAS watches for the matching GitHub event and runs the workflow automatically.

### Add an automatic trigger

Let's add an `on.push.branches` trigger to our existing workflow file.

-   The `on` key defines which GitHub event triggers the workflow
-   The `push` field defines when to trigger
-   The `branches` field allows us to specify that whenever the code is pushed to the `main` branch, the workflow should run automatically

```yaml
name: Hello

on:
  push:
    branches: ['main'] # Fires on every push to main branch

jobs:
  greet:
    steps:
      - run: echo "Hello, Workflows"
```

The `branches` field can include any branch name that exists in a GitHub repository. We can specify multiple branches such as `['main', 'develop', 'staging']` to trigger the workflow when we push to any of those branches.

### Commit the change and push to GitHub

Commit the workflow file and push it to the `main` branch:

```sh
git add .eas/workflows/hello.yml
git commit -m "Add hello workflow"
git push origin main
```

### Verify the automatic trigger

> **Tip:** This step depends on our local Expo project being pushed to GitHub and the GitHub repository being connected to EAS. To confirm: run `git remote -v` to verify the local repo points to a GitHub URL and open the project's [GitHub settings on EAS](https://expo.dev/accounts/%5Baccount%5D/projects/%5BprojectName%5D/github) to check that a repository is connected.

Let's open our project's [Workflows page](https://expo.dev/accounts/%5Baccount%5D/projects/%5Bproject%5D/workflows) on the EAS dashboard to see the new job.

In the EAS dashboard, notice that the **Workflow graph** tab shows the commit message and branch reference for this push. In the status under the workflow header, the **Trigger** and **Triggered by** reflect the same information.

## Other trigger types

Other trigger types we use later in this tutorial:

-   `on.pull_request`: Runs when a pull request opens or updates against a matching branch
-   `on.push` with `tags`: Runs when a matching tag pattern is pushed to the GitHub repository
-   `on.pull_request_labeled`: Runs when a specific label gets added to a pull request

## Chain jobs together

So far, we have implemented a single job in our workflow file. A workflow file can have multiple jobs. A job's input can depend on the output of the previous job. Let's chain multiple jobs together in a workflow file.

### Update hello.yml

Let's update the **hello.yml** file to add a custom job and a pre-packaged job. This pre-packaged job is called [`doc`](/eas/workflows/pre-packaged-jobs.md#doc) and it renders Markdown on the EAS dashboard:

```yaml
name: Hello

jobs:
  greet:
    outputs:
      greeting: ${{ steps.set_greeting.outputs.greeting }} # Expose the step output at the job level
    steps:
      - id: set_greeting
        run: set-output greeting "Hello from EAS Workflows"
  show_info:
    needs: [greet] # Wait for greet to finish, then read its outputs
    type: doc # Pre-packaged job that renders Markdown on the dashboard
    params:
      md: |
        # Workflow Output
        **${{ needs.greet.outputs.greeting }}**
```

The above workflow file does the following:

-   Defines a `greet` job, which is a custom job. It uses the [`set-output`](/eas/workflows/syntax.md#set-output) shell function to set a value called `greeting`. The `outputs` field at the job level exposes the step output so other jobs can reference it.
-   Defines a `show_info` job which uses `needs: [greet]` to wait for the `greet` job to complete and access its output. Then, it uses the pre-packaged `doc` job type to render a Markdown document in the EAS dashboard. The Markdown content includes the value of `greeting` set in the previous job.

> **Tip:** In EAS Workflows, jobs run in parallel by default. The `needs` field is what makes a job wait: it lists the job IDs that must complete successfully before the job using `needs` can start. If a job in the `needs` list fails, the dependent job is skipped.

### Run the chained workflow

Run the following command from a terminal window to see the chained jobs in action:

```sh
eas workflow:run .eas/workflows/hello.yml
```

Open the EAS dashboard link from the EAS CLI. Under **Logs**, notice that the `greet` job runs first. Once it completes, the `show_info` job runs and renders the Markdown content using the output from the previous job.

## Clean up

After this chapter, delete the **hello.yml** file because we do not use it in the next chapters. If we prefer to keep it, remove the `on.push` trigger so it does not fire on every push to `main`.

## Summary

Chapter 1: First EAS Workflows job

We created a custom job workflow, triggered it manually and automatically, chained jobs using needs and outputs, and used the EAS dashboard to verify job execution.

In the next chapter, learn how to automate development builds with fingerprinting to skip unnecessary rebuilds.

[Next: Chapter 2: Development builds](/tutorial/cicd/development-builds.md)
