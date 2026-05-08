# How to Harness

<p align="left">
  <a href="./README.md">简体中文</a> ·
  <a href="./README-en.md"><b>English</b></a>
</p>

<p align="left">
  <a href="https://github.com/dimayip/how-to-harness/stargazers"><img src="https://img.shields.io/github/stars/dimayip/how-to-harness?style=flat-square" alt="Stars"></a>
  <a href="https://github.com/dimayip/how-to-harness/network/members"><img src="https://img.shields.io/github/forks/dimayip/how-to-harness?style=flat-square" alt="Forks"></a>
  <a href="https://github.com/dimayip/how-to-harness/issues"><img src="https://img.shields.io/github/issues/dimayip/how-to-harness?style=flat-square" alt="Issues"></a>
  <a href="./LICENSE"><img src="https://img.shields.io/github/license/dimayip/how-to-harness?style=flat-square" alt="License"></a>
</p>

> **A facilitation skill dedicated to designing Harness-style closed-loop, self-optimizing, human-steer-plus-agent-execute systems.**

`how-to-harness` operationalizes OpenAI's [Harness Engineering](https://openai.com/index/harness-engineering/) methodology — **"Humans steer, agents execute"** — into a rule set that can actually be run inside a real conversation. On top of ordinary Socratic facilitation, it stacks an extra layer of **hard domain constraints** (Layer 0: Harness Principles), so that every locked-in decision is forced to align with the 7 core Harness principles.

---

## When to use it

This skill has a deliberately narrow scope. Use it only when the user's system has at least one of the following core traits:

- The system contains a **closed-loop / self-optimizing** structure (agent loop / Ralph loop / CI-style eval loop / periodic self-improvement).
- The system has explicit **human–machine collaboration gates** (tiered automation L1/L2/L3, who approves what, when does it escalate to a human).
- The system is **evaluation-driven** (Gold Set, regression tests, LLM-as-judge, hard gates / soft evals).
- The user explicitly mentions Harness Engineering / Ralph loop / LLM-as-judge / gatekeeper / closed-loop / autonomy / agent autonomy / escalation path.

**Key diagnostic question**: *"Will this system execute itself, and improve itself, on its own?"* If the answer is not clearly "yes", this skill does not apply.

---

## Why this skill exists

Design failures in Harness-style systems usually aren't caused by "not asking enough questions" — they're caused by **not asking the right questions**. Typical collapse modes:

- *"We'll skip the eval set for now and ship first."* → Three months later you want to add evals, but you can no longer tell whether any change is good or bad → violates HP-1.
- *"The manager will personally rewrite the prompts."* → The manager becomes the bottleneck; the system slows down the more it grows → violates HP-2.
- *"It's fine if each run produces different results."* → Impossible to reproduce, debug, or roll back → violates HP-3.
- *"Once improved, ship it directly, no soak period."* → A single failure takes down the whole pipeline → violates HP-4.
- *"Either fully automated or fully manual."* → No tiers, no escalation path → violates HP-5.
- *"Assets live on someone's laptop."* → Three months later nobody knows who changed what → violates HP-6.
- *"If we're short on people, we'll just work overtime."* → Hidden costs blow the budget → violates HP-7.

`how-to-harness` internalizes these 7 principles as **Layer 0 hard constraints** and forces them to be checked on every Socratic turn, blocking the class of failure where "a decision gets locked in while violating a Harness principle".

---

## The final deliverable is not just "a PRD"

The output of this skill is **chosen based on the real consumer**, not fixed in advance:

| Scenario | Recommended deliverable | Primary consumer |
|----------|-------------------------|------------------|
| New system from scratch (engineering-heavy) | **Design Doc + Kickoff checklist** | Developers + AI coding agent |
| Product pitch / upward reporting | **PRD + One-Pager** | Product review + leadership |
| Internal tech refactor / cross-team work | **RFC / technical proposal** | Engineering team + architecture review |
| Requirements still exploratory | **ADR + open-question list** | Self + follow-up |
| Existing plan needs execution | **Milestone plan + ownership matrix** | Execution team |

Multiple deliverables can be produced in parallel (e.g. PRD + One-Pager + Kickoff as a bundle, each aimed at a different audience). Templates and a selection decision tree live in [`references/deliverables.md`](./references/deliverables.md).

---

## Core methodology — 5-layer stacked framework

```
Layer 0: Harness Principles (non-negotiable domain constraints) ← the core of this skill
   ↓
Layer 1: Capture Context — methodology anchor + current state + deliverable consumer
   ↓
Layer 2: Brainstorming discipline (internalized) — 6 hard rules that enforce tempo
   ↓
Layer 3: Socratic questioning — A/B/C/D candidates + dependency topology + 4 consistency checks
   ↓
Layer 4: Deliverable organization — pick schema by consumer, slot in locked decisions
```

