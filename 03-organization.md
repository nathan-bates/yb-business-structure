# 03 — Organization

*Version 1 — 2026-09-10. Follows [01-portfolio.md](01-portfolio.md) and
[02-stack.md](02-stack.md). Proposals here are `PROPOSED` unless marked
otherwise; this is the doc most likely to need redirection.*

---

## The design constraint

Three to four people running three revenue lines. Capital is not the binding
constraint — **founder attention is**, and it binds twice:

1. **As allocation.** Line 2 has customers asking for more; line 1 has nobody
   asking for anything. Without a rule, attention flows to the loudest line.
2. **As latency.** On an 11-month build with a small core, much of the critical
   path isn't build throughput — it's waiting for a human to decide. Agents can
   double throughput and barely move elapsed time if the queue is decisions.

So the cost curve and the org design are **one problem**, not two. This document
treats them together.

---

## The human core

| Person | Owns | Does not own |
|---|---|---|
| Nathan | Line 1 (ecosystem); group strategy; commercial terms | Delivery execution |
| Co-founder 2 | `OPEN` | |
| Co-founder 3 | `OPEN` | |
| Employee (prospective) | **Platform / line 2 delivery** | Line 1 product decisions |

`PROPOSED` — **the employee sits in Platform**, so line-2 delivery stops
consuming founder attention. That's the whole reason to hire at this stage: not
capacity, but *insulation* of the scarce resource. If line 2 is licensing-led
(01 §Line 2 should be licensing-led), the role is **packaging and licensee
support** rather than embedded delivery — a materially different hire, and one
whose output is reusable rather than consumed.

`OPEN` — co-founder ownership. The portfolio has three lines and one factory;
four ownership slots exist and two are unassigned in this document. Ownership
should be by line, not by function, so that each line has exactly one person
whose attention it can claim.

**The governing metric: humans per revenue line.** ~3–4 : 1 today. The structure
is working if it falls as lines are added, failing if a new app needs a new team —
regardless of how the app performs.

---

## Agent roles

The org chart has agents in it. Stated concretely enough to be wrong.

| Role | Does | Standing authority | Escalates when |
|---|---|---|---|
| **Build** | Implements against specs/ADRs; opens PRs | Merge to feature branches; open draft PRs | Spec ambiguity; cross-seam change |
| **Review** | Adversarial review of build output | Block merge | Disagreement with build agent unresolved in two rounds |
| **Ops / incident** | First responder: triage, diagnose, mitigate | Restart, roll back, scale | Customer-visible impact > threshold; data risk |
| **Research** | Market/competitive/technical investigation with sources | Publish findings to the docs | Findings contradict a recorded decision |
| **Support** | First-line response across apps | Answer from known material | Anything requiring a commitment or refund |
| **Factory** | Maintains harness, tooling, pipelines | Change internal tooling | Changes affecting delivery contracts |

**Two rules that make this work:**

1. **Standing authority is the point.** An agent that must ask before acting
   converts into a decision in a founder's queue, which is the thing being
   optimised away. Every role above has a named default action.
2. **Escalation is by condition, not by discomfort.** The right column is the
   contract. If agents escalate outside it, the boundary is wrong and should be
   changed deliberately rather than eroded case by case.

---

## Decision latency — the time floor

`OPEN` — of VesselHaven's 11 months, roughly what fraction was *waiting on a
human decision* versus *work in progress*? That split determines whether the app
#2 time target is an engineering problem or an org problem, and the remedies are
completely different.

`PROPOSED` remedies, on the assumption the waiting share is material:

- **Pre-committed defaults.** For recurring decision classes, decide once and
  record the default. The agent proceeds unless the case is genuinely novel.
- **Decision SLAs.** A decision request unanswered in N working days executes the
  recorded default. This is uncomfortable and it is the mechanism that actually
  works; without it, the queue is unbounded.
- **Batched review windows** rather than continuous interruption — throughput and
  quality both improve, and it protects the line-1 attention block.
- **Fewer decision points by design.** The largest source of latency is usually
  optionality that was never needed. A spec that names one approach beats one
  that offers three.

---

## How engineering gets bought

The most consequential organisational decision in the group, because it gates the
cost curve (01 §How engineering gets bought).

> Under time-and-materials, productivity gains accrue to the vendor as leisure,
> not to Youbiquity as cost reduction — and a vendor whose billed hours fall with
> tool adoption is rationally opposed to the factory.

`PROPOSED`:

1. **Move factory work out of app delivery.** The core owns and builds tooling;
   delivery consumes it. Contractors are then asked to *use* something, not to
   *invest* in something whose benefit lands on an app they'll never touch.
2. **Fixed price per deliverable for app #2.** The ADRs, specs, C5–C10 contracts
   and invariants make acceptance substantially more checkable than the vague
   criteria that normally make fixed-price degenerate.
