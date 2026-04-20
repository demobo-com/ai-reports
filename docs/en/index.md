---
layout: page
title: Current Progress (2026 Snapshot)
permalink: /en/
lang: en
page_type: progress
---

# Agent Technology Progress by Category (2026 Snapshot)

This document summarizes the current state of major agent action categories, including benchmark coverage, what systems can do today, and where they still fail. Sections are ordered by practical production maturity (most mature first), matching the [Benchmark dashboard]({{ '/en/benchmark/' | relative_url }}). Each section header shows a 0-100 maturity score (green ≥ 70, orange 40-69, red < 40).

## Updates log

<!-- Newest entries first. Weekly agent appends here. Cap at ~20 entries; archive older ones if the list grows long. -->

- **2026-04-20** — Initial snapshot published. Seed data: 15 benchmarks across 7 categories, ~85 datapoints spanning 2023-07 through 2026-04.

## Executive Summary

As of 2026, the strongest agent categories are **CLI / coding agents** and **structured API / tool-use agents**. These are the most production-ready because they operate in relatively constrained environments with clearer feedback loops and easier verification.

**Browser use** and **spreadsheet / file workflows** have improved substantially, but they are still less reliable than coding or structured tool execution.

**Desktop GUI / computer use**, **communication-heavy assistant workflows**, and **robot / device control** remain meaningfully behind. They can produce impressive demos, but long-horizon reliability is still limited.

A critical caveat: benchmark scores across categories are **not directly comparable**. A 70% score on a spreadsheet benchmark does not imply the same level of capability as a 70% score on OSWorld or SWE-bench.

---

