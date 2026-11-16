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
  and the list of ways it could go wrong (look‑ahead bias, overlapping samples,
  overfitting, cost underestimation).
- Writing the prototype itself: the seeded mock‑data generator, the forward‑return /
  baseline computation, the canvas charts, and the UI.
- Drafting this README and Thinking Note as a starting structure.

## 3. Which important decisions did I make myself?

I chose the core research framing: defining a “sharp fall” as a single‑day percentage
drop (default −2 %) and using the **next‑day close** as the entry point to avoid look‑ahead
bias. I also decided that the prototype should expose every assumption as an editable
control before the experiment runs, rather than hiding defaults. The visual style
(notebook‑like, single static HTML) and the explicit split between “what the data shows”
and “what the system concludes” were also my design choices, aimed at keeping the
focus on transparent reasoning instead of flashy UI.

## 4. Did I reject or modify any AI‑generated suggestions? Why?

The AI initially suggested pulling live NIFTY data via an external API. I rejected that
because the assignment emphasizes reproducibility and the research workflow, not data
engineering. Using live data would introduce network dependencies, licensing concerns,
and make the prototype harder to run offline. I also trimmed a suggestion to add a
full‑blown testing framework; the prototype is intentionally lightweight, so I kept the
testing logic inside the single HTML file rather than adding external test files or
build steps.

## 5. What part of the solution am I most proud of?

I am most proud of making the **assumptions visible**. The Clarify step surfaces every
parameter (threshold, holding period, costs, entry timing) and labels what was
inferred versus what the user explicitly asked. Additionally, the Learn screen cleanly
separates raw numbers from the system’s interpretation, clearly marking the latter as
an opinion rather than a fact. This transparency forces the user to confront the
limitations of the back‑test and aligns with good scientific practice.
