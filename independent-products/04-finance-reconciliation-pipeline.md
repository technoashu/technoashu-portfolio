# Household Finance Reconciliation Pipeline

*A pipeline of Claude Code skills that turns a pile of bank and credit-card statement PDFs into a trustworthy household ledger — and asks for human input only when it genuinely can't be sure.*

**Engagement:** Solo build — a personal automation project, not a product or client work.

## The problem

Every month, statements from multiple financial institutions arrive in different shapes — different layouts, date formats, some clean PDFs, some scans needing OCR. Reconciling it all by hand into one coherent picture of where money is and went is small individually, enormous cumulatively: a round of copy-paste-and-squint per statement, dozens of times a year. The interesting failure mode isn't a missed transaction — it's one silently miscategorized, or a self-transfer double-counted as spend, unnoticed for months.

## The constraint

This pipeline touches real account data, so the build had one non-negotiable rule: never let automation guess quietly. Any classification the system can't ground in evidence surfaces to a human instead of getting invented. Concretely: a "propose-only" lifecycle for anything the system learns — new merchants, counterparties, patterns — active only after explicit confirmation, with two narrow, documented exceptions (globally-recognized merchants, and memory of the user's own prior answers). A separate check independently verifies every auto-classified entity actually has ledger evidence before a phase is called done. Re-running the pipeline on the same input must also produce identical output — drift is a bug, not a quirk.

## The approach

A chain of single-purpose skills, each with a narrow job and a clean handoff:

- **Fetch** — pulls new statement PDFs from an inbox via an allowlisted sender list, dedupes against disk, and stops there (never auto-triggers processing).
- **Parse/OCR** — institution-specific parsers normalize each format's quirks into one shape; encrypted statements are handled explicitly, not silently skipped.
- **Dedupe + reconcile** — fuzzy matching catches near-duplicate rows from parser drift; a reference-based matcher pairs self-transfers so they aren't double-counted as spend.
- **Classify** — a knowledge graph of merchants, counterparties, and patterns auto-tags what it already knows, flagging only genuinely new aliases — never the user's own name, already-classified entities, or world-known brands.
- **Surface discrepancies** — anything ambiguous lands in one dedicated review surface, never buried in the analytics view. Anything actively wrong runs a fixed seven-step playbook: capture, investigate, diagnose root cause, propose a fix, get explicit confirmation *before* touching anything, apply, re-verify, then log a regression test so it can't silently recur.
- **Periodic review** — a monthly narrative on net spend, biggest movers, live anomalies (a recurring payment quietly stopped, money lent never returned), and an open-action queue.

What makes this trustworthy rather than merely automated: a hard phase gate (awareness fully solid before touching spend optimization), an explicit "kill" ledger alongside every "build," and a test suite growing through regression-pinning — every real bug the user ever caught becomes a permanent test.

## Outcome

The system is real and in daily use — it has processed thousands of transactions across several institutions over multiple months, driven unidentified spend toward zero, and now fetches new statements automatically instead of waiting for manual upload. Its test suite has grown past several hundred cases purely by pinning down real, previously-caught mistakes.

## What I'd do differently

Parser coverage lagged fetch automation — several formats now arrive automatically but still have no dedicated parser, sitting un-ingested until someone writes the format-specific logic. Building "can fetch it" and "can parse it" as one unit from the start, not sequential milestones, would have closed that gap sooner.
