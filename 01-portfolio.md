# 01 — Portfolio

## The governing idea

Youbiquity's four assets are not peers. One earns revenue from end customers,
one is defensible IP, one is the factory that builds the others, and one is the
control surface for the factory. Treating them as a flat "product portfolio" is
the most common way this kind of group gets structured badly — it produces an
org chart with a team per product, which is precisely the outcome a
minimal-human-core company must avoid.

The structure proposed here follows from a single thesis:

> **The factory is the company. The portfolio is how the factory is monetized.**
> Value accrues if the marginal cost of launching vertical app N+1 falls toward
> zero — not from any one app succeeding.

That thesis implies one governing metric, and the whole structure should be
legible against it:

**Humans per revenue line.** Today ~3–4 humans / 1 revenue line. The structure
is working if that ratio falls as revenue lines are added, and failing if a new
app requires a new team — regardless of how well the app itself performs.

## Asset inventory

| Asset | Class | What it is | Customer | Status |
|---|---|---|---|---|
| **VesselHaven** | Revenue vertical | Yacht management SaaS — vessels, quotes, invoices, transactions, role/permission model | End customers (marine) | Live; only asset with a plausible external customer today |
| **Sextant** | IP engine | Design ≡ code ≡ test plan ≡ graph equivalence; autonomous exploration, gap analysis, coverage-driven test generation | Internal (today) | Working; unproductised |
| **Claude Code Cloud** (`llm-slack-channel-bridge`) | Factory | Slack-spawned cloud agent sessions, per-user credential brokering, layered memory, co-browse, sandbox provisioning | Internal (today) | Load-bearing; runs daily |
| **Archon + ProductLens** | Work layer | Work-item ↔ conversation ↔ codebase mapping; PR association; the factory's intake and tracking surface | Internal | Mid-consolidation into Claude Code Cloud |

> **ASSUMPTION** — Archon and ProductLens are one asset, not two: they are being
> consolidated, and neither stands alone commercially. Treated as a single
> "work layer" throughout. Say if you'd rather keep them distinct.

> **NEEDS YOU** — Revenue and IP facts I can't determine from the repos:
> 1. Does VesselHaven have paying customers / signed pilots, or is it pre-revenue?
> 2. Is VesselHaven **your** IP, or a client's? If a client's, it is services
>    revenue, not a product line, and the entity shape below changes materially.
> 3. Is any revenue landing today from consulting/services against these assets?

## Classification, and why it drives the entity shape

Three of the four assets share a property: **they have no external customer and
are not intended to be sold separately.** They are the means of production. The
fourth is the only thing a customer currently pays for.

That asymmetry, not the product taxonomy, is what the legal structure should
express. Concretely: you want to be able to sell, spin out, or lose VesselHaven
**without touching the factory**, and you want the factory's IP owned somewhere
that no single app's fate can reach.

## Proposed entity shape

```
                    Youbiquity Group  (umbrella / holdco)
                    - investor equity sits here
                    - owns all platform IP
                    - employs the human core
                              |
              +---------------+---------------+
              |                               |
     Youbiquity Platform            Youbiquity Apps  ("App Portfolio")
     (the factory — cost centre)    (holding co for verticals)
     - Sextant                                |
     - Claude Code Cloud              +-------+-------+
     - Archon / ProductLens           |               |
     - licenses down to Apps     VesselHaven      App #2, #3...
                                 (grandchild)
```

**DECISION (proposed) — VesselHaven is a grandchild, not a direct subsidiary.**
You asked which. Grandchild, for one reason that outweighs the added
administrative overhead: it makes each vertical **individually disposable**. A
buyer for VesselHaven can acquire a clean entity whose assets are the app, its
contracts and its customers — while the Sextant and Claude Code Cloud IP that
built it stays in the group, still licensed to every other app. A direct
subsidiary holding app-and-platform-entangled IP forces you to carve out at sale
time, under time pressure, with a buyer's lawyers setting the pace.

