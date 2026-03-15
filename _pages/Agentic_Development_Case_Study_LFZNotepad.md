---
title: "Agentic Development Across Multiple AI Platforms"
description: "A project manager who has never written a line of Zig built a complete Windows desktop application — from empty directory to working binary — in 5 hours. He did not learn Zig. He did not study the Win32 API. He designed a methodology, set up the proof of concept, directed three AI coding agents across three different platforms, and the application was delivered with full file operations, find and replace, font selection, keyboard shortcuts, and a four-part status bar. The technology stack was chosen specifically because he had no expertise in it.

That is the headline. What follows is how it happened, and what it reveals about the kind of practitioner who can make agentic development work at enterprise scale."
pubDate: 2025-02-28
permalink: /agent_case_study_multi_pc/
author: "Lee Harrington"
tags: ["agentic-coding", "AI", "google-antigravity", "software-development"]
---
# Agentic Development Across Multiple AI Platforms

### A Multi-Agent Implementation of LFZNotepad Using Zig and Win32

**February 2026**
*Prepared by Lee — Trusted Advisor in Generative AI*

---


## Executive Summary

A project manager who has never written a line of Zig built a complete Windows desktop application — from empty directory to working binary — in 5 hours. He did not learn Zig. He did not study the Win32 API. He designed a methodology, set up the proof of concept, directed three AI coding agents across three different platforms, and the application was delivered with full file operations, find and replace, font selection, keyboard shortcuts, and a four-part status bar. The technology stack was chosen specifically because he had no expertise in it.

That is the headline. What follows is how it happened, and what it reveals about the kind of practitioner who can make agentic development work at enterprise scale.

---

Lee is a Generative AI advisor who designed AgentFlow, the document-first methodology that governed this project. His background is in project leadership and AI coordination — not systems programming. He conceived this project as a deliberate proof of concept: could a practitioner with the right methodology and supervisory skills deliver a complex application in an unfamiliar stack, using AI agents as the implementation layer? He operated exclusively in supervisor mode throughout. His contributions were methodology design, architectural direction, load-bearing decisions, output verification, and agent transition management. He did not write code.

The technology stack — Zig 0.15 with raw Win32 APIs and manual memory management — was deliberately selected outside his implementation expertise, creating a controlled test of a specific thesis: that the right methodology and leadership approach absorb the complexity traditionally requiring deep specialist knowledge, leaving the human's role unchanged regardless of stack difficulty.

Three AI agents contributed in sequence: Antigravity (Sonnet 4.6) handled planning and initial scaffolding; Kimi Code (Moonshot AI) executed the full implementation across all five sprints; and OpenCode (GPT 5.3 Codex) performed UAT stabilization and debugging. The first transition was forced by exhausted model credits. The second was a deliberate decision — Lee grew dissatisfied with the time Kimi Code was spending on Zig-specific compilation problems and, the following morning, switched to OpenCode and what he considered the best-in-class coding model for stabilization work. The AgentFlow methodology enabled seamless continuity across all three platforms because Lee had designed it for exactly this kind of portability.

Two conclusions emerge from this project with direct implications for enterprise AI strategy:

**First, agentic coding collapses implementation difficulty.** The human tasks — direction-setting, decision-making, verification — remained identical to those required for a prior C# WinForms implementation of the same application. The systems-level complexity of Zig and Win32 was absorbed entirely by the AI agents. Language and framework selection became secondary to architecture, supervision, and verification.

**Second, artifact-driven workflows enable cross-agent continuity.** Work continued across three different AI platforms without loss of context, architectural drift, or rework. Token limits, credit exhaustion, and deliberate platform switches — real constraints in production AI usage — did not block progress. The AgentFlow documents functioned as durable project memory, execution contracts, and cross-agent interfaces.

---

## Project Background

### Origin

