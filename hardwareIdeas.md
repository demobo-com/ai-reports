# AI Agent Hardware Brainstorm

Working document for product ideas where the hardware bridges AI agents into
the physical / display world. Each idea is tagged with a **feasibility tier**
anchored in the maturity scores from `docs/_data/categories.yml`:

| Tier | Color | Condition | Meaning |
|---|---|---|---|
| Feasible today | 🟢 green | core categories ≥ 70 | ship this year with standard safeguards |
| Ambitious | 🟠 orange | core categories 40–69 | useful MVP, expect polish pain and narrow scope |
| Experimental | 🔴 red | core categories < 40 | cool demo; treat autonomy claims with suspicion |

Reference scores as of 2026-04:

- CLI / Coding: **85** 🟢
- API / Tool Use: **72** 🟢
- Spreadsheet / Files: **58** 🟠
- Web / Browser: **55** 🟠
- GUI / Computer Use: **42** 🟠
- Communication / Assistant: **38** 🔴
- Device / Robotics: **22** 🔴

---

## Seed ideas

### 1. HDMI dongle for on-demand digital signage

**Tier:** 🟢 Feasible today
**Primary category:** CLI / Coding (85) + creative generation (outside our 7 categories but mature)

A small HDMI stick that plugs into any TV and generates digital signage on demand from natural-language prompts ("menu for today's specials", "waiting-room greeting", "conference Room 3, Q2 all-hands at 10AM"). Uses Claude Design or a similar structured-layout generator.

Why it's easy:
- One-shot generation — no long-horizon planning, no state to maintain.
- Output is visually verifiable by a human.
- Task scope is bounded — rectangular canvas, short text, optional images.
- The hardware is dumb: it's just a browser renderer pointed at a server endpoint.

Go-to-market: restaurants, conference rooms, retail, small offices, real-estate open-houses.
MVP risk: low — the hardest engineering is the content generation UX, not autonomy.

### 2. KVM-style gadget: agent-as-user for computers / iPad / iPhone

**Tier:** 🟠 mixed — individual tasks range from 🟢 Feasible to 🔴 Experimental
**Primary categories:** GUI / Computer Use (42), Spreadsheet (58), Communication (38)

A USB-C or hardware KVM bridge that attaches to a user's computer, iPad, or iPhone and lets a remote agent act as the human — moving the cursor, typing, tapping, taking screenshots. The hardware is the cheap part; the capability ladder is what determines feasibility.

Ranked by difficulty (from the maturity scores):

| Task | Tier | Why |
|---|---|---|
| **Play Chess autonomously** | 🟢 Feasible | Bounded state space; close to API/tool-use (72). Chess engines solve the hard part; the agent just drives the UI. |
| **Batch-process Excel files** | 🟠 Ambitious | Spreadsheet category at 58. Works for clean data + clear instructions; brittle on messy real-world files. |
| **Photo editing (Lightroom, Photoshop)** | 🟠 Ambitious | GUI use at 42. Agents can follow a recipe ("crop, adjust exposure, export") but struggle with taste/iteration. |
| **3D printing slicer operations** | 🔴 Experimental | GUI + narrow domain software (Cura, PrusaSlicer). Few training examples; errors are physical. |
| **Social media content + replies** | 🔴 Experimental | Communication at 38. Drafting works; judgment about tone, context, brand voice is unreliable. |
| **Video editing (DaVinci, Premiere)** | 🔴 Experimental | GUI + creative judgment + long workflows. Demos exist; production reliability does not. |

**Design implication:** sell the KVM with the chess / spreadsheet tier as the anchor value prop, and position the creative tasks as "preview" or "beta" rather than shipped autonomy. Users who trust the gadget with Excel today will trust it with photos next year as scores climb.

---

## Additional ideas

### 3. Autonomous coding box for overnight dev work

**Tier:** 🟢 Feasible today
**Primary category:** CLI / Coding (85)

