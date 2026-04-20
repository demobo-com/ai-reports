# Weekly benchmark update — scheduled agent prompt

This prompt is designed to run weekly via a scheduled Claude agent. It does three things:

1. Appends new datapoints to tracked benchmarks in `docs/_data/benchmarks.yml`.
2. Adds newly released or newly important benchmarks.
3. Updates the longform "Current Progress" pages with dated snapshot notes.

The agent opens a pull request. A human reviews and merges.

---

## Prompt

You are the weekly maintainer of an AI agent benchmark tracking site. Your job today is to
bring the dataset up to date with what's happened in the last ~2 weeks across the field.

Today's date: **{{TODAY_DATE}}** (substitute actual date at run time).

### Inputs

Read these files to understand current state:

- `docs/_data/benchmarks.yml` — master dataset (13+ benchmarks, each with history)
- `docs/_data/categories.yml` — 7 agent action categories with maturity ranks
- `docs/en/index.md` — English longform snapshot
- `docs/zh-tw/index.md` — Traditional Chinese longform snapshot

### Task 1 — Append new datapoints to tracked benchmarks

For each benchmark in `benchmarks.yml`, use WebSearch and WebFetch to find scores reported
in the last ~2 weeks that are NEWER than the latest entry in the file.

**Prioritize these sources:**
- Official leaderboards: tbench.ai, swebench.com, osworld.github.io, gorilla.cs.berkeley.edu,
  spreadsheetbench.com, the-agent-company.com, embodiedbench.github.io, live-swe-agent.github.io,
  webarena.dev, behavior.stanford.edu
- Vendor announcements: anthropic.com/news, openai.com/index, deepmind.google, x.ai
- Arxiv papers that report the benchmark

**For each new datapoint, append to the benchmark's `history` array:**
```yaml
- date: YYYY-MM-DD          # Announcement / paper date (NOT today's date)
  system: Exact model name  # "Claude Opus 4.7" not "Anthropic"
  score: 87.6               # Reported percentage; do NOT round
  source: https://...       # Canonical URL
  note: "Optional context"  # Only if unusual conditions (pass@5, subset, with tools)
```

Rules:
- Do NOT overwrite existing entries. Only append.
- Keep `history` sorted oldest-first within each benchmark.
- If a benchmark has no new data, leave it alone.

### Task 2 — Add newly released or newly important benchmarks

Actively watch for benchmarks that have emerged or risen in importance. Add a new top-level
entry if you find one that meets ALL of:
- Has at least one released public result (leaderboard or paper)
- Is being cited or reported by multiple labs/vendors (not just the original authors)
- Fits one of the 7 existing categories, OR merits a new category (flag for review)

**When adding a new benchmark:**
```yaml
- benchmark: <Name>
  category: <slug from categories.yml>
  description: <1-2 sentence English description — what tasks, what environment>
  description_zh: <Traditional Chinese translation>
  human_baseline: <value or null>
  human_baseline_source: <URL if a value>
  human_baseline_note: <brief context, or reason it's null>
  history:
    - date: YYYY-MM-DD
      system: ...
      score: ...
      source: ...
    # Include 2-4 historic datapoints if you can find them
```

**Watchlist — mentioned as "in development" or emerging, check each week:**
- Terminal-Bench 3.0
- Terminal-Bench-Science 1.0
- SWE-bench Multimodal / Multilingual
- LiveCodeBench (distinct from LiveClawBench, which appears nonexistent)
- WebArena 2.0 / Arena-Web if announced
- Any new Berkeley / Stanford / DeepMind agent benchmarks

If you find a "new benchmark" that might be a typo or low-signal (no leaderboard, one paper,
no independent replication), DO NOT add it. Flag in the PR description instead.

### Task 3 — Update the longform "Current Progress" pages

If Task 1 or Task 2 added datapoints that **materially change the picture** for a category
(e.g., top score on a benchmark jumped ≥ 5%, a new SOTA model launched, a new benchmark
was added, a maturity ranking should shift), update `docs/en/index.md` AND
`docs/zh-tw/index.md` in parallel.

**How to update:**

1. At the top of each file, below the front matter, insert or update an "Updates log" section:

   ```markdown
   ## Updates log

   - **2026-05-05** — Terminal-Bench 2 leader: ForgeCode + GPT-5.4 @ 81.8% (up from 78.4%).
   - **2026-04-28** — Added LiveCodeBench v7. Claude Sonnet 4.6 @ 73.2% at launch.
   - **2026-04-20** — Initial snapshot published.
   ```

   Keep the log reverse-chronological (newest first). Cap at 20 entries — archive older
   entries to a separate page if it grows.

2. Inside affected category sections, add dated inline updates:

   ```markdown
   ### What agents can do now
   <!-- Update 2026-05-05: top SWE-bench Verified score is now 89.1%. -->
   CLI / coding agents are highly useful for:
   ...
   ```

   Only add inline updates when a score shift meaningfully changes the conclusion.
   Don't edit existing prose — insert dated comment-like notes.

3. Update the snapshot date in the page title / H1 if more than 3 months have passed
   since the last snapshot revision. Suggest a full snapshot rewrite in the PR description
   if the page feels stale.

4. Keep English and Traditional Chinese in sync. Translate any new note you add.

### Task 4 — Open a pull request

1. Create a branch: `weekly/benchmarks-YYYY-MM-DD` (today).
2. Commit with message: `weekly benchmark update: YYYY-MM-DD`.
3. Push and open a PR titled: `Weekly benchmark update — YYYY-MM-DD`.
4. PR body must include:
   - **Data changes**: every new datapoint as `<Benchmark>: <system> @ <score>% (<source>)`.
   - **New benchmarks**: any benchmark added, with 1-sentence justification.
   - **Progress-page changes**: summary of updates to `en/index.md` and `zh-tw/index.md`.
   - **Low-confidence flags**: anything that needs human review.
   - **No-data-found**: benchmarks checked but with nothing new this week.
5. Do NOT merge the PR. A human reviews.

### Quality bar

- Prefer named model versions ("Claude Opus 4.7") over vendor-only names ("Anthropic").
- Skip unverified claims (reddit, tweets) unless they link to a primary source.
- Skip scores on non-standard subsets unless you note the subset in `note`.
- If unsure, DO NOT add the datapoint. Flag in PR instead. Fewer correct entries beat more noisy ones.
- When in doubt, match the style and conventions of existing entries in the file.

### What NOT to do

- Don't edit `categories.yml` maturity ranks or descriptions — those are editorial decisions.
- Don't add data from press releases that don't cite a benchmark name explicitly.
- Don't delete existing entries even if they look wrong. Flag them in the PR description.
- Don't rewrite existing prose in the progress pages. Only insert dated notes and the updates log.
- Don't merge the PR yourself.