LFZNotepad is a sibling project to LFNotepad, a C# WinForms Notepad clone that Lee had previously built using agentic development methods. That earlier project demonstrated the basic viability of AI-supervised application development. But Lee's intent with LFZNotepad was more ambitious: he designed it as a controlled experiment to answer a specific question for enterprise audiences — does the human's effort increase when the implementation complexity increases dramatically?

The C# WinForms version of Notepad relies on a mature, well-documented framework with extensive IDE support. The Zig/Win32 version required manual memory management, direct system API calls, and navigation of a rapidly evolving compiler (Zig 0.15) with limited community documentation. If the human's workload remained comparable across both implementations, it would demonstrate that implementation difficulty had genuinely shifted from the human to the AI agents.

### Experiment Design

Lee designed the experiment end-to-end before any AI agent was engaged. He locked the technology stack (Zig 0.15.2 with the zigwin32 bindings library, targeting RichEdit20W, no resource files, fully programmatic dialog creation), defined the full feature set to match the C# version, created the AgentFlow artifacts that would govern all work, and established the rules of engagement in AGENTS.md.

The critical design choice was operational: Lee would not write implementation code. He would operate as methodology designer and project leader — creating planning artifacts, setting direction, making architectural decisions, reviewing and testing outputs, and managing agent transitions. The AI agents would handle all implementation work autonomously under Mode 3 (Autonomous) operation. This was not an accident of the workflow; it was the hypothesis under test.

---

## Methodology: AgentFlow

AgentFlow is a document-first development methodology created by Lee for multi-agent AI workflows. It addresses a fundamental challenge in agentic development: AI agents have no persistent memory across sessions, and different platforms share no common state. Lee's solution centers on persistent artifacts that serve simultaneously as project memory, execution contracts, and cross-agent interfaces. In this project, six core documents governed all work:

| Artifact | Function |
|---|---|
| **AGENTS.md** | Rules of engagement: tech stack, coding conventions, autonomy level, development loop, guardrails |
| **context.md** | Session state: current phase, sprint progress, locked decisions, known issues |
| **product-definition.md** | Complete feature list with P0/P1 priority classification |
| **design.md** | Win32 architecture, AppState struct, encoding strategy, API mapping table |
| **sprint-plan.md** | Five-sprint breakdown with task checklists and definitions of done |
| **WHERE_AM_I.md** | Milestone tracker for cross-session orientation |

### Supervisor-Mode Interaction

Lee's interaction with each agent followed a consistent pattern: orient the agent to the artifacts, authorize execution, monitor progress, provide bug reports or process corrections, and verify outputs. He never specified implementation details, wrote code, or overrode technical decisions at the code level. His interventions were consistently at the level of architecture, process, and quality — the same interventions he would make with a human engineering team.

### Definitions of Done

Each sprint in the plan included explicit completion criteria. Sprint 1 required a launching window with a RichEdit control, visible menus, and an updating status bar. Sprint 2 required functional file open, save, and encoding detection. These definitions of done functioned as verification contracts — they told both the agent and the supervisor exactly what "complete" meant. When Kimi Code reported Sprint 2 as complete but file operations crashed, Lee's bug report was grounded in the definition of done, not in subjective dissatisfaction.

### Portability and Resilience

The artifacts were deliberately designed to be model-agnostic. They contained no references to specific AI platforms, no assumptions about context window sizes, and no platform-specific instructions. Any capable coding agent could read AGENTS.md and the supporting documents and begin productive work. This portability proved essential when agent transitions occurred — whether forced by credit exhaustion or driven by a deliberate decision to switch platforms.

---

## Multi-Agent Development Narrative

### Phase 1: Antigravity / Sonnet 4.6 — Planning and Foundation

The project began with a planning session in Antigravity (Google DeepMind) that produced all six AgentFlow artifacts. A second Antigravity session then initiated Sprint 1 implementation: the foundational window, RichEdit control, menu bar, and status bar.

The agent created eight source files from scratch, including the build system (build.zig, build.zig.zon), the window procedure with AppState management, a complete menu bar, and a four-part status bar. The architecture decisions made during this phase — module-level global state for a single-window application, locally defined Win32 message constants for pragmatic reasons, and hard-coded RichEdit style bits to work around zigwin32's packed struct limitations — were sound and survived through all subsequent phases without requiring rework.

