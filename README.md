# Harness Skill

`Harness Skill` is a reusable Codex skill for designing, reviewing, and hardening AI agents, skill systems, and multi-tool harnesses.

It is built around one idea:

> In uncertain agent systems, boundaries are more valuable than paths.

This skill is intended for work such as:

- turning open-ended agent behavior into bounded skills
- reviewing tool routing and toolset design
- separating workflow problems from true agent problems
- defining memory policy, validation rules, and failure handling
- translating architecture patterns from projects like `NousResearch/hermes-agent` into reusable engineering rules

## What This Skill Does

Instead of giving the model a fixed SOP for every problem, this skill teaches it to:

1. classify the task first
2. define triggers, exclusions, and success criteria
3. minimize the exposed tool surface
4. require fact validation where it matters
5. design failure routes and human checkpoints
6. evaluate the skill or harness before patching prompts

This makes it useful for `workflow / hybrid / agent` boundary decisions, skill design, harness audits, and reliability-focused architecture work.

## Repository Structure

```text
harness-skill/
|-- README.md
|-- SKILL.md
|-- agents/
|   `-- openai.yaml
`-- references/
    |-- architecture-map.md
    |-- harness-adaptation.md
    |-- eval-samples.md
    |-- zh-cn-summary.md
    |-- zh-cn-architecture-map.md
    `-- zh-cn-eval-samples.md
```

## How To Use

### 1. Install As A Local Skill

Codex discovers skills from the local skills directory. The final installed layout should be:

```text
$CODEX_HOME/skills/harness-skill/
```

or, when `CODEX_HOME` is unset:

```text
~/.codex/skills/harness-skill/
```

This repository is already structured so that the repository root can be used directly as the skill folder.

### 2. Invoke It

Example prompts:

- `Use $harness-skill to review this agent architecture for tool boundary problems.`
- `Use $harness-skill to turn this prompt-driven agent workflow into a reusable skill.`
- `Use $harness-skill to decide whether this problem should be a workflow, hybrid flow, or true agent.`
- `Use $harness-skill to audit this toolset design for overlap, validation gaps, and failure paths.`

### 3. Use The References

Start with these files depending on the task:

- [`SKILL.md`](./SKILL.md): main operating rules
- [`references/architecture-map.md`](./references/architecture-map.md): Hermes Agent subsystem map
- [`references/harness-adaptation.md`](./references/harness-adaptation.md): reusable harness rules
- [`references/eval-samples.md`](./references/eval-samples.md): evaluation prompts and scoring

Chinese companion references:

- [`references/zh-cn-summary.md`](./references/zh-cn-summary.md)
- [`references/zh-cn-architecture-map.md`](./references/zh-cn-architecture-map.md)
- [`references/zh-cn-eval-samples.md`](./references/zh-cn-eval-samples.md)

## Design Principles

This skill encodes the following engineering stance:

- Determinism is more valuable than occasional brilliance.
- Least surprise beats clever but unstable behavior.
- Failure handling should be designed before happy-path autonomy.
- Tool overlap is a routing bug.
- Missing validation is often a bigger problem than missing knowledge.
- Prompt patching should be the last intervention, not the first.

## Derived From Hermes Agent

This skill was distilled from the public repository:

- [NousResearch/hermes-agent](https://github.com/NousResearch/hermes-agent)

It does **not** try to clone the repository structure blindly.

Instead, it extracts the reusable architecture patterns:

- layered prompt assembly
- self-registering tools
- composable toolsets
- progressive-disclosure skills
- split long-term memory vs session history
- frozen memory snapshots for session stability
- context compression with structured handoff
- separate runtime surfaces for CLI, gateway, and cron

## Good Fit / Bad Fit

Use this skill when:

- you are designing or reviewing an AI harness
- you need a reusable skill definition, not a one-off answer
- you are dealing with open-ended agent behavior
- reliability and explainability matter

Do not use this skill when:

- the task is a straightforward fixed workflow
- you only need installation instructions for a specific tool
- you only need a command reference or API cheat sheet

## Sharing

This repository is repo-root ready for GitHub:

- the skill lives at the root
- references are linked with relative paths
- UI metadata is included in `agents/openai.yaml`
- no extra build step is required

If you want, you can add a license and initialize Git in this directory, then push it directly.
