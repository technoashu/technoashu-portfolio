# S&OP Reconciliation Agent

*An AI system that finds the one number that broke across sales, supply, and finance forecasts — and proposes a fair, constraint-aware fix.*

## The problem

In any large manufacturing or CPG business, Sales & Operations Planning runs on a simple premise: everyone agrees to "one number" for demand and supply, then executes against it. In practice, that number drifts the moment it cascades through the organization — sales revises its view, supply commits to something more conservative, finance reforecasts against margin targets — and each function re-derives its own version in its own spreadsheet or system.

Nobody owns the divergence. The cost shows up diffusely: excess inventory in one place, stockouts in another, expediting fees, write-offs, planners spending days reconciling numbers by hand every cycle. And critically, not every gap is worth chasing — a large enterprise's planning data can contain thousands of SKU × location × time-bucket divergences in a single cycle, and only a handful of them actually move money or service levels. Planners either drown in noise or default to gut feel about what matters.

**Engagement:** Solo build — generalizes patterns observed in earlier domain work, not itself a client engagement.

## The constraint

The domain exposure behind this system came from confidential enterprise work, which meant the honest engineering choice was to not touch that data at all. Everything here had to be built and proven on synthetic data by design — which proves the mechanism works, but also means it hasn't yet been calibrated against a real enterprise's actual, messier cost structure.

## The approach

The system is built around three components that map onto how planners actually think about reconciliation:

**A criticality engine.** Every divergence is converted into one comparable currency: `(actual − target) × contribution margin per unit`. "Sales says 40,000 units, supply says 35,000" is a 5,000-unit gap; multiplied by that SKU's per-unit margin, it becomes a priced gap — so a SKU/region/week combination with a small unit gap but high margin can outrank one with a larger gap but low margin. This is the primitive everything else consumes — without a shared currency, forecasts from different functions simply aren't comparable. In plain terms: it tells a planner which handful of mismatches actually cost money, instead of making them eyeball thousands of rows to guess.

**Divergence localization and root-cause reasoning.** Once every gap is priced, the engine ranks them and surfaces only the ones that matter — the "one number that broke" — instead of a wall of variances. A separate reasoning layer then attributes a typed cause to each flagged break (demand bias, a bill-of-materials shortfall, a capacity pinch, a sourcing mismatch), carrying its own evidence and confidence rather than asserting a cause with false certainty.

**A constraint-aware allocator.** When supply, capacity, or material genuinely can't cover the agreed number, the system doesn't just report the shortfall — it proposes a reconciled plan that fair-shares the constrained volume by criticality, topping up the most critical nodes first and iterating toward the target, respecting real limits like truck loads and minimum order quantities.

A key design discipline: the LLM never does arithmetic. All pricing, ranking, and allocation math runs through deterministic tools; the model handles recognition, root-cause narrative, and routing between config-defined steps. This keeps the system auditable — every number in the output brief is traceable to a deterministic calculation, not a language-model guess.

Proving this without real client data meant validating entirely against a synthetic dataset shaped like a real enterprise S&OP cycle, with seeded, known divergences (forecast bias, a shortage, a capacity pinch, a sourcing mismatch) used to check that the engine finds the right root causes for the right reasons.

## Outcome

As a proof-of-concept, the system takes a synthetic planning dataset with deliberately seeded divergences and produces a reconciliation brief that surfaces the handful of breaks that actually matter, attributes a plausible typed cause to each with supporting evidence, and proposes a fair-share reconciled plan under the stated constraints — illustrating, on synthetic data, how much noise a criticality-first approach can filter out before a planner ever opens a spreadsheet. Any efficiency or cost-avoidance framing here is illustrative of the mechanism's potential in a proof-of-concept setting, not a measured or realized business outcome.

## What I'd do differently

More synthetic scenarios won't teach me much else at this point. What I actually need is one real, even partial and anonymized, planning cycle — to see whether the criticality ranking and root-cause taxonomy hold up against data that wasn't designed to be found.
