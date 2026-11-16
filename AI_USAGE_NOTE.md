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

I chose the core research workflow (Ask → Clarify → Define → Test → Learn) and the
specific definition of a “sharp fall” as a single‑day percentage drop, because it is
the most intuitive interpretation for most users and keeps the experiment simple.
I also decided to use a static HTML/JavaScript implementation with no build step,
ensuring the prototype can be run instantly in any browser. The cost model, entry
timing, and holding period defaults were selected to illustrate the methodology while
remaining editable by the user.

## 4. Did I reject or modify any AI‑generated suggestions? Why?

The AI initially suggested pulling live NIFTY data via an external API. I rejected
that because the assignment permits simulated data and using a live feed would add
network, licensing, and reproducibility concerns that distract from the research
workflow focus. I also trimmed down some UI ideas (e.g., a full dashboard with multiple
charts) to keep the prototype lightweight and aligned with the “single static file”
constraint.

## 5. What part of the solution am I most proud of?

I am most proud of making every assumption visible to the user before the experiment
runs. The Clarify step surfaces what was said, what was assumed, and what the system
needs to ask, preventing hidden biases. Additionally, separating “what the data shows”
from “what the system concludes” forces a clear distinction between evidence and
interpretation, which is essential for responsible research communication.
