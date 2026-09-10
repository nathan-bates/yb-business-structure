# Open questions

*Version 2 — 2026-09-10. Ordered by what each answer blocks. Two are
time-boxed by external events and lose the window if not answered.*

---

## Time-boxed — the window closes on its own

*Three clocks now run, not two: VesselHaven's ship date (Q1), app #2's start (Q2),
replay's ~12–18 month competitive window, and the ecosystem's ~18–36 month
data-possession window. The last is continuous rather than a deadline — it costs
depth every month rather than expiring on a date.*

### Q1 — One community, or one per app?
**Closes when VesselHaven's community ships.**

Community is the cross-app surface — the place the person appears as a person
rather than as a boat owner. *One community, many apps* is the only version in
which the portfolio compounds; *many apps, each with a community* is N cold
starts and N thin datasets. This is an identity-model decision and those calcify.

Related build call (02 §Sequencing): community should be an **extractable shared
service** sitting beside `library`, not a VesselHaven module.

### Q2 — Does the incumbent bid fixed-price on app #2?
**Closes when app #2 starts.**

Under time-and-materials the buyer captures none of the productivity gain, so the
cost curve is unreachable by tooling alone. The cheapest diagnostic is to request
a fixed-price bid; a vendor who won't price their own output has answered the
question. The cheapest moment to change commercial terms is a project boundary.

---

## Blocks the strategy

### Q2b — What is the kill rule for an app?
The app tail is a portfolio of options; options expire but products linger.
Without an explicit criterion for shutting or parking an app, the tail consumes
the budget through operations — the asymptote in the cost model. Cheap to decide
now, expensive to decide while attached to something.

### Q3 — Who is the cohort?
Name the person the next three apps all serve, in a sentence. If that can't be
done, the cross-app preference layer is a hope rather than a plan, and the
portfolio is N disjoint datasets. This is the single largest risk in 01.

### Q4 — What is the cost floor F, and what's in it?
If halving held forever the entire portfolio would cost ~$500k. It won't — F
alone determines how many apps the portfolio can hold. The likely floor is
operational (support, GTM, compliance, content), not engineering.

Sub-question, cheap and time-sensitive: **of VesselHaven's $250k, how much was
non-recurring factory capability vs. VH-specific work?** Much easier at launch
than reconstructed later.

### Q5 — How much of VH's 11 months was waiting on a human decision?
A rough split is enough. It decides whether the app #2 time target is an
engineering problem or an org problem — completely different remedies (03
§Decision latency).

### Q6 — Monetisation, given no take rate
Subscription, B2B licensing to app owners, or per-vertical SaaS? At ~$250k per
app, break-even on build alone is roughly a thousand sustained subscribers per
app for a year at $20/month. Sanity-check that against realistic cohort sizes
**before** choosing app #2 — it may rule out small verticals entirely.

---

## Shapes the build

### Q7 — Which two execution providers does `market` wrap?
One implementation is a dependency, not an abstraction. Concrete sub-question
with a findable answer: does wrapping a **signed** agent provider inherit its
access under the Cloudflare (15 Sept 2026) and Shopify identity gating?

### Q8 — What is the legal posture on co-browse?
It's the one execution mode that degrades gracefully as identity gating tightens,
and the one a merchant could characterise as routing around its controls. The
Ninth Circuit's reversal of Amazon's injunction against Perplexity helps; the case
is unresolved. Deliberate posture with legal input, not an emergent property.

### Q9 — Does Sextant clear its bar?
2–4 weeks. Aim it at the durable artifact — a reusable graph usable as an
acceptance oracle — not at writing tests faster, which is on the model curve.
Natural test: run it against VesselHaven at launch. Does it find real defects a
human pass missed, and measurably reduce regression effort? Set the stop rule
before starting.

### Q10 — Is app #2 testing human-team-plus-tools, or agent-executed?
Instrument **agent-originated share of merged work**. Cost says whether the curve
bent; that ratio says which path bent it, and therefore whether it generalises.
A blended result with no instrumentation is unreadable, and there's only one
first measurement.

---

## Structural

### Q11 — Jurisdiction and existing entities
The entity shape is jurisdiction-neutral; the intra-group IP licensing mechanics
are not, and transfer pricing will shape them.

### Q12 — Co-founder line ownership
Three lines, one factory, four ownership slots — two unassigned. Ownership should
be by line so each line has exactly one person whose attention it can claim.
Unassigned lines default to unowned, and it will be line 1.

### Q13 — Line 2: what gets licensed first, and what are the two caps?
*(Commercial plan for the leading candidate is now in [04-replay-gtm.md](04-replay-gtm.md).)*
Licensing has a far better hour:income ratio than embedded consulting and scales
without adding people — and licensing the preference/interface layer into other
people's apps *is* line 1's B2B2C channel, which makes line 2 a go-to-market
rather than a distraction (01 §Line 2 should be licensing-led).

Three parts: **which asset first** (Claude Code Cloud is readiest; the
`library` + Kay layer has the highest ceiling; not Sextant until it's real), **on
what pricing basis** (per-seat, per-app, per-end-user, revenue share), and **the
two caps** — a time cap on any embedded consulting, and a *commitment* cap on
licensing, since what you promise licensees constrains what the architecture can
still change.

---

## Answered

| Question | Answer |
|---|---|
| Sell the factory as a self-serve product? | No — but **license** it to negotiated customers; that is a different proposition and is the preferred line-2 model |
| Does `market` build drivers? | No — it wraps providers |
| VesselHaven: subsidiary or grandchild? | Grandchild — keeps verticals individually disposable |
| Where does the ecosystem play sit? | Incubates in the Group; no entity yet |
| Is marine the domain to compound in? | Wrong question — VH is one app of many |
| Is there a defensible technical niche in the consumer agent layer? | No. See 01 §Appendix — six candidates retired with sources |
| Where does the prospective employee sit? | Platform, to insulate founder attention *(proposed)* |
