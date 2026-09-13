# NIFTY Sharp-Fall Research Console

A small prototype demonstrating the **ASK → CLARIFY → DEFINE → TEST → LEARN** research
workflow, applied to the question: *"Does buying NIFTY after a sharp fall work?"*

See `THINKING_NOTE.md` for the reasoning behind every design decision below, and
`AI_USAGE_NOTE.md` for how AI tools were used while building this.

## What it does

1. **Ask** — user enters the question as-is.
2. **Clarify** — the system separates *what the user said* / *what it assumed* /
   *what it would ask a real user*, and exposes every assumption (fall threshold,
   holding period, entry timing, cost) as an editable control. Nothing is decided
   silently.
3. **Define** — the (editable) assumptions are compiled into a structured experiment
   spec: market, condition, entry, exit, holding period, test period, costs, hypothesis.
4. **Test** — the experiment runs against a **simulated** NIFTY-like daily price series
   (seeded random walk with volatility clustering and fat-tailed crash days), computing
   forward returns after trigger days vs. a random-day baseline.
5. **Learn** — results are shown as: raw numbers ("what the data shows"), a labeled
   interpretation ("what the system concludes" — explicitly flagged as an interpretation,
   not a fact), an explicit list of reasons to doubt the result, and suggested follow-up
   questions.

## Why simulated data, not a live/real dataset

The assignment explicitly allows sample/mock/simulated data, and the brief's emphasis is
on **workflow and reasoning**, not data engineering. Wiring up a real historical NIFTY
feed would spend the time budget on data-sourcing/cleaning rather than on the
ASK→LEARN loop the assignment actually evaluates. The simulator is seeded (reproducible)
and deliberately built to have realistic properties relevant to this specific question:
volatility clustering, occasional large negative jumps, and small-sample rarity of
"sharp fall" events — the same properties that make the *real* question hard.

## Architecture & tech choices

- **Single static HTML file** (`index.html`), vanilla JS + CSS, no build step,
  no backend, no dependencies.
- **Why:** the whole point of the exercise is the workflow, the assumption-handling, and
  the interpretation of results — not infrastructure. A framework/build pipeline would
  add surface area without adding to what's being evaluated, and a static file deploys
  anywhere (GitHub Pages, Netlify, Vercel, or literally opening the file) with zero setup.
- **State model:** a single `params` object (threshold, holding period, entry timing,
  cost) drives Clarify → Define → Test → Learn. Changing a parameter and re-running is a
  first-class action (see the "Change assumptions & re-run" button on the Learn screen),
  because a big part of "thinking like a researcher" is checking whether a result is
  robust to nearby settings, not just accepting one number.
- **Rendering:** plain `<canvas>` for the price chart and forward-return histogram —
  no charting library, kept intentionally lightweight.
- **Design direction:** a "research notebook" aesthetic (paper background, serif
  headings, monospace for data labels/figures) rather than a dark trading-terminal
  look — chosen because the product is about *evidence and reasoning*, not live trading.

## Key assumptions (see `THINKING_NOTE.md` for full reasoning)

- "Sharp fall" = single-day close-to-close return ≤ threshold (default −2%), not a
  multi-day drawdown — editable in the Clarify step.
- Entry at the **next** day's close (not the fall day's own close) to avoid look-ahead
  bias.
- Flat illustrative round-trip cost (default 0.10%) — real slippage on volatile days is
  likely higher.
- "NIFTY" is treated as the index level (a simplification — the index itself isn't
  directly tradeable; a real version would need to pick futures/ETF and model that
  instrument's specific costs).

## How to run

No install needed — it's a single static file.

```bash
# Option 1: just open it
open index.html          # macOS
xdg-open index.html      # Linux
# or double-click it on Windows

# Option 2: serve it locally
python3 -m http.server 8000
# then visit http://localhost:8000
```

**To deploy:** drag the folder into Netlify Drop, or push to a GitHub repo and enable
GitHub Pages on the `main` branch — no build command required.

## What I'd improve with more time

- Add a **sensitivity scan** (heatmap of edge across a grid of thresholds × holding
  periods) instead of a single point estimate, to make overfitting visually obvious.
- Support a **multi-day drawdown** definition of "sharp fall" as a toggle, and let the
  user compare both definitions side by side.
- Swap the simulator for a real historical NIFTY series (with the same interface), and
  show simulated vs. real results side by side so the user can see how much of the
  result depends on the specific data source.
- Add a basic significance measure (e.g. a permutation test against random-day samples
  of the same size) instead of only reporting the raw event count as a confidence proxy.
- Persist experiment runs so a user can compare several parameter settings at once
  rather than overwriting the last run.

## AI tools used

See `AI_USAGE_NOTE.md`.
