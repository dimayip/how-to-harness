---
name: how-to-harness
description: >
  A domain-specific facilitation framework for designing Harness-style closed-loop, self-optimizing, human-steer+agent-execute systems — Ralph loops, LLM-as-judge harnesses, evaluation-driven improvement pipelines, tiered-automation governance, gatekeeper workflows, AI agent autonomy systems, or any architecture where agents execute and humans steer through gates. Use this skill whenever the user says things like "帮我设计一个 AI 闭环 / 自优化系统"、"Ralph loop"、"Harness Engineering"、"评测驱动的改进回路"、"LLM-as-judge 体系"、"闸门分级 / 熔断 / 升级路径"、"自治治理 Agent系统"、"设计自执行但人类掌舵的系统", or when the topic explicitly involves agent autonomy, evaluation harnesses, steering/gating mechanisms, idempotent/resumable loops, tiered automation with escalation, or closed-loop improvement. This skill is fully self-contained: it internalizes Socratic brainstorming discipline + mainstream design-document schemas + Harness methodology hard constraints (Eval-first, Gates-not-code, Idempotent & resumable loops, Small reversible steps, Automation tiers with escalation, Asset versioning, Human time budget as first-class constraint) as non-negotiable checks on every decision. The final deliverable is tailored to the user's real need (PRD / Design Doc / RFC / Kickoff checklist / ADR / One-Pager), not a fixed format.
---

# How to Harness · 闭环 / 自优化系统的设计引导框架

> **核心思想**："Humans steer, agents execute." 在闭环 / 自优化类系统里，仅有 Socratic 提问纪律还不够——用户会把决策锁死在**违反 Harness 方法论的位置**。本 skill 在常规的设计引导之上额外叠加一层**硬性领域约束**，确保所有决策都对齐 Harness Engineering 的核心原则。

