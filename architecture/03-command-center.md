# Portfolio Command Center

*A CLI that scans dozens of active projects, turns real project state into ranked decisions, and gates every irreversible action behind an explicit approval.*

## The problem

Running a few dozen concurrent projects means state is scattered — a status file here, a stalled build there, an open question nobody circled back to. None of that surfaces on its own; re-opening every project by hand to check "did anything change" doesn't scale and always misses something. What's needed isn't another dashboard that requires a habit of checking it — it's a tool that reads the ground truth itself and only interrupts when a decision is actually owed.

## The constraint

The tool only has ground-truth authority over what it's actually told to scan. A project missing from the registry, or a status file that's stale by omission rather than by an explicit marker, is invisible to it — the design trades completeness (see everything) for correctness (never assert something false about what it does see).

## The approach

`cc.py` is a Python CLI (with a small local web dashboard) that treats a registry of project paths as its only source of truth. On every scan it reads each registered project's real status files, decision log, open failure signals, and git state directly — no caching, no symlinks, nothing that can silently go stale. It turns that into **decision cards**: situation, evidence (the actual bytes it read), options, a recommendation, and a reversibility rating — ranked into three tiers (needs-you, can-wait, fyi) so the signal that actually requires a human isn't buried under routine housekeeping. Where the answer genuinely isn't knowable from what it read, it refuses to guess: it writes a clarification marker back into that project instead of inventing a plausible one, and the marker resurfaces on the next scan until it's answered.

Every card can drive a follow-up action — dismiss, snooze, run a suggested fix, execute a next step — and every action is written to a hash-chained journal (choice, reason, expected outcome), so decisions are auditable and, on a scheduled review pass, checked against what actually happened.

The most consequential design decision is that capability and safety are tuned separately. Anything reversible — reading, flagging, snoozing, drafting a fix — runs with no friction. Anything irreversible — a commit, a push, a deploy, deleting state — is refused by default. Executing it requires an explicit `--approve <token>` flag, and the token isn't a static secret: it's a hash of the project, the chosen action, *and* the exact ground-truth state the human reviewed (e.g. the current commit history) at approval time. If that state changes before the flag is used — a new commit lands, the branch moves — the token silently stops matching. A captured or reused approval can't execute against a world that's since moved on.

That gate is backed by real discipline, not just intent. The handler registry is swept at module load to assert every action marked "irreversible" actually points at a real handler rather than a stub — a prior version shipped a no-op stub behind the gate, and only a positive-execution test (not just a refusal test) caught it. The scheduled pieces — a daily review pass, a periodic integrity check, a dashboard process kept alive by the OS as a dead-man's switch — are auto-restarted rather than left to be remembered. The whole thing has a real test suite: 25 test files, 142 passing, covering the approval gate, hash-chain integrity, concurrency locking, and rendered UI output — not just the API layer underneath it.

## Outcome

A tool that turns "context-switch across dozens of projects to see what needs attention" into a ranked list of decisions with the evidence attached — and one that treats "don't take an action you can't take back" as a design constraint enforced by tests, not a policy stated in a comment.

## What I'd do differently

Registry maintenance is still manual — a new project has to be explicitly registered before the tool can see it. Having the tool detect and propose newly-created project directories itself, rather than requiring that registration step, is the natural next move.
