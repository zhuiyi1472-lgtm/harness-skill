# Harness Skill

[中文](#zh-cn) | [English](#english)

---

<a id="zh-cn"></a>
## 中文

`Harness Skill` 是一个可复用的 Codex Skill，用于设计、评审和加固 AI Agent、Skill 体系与多工具 Harness。

它的核心立场很简单：

> 在高不确定性的 Agent 系统里，边界比路径更重要。

这个 Skill 适合处理以下问题：

- 把开放式 Agent 行为收敛成可复用 Skill
- 评审工具路由与 Toolset 设计
- 区分哪些问题应该做成 Workflow，哪些才是真正的 Agent
- 设计 Memory 策略、事实校验规则和失败退路
- 将 `NousResearch/hermes-agent` 这类项目里的架构模式蒸馏成可迁移的工程规则

### 这个 Skill 解决什么

它不主张把所有任务都写成固定 SOP，而是要求模型先做这些事：

1. 先判任务类型
2. 定义触发条件、排斥条件和成功定义
3. 收缩到最小完备工具集
4. 明确哪些内容必须事实核验
5. 设计失败退路和人工介入点
6. 先做 Eval，再决定是否继续补 Prompt

因此它特别适合：

- `workflow / hybrid / agent` 边界判断
- Skill 设计与重构
- Harness 架构评审
- 多工具系统的稳定性设计

### 仓库结构

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

### 怎么使用

#### 1. 作为本地 Skill 安装

Codex 默认从本地 skills 目录发现 Skill。安装后的目标路径应为：

```text
$CODEX_HOME/skills/harness-skill/
```

如果没有设置 `CODEX_HOME`，则通常放在：

```text
~/.codex/skills/harness-skill/
```

本仓库已经按 Skill 根目录格式整理好，仓库根目录本身就可以直接作为 Skill 文件夹使用。

#### 2. 调用方式

示例：

- `Use $harness-skill to review this agent architecture for tool boundary problems.`
- `Use $harness-skill to turn this prompt-driven agent workflow into a reusable skill.`
- `Use $harness-skill to decide whether this problem should be a workflow, hybrid flow, or true agent.`
- `Use $harness-skill to audit this toolset design for overlap, validation gaps, and failure paths.`

#### 3. 参考文档入口

主入口：

- [`SKILL.md`](./SKILL.md)：主规则与方法论
- [`references/architecture-map.md`](./references/architecture-map.md)：Hermes Agent 子系统映射
- [`references/harness-adaptation.md`](./references/harness-adaptation.md)：可迁移的 Harness 规则
- [`references/eval-samples.md`](./references/eval-samples.md)：Eval 样本与评分维度

中文说明：

- [`references/zh-cn-summary.md`](./references/zh-cn-summary.md)
- [`references/zh-cn-architecture-map.md`](./references/zh-cn-architecture-map.md)
- [`references/zh-cn-eval-samples.md`](./references/zh-cn-eval-samples.md)

### 设计原则

这个 Skill 体现的是一套偏 Harness Engineering 的工程观：

- 确定性优于偶发的高光智能
- 最小惊讶优于“聪明但不可控”
- 失败设计优先于 Happy Path 自由发挥
- 工具重叠本质上是路由缺陷
- 缺失校验通常比缺失知识更危险
- Prompt Patch 应该是最后一步，而不是第一步

### 它来源于什么

这个 Skill 主要蒸馏自公开仓库：

- [NousResearch/hermes-agent](https://github.com/NousResearch/hermes-agent)

但它**不是**在照抄 Hermes 的文件结构，而是在提炼这些可迁移模式：

- 分层 Prompt 装配
- 自注册工具系统
- 可组合 Toolset
- 渐进披露式 Skill
- 长期 Memory 与 Session History 分离
- 冻结式 Memory 快照
- 结构化上下文压缩
- CLI / Gateway / Cron 分入口运行

### 适合 / 不适合

适合：

- 你在设计或评审 AI Harness
- 你要做的是可复用 Skill，而不是一次性回答
- 你处理的是开放式 Agent 问题
- 你更关心稳定性、边界和可解释性

不适合：

- 任务本身就是固定流程
- 你只需要安装教程
- 你只需要命令速查或 API 速查

### 分享说明

这个仓库已经是可直接放到 GitHub 的结构：

- Skill 位于仓库根目录
- references 使用相对路径
- `agents/openai.yaml` 已包含 UI metadata
- 不需要额外构建步骤

如果你要继续发布，可以再补：

- `LICENSE`
- GitHub Topics
- Release 说明

---

<a id="english"></a>
## English

`Harness Skill` is a reusable Codex skill for designing, reviewing, and hardening AI agents, skill systems, and multi-tool harnesses.

Its core position is simple:

> In uncertain agent systems, boundaries are more valuable than paths.

This skill is useful for:

- turning open-ended agent behavior into reusable skills
- reviewing tool routing and toolset design
- separating workflow problems from true agent problems
- designing memory policy, validation rules, and failure handling
- translating architecture patterns from projects like `NousResearch/hermes-agent` into reusable engineering rules

### What This Skill Does

Instead of forcing every problem into a fixed SOP, this skill teaches the model to:

1. classify the task first
2. define triggers, exclusions, and success criteria
3. minimize the exposed tool surface
4. require fact validation where needed
5. design failure routes and human checkpoints
6. evaluate before patching prompts

That makes it especially useful for:

- `workflow / hybrid / agent` boundary decisions
- skill design and refactoring
- harness architecture reviews
- reliability design in multi-tool systems

### Repository Structure

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

### How To Use

#### 1. Install It As A Local Skill

Codex discovers skills from the local skills directory. The installed target should be:

```text
$CODEX_HOME/skills/harness-skill/
```

or, when `CODEX_HOME` is unset:

```text
~/.codex/skills/harness-skill/
```

This repository is already structured so the repository root itself can be used directly as the skill folder.

#### 2. Invoke It

Examples:

- `Use $harness-skill to review this agent architecture for tool boundary problems.`
- `Use $harness-skill to turn this prompt-driven agent workflow into a reusable skill.`
- `Use $harness-skill to decide whether this problem should be a workflow, hybrid flow, or true agent.`
- `Use $harness-skill to audit this toolset design for overlap, validation gaps, and failure paths.`

#### 3. Reference Entry Points

Primary files:

- [`SKILL.md`](./SKILL.md): main operating rules
- [`references/architecture-map.md`](./references/architecture-map.md): Hermes Agent subsystem map
- [`references/harness-adaptation.md`](./references/harness-adaptation.md): reusable harness rules
- [`references/eval-samples.md`](./references/eval-samples.md): evaluation prompts and scoring

Chinese companion references:

- [`references/zh-cn-summary.md`](./references/zh-cn-summary.md)
- [`references/zh-cn-architecture-map.md`](./references/zh-cn-architecture-map.md)
- [`references/zh-cn-eval-samples.md`](./references/zh-cn-eval-samples.md)

### Design Principles

This skill encodes a harness-engineering style approach:

- determinism is more valuable than occasional brilliance
- least surprise beats clever but unstable behavior
- failure design comes before happy-path autonomy
- tool overlap is a routing defect
- missing validation is often more dangerous than missing knowledge
- prompt patching should be the last intervention, not the first

### Derived From Hermes Agent

This skill was distilled primarily from:

- [NousResearch/hermes-agent](https://github.com/NousResearch/hermes-agent)

It does **not** try to clone the repository structure directly.

Instead, it extracts reusable patterns such as:

- layered prompt assembly
- self-registering tools
- composable toolsets
- progressive-disclosure skills
- split long-term memory vs session history
- frozen memory snapshots
- structured context compression
- separate runtime surfaces for CLI, gateway, and cron

### Good Fit / Bad Fit

Use this skill when:

- you are designing or reviewing an AI harness
- you need a reusable skill definition instead of a one-off answer
- you are dealing with open-ended agent behavior
- stability, boundaries, and explainability matter

Do not use this skill when:

- the task is already a straightforward fixed workflow
- you only need installation instructions
- you only need a command or API cheat sheet

### Sharing

This repository is already repo-root ready for GitHub:

- the skill lives at the repository root
- references use relative links
- `agents/openai.yaml` contains UI metadata
- no build step is required

If you want to publish it further, you can still add:

- a `LICENSE`
- GitHub Topics
- release notes