A small appliance (Raspberry Pi-class, or even just a container) wired into a
developer's repo with scoped SSH + GitHub credentials. At day's end, the
developer queues issues ("fix #432", "add tests for auth module", "bump deps
and run CI"). The box grinds through overnight and opens PRs for review in the morning.

Why it works:
- SWE-bench Verified at 87.6% for top models — the hardest benchmark-measured coding work is largely solved.
- Scope is naturally constrained (repo + issue tracker).
- Review gate (PR) mirrors exactly the pattern humans already use.

Hardware angle: the differentiation isn't the silicon — it's the trust story of "the box has only your keys, runs in your home, never sees the public internet except for package installs."

### 4. Home office API hub

**Tier:** 🟢 Feasible today
**Primary category:** API / Tool Use (72)

A small on-desk device that unifies calendar, email, Slack, and smart-home APIs behind one voice/chat interface. Acts like a dedicated-hardware Siri/Alexa but backed by a modern agent with tool-use calibration. Unlike phone-based assistants, it's always-on, private-by-default, and the user owns the tool roster.

Why it works: tool-use at 72 is strong for bounded, structured actions. "Schedule a 1:1 with Jeff, 30 min, this week, afternoon preferred" is exactly the kind of task current agents handle reliably.

Differentiation: physical embodiment + privacy + tool-roster curation. Not a moonshot — the software is 80% there; the product is 100% about packaging.

### 5. Spreadsheet appliance for small-business ops

**Tier:** 🟠 Ambitious
**Primary category:** Spreadsheet / Files (58)

Desk-top display + microphone that sits next to the shop POS or office desk. Speak an update ("3 more crates of apples came in, same supplier"), and it reconciles the inventory / bookkeeping / reorder-point spreadsheets. Shows a running summary on the display.

Why ambitious:
- Spreadsheet at 58 means it works on clean structured data but fumbles on messy real-world files.
- Small-business spreadsheets are famously messy.
- MVP would require heavy templating — users get pre-built schemas, not free-form.

Edge: the display-plus-voice form factor sidesteps the GUI reliability gap (42) by doing the work server-side against the data file, not by driving Excel or Google Sheets GUIs.

### 6. Research appliance for long-form web dives

**Tier:** 🟠 Ambitious
**Primary category:** Web / Browser (55), BrowseComp-class tasks

A desktop device that runs overnight persistent-search tasks: "research all publicly available info about Company X's new product line, summarize, save sources". Built around the BrowseComp-type workload (hard persistent-search questions).

Why ambitious: Browser at 55 is useful but brittle. Anti-bot defenses, auth walls, paywalls, and dynamic JS can break runs. Best for research against open/archive sources, not enterprise dashboards.

Differentiation: a dedicated long-running agent is much better than "ask ChatGPT" for jobs that take hours and require source tracking.

### 7. Exec assistant earpiece

**Tier:** 🔴 Experimental
**Primary category:** Communication / Assistant (38)

Earbud + lapel mic that listens to the user's day, offers whispers ("you have a call with Alex in 5 min, last time he brought up budget"), drafts replies to Slack/email, handles calendar requests. Human-in-the-loop on anything outgoing.

Why experimental: TheAgentCompany scores ~30-39% on autonomous task completion. An always-on assistant for a real executive is beyond current reliability. Workable as a *copilot* (suggests, never sends), not as an autonomous agent.

Honest framing: market this as "extended memory + suggestion engine" rather than "your AI Chief of Staff". Drafting assistance is genuinely good; autonomy isn't.

### 8. Household task robot (narrow scope)

**Tier:** 🔴 Experimental
**Primary category:** Device / Robotics (22)

A robot that does one or two bounded household tasks reliably — e.g. folds laundry pulled from a bin, or loads a dishwasher from a pre-arranged rack. Not a general-purpose humanoid.

Why experimental: EmbodiedBench best-model at 28.9%, BEHAVIOR challenge winning entries under 20%. Robotics is the least mature category by a wide margin. Anything claiming broader scope is science fiction for now.

What's viable instead: bounded single-task robots (bed-making, pet-feeding, specific cooking steps) where the physical environment can be standardized.

---

## Cross-cutting product patterns

These apply across difficulty tiers:

1. **Hardware should narrow the problem, not expand it.** An HDMI stick bounds the task to "generate signage"; a general-purpose tablet invites users to expect everything. Constraint creates reliability.

2. **Review gates ship features, not autonomy.** Every orange/red-tier idea above works if you add a "review this" step. The same tech that's embarrassing autonomously is useful as a copilot.

3. **Privacy + on-device is a real story for 2026.** "Your keys, your data, physical hardware you can see" is a differentiator against cloud-only tools — especially in SMB/enterprise.

4. **Pair a strong category with a weak one.** Idea 5 (spreadsheet appliance) survives because it uses the ambitious spreadsheet tier but avoids the weak GUI tier by editing data files server-side. Design around the weak link.

5. **Scores climb ~10-15 pts/year on active benchmarks.** Something in the red tier today may be orange in 18 months. Build a product that gets *better* as the underlying models improve, not one that has to be rebuilt.

---

## Next steps

- Pick 2–3 ideas above to prototype (recommend: #1 HDMI signage + #3 autonomous coding box — both are green-tier and commercially distinct).
- For each, draft a one-page spec: inputs, outputs, failure modes, MVP scope.
- Revisit this file quarterly as maturity scores shift.
