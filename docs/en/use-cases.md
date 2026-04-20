---
layout: page
title: Example Use Cases
permalink: /en/use-cases/
lang: en
page_type: use_cases
---

# Example Use Cases

Hardware product ideas where an AI agent bridges into the physical or display world. Each idea is scored with a **feasibility tier** grounded in the category maturity scores on [Current Progress]({{ '/en/' | relative_url }}):

- 🟢 **Feasible today** — core categories score ≥ 70; ship this year with standard safeguards.
- 🟠 **Ambitious** — core categories score 40-69; useful MVP, expect polish pain and narrow scope.
- 🔴 **Experimental** — core categories score < 40; cool demo, treat autonomy claims with suspicion.

---

## 1. HDMI dongle for on-demand digital signage {% include mat-badge.html score=85 %}
{:#signage}

**Primary category:** CLI / Coding (85) + creative generation

A small HDMI stick that plugs into any TV and generates digital signage on demand from natural-language prompts — "menu for today's specials", "waiting-room greeting", "Room 3, all-hands at 10AM". Uses structured-layout generators like Claude Design.

**Why it's easy**
- One-shot generation — no long-horizon planning, no state to maintain.
- Output is visually verifiable by a human.
- Task scope is bounded: rectangular canvas, short text, optional images.
- The hardware is dumb — it's just a browser pointed at a rendering endpoint.

**Go-to-market:** restaurants, conference rooms, retail, small offices, real-estate open houses.

---

## 2. KVM gadget: agent-as-user for computer / iPad / iPhone {% include mat-badge.html score=55 %}
{:#kvm}

**Primary categories:** GUI / Computer Use (42), Spreadsheet (58), Communication (38)

A USB-C or hardware KVM bridge that attaches to a user's computer, iPad, or iPhone and lets a remote agent act as the human — moving the cursor, typing, tapping, taking screenshots. The hardware is cheap; the **capability ladder** below determines what's actually feasible.

| Task | Tier | Why |
|---|---|---|
| Play Chess autonomously | 🟢 Feasible | Bounded state space. Chess engines solve the strategy; the agent just drives the UI. |
| Batch-process Excel files | 🟠 Ambitious | Spreadsheet category at 58. Works for clean data; brittle on messy real-world files. |
| Photo editing (Lightroom, Photoshop) | 🟠 Ambitious | GUI use at 42. Agents can follow a recipe but struggle with taste and iteration. |
| 3D printing slicer operations | 🔴 Experimental | GUI + narrow domain software. Few training examples; errors are physical. |
| Social media content + replies | 🔴 Experimental | Communication at 38. Drafting works; judgment about tone and context is unreliable. |
| Video editing (DaVinci, Premiere) | 🔴 Experimental | GUI + creative judgment + long workflows. Demos exist; production reliability does not. |

**Design implication:** sell the KVM with the chess / spreadsheet tier as the anchor value prop. Position creative tasks as "preview" or "beta" rather than shipped autonomy. Users who trust it with Excel today will trust it with photos next year as scores climb.

---

## 3. Autonomous coding box for overnight dev work {% include mat-badge.html score=85 %}
{:#coding-box}

**Primary category:** CLI / Coding (85)

A small appliance (Raspberry Pi-class) wired into a developer's repo with scoped SSH + GitHub credentials. At day's end, the developer queues issues. The box grinds overnight and opens PRs for morning review.

**Why it works**
- SWE-bench Verified at 87.6% — the hardest benchmark-measured coding is largely solved.
- Scope is naturally constrained (repo + issue tracker).
- Review gate (PR) mirrors exactly how humans already work.

Hardware differentiation isn't the silicon — it's the trust story: *"the box has only your keys, runs in your home, never sees the public internet except for package installs."*

---

## 4. Home office API hub {% include mat-badge.html score=72 %}
{:#api-hub}

**Primary category:** API / Tool Use (72)

An always-on desk device that unifies calendar, email, Slack, and smart-home APIs behind one voice or chat interface. A dedicated-hardware agent with tool-use calibration — private-by-default, with a user-curated tool roster.

**Why it works**
- Tool-use at 72 is strong for bounded, structured actions.
- "Schedule a 1:1 with Jeff, 30 min, this week, afternoon preferred" is exactly what agents handle reliably today.

Differentiation: physical embodiment + privacy + curated tools. The software is 80% there; the product is 100% about packaging.

---

## 5. Spreadsheet appliance for small-business ops {% include mat-badge.html score=58 %}
{:#spreadsheet-appliance}

**Primary category:** Spreadsheet / Files (58)

Desk-top display + microphone next to the shop POS or office desk. Speak an update ("3 more crates of apples came in, same supplier"), and it reconciles inventory, bookkeeping, and reorder-point spreadsheets. Running summary on the display.

**Why ambitious**
- Spreadsheet at 58 works on clean structured data but fumbles on messy real-world files.
- Small-business spreadsheets are famously messy.
- MVP requires heavy templating — users get pre-built schemas, not free-form.

Edge: the display-plus-voice form factor sidesteps the GUI reliability gap (42) by editing data files server-side, not by driving Excel or Google Sheets GUIs.

---

## 6. Research appliance for long-form web dives {% include mat-badge.html score=55 %}
{:#research-box}

**Primary category:** Web / Browser (55), BrowseComp-class tasks

A desktop device that runs overnight persistent-search tasks: *"research all public info about Company X's new product line, summarize, save sources"*. Built around BrowseComp-type workloads — hard questions where the answer is out there but requires persistent hunting.

**Why ambitious**
- Browser at 55 is useful but brittle.
- Anti-bot defenses, auth walls, paywalls, and dynamic JS can break runs.
- Best for open / archive sources, not enterprise dashboards behind auth.

Differentiation: a dedicated long-running agent is much better than "ask ChatGPT" for jobs that take hours and require source tracking.

---

## 7. Exec assistant earpiece {% include mat-badge.html score=38 %}
{:#earpiece}

**Primary category:** Communication / Assistant (38)

Earbud + lapel mic that listens through the user's day, offers whispers ("call with Alex in 5 min, last time he brought up budget"), drafts Slack/email replies, handles calendar requests. Human-in-the-loop on anything outgoing.

**Why experimental**
- TheAgentCompany scores ~30-39% on autonomous task completion.
- An always-on assistant for a real exec is beyond current reliability.
- Works as a **copilot** (suggests, never sends) — not as an autonomous agent.

Honest framing: market this as "extended memory + suggestion engine", not "your AI Chief of Staff". Drafting assistance is genuinely good; autonomy isn't.

---

## 8. Narrow-scope household task robot {% include mat-badge.html score=22 %}
{:#robot}

**Primary category:** Device / Robotics (22)

A robot that does one or two bounded household tasks reliably — folds laundry from a bin, or loads a dishwasher from a pre-arranged rack. Not a general-purpose humanoid.

**Why experimental**
- EmbodiedBench best-model at 28.9%; BEHAVIOR challenge winning entries under 20%.
- Robotics is the least mature category by a wide margin.
- Anything claiming broader scope is science fiction for now.

What *is* viable: bounded single-task robots (bed-making, pet-feeding, specific cooking steps) where the physical environment can be standardized.

---

## Cross-cutting patterns

1. **Hardware should narrow the problem, not expand it.** An HDMI stick bounds the task to "generate signage"; a general tablet invites users to expect everything. Constraint creates reliability.

2. **Review gates ship features, not autonomy.** Every orange/red-tier idea above works if you add a "review this" step. The same tech that's embarrassing autonomously is useful as a copilot.

3. **Privacy + on-device is a real story for 2026.** "Your keys, your data, physical hardware you can see" differentiates against cloud-only tools — especially in SMB and enterprise.

4. **Pair a strong category with a weak one.** The spreadsheet appliance survives because it uses the ambitious spreadsheet tier but sidesteps the weak GUI tier by editing data files server-side. Design around the weak link.

5. **Scores climb ~10-15 pts/year on active benchmarks.** Something red today may be orange in 18 months. Build a product that gets *better* as models improve, not one that has to be rebuilt.