### Layer 0 · Harness Principles (7 hard constraints)

| # | Principle | One-line meaning |
|---|-----------|------------------|
| HP-1 | Eval is foundation | Any improvement loop starts by building a Gold Set. No evals → no automated changes. |
| HP-2 | Humans steer via gates, not via code | Humans define/approve/tune gates; they do not personally rewrite the code. |
| HP-3 | Loops must be idempotent & resumable | Every step reruns with the same result; interruptions resume from a checkpoint. |
| HP-4 | Small, reversible steps | Small diffs · soak period · failures roll back automatically. |
| HP-5 | Automation tiers with clear escalation | Automation split into L1/L2/L3, each tier has clear boundaries and escalation conditions. |
| HP-6 | Asset versioning | Evolving assets must be versioned; any version must be individually rollback-able. |
| HP-7 | Human time budget is a first-class constraint | Human time is a budget set up front, not something accounted for after the fact. |

**Layer 0 acceptance**: before entering Layer 1, the following 7 questions must be answerable. If any cannot be answered yet, park them — do *not* proceed to deeper Socratic turns:

- [ ] HP-1: Where does the Gold Set come from? What's the cold-start size?
- [ ] HP-2: How many tiers of gates? What are the boundaries and timeout policies per tier?
- [ ] HP-3: Is every step of the loop idempotent? Where is the checkpoint/resume state stored?
- [ ] HP-4: What's the granularity of each change? Soak period? Rollback trigger?
- [ ] HP-5: How many automation tiers? What triggers escalation between them?
- [ ] HP-6: What's the versioning strategy for evolving assets?
- [ ] HP-7: What's the weekly / daily human time budget?

### Layers 1–4 · Standard Socratic design facilitation

These four layers are the mainstream "ask-it-clear → lock decisions → organize deliverable" workflow: Capture Context (ask 4 things + restate) → 6 discipline rules (ask first, one axis per turn, section-by-section approve, etc.) → A/B/C/D candidates + dependency topology → pick schema by consumer and organize. See [`SKILL.md`](./SKILL.md) sections Layer 1–4.

**Layer 3 runs 4 consistency checks per turn**: against the methodology anchor / against prior decisions / against business reality / **against Layer 0 Harness Principles**. Any decision that violates HP-1~HP-7 is flagged explicitly (the user may still choose to accept the violation, but it must be an *informed* violation).

---

## Standard workflow

```
Step 0    Decide whether this skill applies (30 seconds: "does the system run and improve itself?")
   │
   ▼
Step 0.5  Layer 0 hard-constraint acceptance
   │       (if the 7 questions can't be answered, do not proceed to Socratic questioning)
   ▼
Step 1    Capture Context (turn 1: current state + anchor + deliverable form + consumer, with restatement)
   │
   ▼
Step 2    Socratic questioning (N turns, dependency-ordered, lock-in review every ~3 turns, 4 checks per turn)
   │
   ▼
Step 3    Section-by-section presentation (2–5 sections, each ends with "approve or adjust?")
   │
   ▼
Step 4    Organize the deliverable by chosen schema (ship multiple deliverables in parallel if needed)
   │
   ▼
Step 5    Retrospective (optional but strongly recommended) — answer honestly: "what framework did I actually use, and which HP did I hold the line on?"
```

---

## Repository layout

```
how-to-harness/
├── SKILL.md                               # Full skill spec — 5-layer framework + HP-1~HP-7 + workflow + anti-patterns
├── README.md                              # Chinese overview (primary)
├── README-en.md                           # (this file) English overview
└── references/
    ├── decision-checklists.md             # Hard decision checklists for Harness systems (aligned to HP-1~HP-7)
    ├── deliverables.md                    # Templates for 6 deliverable types + selection decision tree
    └── ralph-case-study.md                # Real case: from "I want to build an AI loop" to a full Ralph Harness design in 9 turns
```

- **[`SKILL.md`](./SKILL.md)** — Canonical spec. The file an AI agent actually loads to operate.
- **[`references/decision-checklists.md`](./references/decision-checklists.md)** — Each section aligns with one HP (HP-1 evals / HP-2 gates / HP-3 idempotency / HP-4 granularity / HP-5 tiers / HP-6 versioning / HP-7 budget), providing the hard checklist behind every Socratic turn.
- **[`references/deliverables.md`](./references/deliverables.md)** — Full templates for the 6 deliverable types + a decision tree used in the Capture phase.
- **[`references/ralph-case-study.md`](./references/ralph-case-study.md)** — End-to-end case: starting from *"based on Harness Engineering, build me a self-optimizing loop for my diagnose agent"*, 9 turns of dialogue produce a complete Ralph Harness design. **The single best learning material.**

