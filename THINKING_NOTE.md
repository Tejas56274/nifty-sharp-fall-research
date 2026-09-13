# Thinking Note
### "Does buying NIFTY after a sharp fall work?"

## 1. How I interpret the question

The question bundles three separate research decisions into one sentence:

- **What counts as a "sharp fall"?** — this could mean a single bad day, a multi-day
  drawdown, or a fall relative to volatility (e.g. a 3-sigma move). These give very
  different signals and very different sample sizes.
- **What does "buying" mean operationally?** — buy when, at what price, in what size,
  and held for how long? "Buying" without a holding period isn't a testable strategy,
  it's a feeling.
- **What does "work" mean?** — beats a buy-and-hold baseline? positive expected return?
  positive *risk-adjusted* return? high win-rate? All are different bars.

So the real job isn't to answer the question — it's to turn a vague belief into a
**falsifiable, precisely-specified experiment**, and be honest about which parts of that
specification came from the user and which are my own assumptions.

For this prototype, I define "sharp fall" as: **a single-day close-to-close decline of
more than a chosen threshold (default −2%)**. I chose single-day over multi-day because
it's the simplest, most common lay interpretation of "a sharp fall," and it's easy for a
user to sanity-check against memory ("was there a day the market dropped 2%+?"). A
multi-day/drawdown definition is flagged as an alternative the system should let the user
switch to, not a decision I make silently.

Before testing anything, I'd want: (a) enough daily price history to have a meaningful
number of sharp-fall events, (b) clarity on whether "NIFTY" means the index level or a
tradeable instrument (futures/ETF — since the index itself can't literally be bought),
and (c) the user's real objective — are they looking for a mechanical rule, or trying to
understand market behavior after shocks?

## 2. Assumptions — and what's actually the user's

| What the user said | What I assumed | What the system should ask |
|---|---|---|
| "buying NIFTY" | Proxy via NIFTY index close prices (ignoring the fact that the index itself isn't directly tradeable) | "Do you mean the index, futures, or an ETF? Costs/slippage differ." |
| "after a sharp fall" | Single-day return ≤ −2% (editable) | "How big a fall counts as 'sharp' — a single bad day, or a rolling multi-day drop?" |
| "does it work" | Compare forward returns after sharp-fall days vs. forward returns on all other days, over the same holding period | "What does 'work' mean to you — beating buy-and-hold, a positive win-rate, or something else?" |
| (unstated) entry timing | Enter at the next day's close, to avoid using the fall day's own close as both signal and entry (a common look-ahead trap) | "Enter immediately at that day's close, or wait for the next session?" |
| (unstated) holding period | 10 trading days (~2 weeks), editable | "How long would you actually hold — days, weeks, months?" |
| (unstated) test period | Full available history | "Any specific period you care about — e.g. post-2015, or excluding COVID?" |
| (unstated) costs | 0.05% one-way (illustrative, editable) | "What's your realistic cost/slippage per trade?" |

The key discipline here: the system should **never quietly bake in an assumption** it
hasn't shown the user. Every default in the prototype is visible and editable before the
experiment runs.

## 3. What I'd ask the user (minimum viable clarification)

1. What size fall counts as "sharp" — a percentage, and over how many days?
2. What holding period are you imagining — how long do you stay in after buying?
3. What are you trying to learn — is this "does it beat just holding," or "does it beat
   doing nothing," or "how consistent is the bounce"?
4. Any period of history you want included/excluded (e.g. avoid a single event like
   COVID dominating the result)?

Everything else (costs, entry timing, exact instrument) can ship as a sensible, visible
default rather than a blocking question — asking too much up front kills the "quick
question → quick insight" loop that makes this kind of tool useful.

## 4. Experiment definition

- **Market/instrument:** NIFTY 50 index level (proxy; noted as a simplification)
- **Condition:** Single-day close-to-close return ≤ a threshold (default −2%)
- **Entry:** Next trading day's close, after a condition day (avoids using the same
  close as both trigger and entry price)
- **Exit:** N trading days after entry (default 10)
- **Holding period:** Configurable, default 10 trading days
- **Test period:** Full length of the (simulated) dataset, configurable
- **Filters:** Overlapping trigger days within the holding window are optional to
  include/exclude (affects independence of samples)
- **Costs/assumptions:** Flat round-trip cost in bps (default 10bps), no leverage, no
  dividends/index-adjustment modeled
- **Hypothesis:** "Returns in the N days following a sharp single-day fall are, on
  average, higher than returns following a randomly chosen day."

## 5. What could go wrong (and how the prototype tries to flag it, not hide it)

- **Ambiguous definition of "sharp"** — the threshold is arbitrary; results are shown to
  be sensitive to it, and the tool says so rather than presenting one number as truth.
- **Small sample size** — sharp falls are rare events; a handful of them (especially
  clustered around one crash) can dominate the average. The result screen reports the
  *number of events*, not just the average, so the user can judge confidence themselves.
- **Look-ahead bias** — entering at the next day's close (not the fall day's own close)
  avoids using information not yet available at decision time.
- **Overlapping/non-independent samples** — if fall days cluster (e.g. during a crash),
  the "events" aren't independent draws; this inflates apparent confidence. Flagged
  explicitly in the results.
- **Transaction costs & slippage** — sharp-fall days are exactly when spreads widen and
  slippage is worst; a flat cost assumption likely *understates* real-world cost.
- **Overfitting to one threshold/holding period** — a result that only "works" for one
  specific combination (e.g. exactly −2.3% and 11 days) and not nearby values is
  probably noise, not a real effect. The system should be honest that it tested one
  configuration, not a robust range, unless the user explicitly asks for a sensitivity
  scan.
- **Survivorship/regime bias** — an index-level backtest silently assumes today's index
  composition and market structure applied throughout history, which isn't quite true.
- **Confusing correlation with causality** — even a genuinely positive average doesn't
  mean *this* mechanism is expected to repeat; markets change regimes.

The LEARN stage of the prototype is deliberately split into "what the data shows" (pure
numbers) vs. "what the system concludes" (an interpretation, clearly labeled as such,
with its confidence caveated) — this separation is the single most important design
decision in the whole exercise.