The primary friction was the build system. Zig 0.15 had introduced breaking API changes, and the zigwin32 dependency used a module name ("win32") that differed from its package name ("zigwin32"), causing a runtime panic rather than a compilation error. Terminal output truncation in the development environment further complicated diagnosis. The agent resolved these issues methodically, adopting a workaround of redirecting stderr to files for reliable error capture — a suggestion from Lee that demonstrates the value of supervisor-mode process improvement.

The session ended when model credits were exhausted. The build system was working correctly, but seven compilation errors remained in the source files. The code had never been compiled to a working binary. Sprint 1 was approximately 60% complete.

#### Handoff State

The agent left detailed handoff notes documenting the exact state of every file, known errors, zigwin32 idioms discovered during the session, and specific instructions for the next agent. These notes, combined with the pre-existing AgentFlow artifacts, formed a complete handoff package.

### Phase 2: Kimi Code / Moonshot AI — Full Implementation

Kimi Code entered the project after Antigravity's credits were exhausted. The agent's first action was to read the AgentFlow artifacts, following the same orientation protocol. It discovered a discrepancy: context.md claimed Sprint 1 was at 0% completion, but the source directory contained working Sprint 1 code. This stale documentation — a consequence of Antigravity's session ending before document updates — cost approximately ten minutes of confusion. It is worth noting as a methodology lesson: document updates should be continuous, not deferred to sprint completion.

After resolving the state discrepancy through source code inspection, Kimi Code proceeded to implement all remaining functionality across Sprints 2 through 5. The agent used parallel subagent deployment, spawning multiple concurrent workers for independent components: file I/O with encoding detection, open/save dialogs, edit operations, and the Find dialog in one wave, followed by Replace, GoTo, Format, Print, and About dialogs in a second wave.

The resulting implementation comprised approximately 3,600 lines of Zig across nine new files and significant modifications to window.zig. Major components included a 508-line file helper with full encoding support (UTF-8, UTF-8 BOM, UTF-16 LE/BE, ANSI), modeless Find and Replace dialogs, a modal GoTo dialog, word wrap toggling, font selection via CHOOSEFONTW, zoom with EM_SETZOOM, and print integration via PrintDlgExW.

The initial build after code generation failed with over 15 errors, primarily due to zigwin32 API changes and Zig 0.15 standard library differences. Two fix waves resolved all compilation errors, and the application built successfully. Runtime testing confirmed Sprint 1 features worked correctly: the window launched, text input worked, the status bar updated, and menus were present.

However, file operations crashed on use. Two bugs were identified: a null-termination failure when passing file content to WM_SETTEXT (the content was a Zig slice, not a sentinel-terminated array), and a file filter string format incompatible with Zig's string literal handling. Both were fixed, and file operations became functional.

Lee's interaction during this phase was minimal: an initial orientation and authorization ("proceed, use your multi agent feature as warranted, make me proud"), one resume after an interruption, and a specific bug report ("it bombs when opening and saving files"). The total human intervention during the full implementation of a Win32 application was five prompts.

#### Handoff State

Kimi Code produced four git commits and left the application in a buildable state with all primary features implemented. Several features (Find, Replace, Print, drag-and-drop) had dialog-level verification but lacked comprehensive stress testing. The agent updated context.md and WHERE_AM_I.md before session end.

### Transition: A Deliberate Platform Switch

The transition from Kimi Code to OpenCode was not forced by credit exhaustion. Lee grew dissatisfied with the amount of time and effort Kimi Code was spending on Zig-specific compilation problems during the debugging cycle. The application compiled but was unstable at runtime, and the fix iterations were not converging quickly enough. The following morning, Lee made a deliberate decision to switch to OpenCode, powered by GPT 5.3 Codex — which he considered the best-in-class coding model for the stabilization work that remained.