---

## Anti-patterns (do not do these)

| ❌ Anti-pattern | ✅ Correct move |
|----------------|----------------|
| Skip Layer 0 and jump straight into Socratic | Run the 7-question acceptance first; park until answered |
| Ask *"do we want a Gold Set?"* | Ask *"where does the Gold Set come from?"* (HP-1 is a constraint, not an option) |
| Accept *"the manager will rewrite the code"* | Flag the HP-2 violation; push toward *"tune the gates instead"* |
| Accept *"different runs produce different results"* | Flag the HP-3 violation; ask how idempotency will be achieved |
| Accept *"ship directly after the change, no soak period"* | Flag the HP-4 violation; require a soak period and rollback trigger |
| Default to *"overtime solves the staffing gap"* | Flag the HP-7 violation; lower the automation tier or bring in more roles |
| Start writing the plan the moment the user finishes speaking | Capture + Layer 0 acceptance first, *then* Socratic |
| Ask about 5 axes per turn | One axis per turn, dependency-ordered |
| Use open-ended questions | Use A/B/C/D candidates + a recommendation + reasoning |
| Auto-invent repo / module / concept names | Offer 2–3 candidates, let the user pick |
| Delegate the final write-up to another skill | This skill is self-contained; write the deliverable directly via Layer 4 |

---

## Self-contained by design

`how-to-harness` internalizes Socratic brainstorming discipline + the schema knowledge for 6 mainstream document types + every Harness hard constraint, and **does not depend on other skills**. Why:

1. **Keeping the complete methodology in one skill** makes it easier to teach, evolve, and retrospect on.
2. **Eliminates orchestration risk** — if an external dependency fails to trigger, the whole chain breaks.
3. **Allows cross-phase references** — decisions locked in the Capture phase can be used directly in the Layer 4 write-up without losing context.

---

## Install

Via [skills.sh](https://skills.sh) (works for Claude Code / Cursor / Codex / CodeBuddy / OpenCode / 50+ agents):

```bash
# global (available in all projects)
npx skills add dimayip/how-to-harness -g -a claude-code

# project-only (committed with your repo)
npx skills add dimayip/how-to-harness -a codebuddy
```

Or drop the repo manually into your agent's skills directory, e.g.
`~/.claude/skills/how-to-harness/` or `.codebuddy/skills/how-to-harness/`.

Compatible with the [Agent Skills Specification](https://agentskills.io).

---

## How to use this skill with an AI agent

On platforms that support skill loading (CodeBuddy, Claude skills, or similar):

1. Install it via `npx skills add dimayip/how-to-harness` (see above) — or place this directory under the platform's skills folder (e.g. `.codebuddy/skills/how-to-harness/`).
2. When the user's request clearly involves closed-loop / self-optimizing / agent-loop / eval-driven systems (mentions of Harness Engineering / Ralph / LLM-as-judge / gatekeeper, etc.), the agent loads this skill's `SKILL.md`.
3. Files under `references/` are loaded **on demand** — the agent pulls them only when the current turn genuinely needs the decision checklist, a deliverable template, or the case study, keeping `SKILL.md` itself lean.

If you're using it as a human facilitator: read `SKILL.md` end-to-end once, internalize HP-1~HP-7 until they become intuition, and keep the three reference files open during conversations.

---

## Meta principle

> **The point of this skill is not to make an agent better at *asking* — it's to add one more layer on top of good questioning: use the Harness principles as a ruler and *refuse* to let decisions lock in while violating domain constraints.**
>
> "Good questions + domain criteria = good decisions = good deliverables" (whether the output is a PRD, RFC, Design Doc, or something else).
> The user always owns the decisions — the agent just helps the user express them, and **honestly calls out when the user is violating a Harness principle**.

---

## ⭐ Star History

[![Star History Chart](https://api.star-history.com/svg?repos=dimayip/how-to-harness&type=Date)](https://star-history.com/#dimayip/how-to-harness&Date)

---

## License

Unless noted otherwise in individual files, content in this repository is released under the MIT License. See [`LICENSE`](./LICENSE) if present.

---

## Credits

Designed and maintained by [@dimayip](https://github.com/dimayip). The core ideas come from OpenAI's [Harness Engineering](https://openai.com/index/harness-engineering/), reforged here into a rule set that can actually be run inside a real conversation — with Layer 0 Harness Principles (HP-1~HP-7) acting as hard domain constraints that guide users through locking in the design of Harness-style closed-loop systems.
