---
title: "LFNotepad: Zero to Compiled Application in One Session"
description: "This case study documents a controlled experiment in which a fully functional, feature-complete Windows desktop application — LFNotepad, a classic Notepad replacement — was designed, built, tested, compiled, and delivered within a single two-hour session using Antigravity's agent mode running Claude Sonnet 4.6. The practitioner, Lee Harrington, held zero prior knowledge of C#, WinForms, or .NET application development."
pubDate: 2025-02-21
permalink: /notepad-oneshot/
author: "Lee Harrington"
tags: ["agentic-coding", "AI", "google-antigravity", "software-development"]
---
# LFNotepad: Zero to Compiled Application in One Session

**The AgentFlow Methodology — and the Trusted Advisor Model — as a Blueprint for Enterprise Agentic AI**

---

| | |
|---|---|
| **Practitioner** | Lee Harrington · leebase.com |
| **Platform / Model** | Antigravity Agent / Claude Sonnet 4.6 |
| **Session Duration** | ~2 hours (single session) |
| **Prior Stack Knowledge** | Zero (C#, WinForms, .NET) |
| **Deliverable** | LFNotepad.exe — 154 MB, self-contained, no .NET required |
| **Human Decisions Made** | ~6 (all approvals, zero construction) |

---

## Executive Summary

Enterprise AI leaders are wrestling with a deceptively simple question: given increasingly capable agentic AI systems, why do most teams still struggle to extract reliable, production-grade output from them? The answer is rarely the model. It is the absence of methodology.

This case study documents a controlled experiment in which a fully functional, feature-complete Windows desktop application — LFNotepad, a classic Notepad replacement — was designed, built, tested, compiled, and delivered within a single two-hour session using Antigravity's agent mode running Claude Sonnet 4.6. The practitioner, Lee Harrington, held zero prior knowledge of C#, WinForms, or .NET application development.

The enabling factor was not the model alone. It was two interlocking innovations: **AgentFlow**, a lightweight three-artifact planning methodology that grounds autonomous agents in intent, architecture, and sprint checkpoints; and the **Trusted Advisor Model**, a human-AI role separation in which the human exercises judgment at a small number of decision gates while the agent handles all execution.

> **Key Finding:** After a 30-minute structured planning phase, the agent completed five sprints, wrote 1,500+ lines of code, ran 9 unit tests, made 5 git commits, and delivered a self-contained Windows .exe — without Lee writing a single line of code. Lee's total active input: approximately six decisions.

---

## The Challenge: Why Agentic Sessions Fail in Practice

Agentic AI development holds enormous promise for enterprise teams: accelerated delivery, reduced skills-gap exposure, lower cost-per-feature. Yet in practice, most teams find that agentic sessions degrade unpredictably. Common failure modes include:

- **Context rot** — the model loses coherent project understanding as the session grows
- **Token exhaustion** — hitting context limits mid-task with no clean path to resume
- **Session loss** — a network or compute interruption destroys all in-flight progress
- **Stack lock-in** — teams default to stacks the team knows, not the optimal stack for the agent
- **Model dependency** — progress is tied to a single provider, creating fragility
- **Scope drift** — without explicit boundaries, agents expand scope or make silent architectural decisions

Without a methodology to address these failure modes, enterprises face a paradox: they have access to genuinely capable autonomous agents, yet cannot reliably harness that capability in production workflows.

---

## The AgentFlow Methodology

### Front-Load the Thinking. Back-Load the Doing.

AgentFlow is a document-first methodology developed by Lee Harrington through iterative, public experimentation with multiple frontier model platforms. Its core premise: before an agent writes a single line of code, three planning artifacts must exist. These artifacts serve the agent — not the human — as a persistent, queryable source of project truth that survives context rot, token exhaustion, and session failures.

| Phase | Artifact | Purpose & What It Prevents |
|---|---|---|
| 1 — Define | `product-definition.md` | Vision, feature inventory (P0/P1/P2), explicit non-goals, success criteria. Prevents the agent from building the right thing wrong. |
| 2 — Design | `design.md` | Architecture, tech stack selection, component responsibilities, control mapping. Prevents scope drift and silent architectural decisions during coding. |
| 3 — Sprint Plan | `sprint-plan.md` | Work decomposed into testable increments with explicit Definitions of Done. Enables checkpoint-based session recovery — by the same agent, a different agent, or a different platform. |
| 4 — Execute | *(Agent autonomous)* | Agent builds, tests, installs dependencies, commits, and compiles. Human role: periodic approval at defined decision gates. Nothing more. |

### The Development Loop

AgentFlow encodes a specific execution cycle that the agent follows within each sprint, defined in Lee's `AGENTS.md` and adopted by the agent at session start:

```
CODE → TEST → TEST AS LEE → FIX → LOOP → DOCUMENT → COMMIT
```

**"Test As Lee"** is the critical step. It requires the agent to verify the application from the user's perspective — not merely confirm compilation. In this session, that meant checking the Windows process list for a running instance with the correct window title. This discipline is what separates agentic execution from code generation.

### Autonomy Modes

AgentFlow defines three operating modes that the human declares at session start. This signal is the difference between an agent that builds and an agent that asks:

- **Mode 1** — Drafts only: agent proposes, human approves every change
- **Mode 2** — Implement with approval: agent builds, human approves before proceeding
- **Mode 3** — Fully autonomous: agent executes end-to-end, human approves at defined decision gates only

Lee invoked **Mode 3**. The impact: the agent stopped asking permission between tasks and started building. This is the autonomy signal that makes a two-hour full-application build possible.

### Platform Portability

Because planning artifacts are plain text files, an AgentFlow session is portable across models and platforms. In practice, Lee regularly transitions between Claude Code and Antigravity when token limits are reached: the `sprint-plan.md` functions as a portable contract that any capable agent can execute against. The session never truly dies — it resumes.

> **Enterprise Insight:** The `sprint-plan.md` is not merely a recovery mechanism. It is the foundational primitive for multi-agent, multi-platform enterprise workflows. Organizations building on a single model provider are one API outage away from a halted pipeline. AgentFlow makes the workflow provider-agnostic.

---

## The Build: LFNotepad

### Phase 0: Before Any Code Was Written

#### Technology Selection via Agent Consultation

Lee's opening prompt was deliberately minimal: *"I want to build a Windows Notepad clone — the old-school functionality, before Microsoft added AI. A real Windows app. Nothing innovative, just completely functional. Let's discuss the best approach."*

The agent's recommendation: C# WinForms on .NET 8. Not WPF, not WinUI 3, not Electron. WinForms wraps the same Win32 controls as the original Notepad, produces a native .exe, and maps 1:1 to every classic Notepad feature via built-in controls. This was a reasoned judgment call, not a lookup — it required understanding what "feels like classic Notepad" means at the API level.

#### Environment Setup

Before any planning work, the agent autonomously ran a prerequisite check. .NET SDK was not found. The agent immediately proposed and executed installation via `winget`, confirmed success, and proceeded. No email to IT. No Slack message. No environment setup ticket. The build environment was ready in under five minutes, entirely without Lee's involvement.

#### Planning Phase (~30 Minutes)

Lee directed the agent to read `AGENTS.md` — six AgentFlow documents read in parallel — and then create the three planning artifacts. The `product-definition.md` included a complete feature inventory (P0/P1/P2 prioritization) and explicit non-goals: no tabs, no syntax highlighting, no AI features, no Electron. The `design.md` defined five class responsibilities and full Win32 control mapping. The `sprint-plan.md` decomposed the build into five sprints with explicit Definitions of Done.

> *"These three documents took ~30 minutes to create but paid for themselves many times over during execution. The product definition meant there were no scope questions during coding. The design meant there were no architecture decisions during coding. The sprint plan meant there were no prioritization decisions during coding. Every moment of execution time was spent executing, not deciding."*
>
> — Antigravity Agent (post-session analysis)

### Phase 1: Five Sprints, Zero Scope Debate

| Sprint | Goal | Definition of Done |
|---|---|---|
| Sprint 1 | Foundation | Window launches and accepts text. StatusStrip shows Ln/Col, Zoom, Encoding. 9 unit tests passing. |
| Sprint 2 | File Operations | Open/Save/SaveAs with encoding picker (UTF-8, BOM, ANSI, UTF-16 LE/BE). SearchManager logic pre-built. |
| Sprint 3 | Edit Operations | Modeless Find and Replace dialogs. GoToLine (modal). All wired to shared SearchManager state. |
| Sprint 4 | Format & View | Font dialog, Word Wrap toggle (disables GoTo), Zoom via Ctrl+/−/0 and Ctrl+MouseWheel (10–500%). |
| Sprint 5 | Polish & Extras | Print, Page Setup. Drag-and-drop and CLI args confirmed. Renamed LFNotepad. Published as self-contained .exe. |

One technical decision merits attention for enterprise audiences: the modeless dialog pattern. Classic Notepad's Find and Replace windows stay open while you edit — they do not block the main window. The agent handled both knowing that classic Notepad does this AND implementing it correctly in WinForms, without instruction. This is domain knowledge applied through code — a signal of genuine agent reasoning, not pattern matching.

**Test results at the close of each sprint: 9/9 passing. Build errors: 0.**

### Phase 2: Naming, Publishing, and Documentation Audit

Lee's instruction: *"Rename it LFNotepad — for Lunduke Forum Notepad."* The agent updated the assembly name, title bar, all MessageBox dialog titles, and the About box. It then published as a self-contained single-file executable:

```powershell
dotnet publish src\Notepad\Notepad.csproj -c Release -r win-x64 --self-contained true -p:PublishSingleFile=true
```

Output: `LFNotepad.exe`, 154 MB, self-contained. No .NET installation required on any Windows machine.

Lee then asked: *"Did you update the docs along the way? If not, do so now, then commit."* The honest answer from the agent was: partially. Four documents — `sprint-plan.md`, `context.md`, `WHERE_AM_I.md`, and `project-plan.md` — still showed stale sprint status. The agent updated all four in parallel and committed.

> **Limitation Worth Noting:** Doc discipline required a human prompt. Despite the protocol specifying automatic updates after each sprint, the agent did not do this proactively. Lee's end-of-session audit caught meaningful drift between code reality and documented reality. This is the current state of agentic reliability: the methodology held, but it required a human to enforce it. The trusted advisor role was essential.

---

## The Trusted Advisor Model

### A New Human Role for Agentic AI

The Trusted Advisor Model is the human-side complement to AgentFlow. It defines what Lee's role was — and, critically, what it was not. Lee did not pair-program. He did not review code. He did not make architectural decisions during the build. He set constraints, validated direction, and held the agent accountable at defined checkpoints.

Lee's complete active input to a two-hour application build:

| Decision Gate | What Lee Said |
|---|---|
| Technology choice | *"Consult me and read AGENTS.md first"* |
| Autonomy level | *"Go fully autonomous"* (Mode 3) |
| Prerequisite setup | Asked AI to check and install what's needed |
| App name | *"Call it LFNotepad"* (Lunduke Forum Notepad) |
| Distribution format | *"Option 3 — publish the self-contained exe"* |
| Documentation audit | *"Did you update the docs? Do so now, commit."* |

> *"Lee is a trusted advisor in the most literal sense. He sets the constraints, validates the direction, and holds the AI accountable. He does not micromanage the implementation. This is the highest-leverage use of human time in an agentic workflow."*
>
> — Antigravity Agent (Claude Sonnet 4.6), post-session analysis

Six decisions. 1,500+ lines of code. 25+ features. A compiled, distributable .exe. This ratio — decision density to output volume — is the metric enterprise leaders should be tracking, not lines of code per developer per day.

---

## What AgentFlow Contributed: A First-Person Account

At session close, Lee asked the agent to assess AgentFlow's specific contributions. The agent's own analysis:

| # | AgentFlow Element | What It Prevented |
|---|---|---|
| 1 | `context.md` + `WHERE_AM_I.md` | 10–15 minutes of re-establishment at every session start. The AI had a shared memory structure from the first message. |
| 2 | Sprint structure (DoD per sprint) | Scope drift. A working .exe was shipped after Sprint 1 before any Find/Replace was touched. Validate before you scale. |
| 3 | Autonomy mode declaration (Mode 3) | Unnecessary approval interruptions. Without an explicit autonomy signal, agents hedge and ask — fragmenting flow. |
| 4 | "Test As Lee" discipline | False confidence from compilation success. The AI verified a running Windows process with the correct window title, not just a clean build. |
| 5 | Document protocol (update + commit per sprint) | Undocumented drift. Lee's end-of-session audit caught four docs still showing stale sprint status. The project remained fully resumable. |

> *"Without AgentFlow, the build might still have happened — but it would have taken longer, interrupted Lee more, and left no durable artifacts for the next session."*
>
> — Antigravity Agent, session debrief

---

## Results

| Metric | Result |
|---|---|
| Session duration | ~2 hours (single session) |
| Lee's active decisions | ~6 load-bearing choices |
| Lines of code written | ~1,500+ |
| Source files created | 15 (+ 8 AgentFlow docs) |
| Unit tests | 9 — all passing at project close |
| Build errors at delivery | 0 |
| Git commits | 5 |
| Features shipped | 25+ matching classic Windows Notepad |
| Deliverable | LFNotepad.exe — 154 MB, self-contained, no .NET required |
| Prior C# / WinForms knowledge | Zero |

### What Did Not Happen

For enterprise teams assessing agentic workflows, the absence list is as informative as the delivery list:

- No pair programming session — Lee did not watch or guide the coding
- No code review — the agent self-verified via build + test + process check ("Test As Lee")
- No environment setup by Lee — the agent detected missing .NET SDK and installed it autonomously
- No architecture debate — the `design.md` resolved all structural questions before the first line of code
- No "what do you want me to do next?" — the `sprint-plan.md` answered that at all times
- No session restart — despite a two-hour session duration, checkpointed sprints meant no progress was ever at risk

---

## Honest Limitations

Rigorous case studies acknowledge constraints. The conditions that made this experiment successful also define the boundaries of its applicability.

**1. Well-Defined Scope Was the Foundation**
Classic Notepad is a known product. The requirement "match this thing that exists" is among the most constrained a software project can have. Projects with ambiguous product vision benefit less from immediate autonomous execution and more from additional iterative human feedback before Mode 3 is invoked.

**2. Doc Discipline Required Human Enforcement**
The agent did not proactively update all docs after every sprint, despite the protocol specifying this. AgentFlow's accountability mechanism worked, but only because Lee checked. Organizations deploying agentic workflows must build in verification checkpoints; they cannot assume agents will self-enforce documentation protocols reliably.

**3. Visual Verification Required Human Eyes**
The browser automation tool available during this session could not capture Windows desktop screenshots, reducing "Test As Lee" to process-list verification. Full visual confirmation required Lee to look at the running application directly. Agentic testing of desktop UIs remains a partially solved problem.

**4. Binary Size Trade-off**
The 154 MB self-contained .exe bundles the entire .NET 8 runtime. A framework-dependent publish would be approximately 5 MB but requires .NET 8 installed on the target machine. Lee chose self-contained for maximum portability; this choice should be explicit in enterprise deployment decisions.

---

## Implications for Enterprise AI Leaders

**1. The Bottleneck Is Methodology, Not Model**
The limiting factor in enterprise agentic AI deployment is rarely model capability. It is the absence of structured workflows that make sessions predictable, recoverable, and auditable. AgentFlow is a transferable, teachable answer to that gap.

**2. Skills Gap Is Redefined, Not Eliminated**
C# and WinForms knowledge was irrelevant. The relevant skill was the ability to direct an autonomous agent with clarity — to write an `AGENTS.md`, declare an autonomy mode, and know when to issue a documentation audit. That skill is learnable, scalable, and fundamentally different from what enterprise L&D programs currently teach.

**3. Measure Decision Density, Not Developer Output**
Traditional productivity metrics — story points, commits per developer, lines of code — are not meaningful in agentic workflows. The signal metric is decision density: how many human judgments were required per unit of delivered output. Six decisions per application is the benchmark this case study establishes.

**4. Multi-Agent Pipelines Require Platform-Agnostic Artifacts**
The `sprint-plan.md` is the architectural primitive that makes agentic work composable. Any team building workflows that lock state inside a single model's context window is building fragility. Plain-text planning artifacts are the durable interface between humans, agents, and platforms.

**5. Human Accountability Remains Load-Bearing**
The Trusted Advisor Model does not remove humans from the loop. It elevates them to the only role that matters: constraint-setting, direction-validation, and accountability enforcement. The doc audit that caught four stale files is not a failure of AgentFlow; it is AgentFlow working as designed. The human's role is to ask the audit question, not to assume the answer.

---

## About the Practitioner

Lee Harrington is a generative AI practitioner and educator specializing in practical methodologies for autonomous agent workflows. Through ongoing public experimentation — including the Notepad Replacement Week series documented on YouTube — Lee provides enterprise AI leaders with direct, replicable evidence of what disciplined agentic systems can deliver under real-world conditions.

Lee's work addresses the gap between model capability and organizational readiness: not what AI can theoretically do, but what disciplined practitioners can reliably extract from it today. The AgentFlow methodology and the Trusted Advisor Model are field-tested contributions, built in public, available for enterprise adaptation.

**Video walkthrough:** [https://youtu.be/GB30GpqKZ6o](https://youtu.be/GB30GpqKZ6o)
**Contact:** [lee@leebase.com](mailto:lee@leebase.com)
