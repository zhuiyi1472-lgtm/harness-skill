# Hermes Agent 架构映射

这个文档把 `NousResearch/hermes-agent` 的关键结构翻译成适合中文团队讨论的架构语言。

## 一、整体分层

Hermes 更适合被理解为五层系统，而不是一个“大 Prompt Agent”：

| 层 | 关键模块 | 作用 |
|---|---|---|
| Prompt Layer | `agent/prompt_builder.py` | 组装系统提示，包括身份、平台、项目上下文、skills 索引、记忆规则、执行规则 |
| Capability Layer | `tools/registry.py`、`toolsets.py` | 注册工具、定义 toolset、按入口裁剪能力 |
| State Layer | `tools/skills_tool.py`、`tools/memory_tool.py`、session history | 管理 skills、memory、会话记录等不同寿命的状态 |
| Runtime Layer | `gateway/run.py` | 把同一 agent core 接到 CLI、消息平台、API 等不同入口 |
| Automation Layer | `cron/scheduler.py` | 定时任务、自动投递、静默输出、重复触发保护 |

## 二、最值得借鉴的点

### 1. Prompt 不是一段文本，而是一个装配过程

Hermes 把这些东西显式分开：

- identity
- platform hints
- project context
- skill index
- memory guidance
- tool-use enforcement

这意味着：

- 更容易替换某一层
- 更容易定位行为问题
- 更容易做入口差异化

### 2. Tool 不靠 Prompt 描述，而靠注册表管理

`tools/registry.py` 的价值不只是“存工具”，而是把这些东西放在一个统一对象里：

- name
- schema
- handler
- toolset
- availability check
- result size policy

这能减少“Prompt 说有这个工具，但运行时其实没有”的漂移。

### 3. Toolset 是控制面，不是装饰层

Hermes 不是默认把所有工具都给模型，而是按场景分组。

这背后的工程价值：

- 缩小选择空间
- 降低误选率
- 让不同入口共享 core，但有不同能力边界

### 4. Skill 是渐进披露，不是全文常驻

Hermes 的技能体系先给 metadata，再按需加载 `SKILL.md` 和 linked files。

这比“每轮对话都塞完整文档”更适合生产环境，因为：

- 节省上下文
- 减少噪声
- 降低旧说明污染当前任务的概率

### 5. Memory 和 Session 是两回事

Hermes 把这两类信息分开：

- `memory`: 稳定事实、用户偏好、工具习惯
- `session history`: 某次任务过程中的上下文与记录

这是非常关键的分层。否则系统会把“长期知识”和“短期任务痕迹”混在一起，越跑越脏。

### 6. Memory 快照冻结是很强的 Harness 手法

`memory_tool.py` 里最值得借的是：

- 写入 memory 会持久化到磁盘
- 但当前 session 的 system prompt 不会立刻改变

这能带来两个好处：

- 保持行为稳定
- 保持 prefix cache 稳定

### 7. Cron 是独立入口，不是对话的附属品

`cron/scheduler.py` 说明 Hermes 把自动化执行当成独立 runtime surface。

因此它额外处理：

- 调度锁
- 自动投递
- silent 输出
- 失败记录
- skill 注入

这比“拿一个普通 prompt 去定时跑”要成熟得多。

## 三、Hermes 的失败设计

Hermes 里已经能看到比较明显的 failure-first 思路：

| 风险 | 对应措施 |
|---|---|
| 项目上下文可能带 prompt 注入 | 扫描 context files |
| 持久 memory 可能被污染 | 扫描 memory content |
| API / tool 可能卡死 | inactivity timeout + interrupt |
| 长上下文可能爆炸 | context compression + structured summary |
| 定时任务可能重复执行 | file lock |
| skill 可能不适配当前平台 | 平台过滤、禁用过滤、setup 状态 |

## 四、迁移到自己的 Harness 时怎么用

推荐迁移顺序：

1. 先抽层，不先抄代码
2. 先定边界，不先补 prompt
3. 先减工具，不先加工具
4. 先补验证，不先补知识
5. 先做失败路径，不先做 happy path

## 五、不要照抄的部分

不要机械复制这些东西：

- 文件结构
- 工具名字
- 平台适配器实现
- 具体 prompt 文案

应该复制的是工程模式：

- 分层
- 注册
- 渐进披露
- 状态分寿命
- 失败优先
