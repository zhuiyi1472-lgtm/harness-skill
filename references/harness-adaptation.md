# Harness Adaptation Guide

Use this file when turning Hermes-inspired observations into reusable harness or skill design rules.

## Pattern Translation

| Hermes Pattern | Reusable Rule | Common Anti-Pattern |
|---|---|---|
| Prompt assembly is layered | Define prompt layers with clear ownership | Hide everything in one untraceable system prompt |
| Tools self-register into one registry | Keep one authoritative capability map | Maintain separate prompt docs, schemas, and handlers that drift apart |
| Toolsets are scenario-specific | Minimize exposed tools per task surface | Give the model all tools and hope instructions compensate |
| Skills use progressive disclosure | Keep metadata small, load detail on selection | Inject full manuals into every turn |
| Memory is split from session history | Separate durable facts from conversation recall | Store everything in one memory bucket |
| Memory snapshot is frozen per session | Preserve cache stability and behavioral consistency | Mutate system prompt state every turn |
| Context compression preserves structure | Summarize work state, decisions, files, and next steps | Drop history naively and lose task continuity |
| Gateway and cron are distinct entry points | Model runtime behavior by surface | Assume CLI, API, and automation need identical policies |

## Skill Design Rules

When converting open agent behavior into a skill:

1. Start with task type.
   - `workflow`: codify route
   - `hybrid`: route only the stable path
   - `agent`: codify boundaries, evidence, and stop conditions

2. Write trigger and exclusion conditions before workflow text.

3. Shrink the toolset before tuning prompts.

4. State what must be verified from live source.

5. Define how failure is reported.

6. Define where the human must review.

## Minimal Toolset Heuristic

For every tool under consideration, ask:

- Is it necessary to complete the task?
- Is it distinguishable from the neighboring tools?
- Would removing it make model choice easier?

If the answer to the third question is `yes`, remove it.

## Memory Policy Heuristic

Store only facts that:

- survive beyond the current task
- reduce future correction cost
- are small enough to remain injected safely

Do not store:

- temporary progress
- task logs
- large raw outputs
- unstable assumptions

## Failure-First Questions

Ask these before finalizing a harness or skill:

- What if the source of truth is unavailable?
- What if two tools can both plausibly answer?
- What if a context file is hostile?
- What if a background task fires twice?
- What if the model confidently continues with weak evidence?

If there is no answer, the harness is incomplete.

## Review Template

Use this structure when reviewing a harness or skill:

- Goal
- Task type
- Boundary definition
- Tool surface
- Validation path
- State model
- Failure controls
- Human checkpoints
- Eval readiness

## Strong Default Recommendation

Prefer this order of intervention:

1. fix task classification
2. fix boundary definition
3. fix tool surface
4. fix validation edges
5. fix failure routing
6. patch prompts only after the above are correct

Prompt patches are the last mile, not the foundation.
