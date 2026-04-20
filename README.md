# AI Agent Progress

Tracks AI agent technology progress across benchmarks and categories. Published as a
static site via GitHub Pages.

## Site structure

```
docs/                         # GitHub Pages source (Jekyll)
  _config.yml
  _data/
    benchmarks.yml            # Single source of truth for benchmark scores
    categories.yml            # Category metadata + maturity ranking
  _layouts/
    default.html              # Shared header/nav + language switcher
    page.html                 # Long-form article wrapper
  index.html                  # Dashboard — progress table by category
  en.md                       # English longform report (/en/)
  zh-tw.md                    # 繁體中文 longform report (/zh-tw/)
  assets/css/style.css

scripts/
  weekly_benchmark_update.md  # Prompt for the scheduled agent
```

## Enable GitHub Pages

1. Push this branch to GitHub.
2. Go to **Settings → Pages**.
3. Under **Source**, select **Deploy from a branch**.
4. Set **Branch** to `main` and folder to `/docs`. Save.
5. Wait ~30 seconds, then visit the URL shown on that page.

No GitHub Actions workflow is required — GitHub Pages builds Jekyll automatically
from the `/docs` folder.

## Run locally

```bash
cd docs
bundle init && bundle add jekyll
bundle exec jekyll serve --livereload
# open http://127.0.0.1:4000/
```

## Update benchmark data

The dashboard reads `docs/_data/benchmarks.yml`. To add a datapoint, append to the
appropriate benchmark's `history` array:

```yaml
- benchmark: SWE-bench Verified
  category: cli_coding
  history:
    - date: 2026-04-20
      system: Claude Opus 4.7
      score: 82.5
      source: https://www.anthropic.com/news/...
```

Commit and push — GitHub Pages rebuilds automatically.

## Weekly automated updates

A scheduled Claude agent can open a PR each week with new benchmark datapoints.
See `scripts/weekly_benchmark_update.md` for the prompt.

To set it up with Claude Code's `schedule` skill:

```
/schedule
```

Then provide:
- **Schedule**: `0 9 * * 1` (every Monday 9am local) or `weekly`.
- **Prompt**: the contents of `scripts/weekly_benchmark_update.md`.
- **Working directory**: this repo.

The agent will search the web, edit `docs/_data/benchmarks.yml`, and open a PR.
You merge after review. No scraping to maintain — the agent adapts to leaderboard
layout changes on its own each run.

## Adding a new benchmark

1. Add an entry under the right `category` in `docs/_data/benchmarks.yml`.
2. Add at least one history entry so the column has a value.
3. If the category doesn't exist yet, add it to `docs/_data/categories.yml`.
4. Optionally update the longform `en.md` / `zh-tw.md` with a section on the benchmark.

## Adding a new category

1. Add to `docs/_data/categories.yml` with a `slug`, `name`, `name_zh`, `maturity_rank`,
   and `summary`.
2. Re-rank other categories if needed — `maturity_rank` controls display order on the
   dashboard.

## Notes on comparability

**Scores are not comparable across benchmarks.** The dashboard surfaces within-benchmark
trends over time. Do not rank systems by averaging across benchmarks.
