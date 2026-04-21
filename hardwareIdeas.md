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

**Status key:** ✅ = selected for further exploration · ❌ = not selected (preserved for future revisit).

---

## ✅ 1. HDMI dongle for on-demand digital signage

**Tier:** 🟢 Feasible today
**Primary category:** CLI / Coding (85) + creative generation

A small HDMI stick that plugs into any TV and generates digital signage on demand from natural-language prompts ("menu for today's specials", "waiting-room greeting", "conference Room 3, Q2 all-hands at 10AM"). Uses Claude Design or a similar structured-layout generator.

### Why it's easy

- One-shot generation — no long-horizon planning, no state to maintain.
- Output is visually verifiable by a human.
- Task scope is bounded — rectangular canvas, short text, optional images.
- **The hardware can be trivially dumb: a media player (image / video / animation) pulling pre-rendered AI assets from cloud storage.** No browser, no live rendering, no HTML runtime on the dongle. Generation happens upstream in the cloud; the dongle just plays what's queued.

### What this unlocks for the customer

- **Designer-quality output without hiring a designer.** Non-designers (shop owner, office manager, event host) can produce layouts that look professionally made. The model handles typography, hierarchy, color, whitespace.
- **Motion content, not just static HTML.** Because generation is decoupled from display, the dongle can play anything the cloud can render — short video loops, animated transitions, Ken Burns motion on photos — on the same cheap hardware.
- **Real-time on-premise correction.** Staff standing in front of the TV can say *"bigger headline"*, *"swap to today's soup"*, or *"add a QR code to the lunch menu"*. The cloud regenerates the asset, pushes to the dongle, display updates within seconds.
- **Context-aware content refresh.** Tie it to time/day/occupancy and the signage adapts itself: lunch specials auto-switch to dinner, meeting rooms update their "next session" card, waiting rooms rotate through relevant messages.
- **Cheap to scale.** Generation happens once per variation and caches on a CDN. A 50-location chain pays for one render, not 50. Fleet-wide refreshes are just cache invalidations.

### Input methods — how users request content

Three options, each with distinct hardware and UX tradeoffs:

1. **Voice via microphone on the dongle** — most natural UX (*"waiting-room greeting, bigger headline"*). Requires mic hardware + an audio pipeline — higher BOM, more privacy surface.
2. **Dedicated email or WhatsApp account per dongle** — user texts or emails the dongle's assigned address (*"menu for today's special: arugula salad"*). No mic needed; hardware stays trivially dumb. Text is explicit, auditable, asynchronous — convenient enough for most cases.
3. **Companion web app or mobile app** — structured form with templates, media upload, scheduling. Most control, heaviest UX.

**Recommended MVP: option 2 (email / WhatsApp).** Cheapest hardware, fastest to ship, and fits how small-business staff already communicate. Voice ships as v2 once fleet volume justifies the mic BOM.

Go-to-market: restaurants, conference rooms, retail, small offices, real-estate open houses, clinics, coworking lobbies.
MVP risk: low — the hardest engineering is the content-generation pipeline and input routing, not autonomy.

---

## ✅ 2. KVM-style gadget: agent-as-user for computers / iPad / iPhone

**Tier:** 🟠 mixed — individual tasks range from 🟢 Feasible to 🔴 Experimental
**Primary categories:** GUI / Computer Use (42), Spreadsheet (58), Communication (38)

A USB-C or hardware KVM bridge that attaches to a user's computer, iPad, or iPhone and lets a remote agent act as the human — moving the cursor, typing, tapping, taking screenshots. The hardware is the cheap part; the capability ladder is what determines feasibility.

### Capability ladder (expanded)

