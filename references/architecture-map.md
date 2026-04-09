# Hermes Agent Architecture Map

This reference distills the public `NousResearch/hermes-agent` repository into stable subsystems.

Use this file when the user asks for architecture explanation, module mapping, or Hermes-to-harness translation.

## Core Subsystems

| Subsystem | Verified Repo Evidence | Role In Hermes | Harness Takeaway |
|---|---|---|---|
| Prompt assembly | `agent/prompt_builder.py` | Builds the effective system prompt from identity, platform hints, project context, skill index, memory guidance, and tool-use rules | Treat prompt assembly as an explicit layer, not as one giant hidden string |
| Tool registry | `tools/registry.py` | Tools self-register with schema, handler, toolset, availability checks, and result limits | Centralize tool metadata so the model reasons over a coherent capability graph |
| Toolset composition | `toolsets.py` | Groups tools by scenario and platform, supports composition and resolution | Expose fewer tools per context; tool grouping is a routing control surface |
| Skills system | `tools/skills_tool.py` | Lists skills by metadata, loads full `SKILL.md` only on demand, supports linked references/scripts/assets | Use progressive disclosure; do not inject heavy instructions until the skill is selected |
| Memory system | `tools/memory_tool.py` | Separates `MEMORY.md` from `USER.md`, persists both, freezes prompt snapshot for session stability | Long-lived facts and user profile should not be mixed with task transcript state |
| Context compression | `agent/context_compressor.py` | Prunes old tool outputs, protects head/tail context, inserts structured summaries | Compression should preserve handoff quality, not just reduce tokens |
| Gateway runtime | `gateway/run.py` | Adapts one agent core to CLI, messaging, webhook, and other session surfaces | Entry points should differ by delivery and session behavior, not by core reasoning logic |
| Cron execution | `cron/scheduler.py` | Runs due jobs with locking, optional skill loading, auto-delivery, and silent mode | Scheduled tasks are another agent entry point with distinct constraints |

## Stable Architecture Pattern

Hermes is best understood as:

1. One core agent runtime
2. One capability registry
3. Several entry surfaces
4. Several persistence layers with different lifetimes
5. Several reliability controls

That pattern is more important than any individual file name.

## Reliability Controls Observed In Hermes

| Control | Verified Evidence | Why It Matters |
|---|---|---|
| Context file scanning for prompt injection | `agent/prompt_builder.py` | Project context is treated as untrusted input |
| Memory content scanning | `tools/memory_tool.py` | Persistent state is also treated as an injection surface |
| Inactivity timeout and interrupt handling | `gateway/run.py`, `cron/scheduler.py` | The system assumes tools or providers can hang |
| Locking around cron ticks | `cron/scheduler.py` | Background automation must resist duplicate execution |
| Disabled-skill and platform checks | `tools/skills_tool.py`, `agent/prompt_builder.py` | Skill availability is filtered by runtime context |
| Prompt-cache-friendly frozen memory snapshot | `tools/memory_tool.py` | Stability and cost are engineered together |

## What To Reuse In Other Harnesses

- Separate stable architecture layers from volatile prompts
- Make tool selection easier by shrinking the visible tool surface
- Keep durable memory small and factual
- Use session history for narrative recall, not for long-term memory
- Add explicit failure behavior for timeout, injection, and delivery errors

## What Not To Cargo-Cult

- Exact file layout
- Exact tool names
- Exact platform adapters
- Exact prompt text

Copy the engineering pattern, not the repository shape.
