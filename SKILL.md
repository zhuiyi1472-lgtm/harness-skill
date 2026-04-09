---
name: harness-skill
description: Use when designing, auditing, or refining an AI agent, skill system, or multi-tool harness where reliability, boundary clarity, tool routing, memory policy, scheduling, and failure handling matter more than raw autonomy. Also applies to requests about Agent/Skill/Workflow 边界、工具路由、事实校验、失败退路与 Eval 设计.
---

# Harness Skill

## Overview

Treat agent work as harness engineering.

Do not optimize for the smartest possible route. Optimize for stable, explainable behavior under uncertainty.

Use these principles:

- Determinism over peak intelligence
- Least surprise over cleverness
- Failure-first design over optimistic prompting

Chinese companion references are available for sharing and onboarding:

- [zh-cn-summary.md](references/zh-cn-summary.md)
- [zh-cn-architecture-map.md](references/zh-cn-architecture-map.md)
- [zh-cn-eval-samples.md](references/zh-cn-eval-samples.md)

## Classify The Task First

Classify the request before proposing architecture or writing a skill:

- `workflow`: fixed input, fixed steps, fixed output
- `hybrid`: mostly stable flow with a few judgment nodes
- `agent`: unknown path, multi-intent, incomplete information, or competing goals

Default rule:

- If `workflow`, prefer scripts, forms, validators, or strict SOPs
- If `hybrid`, keep the workflow fixed and isolate only the uncertain nodes
- If `agent`, define boundaries and evidence rules instead of prescribing a route

## Output Contract

Unless the user asked for another format, structure the answer as:

- Goal
- Constraints
- Boundary definition
- Minimal toolset
- Fact validation rules
- Failure paths
- Human checkpoints
- Eval samples

For implementation-oriented requests, also include:

- Key modules
- State model
- Entry points
- Verification plan

## Five-Layer Skill Design

### 1. Define Boundaries, Not Routes

Specify:

- Trigger conditions
- Exclusion conditions
- Success definition

Avoid step-by-step paths unless the task is actually deterministic.

### 2. Minimize The Tool Surface

Give the model the smallest complete toolset that can finish the task.

For each tool, define:

- Primary responsibility
- Inputs it is trusted to handle
- Adjacent tasks it must not absorb

Tool overlap is a routing bug.

### 3. Replace Knowledge With Verification

Do not ask the model to "know more" when the real issue is missing verification.

Define:

- What must be checked against source of truth
- What can be inferred from stable architecture
- What must be labeled as assumption

### 4. Prevent Prompt Corruption

Do not keep adding case-by-case patches to a weak boundary.

When behavior drifts:

- Find the boundary failure
- Refactor the skill or harness rule
- Remove redundant prompt patches

### 5. Eval Before Patching

Every important skill needs explicit pressure samples.

Test for:

- Wrong tool choice
- Wrong parameter choice
- Missing evidence
- Unsafe continuation under uncertainty
- Failure to stop at human checkpoint

Read [eval-samples.md](references/eval-samples.md) when preparing evaluation prompts.

## Hermes-Derived Architecture Checklist

Use Hermes Agent as a reference architecture, not as a blueprint to copy blindly.

Check whether the target system has clear answers for these:

- How is the final system prompt assembled from identity, platform, context, skills, memory, and runtime rules?
- Is tool registration centralized, or does the model reason over an ambiguous flat list?
- Are toolsets grouped by scenario or platform?
- Are long-lived memories separated from session transcripts?
- Does the memory system preserve cache stability by freezing prompt snapshots during a session?
- Is there an explicit strategy for context compression and handoff summaries?
- Are gateway, API, CLI, and scheduled execution treated as different entry points on top of one agent core?
- Are timeout, injection, and delivery failures handled as first-class cases?

Read:

- [architecture-map.md](references/architecture-map.md) when you need the Hermes subsystem map
- [harness-adaptation.md](references/harness-adaptation.md) when you need design rules distilled from Hermes patterns

## Fact Validation Rules

When analyzing a live repository or runtime:

- Verify file names, module locations, tool names, and behavior from source
- Separate stable abstractions from branch-specific implementation details
- Mark anything not directly evidenced as an inference

Use source-first language:

- `Verified`: directly supported by code or docs
- `Inferred`: strong architectural conclusion from multiple verified pieces
- `Unknown`: not safely established

## Failure Routes

When the evidence is weak or the design is unstable:

- If tool boundaries are unclear, reduce the toolset before tuning prompts
- If the model keeps hallucinating, add verification edges before adding instructions
- If a skill keeps accreting exceptions, rewrite the trigger and exclusion logic
- If a task can destroy data, require a human checkpoint
- If a request is underspecified, deliver a workable first version with explicit assumptions

## Human Checkpoints

Require human review when any of these are true:

- The action is irreversible
- The design changes responsibility boundaries between systems or teams
- The evidence chain is incomplete but the cost of error is high
- The model is choosing between materially different business outcomes

## Deliverable Shapes

This skill is most useful for producing:

- Agent architecture reviews
- Skill definitions
- Harness design notes
- Boundary refactors
- Toolset redesigns
- Eval plans

Keep the result compact, operational, and reusable.