## 1. CLI / Coding {% include mat-badge.html score=85 %}
{:#cli_coding}

### Main benchmarks
- **SWE-bench Verified**
- **SWE-bench-Live**
- **Terminal-Bench** / **Terminal-Bench 2**

These benchmarks evaluate coding agents on real software engineering tasks, such as fixing bugs, writing tests, resolving GitHub issues, and completing Linux terminal tasks.

As of 2026, this is the strongest agent category.
The official SWE-bench leaderboard has shown top systems reaching the high-80% range, with Claude Opus 4.7 at **87.6% on SWE-bench Verified**. Terminal-Bench 2 tops around 80% for the best GPT-5.4 / ForgeCode combinations.

### What agents can do now
CLI / coding agents are highly useful for:
- fixing localized bugs
- writing or updating tests
- performing repo-wide refactors
- running commands and interpreting outputs
- diagnosing build failures
- making iterative code changes
- resolving many real-world repository issues

### What they still cannot do reliably
They still struggle with:
- vague product requirements
- deep architectural redesign
- poorly instrumented environments
- flaky builds
- hard bugs that require business context
- large cross-system debugging problems
- nonstandard platforms or setup environments

### Bottom line
CLI / coding agents are currently the most mature and commercially useful class of agents, especially with sandboxing and human review.

---

## 2. API / Tool Use {% include mat-badge.html score=72 %}
{:#tool_use}

### Main benchmarks
- **BFCL**
- **τ-bench**

BFCL measures raw function-calling accuracy.
τ-bench is more realistic: it tests multi-turn, policy-sensitive, tool-using agents in domains like retail and airline support.

### What agents can do now
Tool-using agents are good at:
- selecting the right tool from a constrained set
- extracting arguments from clear instructions
- chaining tools when workflows are explicit
- operating on structured systems such as CRM, SQL, search, calendar, and ticketing tools

### What they still cannot do reliably
They still struggle with:
- underspecified user requests
- policy-heavy decisions
- partial tool failures
- tracking state over long workflows
- consistency across repeated runs
- combining business rules, dialogue, and action execution

This is the major gap between "valid function calling" and "dependable operational work."

### Bottom line
Raw tool calling is already strong.
Reliable multi-step agentic tool use is much harder and still noticeably brittle.

---

## 3. Spreadsheet / Files {% include mat-badge.html score=58 %}
{:#spreadsheet}

### Main benchmark
- **SpreadsheetBench**

SpreadsheetBench evaluates end-to-end spreadsheet tasks such as debugging, modeling, and visualization across complex workbooks.

Reported public results include:
- **Gemini in Google Sheets: 70.48%**
- **Excel Agent Mode: 57.2%**
- **ChatGPT agent: 45.5%** in one reported spreadsheet-editing setup

### What agents can do now
Spreadsheet and file agents can often:
- read and summarize sheets
- write formulas
- repair some broken spreadsheets
- create visualizations
- perform multi-step workbook edits
- answer questions about structured files

### What they still cannot do reliably
They still struggle with:
- ambiguous spreadsheet conventions
- subtle financial assumptions
- highly messy real-world sheets
- presentation polish
- refreshability and auditability
- preserving human-understandable logic in complex models

For documents, PDFs, and slides, benchmark coverage is less mature than in coding or browser tasks. Capabilities are real, but evaluation remains fragmented.

### Bottom line
Spreadsheet and file workflows are becoming genuinely useful, but reliability varies a lot depending on structure and cleanliness of the source files.

---

## 4. Web / Browser {% include mat-badge.html score=55 %}
{:#browser}

### Main benchmarks
- **WebArena**
- **WebVoyager**
- **VisualWebArena**
- **BrowseComp**

These benchmarks evaluate web navigation, information gathering, search persistence, form completion, and visually grounded browsing tasks.

Reported results for OpenAI's CUA included:
- **58.1% on WebArena**
- **87.0% on WebVoyager**

WebVoyager has since been pushed above 97% by Surfer 2. BrowseComp is a harder benchmark aimed at persistent search and difficult-to-find answers on the live web; its humans baseline is only **29.2%**, reflecting how hard the questions are.

### What agents can do now
Browser agents are reasonably good at:
- navigating mainstream websites
- extracting specific information from multiple pages
- completing moderate-length web forms
- shopping comparison and information retrieval
- interacting with dashboards and CRUD-style systems
- handling moderately structured browsing workflows

### What they still cannot do reliably
They still struggle with:
- dynamic or visually complex sites
- authentication friction
- anti-bot barriers
- long branching workflows
- unclear intent
- combining context from multiple systems
- end-to-end checkout or booking tasks with many hidden constraints

A browser agent may succeed at finding candidate flights, but fail at the actual booking if loyalty rules, seat constraints, hidden fees, or policy nuances matter.

### Bottom line
Browser use is useful today, but reliability still drops sharply when workflows become long, ambiguous, or highly stateful.

---

## 5. Communication / Assistant {% include mat-badge.html score=38 %}
{:#assistant}

### Main benchmark proxies
- **TheAgentCompany**
- **τ-bench**

These benchmarks or benchmark-like environments capture parts of assistant work such as email, scheduling, communication, browsing, and multi-system coordination.

TheAgentCompany reported that the strongest agent in its setting completed only about **30%** of tasks autonomously.

### What agents can do now
They are useful for:
- inbox triage
- meeting preparation
- summarizing threads
- drafting responses
- extracting action items
- scheduling when constraints are explicit

### What they still cannot do reliably
They still struggle with:
- social nuance
- hidden preferences
- incomplete instructions
- cross-system coordination
- changing constraints mid-task
- memory and personalization consistency
- end-to-end executive-assistant style autonomy

### Bottom line
Communication-heavy agents are already useful as copilots, but they are not yet dependable as unsupervised assistants in messy real organizations.

---

## 6. GUI / Computer Use {% include mat-badge.html score=42 %}
{:#gui_computer}

### Main benchmark
- **OSWorld**

OSWorld evaluates real computer tasks across Ubuntu, Windows, and macOS, covering desktop applications, file manipulation, browser tasks, and cross-application workflows.

The original OSWorld paper reported:
- Human success rate: **72.36%**
- Best model at the time: **12.24%**

By 2026-Q1 the top model (Claude Opus 4.7) reaches **78.0% on OSWorld-Verified** — above the original human baseline, though the GUI / computer-use domain remains meaningfully less reliable than CLI or tool-use in practice.

### What agents can do now
Current computer-use agents can often complete:
- short desktop workflows
- basic file operations
- form filling
- copying information between apps
- straightforward settings changes
- visually simple multi-step procedures

These systems are increasingly usable when the UI is clean and the task has limited branching.

### What they still cannot do reliably
They still struggle with:
- GUI grounding
- ambiguous interface elements
- hidden state
- pop-ups and modal interruptions
- long workflows with recovery
- unexpected layout changes
- cross-app tasks where one mistake cascades

In practice, agents often click the wrong thing, fail to recover from an interruption, or lose context after an unexpected dialog.

### Bottom line
Computer use is real and improving quickly, but it is not yet reliable enough for broad unsupervised delegation on a general desktop.

---

## 7. Device / Robotics {% include mat-badge.html score=22 %}
{:#robotics}

### Main benchmarks
- **BEHAVIOR**
- **EmbodiedBench**

These evaluate embodied agents on household and robotic tasks involving navigation, planning, and manipulation.

EmbodiedBench reported that even the best tested model in that study, GPT-4o, achieved only:
- **28.9% average success**

### What agents can do now
Embodied agents can often:
- plan at a high level in simulation
- interpret instructions
- complete simple manipulation or navigation tasks
- produce impressive constrained demos

### What they still cannot do reliably
They still struggle with:
- dexterous low-level manipulation
- long-horizon execution
- partial observability
- compounding physical errors
- failed-grasp recovery
- sim-to-real transfer
- robust performance in unstructured real environments

### Bottom line
Robotics and device-control agents remain the least mature category in terms of general-purpose reliability.

---

## General Pattern Across Categories

### Agents are strongest when:
- goals are clear
- tools are bounded
- workflows are short to medium length
- intermediate states are verifiable
- policies and preferences are explicit

### Agents are weakest when:
- intent is underspecified
- workflows are long and branching
- state must be tracked across many steps
- recovery is required after failure
- multiple systems must be coordinated
- hidden preferences or business rules matter

---

## Final Takeaway

The honest 2026 picture is:

- Agents are no longer just demos.
- They are already valuable in constrained environments.
- They are still far from being universally reliable autonomous workers across the full digital and physical world.

For product builders, the best near-term opportunities remain in categories where goals, tools, and evaluation are constrained:
- coding
- structured tool use
- spreadsheets
- narrowly defined browser workflows
