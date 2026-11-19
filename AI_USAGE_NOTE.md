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

## 3. Which important decisions did I make myself?

I chose the core research framing and the concrete defaults that drive the prototype.
Specifically, I defined a “sharp fall” as a single‑day close‑to‑close decline (default
‑2 %), selected the next day’s close as the entry point to avoid look‑ahead bias, and set a
flat round‑trip transaction cost of 0.10 % as a simple illustrative figure. I also decided
to keep the entire application as a single static HTML file with vanilla JavaScript,
because the assignment focuses on the research workflow rather than on infrastructure
or build tooling. Finally, I deliberately separated the raw data results from the
interpretation layer so that users can see the distinction between evidence and
conclusion.

## 4. Did I reject or modify any AI‑generated suggestions? Why?

The AI suggested adding a real‑time NIFTY data feed and a more sophisticated charting
library. I rejected those ideas because they would introduce external dependencies,
increase the size and complexity of the prototype, and distract from the primary goal
of demonstrating the ASK → CLARIFY → DEFINE → TEST → LEARN loop. I also trimmed a
suggested “sensitivity heat‑map” feature, keeping the UI minimal and focused on a
single configurable experiment, which aligns with the time‑boxed nature of the task.

## 5. What part of the solution am I most proud of?

I am most proud of making every assumption visible and editable before the experiment
runs. The Clarify step surfaces what the user said, what I assumed, and the questions
the system should ask, preventing hidden biases. Additionally, the explicit split
between “what the data shows” and “what the system concludes” enforces a clear
separation between evidence and interpretation, which is essential for responsible
research and helps avoid overstating the results.
