# Multi-Agent Research Template

A Pi recipe inspired by Anthropic's orchestrator-worker research system: a lead researcher delegates independent searches, synthesizes the evidence, and runs a citation pass.

## Quick start

Install Node.js 24 or newer and the Introspection CLI, then create your own recipe from this template. `init` also installs the compatible Pi harness and Recipes extension.

```bash
npm install -g @introspection-ai/cli
introspection init multi-agent-research-agent template-multi-agent-research
cd multi-agent-research-agent
```

Web search is required. Get a key from [Parallel](https://parallel.ai) and export it for the local Pi process:

```bash
export PARALLEL_API_KEY=your-key
```

Validate the recipe and start a fresh local Pi session:

```bash
introspection local --runtime multi-agent-research-agent
```

Ask:

> Compare the official release notes for the latest stable versions of Node.js and Python. Cite the primary sources.

Confirm that the lead delegates the research, the workers return sourced findings, and the final answer contains working citations. The local loop needs no Introspection login or cloud runtime. Pi may ask you to configure the Anthropic model provider on the first run.

## Move the recipe through its lifecycle

The everyday flow is:

```text
Local Pi → Development → Staging → Production → Learn and repeat
```

### 1. Change and prove it locally

Customize `SYSTEM.md`, the definitions under `agents/`, the web and orchestration tools under `extensions/`, or `skills/research-process/SKILL.md`. Repeat the comparison prompt above in fresh sessions. Check simple fact finding as well as multi-part research, and verify both source quality and citation coverage.

### 2. Create the runtime and test development

Development requires the recipe's first runtime. Commit the locally proven recipe, push it to your own GitHub repository, then:

1. In the Introspection app, open your organization's **Integrations** page and grant the Introspection GitHub App access to the repository.
2. Open the target project, go to **Runtimes**, and select **New runtime**.
3. Choose the repository and the runtime in `.introspection/multi-agent-research-agent.yaml`, confirm that its recipe path is `.`, and create the first version from `main`.
4. Configure the required Development `PARALLEL_API_KEY` variable described below. The research system has no web source without it.
5. In **Versions**, confirm that the immutable version's recipe commit matches the `main` commit you intended to deploy.

#### Connect the Development Parallel binding

Create the Development-scoped variable, then connect the local recipe:

```bash
introspection login
export PARALLEL_API_KEY=your-key
introspection bindings variables create \
  --name PARALLEL_API_KEY \
  --from-env PARALLEL_API_KEY \
  --runtime-group multi-agent-research-agent \
  --environment development \
  --query name
introspection dev --runtime multi-agent-research-agent
```

Use a dedicated, revocable Parallel key because variable bindings are readable inside the task sandbox. See the [bindings documentation](https://docs.introspection.dev/platform/bindings) for other environments and scoping options.

Leave the command running, open the development chat URL it prints, and repeat the release-notes comparison. Saved recipe changes are picked up without a commit or push. The development task resolves the Development-scoped `PARALLEL_API_KEY` variable you created above; `introspection dev` does not read or upload the key exported for a local Pi session. Stopping the command removes the local overlay and does not deploy a version.

### 3. Verify a pull-request candidate in staging

Push a feature branch and open a pull request. The GitHub integration creates an immutable candidate version for that commit; do not create another runtime for the same agent.

In the runtime's **Versions** view, find the candidate by branch and commit, pin it to **Staging**, and select **Preview**. Repeat the release-notes comparison. Confirm that Parallel search works, the lead delegates, the final answer cites primary sources, and the conversation used the intended candidate commit without an unhandled tool or binding error.

### 4. Merge it to production

After the staging behavior and pull request are approved, merge into the repository's configured production branch. The GitHub integration creates the immutable version and activates it for production; there is no separate promotion command.

Run one small production research check through the stable `multi-agent-research-agent` runtime and confirm that it resolves to the merged recipe commit.

### 5. Learn and repeat

Use production conversations and recurring patterns—especially unsupported claims, weak sources, and missed delegation—to choose the smallest useful change, then return to the local loop. See the [agent development lifecycle](https://docs.introspection.dev/guides/development-lifecycle) for the full workflow.

## What's included

- A lead researcher that plans, delegates, and synthesizes
- Parallel research subagents
- A final citation pass
- Runtime metadata in `.introspection/multi-agent-research-agent.yaml`

[Read the Recipes documentation →](https://docs.introspection.dev/recipes)