This transition is instructive. In agentic workflows, the human supervisor retains the ability to make strategic platform decisions based on observed agent performance — not just when forced by external constraints, but proactively, when a different tool appears better suited to the current phase of work. The AgentFlow artifacts made this switch frictionless.

### Phase 3: OpenCode / GPT 5.3 Codex — UAT Stabilization

OpenCode entered the project with the application in a compilable but runtime-unstable state. User acceptance testing had revealed significant issues across multiple features: file operations still caused occasional crashes, keyboard shortcuts were non-functional, Edit menu actions failed, Find/Replace/GoTo dialogs were unreliable, and Format operations triggered unexpected exits.

OpenCode operated as a diagnostic and stabilization agent. Unable to run the GUI directly, it relied on an iterative cycle: Lee would run the application, report specific symptoms, and OpenCode would trace the code path, identify the root cause, apply a minimal patch, and rebuild. Lee would then test again. This cycle repeated across all major feature areas.

The root causes fell into several categories. An allocator ownership mismatch in the open/save dialog returned a truncated slice that caused an invalid free on deallocation. Keyboard shortcuts required a Win32 accelerator table (CreateAcceleratorTableW + TranslateAcceleratorW) that the previous implementation had omitted. Find and Replace dialogs were routing operations to the parent window handle rather than the RichEdit child control. GoTo had control ID collisions causing input to be read from the wrong dialog element. Word wrap required RichEdit-specific EM_SETTARGETDEVICE messages, not just style bit manipulation. Font application needed simplification to CreateFontIndirectW plus WM_SETFONT.

Each fix was targeted and minimal. OpenCode did not rewrite modules or refactor architecture; it patched specific failure points in the existing code structure. This approach preserved the work of previous agents and minimized regression risk.

Lee's role during this phase shifted to active UAT tester. He ran the application after each build, reported concrete symptoms, and confirmed fixes. His final assessment: "brilliant — everything works." OpenCode made two commits and updated the AgentFlow documents to reflect the stabilized state.

---

## Key Observations

### Human Role Consistency

Lee's role did not change across any of the three agent phases. With Antigravity, he oriented the agent, authorized work, and provided process improvements. With Kimi Code, he authorized work and provided a bug report. With OpenCode, he ran UAT cycles and reported symptoms. In every case, his interventions were at the level of direction, verification, and process — never implementation. The same supervisory skill set applied regardless of which AI platform was performing the work.

### Implementation Difficulty vs. Human Effort

The Zig/Win32 implementation involved manual memory management, packed struct type systems, Win32 message routing with LPARAM/WPARAM casting, sentinel-terminated string handling, and a rapidly evolving compiler with breaking API changes between versions. None of this complexity reached the human supervisor. Lee's effort was comparable to the prior C# WinForms implementation of the same application — a stack where the framework handles most of these concerns automatically. The implementation difficulty was absorbed by the agents.

### Agent Autonomy and Capability Variation

Each agent exhibited different strengths. Antigravity was methodical in build system troubleshooting and produced detailed handoff documentation. Kimi Code demonstrated aggressive parallelization, spawning concurrent subagents to accelerate implementation. OpenCode excelled at systematic debugging, tracing user-reported symptoms to root causes through code path analysis. These capability differences did not require the human to adapt his supervision approach — the AgentFlow artifacts provided a consistent interface regardless of agent behavior.

### Artifact Durability

The AgentFlow documents survived all three agent transitions intact. Each agent read them at session start and found them sufficient for orientation. The one failure — stale sprint status in context.md after Antigravity's unplanned exit — was resolved in ten minutes through source code inspection. The artifacts functioned as intended: durable project memory that any agent could use to reconstruct project state.

### Failure Modes and Recovery

The project encountered several failure modes. Credit exhaustion forced the Antigravity-to-Kimi transition mid-sprint. Stale documentation caused brief confusion at the Kimi entry point. Compilation errors from Zig API changes required fix waves in both the Kimi and OpenCode phases. Runtime bugs in Win32 message routing survived compilation and required iterative UAT to surface. In each case, recovery was straightforward because the artifacts maintained architectural coherence and the modular code structure limited the blast radius of individual bugs.