| Task | Tier | Why |
|---|---|---|
| **iMessage / text autonomous replies** | 🟢 Feasible | Proven to work. Short messages, known recipients, routine content. Agent drafts and sends. |
| **Chess autonomously** | 🟢 Feasible | Bounded state space. Chess engines solve the strategy; the agent just drives the UI. |
| **File organization** (rename, sort, tag by content) | 🟢 Feasible | Structured, tool-use-like. Agent reads filenames and metadata and acts. |
| **Batch-process Excel files** | 🟠 Ambitious | Spreadsheet 58. Works for clean data; brittle on messy real-world files. |
| **Form filling** (tax, insurance, onboarding) | 🟠 Ambitious | Tool-use + GUI. Standard forms fine; long multi-page flows trickier. |
| **Computer troubleshooting** (disk cleanup, BSOD, viruses) | 🟠 Ambitious | Multi-step GUI across admin tools. Needs guardrails on destructive actions. |
| **Meeting setup** (join Zoom, share screen, audio control) | 🟠 Ambitious | GUI varies across Zoom/Meet/Teams; small action space but many edge cases. |
| **Software install + configuration wizards** | 🟠 Ambitious | Structured GUI flow with branching prompts. |
| **Photo editing** (Lightroom, Photoshop) | 🟠 Ambitious | GUI 42. Recipe-following works; taste/iteration unreliable. |
| **3D printing slicer operations** | 🔴 Experimental | GUI + narrow domain software. Few training examples; errors are physical. |
| **Social media content + replies** | 🔴 Experimental | Communication 38. Drafting works; tone/brand judgment unreliable. |
| **Video editing** (DaVinci, Premiere) | 🔴 Experimental | GUI + creative judgment + long workflows. Demos exist; production reliability doesn't. |

**Design implication:** sell the KVM with the green-tier tasks as the anchor value prop — iMessage, chess, file organization. Position orange-tier as "works with oversight" (troubleshooting, forms, Excel, meeting joins). Frame red-tier as "preview / beta" until scores climb. Users who trust the gadget with iMessage today will trust it with photo editing next year as the ladder fills in.

---

## ❌ 3. ~~Autonomous coding box for overnight dev work~~ · _Not selected_

**Tier:** 🟢 Feasible today
**Primary category:** CLI / Coding (85)

A small appliance (Raspberry Pi-class, or even just a container) wired into a developer's repo with scoped SSH + GitHub credentials. At day's end, the developer queues issues. The box grinds through overnight and opens PRs for review in the morning.

Why it works:
- SWE-bench Verified at 87.6% — the hardest benchmark-measured coding work is largely solved.
- Scope is naturally constrained (repo + issue tracker).
- Review gate (PR) mirrors exactly the pattern humans already use.

Hardware angle: the differentiation isn't the silicon — it's the trust story of "the box has only your keys, runs in your home, never sees the public internet except for package installs."

---

## ❌ 4. ~~Home office API hub~~ · _Not selected_

**Tier:** 🟢 Feasible today
**Primary category:** API / Tool Use (72)

A small on-desk device that unifies calendar, email, Slack, and smart-home APIs behind one voice/chat interface. Acts like a dedicated-hardware Siri/Alexa but backed by a modern agent with tool-use calibration. Unlike phone-based assistants, it's always-on, private-by-default, and the user owns the tool roster.

Why it works: tool-use at 72 is strong for bounded, structured actions. "Schedule a 1:1 with Jeff, 30 min, this week, afternoon preferred" is exactly the kind of task current agents handle reliably.

Differentiation: physical embodiment + privacy + tool-roster curation. Not a moonshot — the software is 80% there; the product is 100% about packaging.

---

## ❌ 5. ~~Spreadsheet appliance for small-business ops~~ · _Not selected_

**Tier:** 🟠 Ambitious
**Primary category:** Spreadsheet / Files (58)

Desk-top display + microphone that sits next to the shop POS or office desk. Speak an update ("3 more crates of apples came in, same supplier"), and it reconciles the inventory / bookkeeping / reorder-point spreadsheets. Shows a running summary on the display.

