# Youbiquity — Business Structure

A working proposal for the Youbiquity portfolio, the technical stack that
supports it, and the organization that maps to both.

**Audience:** one investor, three co-founders, and (prospectively) one employee.
Written for readers with full context — no pitch-deck padding, decisions stated
plainly so they can be argued with.

| Doc | Covers | Status |
|---|---|---|
| [01-portfolio.md](01-portfolio.md) | Thesis, three lines, where value accrues, economics, entity shape | v2 |
| [02-stack.md](02-stack.md) | Layer map, build/wrap calls, shared-vs-per-app, seams, data architecture | v1 |
| [03-organization.md](03-organization.md) | Human core, agent roles, contracting model, attention and decision latency | v1 |
| [QUESTIONS.md](QUESTIONS.md) | Open questions, ordered by what they block | v2 |

## Conventions

- `OPEN` — undecided; a founder call is needed. Tracked in [QUESTIONS.md](QUESTIONS.md).
- `ASSUMPTION` — inferred, safe to act on until contradicted.
- `PROPOSED` — a recommendation, argued but not settled.
- `DECIDED` — settled; changing it means revisiting the docs that depend on it.
- `CONTINGENT` — depends on an open question; don't build against it yet.

**One wording rule.** Never describe any bet as having *no deadline*. Every bet here
has a first-mover deadline; some are dated and some are **unknown**. "No deadline"
invites postponement and is factually wrong — say **unknown deadline**, or name what
creates the urgency. Where a line has no *customer* demanding attention, say that
precisely, because it is a statement about contention, not about time.

Every substantive change lands as its own commit, so the argument can be read as a
diff. The docs were rewritten clean on 2026-09-10; the incremental argument that
produced them is in the git history, and the positions that were tried and
retired are summarised in 01's appendix so they don't get re-litigated.
