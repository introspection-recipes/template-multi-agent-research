# Multi-agent research

A starting point for your research agent, inspired by Anthropic's orchestrator-worker multi-agent architecture.

## Quickstart

Install Pi and the Recipes extension once per machine:

```bash
pi install npm:@introspection-ai/recipes
```

Then clone this template and run it:

```bash
git clone https://github.com/introspection-recipes/pi-multi-agent-research
pi --recipe ./pi-multi-agent-research
```

## Make it yours

This is a directory, so change it like any other source. Nothing here is
generated and nothing is hidden.

| Path | What it is |
| --- | --- |
| `SYSTEM.md` | instructions every agent in the package starts from |
| `agents/agent.yaml` | the lead researcher, and the agent you run |
| `agents/research.yaml` | parallel research subagents |
| `agents/citations.yaml` | attaches sources before the answer returns |
| `skills/research-process/` | the research method, as a skill |

Check your changes before you commit them:

```bash
introspection check
```

## Docs

The format, the agent file, MCP policy and judges are documented at
[pi.recipes/docs](https://pi.recipes/docs).