Why ambitious:
- Spreadsheet at 58 means it works on clean structured data but fumbles on messy real-world files.
- Small-business spreadsheets are famously messy.
- MVP would require heavy templating — users get pre-built schemas, not free-form.

Edge: the display-plus-voice form factor sidesteps the GUI reliability gap (42) by doing the work server-side against the data file, not by driving Excel or Google Sheets GUIs.

---

## ❌ 6. ~~Research appliance for long-form web dives~~ · _Not selected_

**Tier:** 🟠 Ambitious
**Primary category:** Web / Browser (55), BrowseComp-class tasks

A desktop device that runs overnight persistent-search tasks: "research all publicly available info about Company X's new product line, summarize, save sources". Built around the BrowseComp-type workload (hard persistent-search questions).

Why ambitious: Browser at 55 is useful but brittle. Anti-bot defenses, auth walls, paywalls, and dynamic JS can break runs. Best for research against open/archive sources, not enterprise dashboards.

Differentiation: a dedicated long-running agent is much better than "ask ChatGPT" for jobs that take hours and require source tracking.

---

## ❌ 7. ~~Exec assistant earpiece~~ · _Not selected_

**Tier:** 🔴 Experimental
**Primary category:** Communication / Assistant (38)

Earbud + lapel mic that listens to the user's day, offers whispers ("you have a call with Alex in 5 min, last time he brought up budget"), drafts replies to Slack/email, handles calendar requests. Human-in-the-loop on anything outgoing.

Why experimental: TheAgentCompany scores ~30-39% on autonomous task completion. An always-on assistant for a real executive is beyond current reliability. Workable as a *copilot* (suggests, never sends), not as an autonomous agent.

Honest framing: market this as "extended memory + suggestion engine" rather than "your AI Chief of Staff". Drafting assistance is genuinely good; autonomy isn't.

---

## ❌ 8. ~~Household task robot (narrow scope)~~ · _Not selected_

**Tier:** 🔴 Experimental
**Primary category:** Device / Robotics (22)

A robot that does one or two bounded household tasks reliably — e.g. folds laundry pulled from a bin, or loads a dishwasher from a pre-arranged rack. Not a general-purpose humanoid.

Why experimental: EmbodiedBench best-model at 28.9%, BEHAVIOR challenge winning entries under 20%. Robotics is the least mature category by a wide margin. Anything claiming broader scope is science fiction for now.

What's viable instead: bounded single-task robots (bed-making, pet-feeding, specific cooking steps) where the physical environment can be standardized.

---

## Cross-cutting product patterns

These apply across difficulty tiers:

1. **Hardware should narrow the problem, not expand it.** An HDMI stick bounds the task to "generate signage"; a general-purpose tablet invites users to expect everything. Constraint creates reliability.

2. **Review gates ship features, not autonomy.** Every orange/red-tier task works if you add a "review this" step. The same tech that's embarrassing autonomously is useful as a copilot.

3. **Privacy + on-device is a real story for 2026.** "Your keys, your data, physical hardware you can see" is a differentiator against cloud-only tools — especially in SMB/enterprise.

4. **Pair a strong capability with a weak one.** The signage dongle succeeds because creative generation is mature AND the task is bounded to a single rectangle. The KVM gadget's ladder exists because each task mixes mature categories (tool-use) with weaker ones (GUI). Design around the weak link.

5. **Scores climb ~10-15 pts/year on active benchmarks.** Something in the red tier today may be orange in 18 months. Build a product that gets *better* as the underlying models improve, not one that has to be rebuilt.

---

## Next steps

- Prototype plan: #1 HDMI signage (MVP scope + pilot customers) + #2 KVM capability ladder (pick the first 3 green-tier tasks to ship as v1).
- For each selected idea, draft a one-page spec: inputs, outputs, failure modes, MVP scope, pricing hypothesis.
- Revisit the ❌ list quarterly as maturity scores shift — the underlying category scores will change and some may move back into consideration.
