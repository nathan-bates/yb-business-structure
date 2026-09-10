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
allocate accordingly.** Open the protocol, open the renderer, keep the User
Agent's knowledge model and its accumulation loop closed. Concretely, that means
the C7 presentation-query boundary is a *commercial* boundary as much as an
architectural one — worth being deliberate about now, while it's cheap.

### 2. Sextant solves the standards cold-start problem, and that's its real job.

Every standards play dies the same way: nobody adopts it because nobody has
adopted it. AUX needs apps that speak it; almost no app does.

Sextant maps an application you don't control into a structured graph — without
docs, without an API, by exploration. That is a **bridge from the non-AUX world
into the AUX world**: you don't need an app to adopt the protocol if you can
derive its surface and drive it. Which makes Sextant not a testing tool, not
merely "map-making IP", but *the adoption strategy*.

If that holds, Sextant is underinvested relative to its role, and the consulting
line (which sells it) is competing directly with the ecosystem line for the same
asset. That tension is currently unmanaged.

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
  exist yet, and `project-k` explicitly holds its product architecture undecided.
  That is the right posture for research and an expensive one to leave open
  indefinitely; it should have a decision date.
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
5. **Research with no decision date.** "Intentionally undecided" is correct for
   now and corrosive if it persists past the point where a decision is possible.

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
4. What would make `project-k`'s product architecture decidable — and by when?
5. VesselHaven — paying customers? Whose IP?
6. Jurisdiction and existing entities.
7. Where does the prospective employee sit? Platform, on this reading.
