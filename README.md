# Harness Skill

[中文](#zh-cn) | [English](#english)

---

<a id="zh-cn"></a>
## 中文

`Harness Skill` 是一个可复用的 Codex Skill，用来把开放式 AI Agent 问题收敛成更稳定、可验证、可评审的工程结果。

它的核心原则：

- 确定性优于偶发高光
- 边界优于路径
- 校验优于臆测
- 失败退路优先于乐观提示

### 它适合做什么

- 审查 Agent / Skill / Workflow 的边界是否清晰
- 设计或重构多工具系统的工具职责与路由规则
- 明确哪些信息必须事实校验，哪些只能标记为假设
- 为高风险任务补上失败退路、人工介入点和 Eval 样本
- 把 `NousResearch/hermes-agent` 这类项目的经验蒸馏成可复用方法

### 面向游戏策划的用法

如果你是游戏策划，这个 Skill 适合把 AI 输出约束成可以直接用于策划案、程序协同、数值验证和配表整理的结构化结果。

常见场景：

- 战斗 / SLG 系统设计
- 英雄 / 技能设计
- 数值 / 经济系统验证
- 配表字段设计
- 原型或小工具需求整理

### 中文调用方式

技能名保持为 `$harness-skill`，但需求描述可以直接写中文。

```text
使用 $harness-skill 把这个 SLG 战斗想法收敛成一个有边界的系统设计。
需要：目标、约束、核心循环、战斗规则、极端情况、平衡风险、配表字段、轻量验证方案。
```

```text
使用 $harness-skill 把这个英雄设计整理成可落地的策划实现稿。
需要：定位、触发器、条件、目标、效果、表现备注、配表字段、调参杠杆。
```

```text
使用 $harness-skill 审查这个成长经济系统。
需要：公式、收益曲线、断点区间、防滚雪球设计、极端情况、适合表格落地的字段定义。
```

如果你希望输出更像策划产物，可以追加这些要求：

- `输出为 Markdown，并带表格。`
- `字段定义要适合 CSV / Excel。`
- `区分已验证事实和假设。`
- `包含调参杠杆、极端情况和风险点。`
- `结果要能直接给策划、程序和数值评审使用。`

### 安装与使用

将仓库放到本地 skills 目录即可：

```text
$CODEX_HOME/skills/harness-skill/
```

如果没有设置 `CODEX_HOME`，通常可放在：

```text
~/.codex/skills/harness-skill/
```

主文件：

- [`SKILL.md`](./SKILL.md)
- [`agents/openai.yaml`](./agents/openai.yaml)
- [`references/architecture-map.md`](./references/architecture-map.md)
- [`references/harness-adaptation.md`](./references/harness-adaptation.md)
- [`references/eval-samples.md`](./references/eval-samples.md)

中文参考：

- [`references/zh-cn-summary.md`](./references/zh-cn-summary.md)
- [`references/zh-cn-architecture-map.md`](./references/zh-cn-architecture-map.md)
- [`references/zh-cn-eval-samples.md`](./references/zh-cn-eval-samples.md)

### 来源

本 Skill 主要蒸馏自：

- [NousResearch/hermes-agent](https://github.com/NousResearch/hermes-agent)

但它不是照抄仓库结构，而是提炼这些模式：

- 分层 Prompt 装配
- 自注册工具系统
- 可组合 Toolset
- 渐进披露式 Skill
- 长期 Memory 与 Session History 分离
- 结构化上下文压缩
- CLI / Gateway / Cron 分入口运行

---

<a id="english"></a>
## English

`Harness Skill` is a reusable Codex skill for turning open-ended agent work into stable, reviewable, and testable outputs.

Core principles:

- determinism over peak brilliance
- boundaries over routes
- verification over guesswork
- failure paths before optimistic prompting

### What It Is Good For

- reviewing boundaries between Agent / Skill / Workflow
- designing tool responsibilities and routing rules
- defining what must be verified vs. what must stay labeled as assumption
- adding failure paths, human checkpoints, and eval samples
- distilling reusable rules from projects like `NousResearch/hermes-agent`

### Chinese Invocation Works

The explicit skill name stays `$harness-skill`, but the request body can be fully written in Chinese.

```text
使用 $harness-skill 把这个 SLG 战斗想法收敛成一个有边界的系统设计。
需要：目标、约束、核心循环、战斗规则、极端情况、平衡风险、配表字段、轻量验证方案。
```

### Install

Put this repository at:

```text
$CODEX_HOME/skills/harness-skill/
```

or:

```text
~/.codex/skills/harness-skill/
```

Main files:

- [`SKILL.md`](./SKILL.md)
- [`agents/openai.yaml`](./agents/openai.yaml)
- [`references/architecture-map.md`](./references/architecture-map.md)
- [`references/harness-adaptation.md`](./references/harness-adaptation.md)
- [`references/eval-samples.md`](./references/eval-samples.md)

Source:

- [NousResearch/hermes-agent](https://github.com/NousResearch/hermes-agent)
