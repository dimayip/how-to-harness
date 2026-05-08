# How to Harness

<p align="left">
  <a href="./README.md"><b>简体中文</b></a> ·
  <a href="./README-en.md">English</a>
</p>

<p align="left">
  <a href="https://github.com/dimayip/how-to-harness/stargazers"><img src="https://img.shields.io/github/stars/dimayip/how-to-harness?style=flat-square" alt="Stars"></a>
  <a href="https://github.com/dimayip/how-to-harness/network/members"><img src="https://img.shields.io/github/forks/dimayip/how-to-harness?style=flat-square" alt="Forks"></a>
  <a href="https://github.com/dimayip/how-to-harness/issues"><img src="https://img.shields.io/github/issues/dimayip/how-to-harness?style=flat-square" alt="Issues"></a>
  <a href="./LICENSE"><img src="https://img.shields.io/github/license/dimayip/how-to-harness?style=flat-square" alt="License"></a>
</p>

> **一个专门用来设计 Harness-style 闭环 / 自优化 / 人类掌舵-Agent 执行系统的 facilitation skill。**

`how-to-harness` 把 OpenAI 的 [Harness Engineering](https://openai.com/index/harness-engineering/) 方法论——**"Humans steer, agents execute"**——落实为一套**可以在真实对话里跑起来**的引导规则集。它在常规的 Socratic facilitation 之上额外叠加一层**硬性领域约束**（Layer 0: Harness Principles），确保每一个锁定的决策都对齐 Harness 方法论的 7 条核心原则。

---

## 何时使用

本 skill 适用范围较窄，仅当用户的系统有以下核心特征之一时才使用：

- 系统存在**闭环 / 自优化**结构（Agent loop / Ralph loop / CI 式评测循环 / 周期性自改进）
- 系统存在明确的**人机协作闸门**（分级自动化 L1/L2/L3、谁审批什么、什么情况升级到人）
- 系统以**评测驱动**（Gold Set、回归测试、LLM-as-judge、硬门禁 / 软评估）
- 用户明确提到 Harness Engineering / Ralph loop / LLM-as-judge / gatekeeper / 闭环 / 自治 / agent autonomy / 熔断升级路径

**判定钥匙**：问一个关键问题——"这个系统会不会**自己执行自己改进自己**？"如果答案不是"会"，本 skill 就不适用。

---

## 为什么需要这个 skill

Harness-style 系统的设计失败，往往不是因为"没问清楚"，而是因为**问得没问在点子上**。常见塌方：

- "我们这个闭环暂时没评测集，先上线再说" → 3 个月后想加评测，但已经没法知道每次改动是好是坏 → 违反 HP-1
- "管理者会亲自下场改 Prompt" → 管理者成了瓶颈，系统越做越慢 → 违反 HP-2
- "每次跑结果不一样也行" → 无法复现、无法调试、无法回滚 → 违反 HP-3
- "改进了就直接上线，没观察期" → 一次失败炸掉整个产线 → 违反 HP-4
- "要么全自动要么全人工" → 没有分级，没有升级路径 → 违反 HP-5
- "资产存在个人电脑上" → 3 个月后没人知道"是谁改的" → 违反 HP-6
- "人不够就加班" → 预算外的隐性成本炸锅 → 违反 HP-7

`how-to-harness` 把这 7 条原则**内化为 Layer 0 硬约束**，在每一轮 Socratic 追问时强制对照检查，从源头阻止"决策锁死在违反 Harness 的位置"这类失败。

---

## 最终产物不是"PRD"一种

本 skill 的输出**按真实消费者选择**，不是固定格式：

| 场景 | 建议产物 | 主要消费者 |
|------|----------|-----------|
| 从 0 到 1 设计新系统（工程为主） | **Design Doc + Kickoff 清单** | 开发者 + AI coding agent |
| 产品立项 / 对上汇报 | **PRD + One-Pager** | 产品评审 + 管理层 |
| 内部技术改造 / 跨团队协作 | **RFC / 技术方案** | 工程团队 + 架构评审 |
| 需求仍在探索 / 决策未完全锁定 | **ADR + 待办问题清单** | 自己 + 后续推进 |
| 已有方案需落地 | **里程碑计划 + 分工表** | 执行团队 |

多种产物可以**并行交付**（例如 PRD + One-Pager + Kickoff 三件套，给不同受众看）。详细模板与选择决策树见 [`references/deliverables.md`](./references/deliverables.md)。

---

## 核心方法论：5 层叠加框架

```
Layer 0: Harness Principles（领域硬约束，不可协商）← 本 skill 的核心层
   ↓
Layer 1: Capture Context —— 方法论锚点 + 业务现状 + 产物消费者
   ↓
Layer 2: Brainstorming 纪律（已内化）—— 6 条硬纪律约束节奏
   ↓
Layer 3: Socratic 追问 —— A/B/C/D 候选 + 依赖拓扑 + **4 项**一致性校验
   ↓
Layer 4: 产物组织 —— 按消费者选 schema，把锁定的决策摆进去
```

### Layer 0 · Harness Principles（7 条硬约束）

| # | 原则 | 一句话理解 |
|---|------|-----------|
| HP-1 | Eval is foundation | 任何改进循环先建 Gold Set，没评测不能自动改 |
| HP-2 | Humans steer via gates, not via code | 人类定义/审批/调整闸门，不亲自下场改代码 |
| HP-3 | Loops must be idempotent & resumable | 每一步重跑结果一致，中途中断可从断点恢复 |
| HP-4 | Small, reversible steps | 改动小 · 有观察期 · 失败可自动回滚 |
| HP-5 | Automation tiers with clear escalation | 自动化分 L1/L2/L3 档，每档有清晰边界与升级条件 |
| HP-6 | Asset versioning | 演化资产必须版本化，任一版本可单独回滚 |
| HP-7 | Human time budget is a first-class constraint | 人类投入时间是预先设定的预算，不是事后算账 |

**Layer 0 验收**：进入 Layer 1 前必须能回答以下 7 问。不能回答的先扣住，不进入后续追问：

- [ ] HP-1：Gold Set 从哪来？冷启动规模多少？
- [ ] HP-2：闸门分几级？每级的边界与超时策略？
- [ ] HP-3：循环的每一步是否幂等？断点补偿态存哪？
- [ ] HP-4：改进粒度？观察期？回滚触发条件？
- [ ] HP-5：有几级自动化？升级触发条件？
- [ ] HP-6：资产版本化策略？
- [ ] HP-7：每周 / 每日的人类时间预算？

### Layer 1–4 · 常规 Socratic 设计引导

这 4 层是主流的"问清楚→锁决策→组织产物"工作流：Capture Context（问 4 件事 + 复述）→ 6 条纪律（先问再写、每轮一维、分节 Approve 等）→ A/B/C/D 候选 + 依赖拓扑追问 → 按消费者选 schema 组织产物。详见 [`SKILL.md`](./SKILL.md) 第 Layer 1–4 节。

**Layer 3 的一致性校验共 4 项**：对方法论锚点 / 对先前决策 / 对业务现实 / **对 Layer 0 Harness Principles**。任何违反 HP-1~HP-7 的决策都要被显式指出（用户仍可选择接受违反，但必须是知情违反）。

---

## 标准工作流

```
Step 0  识别是否需要本 skill（30 秒：系统会不会自己改自己？）
   │
   ▼
Step 0.5  **Layer 0 硬约束验收**
   │    （7 问不过，不进 Socratic 追问）
   ▼
Step 1  Capture Context（第 1 轮：现状 + 锚点 + 产物形态 + 消费者，复述校验）
   │
   ▼
Step 2  Socratic 追问（N 轮，按依赖拓扑，每 3 轮决策锁定回顾，每轮 4 项一致性校验）
   │
   ▼
Step 3  方案分节呈现（2–5 节，每节结束问 "approve 还是调整？"）
   │
   ▼
Step 4  按选定 schema 组织产物（多产物并行交付 + 说明各自消费者）
   │
   ▼
Step 5  复盘（可选，强烈推荐）—— 诚实回答"你用了什么框架、哪些 HP 扣住了用户"
```

---

## 仓库结构

```
how-to-harness/
├── SKILL.md                               # 完整 skill 规范——5 层框架 + HP-1~HP-7 + 工作流 + 反模式
├── README.md                              # （本文件）中文总览
├── README-en.md                           # 英文总览
└── references/
    ├── decision-checklists.md             # Harness 类系统的硬性决策清单（逐条对齐 HP-1~HP-7）
    ├── deliverables.md                    # 6 类产物（PRD / Design Doc / RFC / Kickoff / ADR / One-Pager）详细模板与选择决策树
    └── ralph-case-study.md                # 真实案例：从"想做个 AI 循环"到 Ralph Harness 方案的 9 轮推导过程
```

- **[`SKILL.md`](./SKILL.md)** — Canonical spec。AI agent 真正加载执行的文件。
- **[`references/decision-checklists.md`](./references/decision-checklists.md)** — 每一节都对齐一条 HP（HP-1 评测 / HP-2 闸门 / HP-3 幂等 / HP-4 粒度 / HP-5 分档 / HP-6 版本 / HP-7 预算），是 Socratic 追问的硬性清单。
- **[`references/deliverables.md`](./references/deliverables.md)** — 6 类产物的完整模板 + Capture 阶段的产物选择决策树。
- **[`references/ralph-case-study.md`](./references/ralph-case-study.md)** — 端到端案例：从用户一句"基于 Harness Engineering 给我的排障 AI Agent 做个自优化循环"开始，9 轮对话推出完整 Ralph Harness 方案。**最好的学习材料。**

---

## 反模式（绝对不要做）

| ❌ 反模式 | ✅ 正确做法 |
|----------|-----------|
| 跳过 Layer 0 直接进入 Socratic | 先过 7 问验收，不过先扣住 |
| 问"要不要 Gold Set" | 问"Gold Set 从哪来"（HP-1 是硬约束，不是选项） |
| 接受"管理者下场改代码"的决策 | 指出违反 HP-2，建议改为"调闸门" |
| 接受"每次跑结果不一样" | 指出违反 HP-3，追问幂等性方案 |
| 接受"改完直接上线，没观察期" | 指出违反 HP-4，要求定观察期 + 回滚条件 |
| 默认"人不够就加班" | 指出违反 HP-7，要求先降自动化或引入更多角色 |
| 用户刚说完想法就开始写方案 | 先 Capture + Layer 0 验收，再进入 Socratic |
| 每轮问 5 个维度 | 每轮只问 1 个维度，按依赖拓扑 |
| 用开放式问题 | 用 A/B/C/D 候选 + 推荐 + 理由 |
| 擅自命名仓库 / 模块 / 概念 | 给 2–3 个候选让用户选 |
| 依赖其他 skill 做最终产出 | 本 skill 自给自足，直接按 Layer 4 schema 输出 |

---

## 自给自足原则

`how-to-harness` 内化了 Socratic brainstorming 纪律 + 6 类主流文档的 schema 知识 + 全部 Harness 硬约束，**不依赖其他 skill**。原因：

1. **完整方法论在一个 skill 内**便于传授、演化、复盘；
2. **避免调度失败风险**——依赖的外部 skill 未命中触发条件，整条链就断了；
3. **允许跨阶段引用**——Capture 阶段锁定的决策，Layer 4 书写阶段能直接使用，不丢上下文。

---

## 安装

通过 [skills.sh](https://skills.sh) 一键安装（支持 Claude Code / Cursor / Codex / CodeBuddy / OpenCode 等 50+ agent）：

```bash
# 全局安装（所有项目可用）
npx skills add dimayip/how-to-harness -g -a claude-code

# 项目级安装（跟随项目提交）
npx skills add dimayip/how-to-harness -a codebuddy
```

或手动把整个目录放到你的 agent skills 目录下，例如
`~/.claude/skills/how-to-harness/` 或 `.codebuddy/skills/how-to-harness/`。

兼容 [Agent Skills Specification](https://agentskills.io)。

---

## 如何在 AI agent 中使用这个 skill

支持 skill 加载的平台（CodeBuddy / Claude skills 等）：

1. 用 `npx skills add dimayip/how-to-harness` 一键安装（见上），或手动把整个目录放到平台的 skills 文件夹下（例如 `.codebuddy/skills/how-to-harness/`）；
2. 当用户的请求明确涉及闭环 / 自优化 / Agent-loop / 评测驱动（提到 Harness Engineering / Ralph / LLM-as-judge / gatekeeper 等），agent 会加载本 skill 的 `SKILL.md`；
3. `references/` 下的文件**按需加载**——agent 只在当前对话真正需要决策清单、产物模板或参考案例时才读，让 `SKILL.md` 本身保持精简。

如果你是自己当 facilitator：完整读一遍 `SKILL.md` 把 Layer 0 的 HP-1~HP-7 内化为直觉，把三份 reference 文件在对话时开在旁边随时查。

---

## 元原则

> **本 skill 最重要的不是让 Agent 更会"问"，而是让 Agent 在问得好的基础上再多一层——以 Harness 原则为尺子，拒绝违反领域硬约束的决策。**
>
> "好问题 + 领域判据 = 好决策 = 好产物"（无论最后产出的是 PRD、RFC、Design Doc 还是别的）。
> 用户永远是决策的所有者，Agent 只是帮助用户把决策表达出来——并**在用户违反 Harness 原则时诚实指出**。

---

## ⭐ Star 历史

[![Star History Chart](https://api.star-history.com/svg?repos=dimayip/how-to-harness&type=Date)](https://star-history.com/#dimayip/how-to-harness&Date)

---

## License

除非单独文件另有说明，本仓库内容遵循 MIT License。详见 [`LICENSE`](./LICENSE)。

---

## Credits

由 [@dimayip](https://github.com/dimayip) 设计并维护。核心思想来自 OpenAI 的 [Harness Engineering](https://openai.com/index/harness-engineering/)，在其基础上落地为一套可在真实对话里跑起来的规则集——以 Layer 0 Harness Principles（HP-1~HP-7）为硬性领域约束，引导用户锁定 Harness-style 闭环系统的设计。