3. **Run the diagnostic first.** Ask the incumbent for a fixed-price bid on app
   #2. A vendor who won't price their own output has answered the question, and
   the conversation never has to be about hours.
4. **Decide before app #2 starts.** The cheapest moment to change a commercial
   model is a project boundary; the window closes by default.

**If the partner changes**, app #2 measures a new team's learning curve as much as
the factory's leverage. Expect a noisy reading and treat app #3 as the real test —
better known going in than concluded afterwards.

---

## Attention allocation

`PROPOSED` rules:

- **Lines 2 and 3 are agent-delivered by default.** A founder hour spent there is
  a bug to be automated, not a cost of doing business.
- **Line 1 gets a protected block.** It's the only line whose product doesn't
  exist yet, and the only one that can't be delegated to agents — because there's
  nothing yet to delegate.
- **Line 2 is licensing-first.** Licensing scales without adding people; embedded
  consulting does not. Where services are unavoidable, prefer fixed-price
  projects over time-and-materials.
- **Two different controls, for two different models.** *Embedded consulting*
  carries a stated cap — engagements per year or % of founder time — because it
  is linear in humans and generates no compounding data. *Licensing* carries a
  **commitment cap** instead: what you promise licensees constrains what the
  architecture can still change. `OPEN` — both numbers.

**Shift triggers, stated in advance so reallocation is a decision rather than a
drift:**

| Trigger | Response |
|---|---|
| App #2 exceeds ~$150k or ~7 months | Stop. Revisit the portfolio strategy before app #3 |
| An embedded consulting engagement breaches the cap | Decline, subcontract, or convert to a licence; do not absorb |
| A licence commitment would freeze a moving interface | Refuse the commitment or license a stabilised subset |
| Sextant misses its evaluation bar | Park it; don't extend |
| Two apps ship with no shared users | Revisit the cross-app thesis (01 §failure mode) |

---

## Measurement

Few, and each tied to a decision:

| Metric | Why | Cadence |
|---|---|---|
| **Cost and elapsed time per app** | The thesis | Per app |
| **Agent-originated share of merged work** | Says *which path* bent the curve, and whether it generalises | Monthly |
| **Shared vs per-app spend split** | Estimates the floor F | Per app |
| **Humans per revenue line** | The governing structural metric | Quarterly |
| **Cross-app users** | The failure mode in 01, made visible early | Monthly, from app #2 |
| **Decision latency** — median age of open decision requests | The time floor | Weekly |

Define the measurement conventions **before app #2 starts**: what counts in the
money, what counts as released, what counts as the start. Otherwise the numbers
aren't comparable and the curve is unfalsifiable.

---

## Entities and operating mapping

Legal shape is in 01. What matters organisationally:

| Entity | Holds | Run by |
|---|---|---|
| **Group** | IP, investor equity, the human core, the incubating ecosystem play | Founders |
| **Platform** | Core tech; **licence revenue** plus bounded services; licenses IP down | Employee + agents; one founder accountable |
| **Apps** | Vertical apps as individually disposable subsidiaries | Agent-delivered; one founder accountable |

- Platform is **revenue-generating**, not a cost centre, which makes the
  intra-group licensing arrangement load-bearing rather than administrative.
- The ecosystem play has **no entity yet** — deliberately. It gets one at the
  first of: capital specific to it, something worth protecting, a partner
  requiring a counterparty.

`OPEN` — jurisdiction and existing entities; the IP licensing mechanics and
transfer pricing depend on it.

---

## Governance

Three founders and an investor already in. Worth being explicit about two things:

- **Decision rights by line.** Each line has one accountable owner who decides
  within it. Cross-line decisions — capital allocation, commercial model, entity
  changes — are founder-level. Investor is consulted on capital and structure,
  informed on the rest.
- **Decisions are recorded where the work is.** This repository for group
  strategy; ADRs in the product repos for architecture. A decision that isn't
  written down will be re-argued, and re-arguing is the most expensive thing a
  four-person company does.

---

## What would break this

1. **The employee lands in Apps instead of Platform** — line-2 delivery keeps
   consuming founder attention and nothing insulates line 1.
2. **Line 2 drifts into embedded consulting** because it closes faster than
   licensing does. The cash line becomes the company — the most common failure
   mode for groups in this shape, and the one that arrives disguised as good news.
3. **Agents without standing authority** — every agent action becomes a founder
   decision, and the org gets *slower* with more agents, not faster.
4. **Commercial model unchanged** — the cost curve doesn't bend and the portfolio
   strategy is unavailable regardless of how good the tooling gets.
5. **Ownership unassigned** — three lines and two named owners means at least one
   line is nobody's, and it will be line 1, because it's the one with no
   customers demanding attention.
