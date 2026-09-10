# 01 — Portfolio

> **Revision note.** Two earlier drafts were wrong in ways worth recording.
> The first treated the agent factory as the flagship; it's a building block.
> The second described `aux` as the co-browse "hands" — it is not, it's the
> **AUX protocol**, which changes the strategic shape of the whole group.
> This version is written against the actual contents of `project-k` and `aux`.

## Purpose of this document

The investor is already in. This is not a pitch — it's the allocation argument
among four people who all have context: **where the next unit of attention and
capital goes, and why.** Where I think something is unresolved, it's marked
rather than smoothed over.

## What the big idea actually is

Not "an aggregation platform that schedules things." That's the pared-down
version — one application of a considerably more ambitious claim.

`project-k` describes **a program of cooperating agents with a generative UI
layer**: four agents with hard boundaries — an **Arbiter** (decides what to do
for the person), a **User Agent** (curates that person's knowledge), **Kay**
(decides *form, never content* — composes a UI on the fly from atomic
primitives), and a **Service Agent** (reaches the outside world). The stated
purpose is the **collaboration flywheel**: better signals → better learning →
more trust → more autonomy → more delegation → more signals.

`aux` is the implementation of the seam: a **vendor-neutral, declarative
agent-to-UI protocol** plus a reference renderer, design system, and harness.
Agent emits UI descriptors and autonomy context → host renders native components
→ human approves/edits/gives feedback → structured signals update memory → agent
adapts.

This is the thing that makes "dictate interfaces and patterns for the apps"
concrete: **AUX is a proposed standard.** Scheduling and shopping are
demonstrations that the topology works, not the product.

That reading changes the portfolio question. You are not choosing where to
invest across three products. You are choosing **which layer of a standards play
you intend to own.**

## The three lines

| | **1. Ecosystem** | **2. Core tech** | **3. Apps** |
|---|---|---|---|
| **What** | Four-agent topology + AUX protocol + generative per-person UI | Sell/lease the agent stack as a consulting offering | VesselHaven, App #2, #3… |
| **Assets** | `project-k`, `aux` | bridge, Sextant, Archon/ProductLens | VesselHaven |
| **Horizon** | 3–5 yr | Now | 1–2 yr |
| **Risk** | High — standards adoption is the hardest category to win | Low | Medium |
| **Scales with** | Adoption + accumulated preference data | **Humans (linear)** | Factory maturity (sub-linear) |
| **Status** | v0 POC; flywheel loop verified end-to-end | Running daily | Live |

Line 2 remains the one to watch: the only line linear in headcount, the only one
with customers asking for more today, and therefore the one that wins by default
unless deliberately capped.

## The allocation argument

Three claims, in descending order of confidence.

### 1. The protocol is not the moat. The preference layer is.

Standards are worth little until adopted, and adoption almost always requires
giving the standard away — the `aux` README already anticipates packages lifting
out into OSS repos. If AUX succeeds as an open protocol, anyone can implement it.
So the strategic question isn't "will AUX be adopted", it's **what remains
proprietary when it is.**

The candidates:

- **The User Agent and its knowledge base** — the accumulated model of how one
  specific person wants to be shown things and what they'll delegate. This
  compounds, is per-person, and cannot be copied by a competitor implementing
  the same protocol. `aux-knowledge-protocol` already separates the presentation
  query from the UI types, which is the right instinct.
- **Kay's composition quality** — real but erodible; a well-funded competitor
  can match rendering quality.
- **The app portfolio** — real but bounded by the apps' own markets.

**Recommendation: treat the preference/knowledge layer as the crown jewel and
allocate accordingly.** Open the protocol, open the renderer, keep the knowledge
model and its accumulation loop closed. The C7 presentation-query boundary is a
*commercial* boundary as much as an architectural one.

It has **two halves, with different economics** — worth separating, because they
are often discussed as one thing:

| | **Per-person preference** | **Per-scenario elicitation** |
|---|---|---|
| What | How *this* person wants to be shown things; what they'll delegate | The right questions to ask for a task ("buying a laptop"), and in what order |
| Owner | User Agent KB | **Arbiter** — pattern KB |
| Compounds | Per user | **Across all users** |
| Buys you | Retention, switching cost | Quality on a new user's *first* session — cold-start |
| Status | ADR 0010/0019 schema; no accumulation at scale | `buy-task-experience` is design-ahead-of-evidence; Arbiter KB is an open question |

Consistency is the reason this must be owned rather than delegated to a
general-purpose model: an out-of-the-box LLM asked "what should I ask someone
buying a laptop?" is non-deterministic across runs, and an interface that asks
different questions on Tuesday than it did on Monday cannot accumulate trust —
which is the flywheel's only currency.

**Performance is the same asset viewed twice.** A 30-second think is fatal to
this UX, and the architecture already knows it: ADR 0026 records that Kay
*selects* rather than generates — `decideForm` is a pure exhaustive switch
resolving the same shape to the same Tier-3 pattern, with model-assisted
composition going behind a flag. That is snappiness by construction. The general
principle worth making explicit: **the more you know about the person and the
scenario, the less you have to infer at runtime.** Latency is what you pay for
missing knowledge. So the preference layer isn't merely the moat — it's also the
performance strategy, and the model-assisted flag in 0026 is where latency will
re-enter. It should carry an explicit latency budget, not just a feature flag.

> **Finding worth acting on.** Both halves of the moat — the User Agent's
> preference KB and the Arbiter's elicitation patterns — sit **outside** what
> `project-k` and `aux` are currently building. Nine packages exist for the form
> layer (protocol, renderer, design system, vocabulary, pattern library, Kay).
> The Arbiter has a contract (ADR 0027) but no implementation and an explicitly
> open knowledge base; the User Agent has a query protocol but no store. Build
> effort is concentrated on the layer most likely to be commoditised or given
> away, and absent from the two components identified as proprietary.

### 2. Sextant solves the standards cold-start problem, and that's its real job.

Every standards play dies the same way: nobody adopts it because nobody has
adopted it. AUX needs apps that speak it; almost no app does.

Sextant maps an application you don't control into a structured graph — without
docs, without an API, by exploration. That is a **bridge from the non-AUX world
into the AUX world**: you don't need an app to adopt the protocol if you can
derive its surface and drive it. Which makes Sextant not a testing tool, not
merely "map-making IP", but *the adoption strategy*.

There are three distinct ways to use it, and they are not equivalent:

- **(a) Internal adapter-generator.** Point Sextant at an app you don't control;
  its graph becomes a Service Agent driver. You gain coverage *without anyone
  adopting anything*. This is the cold-start answer, and it needs no
  counterparty's permission — which is both its strength and its ToS risk.
  Concrete seam: ADR 0029/0030's **write recipes** ("authored by the framework,
  armed by a person") are exactly what Sextant's exploration output could
  generate.
- **(b) External integration SDK.** Give it to app owners: "run this, get an AUX
  manifest, you're a citizen." Lowers adoption cost — but it is productisation,
  with docs, support and SLAs, i.e. the human-shaped work that fights the
  minimal-core goal. Cheap for *partners and owned apps*; expensive as a
  self-serve public tool.
- **(c) Pattern-mining source.** Mapping many apps reveals recurring interaction
  patterns, feeding the Tier-3 pattern library and the Arbiter's elicitation
  patterns. Quietly the most compounding of the three.

**Recommendation: (a) now, (c) as a by-product, (b) only for named partners, and
never self-serve until the core is proven.** Note that the consulting line sells
Sextant as a tool — meaning line 2 and line 1 are competing for the same asset,
and pulling it toward (b). That tension is currently unmanaged.

### 3. The apps are the demand side, and one is not enough.

VesselHaven's job is now threefold: prove the factory's cost curve, be the first
AUX citizen, and supply real usage from which preference data accumulates. App #2
matters more under this thesis than under the previous one — a protocol
demonstrated on one app is a demo; on three, it's a pattern.

## Asset inventory

| Asset | Line | What it actually is | Status |
|---|---|---|---|
| **project-k** | 1 | Four-agent topology; Kay = form-never-content rendering layer. Product architecture *intentionally undecided* | Knowledge + architecture; no app code yet |
| **aux** | 1 | AUX protocol, reference renderer, design system, harness, Claude adapter | v0 POC; flywheel verified, 4/4 renderer tests, live path not yet exercised |
| **Sextant** | 1 + 2 | Exploration → graph of apps you don't control. The AUX adoption bridge | Working, unproductised |
| **Claude Code Cloud** | 1 + 2 | Agent runtime, credential brokering, supervision | Load-bearing |
| **Archon / ProductLens** | 2 | Work layer | Mid-consolidation |
| **Co-browse / responsive-surface** | 1 | Execution inside a user's own authenticated session — Service Agent driver | Working PoC |
| **VesselHaven** | 3 | Revenue vertical; first AUX citizen | Live |

Correction to the previous draft: **`aux` and co-browse are different things.**
`aux` is the protocol seam (Kay ↔ host). Co-browse is a Service Agent capability.
Both matter; conflating them hid the fact that the group has *two* separate
answers to "how does an agent act in the world."

## Proposed entity shape

```
                    Youbiquity Group  (umbrella / holdco)
                    - investor equity sits here
                    - owns all IP, licenses it down
                    - employs the human core
                    - INCUBATES project-k / aux (no entity yet)
                              |
              +---------------+---------------+
              |                               |
     Youbiquity Platform            Youbiquity Apps  ("App Portfolio")
     (core tech + consulting)                 |
     - Sextant, bridge,                 +-----+-----+
       Archon/ProductLens               |           |
     - revenue-generating          VesselHaven   App #2, #3...
```

Unchanged from the previous draft, and the AUX reading strengthens two parts:

- **VesselHaven stays a grandchild** — individually disposable without touching
  platform IP, and worth more to an acquirer *as* a working AUX citizen.
- **The ecosystem play stays in the Group.** It gets its own entity at the first
  of: outside capital specific to it, something worth protecting (the preference
  model, plausibly), or a partner requiring a counterparty. Note that an open
  protocol plus a closed knowledge layer may eventually want *two* homes — a
  foundation-ish or OSS-licensed protocol, and a commercial entity holding the
  preference IP. Not yet, but it's a fork worth seeing coming.

## Attention allocation — the binding constraint

Three lines, three horizons, 3–4 people. Capital isn't the constraint; founder
attention is.

- **Lines 2 and 3 are agent-delivered by default.** A founder hour spent there
  is a bug to be automated.
- **Line 1 gets protected attention** — it's the only line whose product doesn't
  exist yet.

**On `project-k` holding its product architecture "intentionally undecided":**
an earlier draft of this doc said it needs a decision date. That was too blunt,
and the ADR record argues against it — ADR 0016 reversed the framing of the
entire repository and produced a materially better architecture precisely because
the question stayed open. Arbitrary dates force premature commitment in genuine
research.

The sharper version: keep the *product architecture* open, but close the
*strategic* question of which layer the company is built on — because that is
what routes the next N agent-months, and it is currently routing them into the
form layer while the moat sits unbuilt. What each open question needs is not a
date but **named evidence that would decide it**, plus a review cadence and an
escalation when the same question survives two reviews. Two things do force
timing, though: the v0 POC proved the mechanism explicitly *not*
learning-at-scale, and the next evidence requires real users accumulating real
preferences — so choosing what to build to get that evidence **is** the product
decision. It cannot stay open much past the point where you want the next
experiment.
- **Shift triggers stated in advance**: app #2 launched under a stated
  human-hour budget; a consulting engagement exceeding the cap; a `project-k`
  milestone slipping two quarters.

> **NEEDS YOU** — Is consulting funding the ecosystem, or a parallel bet? Either
> way it needs a stated ceiling in 03, or it takes the attention by default.

## Risks

1. **Standards adoption.** The hardest category to win, and 2026 is crowded —
   agent-to-UI and agent-to-tool protocols are actively contested by
   better-capitalised parties. Winning on protocol design alone is unlikely;
   winning on *adoption path* (Sextant) or *accumulated preference data* is
   more plausible.
2. **Open-source value capture.** If the protocol and renderer are open and the
   preference layer isn't clearly separated, the group could do the standards
   work and capture none of it.
3. **Counterparty risk** on apps you don't control — ToS, bot detection,
   blocking. Partly mitigated by co-browse (the user is genuinely present).
4. **Credential custody.** Acting across services normally means holding user
   logins — the most common reason users refuse this category. Co-browse
   executing in the user's own session appears to sidestep it. If that reading
   is right it's a market position, not a footnote. *(Still unconfirmed.)*
5. **Building the commoditisable layer.** The live risk is not indecision — it
   is that effort concentrates on the form layer (open, copyable) while the
   preference and elicitation layers (proprietary, compounding) stay unbuilt.

## What has to be true

1. **Delegation compounds.** The flywheel's core claim: people give more autonomy
   as trust accrues. v0 proved the *mechanism*, explicitly not learning-at-scale.
   Everything rests on this and it is not yet evidenced.
2. **Sextant can map apps it didn't co-evolve with.** The adoption path depends
   on it; demonstrated mainly against VesselHaven so far.
3. **A second vertical launches without adding humans.**
4. **Consulting can be capped.** Organizational discipline; the usual failure mode.
5. **The preference layer is separable and defensible.** If it isn't, the
   ecosystem play is a gift to the industry.

## Open questions

1. Consulting: funds the ecosystem, or parallel bet? What's the cap?
2. Is the co-browse "never hold credentials" reading correct?
3. Does the open-protocol / closed-preference-layer split match your intent?
4. Who owns the Arbiter's elicitation KB and the User Agent's preference store,
   and when does build effort shift toward them?
5. VesselHaven — paying customers? Whose IP?
6. Jurisdiction and existing entities.
7. Where does the prospective employee sit? Platform, on this reading.
