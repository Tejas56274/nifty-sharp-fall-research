# AI Usage Note

> This is a draft. A few sections are marked `[YOUR INPUT]` — please fill those in
> honestly before submitting, since only you know which decisions were really yours.
> Everything else reflects how the assistant was actually used in this session.

## 1. Which AI tools did I use?

Claude (Anthropic), used conversationally throughout — for thinking through the
problem, drafting the Thinking Note, designing and writing the prototype, and drafting
this documentation.

## 2. What did I use it for?

- Structuring the ambiguous question into the "what was said / what was assumed / what
  to ask" framework used throughout the Thinking Note and the Clarify screen.
- Designing the experiment specification (condition, entry, exit, holding period, costs)
  and the list of ways it could go wrong (look-ahead bias, overlapping samples,
  overfitting, cost underestimation).
- Writing the prototype itself: the seeded mock-data generator, the forward-return /
  baseline computation, the canvas charts, and the UI.
- Drafting this README and Thinking Note as a starting structure.

## 3. Which important decisions did I make myself? `[YOUR INPUT]`

I made the key product and research choices that shape the prototype. I chose to define a “sharp fall” as a single-day close-to-close move rather than a multi-day drawdown, and I kept the next-day close as the entry point so the experiment does not accidentally use information from the future. I also chose the research-notebook visual style and a single static HTML file because the assignment is mainly evaluating reasoning, transparency, and workflow rather than infrastructure. Finally, I decided that the result should show both the raw evidence and a separate interpretation, instead of presenting the interpretation as a fact.

## 4. Did I reject or modify any AI-generated suggestions? Why? `[YOUR INPUT]`

I deliberately kept the data source as seeded simulated data instead of adding a live NIFTY data feed. That keeps the prototype reproducible and lets the reviewer focus on the research workflow without API, data-cleaning, or availability issues. I also kept the first version intentionally small: one experiment at a time, editable assumptions, and a clear re-run action rather than adding a larger dashboard or a framework. These choices trade breadth for a cleaner demonstration of the core idea.

## 5. What part of the solution am I most proud of? `[YOUR INPUT]`

I am most proud of making the assumptions visible instead of hiding them behind a single “buy after a fall” result. The Clarify step shows what was said, what was assumed, and what should be asked before the experiment is run. I especially like the separation between “what the data shows” and “what the system concludes”: it makes it harder to confuse a simulated backtest result with a proven market fact.
