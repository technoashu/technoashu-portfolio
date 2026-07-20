# Mapping Agentic AI for the Manufacturing Plant Floor

*A cited, source-graded opportunity landscape for plant managers — RICE-scored, verified at scale, and shipped as a live explorer.*

## The problem

Every vendor pitch deck now claims "AI for manufacturing." Plant managers are flooded with vague promises — anomaly detection, predictive maintenance, autonomous quality inspection — with no consistent way to compare them, no shared vocabulary for what's actually deployable versus vaporware, and no honest accounting of what data a plant needs before any of it works. The result: hype absorbs attention while the underlying question — *which of these jobs can an agentic system actually do today, for this kind of plant, given the data this plant actually has* — goes unanswered.

## The constraint

The opportunity scores are only as credible as the public evidence behind each KPI's data-readiness tag. For any specific plant whose actual instrumentation differs from the industry-typical assumption used here, the RICE scores would need re-grading against that plant's real data reality — this is a landscape map, not a plant-specific audit.

## The approach

The research ran as a deliberate, multi-round pipeline rather than a single pass: an initial deep-research sweep, an adversarial challenge round (independent lenses trying to break the findings), a second research pass closing the gaps that survived the challenge, then a market/whitespace pass adding named incumbents and sizing.

The output consolidated into a **13-sheet workbook** spanning 24 personas across the plant, 72 candidate interventions, and 106 linked KPIs — each intervention scored with **RICE** (Reach × Impact × Confidence ÷ Effort). The distinguishing choice: Confidence isn't a gut-feel number. It's driven directly by a **data-readiness classification** for each KPI — "ready now," "conditional," "needs manual capture," "needs IoT retrofit," "needs external feed" — so an opportunity can't score well just because it sounds compelling; it has to be backed by data the plant can actually produce. Opportunities are bucketed into Now / Next / Later / Watch tiers, and cross-referenced against a whitespace map of named incumbents (Augury, Tractian, Siemens Senseye, Cognex, Tulip, Cognite, and others) so each area's competitive crowding is visible alongside its opportunity score.

The standout engineering decision is the refusal to trust the research unverified. A Python test harness (`test_workbook.py`, built on `openpyxl`) runs **20,776 automated checks** against the workbook, all passing. It doesn't just check that cells are populated — it **re-derives** each KPI's data-readiness tag from its underlying availability text and asserts the recomputed value matches what's stored, catching silent drift between a claim and its supporting evidence. It reconciles row counts and tallies across sheets (interventions, KPIs, gap counts must all foot to the same totals), enforces specific regression checks for corrections earned during the adversarial round (e.g., that a manual-only safety metric never gets mistagged as "ready now" automated data), and sweeps every cell in every sheet for encoding artifacts and mangled text. One flagged and corrected claim during this process: a widely-cited "50 fully autonomous use cases" figure attributed to a major retailer did not hold up against the retailer's own primary disclosure, which documents human approval in the loop — a reminder that even well-sourced claims need re-checking against the original, not the summary of the summary.

## Outcome

The findings ship as more than a static report. A **live interactive explorer** — a single self-contained HTML file with no framework and no external dependencies — lets stakeholders browse the landscape by value-stream stage (plan, source, make, quality, maintain, ship, improve), by whitespace/competitive positioning, or as a filterable table, drilling from opportunity area down through interventions to the specific KPIs and data-readiness gates behind each score. It's deployed and publicly reachable at [manufacturing-ai-opportunity-explor.vercel.app](https://manufacturing-ai-opportunity-explor.vercel.app), turning a research artifact into something a plant manager can actually click through and interrogate rather than take on faith.

## What I'd do differently

The verification harness catches internal inconsistency and stale citations well, but it can't catch a source that was wrong yet consistent throughout the workbook. The next iteration should widen the adversarial challenge round itself rather than lean further on the mechanical harness to carry that weight alone.