### Tool Independence

The three agents ran on entirely different platforms: Antigravity (Google DeepMind), Kimi Code (Moonshot AI CLI), and OpenCode (OpenAI GPT 5.3 Codex CLI). They used different underlying models, different tool interfaces, and different execution environments. The project succeeded regardless, because the coordination layer was the AgentFlow documents, not any platform-specific feature.

---

## Technical Outcomes

### Application Completion

LFZNotepad was delivered as a functional Windows Notepad clone with the following verified capabilities: file creation, open, save, and save-as with encoding support; full Edit menu operations (undo, cut, copy, paste, delete, select all, time/date insertion); modeless Find and Replace dialogs; modal GoTo Line dialog; word wrap toggling; font selection; zoom control (10–500%); keyboard shortcuts via accelerator table; four-part status bar with line/column tracking; and drag-and-drop file opening.

### Binary Characteristics

The debug build produced a binary of approximately 1.1 MB with zero runtime dependencies beyond the Win32 subsystem — compared to 154 MB for the C# WinForms version. The application comprises approximately 3,600 lines of Zig across 14 source files.

### Debugging Process

The OpenCode stabilization phase resolved six categories of bugs across two commit cycles. The bugs were characteristic of Win32 development: allocator ownership issues, message routing errors, control ID collisions, string termination requirements, and RichEdit-specific behavior that differs from standard Edit controls. These are precisely the types of platform-specific subtleties that challenge both human and AI developers, and their resolution demonstrates that iterative UAT with agent-driven debugging is a viable model for GUI application stabilization.

### Remaining Limitations

Print functionality was not fully verified at session end due to the absence of a printer in the test environment. A full regression sweep across all menus, hotkeys, and dialog edge cases was not completed. Encoding round-trip testing (writing UTF-16 LE, reopening, verifying content) was not performed. These represent standard QA items, not architectural gaps.

---

## Strategic Implications for Enterprises

### Workforce Productivity

This project demonstrates that a single practitioner operating in supervisor mode can produce a complete, multi-thousand-line desktop application in an unfamiliar technology stack in approximately 5 hours of active involvement. The practitioner's existing skills in architecture, verification, and project management transferred directly; deep Zig or Win32 expertise was not required. For enterprises, this implies a significant shift in how specialist knowledge is valued: the bottleneck moves from implementation expertise to supervisory and architectural judgment.

### Vendor and Model Portability

The project was completed across three different AI vendors and models without rework or significant friction. This has direct implications for enterprise procurement: organizations need not commit to a single AI coding platform. Artifact-driven methodologies like AgentFlow create a vendor-neutral coordination layer that protects against lock-in, pricing changes, and capability shifts.

### Reduced Stack Constraints

The choice of Zig and Win32 — a deliberately challenging stack — did not increase the human's workload. This suggests that technology stack decisions can be made primarily on technical merit (performance, binary size, deployment simplicity) rather than team familiarity, because the AI agents absorb the learning curve. Enterprises can choose optimal stacks without the traditional cost of retraining or hiring specialists.

### Resilience to Outages and Quotas

Agent transitions in this project were driven by both external constraints (credit exhaustion) and deliberate strategic decisions (switching to a stronger model for stabilization). In a production enterprise environment, similar constraints will include API rate limits, model availability, and vendor outages. The AgentFlow methodology proved resilient to these interruptions because the artifacts maintained state independently of any agent or platform session. Enterprises adopting agentic workflows should design for agent interchangeability from the start.

### Supervisory Skill Sets

The practitioner's effectiveness in this project derived from skills that most organizations already possess in their senior engineering and architecture roles: system design, quality verification, process management, and technical judgment. The transition to agentic development does not require exotic new competencies — it requires reframing existing competencies as the primary value contribution, with implementation becoming a delegated concern.

---