本 skill 来源于 OpenAI 的 [Harness Engineering](https://openai.com/index/harness-engineering/) 方法论。

**本 skill 完全自给自足**：内化了 Socratic brainstorming 纪律 + 主流设计文档的 schema 知识 + Harness 方法论的全部硬约束，不依赖其他 skill。

---

## 何时使用（When to trigger）

仅当用户的系统有以下**核心特征之一**时才使用本 skill：

- 系统存在**闭环 / 自优化**结构（Agent loop / Ralph loop / CI 式评测循环 / 周期性自改进）
- 系统存在明确的**人机协作闸门**（分级自动化 L1/L2/L3、谁审批什么、什么情况升级到人）
- 系统以**评测驱动**（Gold Set、回归测试、LLM-as-judge、硬门禁 / 软评估）
- 用户提到 Harness Engineering / Ralph loop / LLM-as-judge / gatekeeper / 闭环 / 自治 / agent autonomy / 熔断升级路径

**不适用场景**：
- 系统不涉及自改进循环（比如普通产品功能 / 内部工具 / 平台改造 / API / 数据模型 / 工作流 / 一次性的对话 / 问答功能）
- 已经想清楚的纯 CRUD 需求文档

**判定钥匙**：问一个关键问题——"这个系统会不会自己执行自己改进自己？"如果答案不是"会"，本 skill 就不适用。

---

## 最终产物不是"PRD"一种（What to deliver）

本 skill 的输出**按用户真实需求选择**，不是固定格式。常见 5 类：

| 场景 | 建议产物 | 主要消费者 |
|------|---------|-----------|
| 从 0 到 1 设计新系统（工程为主） | **设计文档（Design Doc）+ Kickoff 清单** | 开发者 + AI coding agent |
| 产品立项 / 对上汇报 | **PRD + One-Pager** | 产品评审 + 管理层 |
| 内部技术改造 / 跨团队协作 | **RFC / 技术方案** | 工程团队 + 架构评审 |
| 需求仍在探索 / 决策未完全锁定 | **决策记录（ADR）+ 待办问题清单** | 自己 + 后续继续推进 |
| 已有方案需落地 | **里程碑计划 + 分工表** | 执行团队 |

选择建议：**Capture Context 阶段主动问用户"这份材料最终给谁看 / 用来做什么"**，再决定产物形态。详见 `references/deliverables.md`。

多种产物可以**并行交付**（比如同时给 PRD + One-Pager + Kickoff 清单三件套），适用于多利益相关方的场景。

---

## 核心方法论：5 层叠加框架

```
Layer 0: Harness Principles（领域硬约束，不可协商）
   ↓
Layer 1: 用户提供的约束（方法论锚点 + 业务现状）
   ↓
Layer 2: Brainstorming 纪律（已内化）
   ↓
Layer 3: Socratic 追问（A/B/C/D 选项 + 依赖拓扑 + **4 项**一致性校验）
   ↓
Layer 4: 产物组织（按消费者选 schema，详见 references/deliverables.md）
```

**Layer 0 是本 skill 最核心的领域层**：它让每一轮 Socratic 追问都以 Harness 原则为尺子，而不仅仅是"问得清楚"。

每一层都不能跳过，但强度可以按任务复杂度调节。

---

## Layer 0：Harness Principles（领域硬约束）

这是本 skill 的**核心领域层**。以下 7 条原则在每一轮 Socratic 追问时都被强制检查，任何决策若违反这些原则都要被指出（用户可以选择接受违反，但必须是知情违反）。

### HP-1 · Eval is foundation（评测是地基，不是可选项）
任何改进循环**先建评测集（Gold Set）**。没有评测的循环不能自动改。
- 不要问用户"要不要有 Gold Set"——直接问"Gold Set 从哪来"（人工标注 / 规则自动提名 / 历史高质量样本）
- 如果用户说"我们现在没有，先上线再说"→ 拦住，先解决冷启动评测集来源。

### HP-2 · Humans steer via gates, not via code（人类用闸门掌舵，不是亲自改代码）
人类的角色是**定义闸门 / 审批闸门 / 调整闸门**，不是亲自下场改代码 / Prompt / 数据。
- 闸门分级必须明确：L1 自动 / L2 Owner 审 / L3 Manager 审 / 红线熔断
- 每个闸门要有超时策略（多久没人审自动做什么）
- 每个闸门要有升级路径（未通过时升到哪里）

### HP-3 · Loops must be idempotent & resumable（循环必须幂等、断点可续）
循环的任何一步**跑执行两遍不能造成不同结果**，中途中断可以从断点恢复。
- 问：纯垂直执行的每一步都支持重跑吗？来源数据有一致的 ID 吗？中间态存哪里？
- 违反信号：用户说"每次跑结果不一样也行"——硬扛拦住，解释为什么不行。

### HP-4 · Small, reversible steps（改动小、可观察、可回滚）
自动改进的每一步要**粒度小 · 有观察期 · 失败可自动回滚**。
- 问：改进粒度是什么（一条 Prompt / 一个子模块 / 整个 Skill）？观察期多长？回滚的触发条件？
- 违反信号：用户说"改完直接上线"——追问弱一致性 / 影响爆炸半径。

### HP-5 · Automation tiers with clear escalation（分级自动化 + 清晰升级路径）
自动化**不是 0/1 开关**，是 L1/L2/L3 分档。每个档有清晰的权限边界与升级触发条件。
- 问：有几级自动化？每级的权限边界？哪些事件触发从 L1 升到 L2？
- 常用模版：L1 自动合入 · L2 Owner 审批 · L3 Manager 审批 · 红线直接熔断。

### HP-6 · Asset versioning（演化资产必须版本化）
循环演化的资产（Skill / Prompt / Knowledge / Rules）必须有**可追溯的版本历史**，任一版本能单独回滚。
- 问：用 git / 数据库版本 / 快照？三个月后"是谁改的"、"为什么改"、"能不能回到昔日版本"能回答吗？

### HP-7 · Human time budget is a first-class constraint（人类时间预算是硬约束）
管理者 / Owner / 审批人 / 标注人的投入时间是**预先设定的预算**，不是事后算账。
- 问：管理者每周多少小时？Owner 每天多少小时？预期会有多少 ESCALATE / 红线事件？算下来预算够吗？
- 发现不够：必须回头调低自动化量 / 不能默认"人不够就加班"。

### Layer 0 的验收标准
进入 Layer 1 前，必须能回答以下 7 问。不能回答的先扣住，不进入后续 Socratic 追问：
- [ ] HP-1：Gold Set 从哪来？冷启动规模多少？
- [ ] HP-2：闸门分几级？每级的边界与超时策略？
- [ ] HP-3：循环的每一步是否幂等？断点的补偿态存哪？
- [ ] HP-4：改进粒度？观察期？回滚触发条件？
- [ ] HP-5：有几级自动化？升级触发条件？
- [ ] HP-6：资产版本化策略？
- [ ] HP-7：每周 / 每日的人类时间预算？预算先列。

---

## Layer 1：Capture Context（找到方法论锚点）

**为什么重要**：没有锚点，Agent 的建议会漂移。有了锚点（"Harness Engineering"、"DDD"、"Event Sourcing"、某个开源项目），所有后续建议能自动做"不变性对齐"。

**如何做**：

1. **通读用户提供的素材**：如果用户附件/引用了现有代码库 / 文档 / 产品，**在第一轮并行读完**，不要边问边读。
2. **主动询问**：
   - "这套系统有没有你认同的参考方法论 / 行业最佳实践 / 类似产品？"
   - "这份材料最终给谁看？用来做什么？"（这决定产物形态）
   - "业务规模 / 团队资源 / 时间预算的硬约束是什么？"
3. **复述理解**：用自己的话重新组织一遍，让用户校验："我听到的是这样..."

**如果用户没有方法论锚点**：**不要自己造**，基于业务现状推导出一个约束清单作为替代锚点。

**Capture 阶段的输出**：
- 方法论锚点（或 explicit "无"）
- 业务现状的 3-5 个关键数字
- 产物形态（PRD / RFC / Design Doc / ...）
- 团队资源硬约束（人力 / 工期 / 工具栈）

---

## Layer 2：Brainstorming 纪律（已内化，无需依赖外部 skill）

本 skill 的 Layer 2 完整内化了"先问再写"的核心纪律：

### 纪律 1：在关键决策锁定前不写产物
立即动笔写方案会把未锁定的决策写死，后期修改成本高。**锁定 → 书写**是单向流程。

### 纪律 2：每轮只问 1 个维度
问多了用户认知过载，会随便选。宁可多问几轮，每轮问得清楚。

### 纪律 3：分节呈现，逐节 Approve
方案都不一次性输出。每完成一节（架构 / 组件 / 数据模型 / 里程碑），先和用户 Approve 再下一节。

### 纪律 4：每 3 轮做一次决策锁定回顾
防止走着走着忘了前面的选择。格式：
> "复述一下已锁定的决策：1. X = ... 2. Y = ... 3. Z = ...。下一个问题是..."

### 纪律 5：魔鬼代言人
关键决策上主动提出反驳视角：
> "您选了 L2（允许自动拆分 Skill），但这意味着某天醒来可能发现 Skill 被自动拆成 3 个，您能接受吗？"
用户能说清理由 → 接受 + 补安全网；说不清 → 帮用户再想想。

### 纪律 6：命名不擅自决定
系统名、仓库名、核心组件名、关键抽象的命名都应该**给用户 2-3 个候选让用户选**。

**这 6 条纪律贯穿 Layer 3 的每一轮对话，不可违反**。

---

## Layer 3：Socratic 追问策略（核心执行层）

### 3.1 提问模板：A/B/C/D 选项 + 推荐 + 理由

**公式**：
```
❓ 关于 <某维度>，有几个候选方案：

A. <方案 A 的一句话描述> — <优缺点>
B. <方案 B 的一句话描述> — <优缺点>
C. <方案 C 的一句话描述> — <优缺点>
D. <方案 D 的一句话描述> — <优缺点>

💡 我的建议：__ （给出明确倾向 + 为什么）

请问您选哪个？或者排个优先级？
```

**为什么这样写**：
- 候选集合降低认知负担（不用凭空构造）
- 推荐避免"让用户完全自己选"的懒惰
- 理由让用户能反驳或认同（不是盲选）
- 保留顺序排列让用户能用 "DBAC" 这种紧凑方式一次排完 4 个维度

**反例**（别这样做）：
```
❌ "你希望评测系统怎么设计？"   （开放式，信息量太低）
❌ "A or B？"                    （候选太少，强迫二选一）
❌ "A / B / C / D / E / F / G"   （候选太多，认知过载）
```

### 3.2 依赖拓扑排序（不跳步）

**原则**：后面的决策依赖前面的决策，**必须按依赖顺序问**。

**典型依赖链**（Agent 系统）：
```
方法论锚点 → 优先级排序 → 评测消费者 → 评测策略
                                            ↓
                     改进器档位 ← 闸门分级 ← 熔断策略
                                            ↓
                                     ESCALATE 路径
                                            ↓
                                      人类投入时间
                                            ↓
                                  数据模型 / 仓库拓扑
                                            ↓
                                      MVP 里程碑
```

**警戒信号**：用户开始说"这个我还没想好，跟 XX 有关"，说明你问了一个**依赖未满足**的问题，立即回退到前置依赖项。

### 3.3 一致性校验（每轮都做，共 4 项）

在每次用户做完决策后，心里默默做这 **4 项**检查：

1. **与方法论锚点的一致性**：新决策是否违背 Layer 1 的核心原则？
2. **与先前决策的一致性**：是否和前面已锁定的决策矛盾？
3. **与业务现实的一致性**：是否符合 Layer 1 的资源约束？
4. **与 Layer 0 Harness Principles 的一致性**：新决策是否违反 HP-1~HP-7 中任何一条？特别关注：
   - 这个决策让人类下场改代码了吗？（违 HP-2）
   - 这个决策让某一步变得不幂等了吗？（违 HP-3）
   - 这个决策让改进粒度变大或不可回滚了吗？（违 HP-4）
   - 这个决策增加了人类时间成本但没加到预算里吗？（违 HP-7）

**发现冲突时**：立即指出，不要装作没看见。例如：
> "您刚才选了 B（管理者 2h/周），但之前又希望所有失败都走人工（可能产生 20+ 次/周审批）。这两个决策在规模上不匹配，且违反了 **HP-7（人类时间预算是硬约束）**。建议：A. 降低人工介入比例 / B. 增加管理者投入到 5h/周 / C. 引入 Owner 分担"

### 3.4 决策锁定表（看不见但在心里维护）

每轮对话结束，更新一张心里的**决策表**：

| # | 维度 | 决策 | 锁定轮次 | 依赖 |
|---|------|------|---------|------|
| 1 | 优先级 | DBAC | R1 | - |
| 2 | 评测消费者 | RABC | R2 | 1 |
| 3 | 评测策略 | 硬门禁 | R3 | 2 |
| ... |

当**全部关键决策都锁定**后，才进入 Layer 4。

**系统类型对应的典型决策清单** → 见 `references/decision-checklists.md`。

---

## Layer 4：产物组织（内化 schema，不依赖外部 skill）

本 skill 内化了 4 类主流设计文档的 schema。**根据用户 Capture 阶段选择的产物类型**，按对应 schema 组织锁定的决策。

### 4.1 PRD Schema（Strict）

对标产品评审 / 上级对齐场景：

```markdown
# <产品名> - Product Requirements Document

## 1. Executive Summary
- Problem Statement (1-2 句)
- Proposed Solution (1-2 句)
- Success Criteria (3-5 个可度量 KPI)

## 2. User Experience & Functionality
- User Personas（每类用户是谁、目标、痛点）
- User Stories（As a [X], I want to [Y] so that [Z]）
- Acceptance Criteria（每个 Story 的 "Done" 定义）
- Non-Goals（刻意不做的清单）

## 3. AI System Requirements（如适用）
- Tool & Integration Requirements
- Evaluation Strategy（如何度量输出质量）
- Safety & Guardrails

## 4. Technical Specifications
- Architecture Overview
- Component Breakdown
- Data Model
- Integration Points
- Security & Privacy

## 5. Risks & Roadmap
- Technical Risks（含缓解措施）
- Phased Rollout（MVP → V1.1 → V2）
- Dependencies & Assumptions
```

**PRD 质量标准**：
- 不用"快"、"好用"这类模糊词 → 用"200ms 内响应"、"Precision@10 ≥ 85%"这类可度量语
- 每个 User Story 必须有 AC
- Non-Goals 必须显式列出（保护范围）

### 4.2 Design Doc Schema（工程视角）

对标开发团队内部设计：

```markdown
# <系统名> Design Doc

## 1. Background & Goals
- 现状 / 痛点 / 目标 / 非目标

## 2. Architecture
- 顶层架构图（Mermaid）
- 核心抽象 / 分层
- 数据流

## 3. Components
- 每个组件：职责 / 接口 / 内部结构 / 依赖

## 4. Data Model
- 核心实体 Schema
- 数据关系
- 存储选型

## 5. Workflow / State Machine
- 关键流程的状态转移
- 幂等性 / 重试 / 断点续传

## 6. Integration Points
- 调用哪些现有系统 / 被谁调用

## 7. Failure Modes & Recovery
- 熔断 / 降级 / 恢复机制

## 8. Milestones
- 分阶段交付 + 验收标志

## 9. Risks
- 技术风险 + 缓解

## 附录
- 术语表 / 参考资料
```

### 4.3 RFC Schema（Request For Comments）

对标跨团队协作 / 架构评审：

```markdown
# RFC-XXX: <改造主题>

## Context
- 当前状态 / 问题 / 为什么现在改

## Proposal
- 建议的改动（含 diff / 前后对比）

## Alternatives Considered
- 方案 A / B / C 的对比（关键维度：成本 / 风险 / 复杂度）
- 为什么选当前方案

## Impact
- 影响的系统 / 团队 / 用户
- 迁移策略

## Rollout Plan
- 阶段划分 + 回滚方案

## Open Questions
- 还未决定的问题（留给 commenter）
```

**RFC 的独特价值**：显式列出 **Alternatives Considered**，让评审者知道作者想过了什么、为什么没选。

### 4.4 Kickoff Checklist / Milestone Plan

对标执行团队 / AI coding agent 的输入：

```markdown
# <项目名> · Week N Kickoff

## 验收标准
<具体、可观测、二元判定>

## 当前状态
| 状态 | 说明 |

## 任务全景（N 个 Task）
| # | Task | Executor | 时长 | 依赖 | 状态 |

## 任务详情
### T1: <任务名>
- Context / Background
- Repository Layout
- Task Steps
- Constraints（⚠️ 不修改 / 不删除 / 其他硬约束）
- Deliverables
- Validation（如何自测）

### T2: ...
```

**Kickoff 清单的独特价值**：**拆到"每个文件 / 每条命令"的粒度**，AI coding agent 拿到可立即开工。

### 4.5 Decision Log / ADR

对标"决策还在演化中"场景：

```markdown
# ADR-XXX: <决策主题>

## Status
Proposed / Accepted / Deprecated / Superseded by ADR-YYY

## Context
- 决策背景 / 约束 / 驱动因素

## Decision
- 我们决定 XXX

## Consequences
- 积极影响 / 消极影响 / 中性影响

## Alternatives Considered
- 为什么没选 Y / Z
```

**ADR 的独特价值**：**粒度小、可增长**，每个关键决策一个文件，未来回溯极方便。

### 4.6 One-Pager

对标横向沟通 / 对上汇报：

**核心原则**：一页 A4 写完，包含：
- 一句话定位
- 核心问题 → 本方案（3-6 行对照表）
- 架构一图流（Mermaid）
- 关键数据与时间（表格）
- N 周交付节奏
- Top 10 设计决策
- 跟各相关方沟通的要点

**长度**：1500-2500 字。

---

## 标准工作流（Workflow）

从"用户来了"到"交付产物"的完整流程。

### Step 0: 识别是否需要本 skill（30 秒）

用户的需求是否涉及 **复杂决策 / 自循环 / Agent / 评测 / 治理 / 闸门 / 架构设计**？
- 是 → 继续 Step 1
- 否 → 简单文档直接写即可

### Step 1: Capture Context（第 1 轮）

**一次性收集**：
- 业务现状 / 方法论锚点 / 初步想法 / 明确约束
- **产物形态**（PRD / RFC / Design Doc / Kickoff / ADR / 多种并行）
- **产物消费者**（评审 / 开发 / 管理层 / 自己）

**输出**：复述理解让用户校验。不要跳过。

### Step 2: Socratic 追问（N 轮，按依赖拓扑）

每轮结构：【本轮维度】+【3-5 个候选】+【我的建议】+【为什么】

每 3 轮做一次决策锁定回顾。

### Step 3: 方案分节呈现 + 逐节 Approve

决策锁定后，按 2-5 节呈现方案，每节结束问"approve 还是调整？"

### Step 4: 按选定 schema 组织产物

- 根据 Layer 4 的 schema 摆放已锁定的决策
- 如果用户需要多种产物（三件套），并行组织
- 全部交付后告诉用户消费者分工："这份给上级看 / 那份给 AI agent 用"

### Step 5: 复盘（可选，强烈推荐）

用户问"你用了什么框架"时，**诚实回答**：
- Layer 1-4 各用了什么
- 哪轮做得好 / 哪轮本可以更好
- 自己发现的不足

这种**元沟通**建立长期信任。

---

## 反模式（Anti-patterns，绝对不要做）

| ❌ 反模式 | ✅ 正确做法 |
|----------|-----------|
| 用户刚说完想法，立即输出整套方案 | 先 Capture Context，再 Socratic 追问 |
| 一次问 5 个维度 | 每轮只问 1 个维度 |
| 用开放式问题（"你希望怎么设计？"） | 用 A/B/C/D 选项 + 推荐 |
| 接受决策不做一致性校验 | 每轮都对照方法论和先前决策校验 |
| 方案一次性甩给用户 | 分节呈现，逐节 Approve |
| 用 MUST / NEVER 硬约束 | 用"因为 X 所以建议 Y"的理由式表述 |
| 跳过锁定直接写文档 | 先锁再写，单向流程 |
| 擅自命名仓库 / 模块 / 概念 | 给 2-3 个候选让用户选 |
| 假定用户一定要 PRD | 先问清楚产物形态 |
| 依赖其他 skill 做最终产出 | 本 skill 自给自足，直接按 Layer 4 schema 输出 |

---

## 自我检查清单

**Layer 0 硬约束验收**（**本 skill 独有**，没确认之前不进入 Socratic）：
- [ ] HP-1：Gold Set 来源明确？
- [ ] HP-2：闸门分级 + 每级边界 + 超时策略明确？
- [ ] HP-3：幂等性 + 断点恢复方案明确？
- [ ] HP-4：改进粒度 + 观察期 + 回滚触发条件明确？
- [ ] HP-5：自动化分档 + 升级路径明确？
- [ ] HP-6：资产版本化策略明确？
- [ ] HP-7：人类时间预算预设完毕且后续决策均在预算内？

**Capture 阶段**：
- [ ] 问了方法论锚点？
- [ ] 确认了业务现状关键数字？
- [ ] **问了产物形态**（PRD/RFC/Design/Kickoff/ADR）和消费者？
- [ ] 做了"复述 + 校验"？

**Socratic 阶段**：
- [ ] 每轮是不是只问了 1 个维度？
- [ ] 候选是 3-5 个 A/B/C/D 格式？
- [ ] 给了明确建议 + 理由？
- [ ] 每 3 轮做了决策锁定回顾？
- [ ] 在关键决策上扮演了魔鬼代言人？
- [ ] 每轮做了 **4 项**一致性校验（含对 HP-1~HP-7 的检查）？

**呈现阶段**：
- [ ] 分节呈现让用户逐节 Approve？
- [ ] 关键命名是候选而非擅自决定？

**交付阶段**：
- [ ] 按用户选的产物形态组织？（不是默认 PRD）
- [ ] 如果是多产物，各自消费者说明清楚？

**复盘阶段**：
- [ ] 用户问方法论时能诚实回答？

---

## 与其他 skill 的关系（明确边界）

| Skill | 定位 | 与本 skill 关系 |
|-------|------|----------------|
| **how-to-harness（本 skill）** | Harness 闭环 / 自优化系统的设计引导，自给自足 | 主 skill（仅限闭环场景）|
| `brainstorming` | 通用"先问再写"纪律 | 本 skill 已内化核心，无需调用 |
| `prd` | 按 Strict Schema 产出 PRD | 本 skill 已内化 PRD schema，无需调用 |
| `skill-creator` | 创建/优化 skill | 正交关系，不冲突 |

**为什么本 skill 要自给自足**：
1. 完整方法论（含 Layer 0 Harness 硬约束）在一个 skill 内便于传授、演化、复盘
2. 依赖其他 skill 增加"调度失败"风险（对方 skill 未命中触发条件就链条断了）
3. 内化允许**跨阶段引用**（Capture 阶段的决策在 Layer 4 书写阶段能直接使用）

---

## 详细参考

- [references/ralph-case-study.md](references/ralph-case-study.md) — 完整案例：从"想做个 AI 循环"推导出 Ralph Harness 方案（**本 skill 最好的学习材料**）
- [references/decision-checklists.md](references/decision-checklists.md) — Harness 循环类系统的硬性决策清单（基于 HP-1~HP-7 展开）
- [references/deliverables.md](references/deliverables.md) — 6 类产物（PRD / Design Doc / RFC / Kickoff / ADR / One-Pager）的详细模板与选择指南

---

## 元原则（最后一句话）

> **本 skill 最重要的不是让 Agent 更会"问"，而是让 Agent 在问得好的基础上再多一层——以 Harness 原则为尺子，拒绝违反领域硬约束的决策。**
>
> "好问题 + 领域判据 = 好决策 = 好产物"（无论最后产出的是 PRD、RFC、Design Doc 还是别的）。
> 用户永远是决策的所有者，Agent 只是帮助用户把决策表达出来——并**在用户违反 Harness 原则时诚实指出**。