The same structure gives you a second option worth having: if the App Portfolio
ever raises separately or takes a strategic partner, that happens one level below
the group, without diluting platform ownership.

**Platform IP sits in the Group (or a thin IPCo beneath it), not in Platform Ltd.**
Licensed down to each vertical on an intra-group arrangement. This is the piece
your accountant will care about most, and it is much cheaper to establish now,
pre-revenue, than to restructure later.

> **NEEDS YOU** — Jurisdiction(s) and any existing entities to treat as fixed.
> The shape above is jurisdiction-neutral; the IP-licensing mechanics are not,
> and transfer-pricing rules will shape the intra-group arrangement.

## The one strategic decision this document can't make for you

**Is Sextant (and/or Claude Code Cloud) sold externally, or kept as the moat?**

Both are defensible; they lead to different companies, and the choice cascades
into 02 and 03.

*Case for selling the factory:* larger TAM, higher multiples, and Sextant's
core claim — that design, code and tests are one artifact — is a category-level
idea, not a feature. Dev-tools comparables price well.

*Case against, which I think is stronger given your stated goal:* selling
developer infrastructure means supporting other companies' stacks. That means
docs, support, sales engineering, SLAs, backwards compatibility, and a roadmap
you no longer control — all of it human-shaped work that does not compress with
agents. **Selling the factory contradicts minimizing the human core.** Keeping
it internal lets it stay opinionated, coupled to your own stack, and free to
break itself weekly.

**Recommendation:** keep the factory internal for now, monetize through the app
portfolio, and revisit external licensing only once a second vertical has shipped
and proven the marginal-cost thesis. At that point you have evidence for the
claim, and the factory has been hardened by a second consumer — which is exactly
what a first external customer would have forced anyway, but on your schedule.

> **NEEDS YOU** — Accept, reject, or defer. This is the highest-leverage
> decision in the document; 02 and 03 assume the recommendation unless told
> otherwise.

## Keep / kill / park

| Asset | Call | Reasoning |
|---|---|---|
| Claude Code Cloud | **Keep — invest** | The factory. Every other asset's cost curve runs through it. |
| Sextant | **Keep — invest, don't productise** | The moat, per the decision above. Productising is a different company. |
| VesselHaven | **Keep — but as proof, not as the point** | Its job is to demonstrate the marginal-cost thesis, and to pay bills while doing so. |
| Archon / ProductLens | **Park at "good enough"** | Real risk of becoming a product in its own right. It is intake and tracking for a four-person company. Finish the consolidation, then freeze. |
| Co-browse / responsive-surface | **Kill or absorb** | Genuinely clever, but it is a feature of the factory, not an asset. Absorb into Claude Code Cloud and stop tracking it separately. |

> **NEEDS YOU** — Anything here you'd defend? Especially the Archon/ProductLens
> park: it is the call most likely to be wrong, because I can see its code but
> not how much of your week it actually absorbs.

## What has to be true for this to work

Stated as falsifiable claims, so the investor conversation is about evidence
rather than vision:

1. **A second vertical can be launched without adding humans.** Untested. This
   is the thesis; everything else is commentary. Until app #2 ships with the
   existing headcount, the structure is a hypothesis.
2. **Sextant's equivalence claim holds on a codebase nobody hand-tuned for it.**
   Currently demonstrated against VesselHaven, which co-evolved with it.
3. **The factory's operating cost stays sub-linear in app count.** Per-user
   sandbox provisioning and per-session cloud workers are the cost drivers
   worth watching; 02 will put numbers to this.
4. **The human core can supervise the agent fleet at this ratio.** Three to four
   people directing agent-executed engineering across multiple products is the
   organizational bet 03 has to make concrete.

## Open questions blocking 02 and 03

1. VesselHaven: paying customers? Whose IP? (blocks the entity shape)
2. Sell-the-factory decision: accept the recommendation, or argue it?
3. Jurisdiction and existing entities.
4. Where does the prospective employee sit — Platform (factory) or Apps
   (vertical delivery)? The answer says a lot about which you believe is the
   constraint right now.