## Lessons Learned

### What Worked

- **Document-first planning:** Every agent cited the AgentFlow artifacts as their primary source of orientation. The time invested in planning artifacts before any implementation began paid dividends at every transition.

- **Modular architecture:** The separation of concerns (helpers by function, dialogs by dialog, centralized menu IDs) allowed the OpenCode agent to patch individual modules without understanding the entire codebase.

- **Explicit definitions of done:** Sprint completion criteria made verification objective. When file operations crashed, the bug report was grounded in the sprint's definition of done, not in ambiguous expectations.

- **Supervisor-mode discipline:** Lee's consistent refusal to drop into implementation mode kept each session focused and prevented the scope creep that occurs when the supervisor starts "helping" with code.

- **Strategic agent switching:** The deliberate transition from Kimi Code to OpenCode demonstrated that supervisors can and should make active platform choices based on observed agent performance, not just accept whatever tool they started with.

### What Failed

- **Deferred documentation updates:** Antigravity's session ended before context.md could be updated, causing stale state that confused the next agent. Documentation should be updated continuously, not batched at sprint end.

- **Outdated API examples in design documents:** The design.md build.zig example used the Zig 0.13 API, costing time when the agent used it as a template for 0.15. Planning artifacts should include version pinning for code examples.

- **Insufficient runtime testing before handoff:** Kimi Code reported features as complete based on compilation success and dialog-level verification. Actual runtime testing by the human supervisor revealed significant instability, requiring a full stabilization phase.

### What Surprised

- **Agent transitions were less disruptive than expected.** Each agent required minimal ramp-up time. The AgentFlow artifacts provided enough context that no agent asked clarifying questions about architecture or approach.

- The parallel subagent capability in Kimi Code accelerated implementation substantially. Components that would have been sequential in a single-threaded agent workflow were developed concurrently, suggesting that agentic development has its own parallelism advantages beyond what human teams offer.

- Win32 bugs were remarkably consistent across agents. The null-termination bug, the message routing errors, and the accelerator table omission are all classic Win32 development mistakes. The AI agents were not immune to platform-specific subtleties, but the iterative UAT model caught and resolved them efficiently.

### Improvement Opportunities

Future projects should implement continuous document updates (updating context.md after each significant action, not at sprint completion). Design documents should version-pin all code examples. A structured handoff checklist should be added to AGENTS.md for unplanned session terminations. Runtime verification should be required before any sprint is marked complete, with explicit "Test As User" gates in the sprint plan.

---

## Conclusion

LFZNotepad stands as evidence that agentic development has moved beyond proof-of-concept into practical application. A single practitioner — one who designed the methodology, architected the experiment, and led execution without writing a line of implementation code — produced a functional desktop application across three AI platforms in 5 hours of active work. The human's effort did not scale with implementation complexity. The methodology survived both forced and deliberate agent transitions. The artifacts held.

The primary constraint in software creation is shifting from implementation to supervision, architecture, and verification. What this case study demonstrates is that the practitioner who understands this shift — and has built the methodology to operationalize it — can deliver outcomes that would traditionally require a specialized engineering team. Lee's value in this project was not the ability to write Zig or navigate Win32 APIs. It was the ability to design a repeatable methodology, set up the right conditions for agent success, make load-bearing decisions under uncertainty, and maintain coherent direction across platforms and sessions.

These are the skills of a trusted advisor in Generative AI: not someone who uses AI tools, but someone who designs the systems that make AI tools productive at scale.

For enterprises evaluating agentic development workflows, this case study offers a specific, reproducible model: invest in artifact-driven methodologies, design for agent interchangeability, prioritize supervisory skill development, and treat AI coding agents as interchangeable execution resources coordinated through durable documentation. The returns — in velocity, portability, and resilience — are substantial and demonstrable. The starting point is a practitioner who knows how to set this up.

---

*This case study was synthesized from raw session documents produced by each AI agent. All factual claims are grounded in the source material. Imperfections and limitations are preserved where instructive.*
