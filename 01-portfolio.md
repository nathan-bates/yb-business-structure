# 01 — Portfolio

*Version 2 — rewritten 2026-09-10. Supersedes the incremental v1; the argument
history lives in git and the retired positions are summarised in the appendix so
they don't get re-litigated.*

---

## What this is

A proposal for the Youbiquity portfolio: what the group is building, where value
accrues, and what has to be true for it to work.

**Audience:** one investor (already in), three co-founders, one prospective
employee. This is an allocation argument among people with full context — where
the next unit of attention and capital goes, and why — not a pitch. Marked
`OPEN` where something is undecided, `ASSUMPTION` where I've inferred, and
`DECIDED` where a call has been made.

---

## The thesis

Youbiquity is **three revenue lines and one option**, built on a bet that the
marginal cost of launching an application falls fast enough to make a portfolio
viable at a headcount that couldn't normally sustain one.

| | **1. Ecosystem** | **2. Core tech** | **3. Apps** |
|---|---|---|---|
| **What** | Four-agent topology with a generative per-person UI: `project-k`, `aux`, `arbiter`, `library`, `market` | The agent stack sold/leased as consulting | VesselHaven, App #2, #3… |
| **Horizon** | 3–5 yr | Now | 1–2 yr |
| **Risk** | High | Low | Medium |
| **Scales with** | Accumulated preference data | **Humans — linear** | Factory maturity — sub-linear |
| **Role** | The outcome | Runway | Distribution, and proof of the cost curve |

Line 2 is the one to watch. It's the only line linear in headcount, the only one
with customers asking for more today, and therefore the one that wins by default
unless deliberately capped.

### What the ecosystem play actually is

Not an aggregation platform that schedules things — that's a demonstration. It's
**a program of cooperating agents with a generative UI layer**: an **Arbiter**
that decides what to do for a person and owns what gets asked; a **User Agent**
(`library`) that curates that person's knowledge behind a scope gate; **Kay**
that decides *form, never content*, composing an interface from atomic
primitives; and a **Service Agent** (`market`) that reaches the outside world.
`aux` is the protocol seam between them, plus a reference renderer and design
system.

The purpose is the **collaboration flywheel**: better signals → better learning →
more trust → more autonomy → more delegation → more signals.

---

## What is actually true today

Stated plainly, because several sections below depend on not overstating it.

| Asset | Reality |
|---|---|
| `project-k` | Knowledge, architecture, ADRs. Product architecture **intentionally undecided**. No application code. |
| `aux` | v0 POC. Flywheel loop verified end to end, renderer tests pass, live model path barely exercised. Nine packages, all form-layer. |
| `arbiter`, `library` | Reference implementations with `src/` and tests. Status: proposed. |
| `market` | **README only.** Created 2026-09-02. The thinnest component, and the one everything external routes through. |
| Cart spike | Real writes at two merchants (Amazon, Total Wine) in the person's own session; aggregated cart over the merchants' real carts; framework-authored recipes armed by a person. **The most evidenced thing in the portfolio.** |
| Sextant | **Theoretical.** 2–4 week evaluation pending. Nothing in this document should be read as depending on it. |
| Claude Code Cloud | Load-bearing, runs daily. |
| VesselHaven | Nearly launched, pre-revenue. ~$250k, ~11 months. One app of many — a muscle-memory exercise, not a strategic relationship. |

---

## Strategy

### The portfolio is the distribution strategy

No single app is the relationship. The factory makes app N+1 cheaper. So the
shape is **many small apps, each cheap, sharing one preference layer.** The
compounding assets are the cross-app preference profile and the cost curve
itself — neither of which lives in any individual app.

This is genuinely unusual positioning. The giants build one interface for
everyone. Vertical SaaS builds one app for one market. Almost nobody builds many
small apps sharing one accumulating model of the person.

### It has exactly one failure mode

> **Do the apps share a user?**
>
> If the same person uses several, the preference layer compounds and app
> selection follows *same person, different need*. If not, the group holds N
> disjoint tiny datasets — a software shop with excellent tooling. A fine
> business, but not this one.

`OPEN` — **name the cohort, not the market.** Who is the person the next three
apps all serve? If they can't be named in a sentence, the cross-app preference
layer is a hope rather than a plan.

### Community is the spine

VesselHaven's marine functionality doesn't overlap with other apps. **Its
community feature does.** That matters more than it sounds: berth booking teaches
you about a vessel; community teaches you about a *person* — what they care
about, whose opinion they trust, how they want to be addressed. That's the signal
class that transfers across domains, and precisely what `library` exists to hold.

Two consequences:

1. **Community is a platform primitive that happens to launch inside VesselHaven
   first.** Build it to be extracted — shared identity, profile, social graph,
   sitting beside `library` — not as a VH module copied into app #2. Getting this
   wrong is paid twice: rebuilding it, and finding the profiles don't join.
2. `OPEN` — **one community, or one per app?** *One community, many apps* is the
   only version where the portfolio compounds. *Many apps, each with a community*
   is N cold starts and N thin datasets. This is an identity-model decision and
   those calcify; it should be settled **before VH's community ships.**

Community is also plausibly the **wedge**: the one thing a generalist agent
cannot supply, because it isn't a capability — it's other people. ChatGPT cannot
give a yacht owner the other yacht owners.

### Wedge and moat are different questions

A moat protects a position you hold. A wedge is why the first cohort shows up at
all. Moats are late-game and made of accumulated data; wedges are early-game and
usually made of *fit* — solving one workflow so specifically that the generalist
is annoying by comparison.

**A wedge is allowed to depreciate.** You use one to buy a relationship, and the
relationship compounds afterwards. What's fatal is investing in a depreciating
asset and expecting it to be the moat.

---

## Where value accrues

### The model-curve test

For any proposed investment: **does this get more or less valuable when the next
frontier model ships?** Capability gaps depreciate — you're betting against the
curve. Accumulated assets appreciate.

| | Next model ships → | |
|---|---|---|
| Reliability scaffolding, recipes | Subsumed | **Depreciating** |
| Drivers, reach, integrations | Subsumed, and identity-gated besides | **Depreciating** |
| Generative UI mechanics | Subsumed — A2UI, MCP Apps | **Depreciating** |
| Per-person preference data | Worth more | **Appreciating** |
| Domain data the giants don't hold | Worth more | **Appreciating** |
| Earned-autonomy / trust record | Worth more | **Appreciating** |
| Customer relationships in a vertical | Unaffected | **Durable** |

**Build what appreciates. Wrap what depreciates. Time-box anything whose value
assumption is "models won't do this well soon."**

Applied: build `library`, the trust/autonomy record, community, and domain-data
capture inside the apps. Wrap `market`, the model, and reliability scaffolding.
Keep the cart spike for the preference signal it generates and as proof the
topology works — not as a capability demo, because the capability is on the curve.

### The proprietary layer has two halves

| | **Per-person preference** | **Per-scenario elicitation** |
|---|---|---|
| Owner | `library` | `arbiter` |
| Compounds | Per user | **Across all users** |
| Buys | Retention, switching cost | Quality on a new user's *first* session |

Consistency is why this must be owned rather than delegated to a stock model: an
interface that asks different questions on Tuesday than Monday can't accumulate
trust, which is the flywheel's only currency.

**Performance is the same asset viewed twice.** ADR 0026 already has Kay
*selecting* rather than generating — a pure exhaustive switch resolving the same
shape to the same pattern, with model-assisted composition behind a flag. That's
snappiness by construction. The principle: **latency is what you pay for missing
knowledge.** The flag is where latency re-enters, and it should carry an explicit
latency budget.

### `DECIDED` — `market` wraps, it does not build

Integration breadth scales with headcount and capital and is copyable. Four
people don't out-scan a hundred. `market`'s contract is already C10
find-candidates / C9 effect — an interface over execution, not a driver library —
so this is a build decision, not a re-architecture.

What the wrapper must still own, or it's a dependency rather than a strategy:

- **Two live provider implementations.** One is not an abstraction.
- **Outcome verification.** A pass-through that can't confirm its own effects
  owns the blame and none of the control.
- **Per-provider quality signal**, so substitution is evidence-driven.
- **The interaction record.** Providers see the action; they never see the
  approve/edit/feedback loop. That's the flywheel's input and the one thing no
  execution vendor can reconstruct.

---

## Economics

### The cost curve is the strategy, not a metric

| | Cost | Elapsed |
|---|---|---|
| VesselHaven (actual) | ~$250k | ~11 months |
| App #2 (target) | ≤$125k | ≤5.5 months |
| App #3 (target) | ≤$62k | ≤3 months |
| **Floor** | **`OPEN`** | **`OPEN`** |

**The floor is the only number that matters.** If halving held forever, every app
Youbiquity will ever build would cost ~$500k in total. It won't — there's a floor
F, and past the first few apps N apps cost roughly $500k + N × F. **F alone
decides how many apps the portfolio can hold**, and therefore whether this is a
strategy or a slogan. At 20% improvement per app rather than 50%, the series never
converges — app #6 still costs $82k. That gap is the difference between a
portfolio and a software shop.

**What compresses, and what doesn't:**

| Component | Compresses? |
|---|---|
| UI/UX design and build | **Strongly** — Kay/AUX, the biggest lever |
| Auth, payments, notifications, infra | **Strongly** — shared services, one-time |
| Integrations | Yes — the wrapper decision |
| QA / verification | *Unproven* — Sextant is theoretical |
| Domain modelling | **Weakly** — only if app #2 serves the same cohort |
| Content seeding, go-to-market, support | **No — and grows with app count** |

That last row is where the wall probably is: **the asymptote is operational, not
engineering.** Build cost falls while the cost of operating N apps rises roughly
linearly. Portfolio strategies die of operations long before they die of
engineering.

### Payback

At ~$250k and no take rate, break-even on build alone is roughly a thousand
sustained subscribers per app for a year at $20/month. Worth sanity-checking
against realistic cohort sizes **before** app #2 is chosen — it may rule out
small verticals entirely.

`OPEN` — monetisation: subscription, B2B licensing to app owners, or per-vertical
SaaS?

### VesselHaven is not a clean baseline

The acceleration tooling was built *during* VH, and adoption was contested. The
$250k/11 months bundles building the factory with using it. **App #2 is the first
real measurement the group will ever have** — instrument it accordingly, and
define the measurement before it starts: what counts in the money (contractors,
founder time at what rate, agent spend, infra, design, licences), what counts as
"released", what counts as the start.

`DECIDED (proposed)` — **pre-commit the failure threshold.** If app #2 lands above
~$150k or beyond ~7 months, treat it as evidence against the portfolio strategy
rather than a one-off overrun, and revisit before starting app #3.

---

## How engineering gets bought

This is a portfolio question, not an operations detail — it currently gates the
entire cost curve.

> **Under time-and-materials, the buyer captures none of the productivity gain.**
> A tool that doubles a contractor's output becomes their leisure, not
> Youbiquity's cost reduction. If adoption reduces billed hours, the vendor's
> revenue falls — so a body-shop isn't merely indifferent to the factory, it is
> **rationally opposed** to it.

No tooling or advocacy survives that. And the deeper version: an hourly agency's
business model is *selling hours* while the strategy is *reducing hours*. You can
align one project. You cannot align a relationship.

| Option | Captures gain? | Cost |
|---|---|---|
| Fixed price per deliverable | Yes | Scope disputes; needs checkable acceptance criteria |
| Target cost + gainshare | Partly | More complex to administer |
| Equity/employment for continuing people | Fully | Fixed cost — but a small owning core is the goal anyway |
| New partner, fixed-price from day one | Yes | Loses continuity; confounds app #2 as a measurement |
| Agents + thin human review | Eliminates the question | Unproven at whole-app scale |

**Youbiquity is unusually well-placed for fixed-price.** Such engagements
normally degenerate because "done" is arguable; this group produces ADRs, specs,
the C5–C10 contracts and invariants — an artifact set that makes acceptance
substantially more checkable. The architecture is incidentally a procurement
asset. *(Sextant would make acceptance machine-verifiable and strengthen this
considerably — do not price that in until it's real.)*

**Cheapest diagnostic available: ask the incumbent for a fixed-price bid on app
#2.** A vendor who won't price their own output is telling you they don't believe
the tools help or don't intend to use them. The question resolves without a
confrontation about hours.

**The window is open now.** The cheapest moment to change a commercial model is a
project boundary. If app #2 starts on T&M, the terms carry for another cycle.

---

## Entity shape

```
                 Youbiquity Group  (umbrella / holdco)
                 - investor equity; owns and licenses down all IP
                 - employs the human core
                 - INCUBATES the ecosystem play (no entity yet)
                            |
            +---------------+---------------+
            |                               |
   Youbiquity Platform            Youbiquity Apps  ("App Portfolio")
   (core tech + consulting;                 |
    revenue-generating)               +-----+-----+
                                      |           |
                                 VesselHaven   App #2, #3...
```

- **VesselHaven is a grandchild, not a direct subsidiary.** It keeps each vertical
  individually disposable: a buyer acquires a clean entity holding the app, its
  contracts and its customers, while the platform IP that built it stays in the
  group and keeps serving every other app. A direct subsidiary with entangled IP
  forces a carve-out at sale time, under pressure, with the buyer's lawyers
  setting the pace.
- **Platform IP sits at Group level and licenses down.** Much cheaper to
  establish pre-revenue than to restructure later.
- **The ecosystem play stays in the Group** until the first of: capital specific
  to it, something worth protecting, or a partner requiring a counterparty. Note
  an open protocol plus a closed preference layer may eventually want two homes.

`OPEN` — jurisdiction and existing entities. The shape is jurisdiction-neutral;
the IP licensing mechanics are not, and transfer pricing will shape them.

---

## Risks

1. **The apps don't share a user** — the portfolio becomes N disjoint datasets.
   The single largest risk, and it's answerable now rather than discoverable later.
2. **The cost curve doesn't bend**, because the commercial model prevents it.
   Currently the binding constraint.
3. **Operational asymptote** — support, GTM and compliance grow linearly with app
   count and eat the build savings.
4. **Platform risk** — wrapping execution providers means their terms, their
   granularity, their roadmap, and the possibility they build the preference/UI
   layer themselves. Mitigated only by two live implementations and by owning the
   interaction signal.
5. **Identity gating.** From 15 Sept 2026 Cloudflare blocks Agent-classified bots
   by default on ad-serving pages for new domains; Shopify rate-limits agents
   without a signed identity while the incumbents' agents sign theirs. Reach is
   being allocated to recognised players — an argument for wrapping one.
6. **Standards competition** — A2UI, MCP Apps and Open-JSON-UI occupy AUX's
   ground; Anthropic ships an Apache-2.0 commerce-agent blueprint including
   memory-personalisation. Don't expect to win on protocol design.
7. **Legal posture on co-browse.** It's the one execution mode that degrades
   gracefully under identity gating, and the one a merchant could characterise as
   routing around its controls. The Ninth Circuit's reversal of Amazon's
   injunction against Perplexity helps; the case is unresolved. Make it a
   deliberate posture with legal input.
8. **Consulting eats the company.** It's a relationship business in *other
   people's* domains — it generates no compounding data for you while consuming
   the attention that would.

---

## What has to be true

Ordered by how much rests on each.

1. **The apps share a user.** Everything compounding depends on it. Unevidenced.
2. **Delegation compounds** — people grant more autonomy as trust accrues. v0
   proved the mechanism, explicitly not learning at scale.
3. **App #2 costs ≤$125k in ≤5.5 months**, and the decomposition shows why.
4. **The commercial model can change**, or (3) is unreachable regardless of tooling.
5. **Someone else's execution layer is good enough to wrap.** Testable now — the
   spike drives real writes at two merchants.
6. **Operations stay sub-linear in app count.**

---

## Appendix — retired positions

Kept so they don't get re-argued. All were live at some point in the analysis.

| Position | Why it was retired |
|---|---|
| The factory is the flagship | It's a building block; the ecosystem play is the centre of gravity |
| `aux` is the co-browse "hands" | `aux` is the agent-to-UI protocol; co-browse is a separate Service Agent capability |
| Sell Sextant as a product | Productisation adds human-shaped work that fights the minimal core; consulting/lease is the fit |
| `market` builds a driver library | Arms race against 10–100× headcount |
| Sextant as the AUX adoption bridge | Needs source access; and the wrapper decision removed the need |
| "Act in the person's own accounts" as a niche | Industry standard — OpenAI deprecated Instant Checkout Mar 2026; UCP Identity Linking Apr 2026 |
| "Clean aggregation" as a niche | Google Universal Cart, May 2026 — Nike, Target, Walmart, Sephora, Wayfair |
| "Permission-free reach" as a niche | Any computer-use agent does it; Ninth Circuit lifted Amazon's injunction |
| "Reliability scaffolding" as a niche | On the model curve — 80% was a generation-old model |
| Generative UI mechanics as a niche | A2UI, MCP Apps, native composition |
| Marine as the domain to compound in | VesselHaven is one app of many, not a relationship |
| "The moat is a relationship in a domain" | True for a company that has one; premature for a company that doesn't. Replaced by wedge-then-moat |

**Sources.** [Agentic commerce protocols, Instant Checkout deprecation, UCP Identity Linking](https://opascope.com/insights/ai-shopping-assistant-guide-2026-agentic-commerce-protocols/) ·
[Google Universal Cart](https://www.digitalcommerce360.com/2026/05/20/google-universal-cart-for-agentic-commerce/) ·
[Grok Bot shopping](https://www.axios.com/2026/06/03/exclusive-spacexai-and-gopuff-help-you-shop-for-more-stuff) ·
[Claude Commerce Agents](https://www.marktechpost.com/2026/09/03/anthropic-released-claude-commerce-agents-an-apache-2-0-blueprint-for-shopping-and-merchant-agents-across-retail-travel-telecom-and-entertainment/amp/) ·
[Generative UI, A2UI, MCP Apps](https://www.copilotkit.ai/blog/the-developer-s-guide-to-generative-ui-in-2026) ·
[Amazon v. Perplexity injunction](https://www.cnbc.com/2026/03/10/amazon-wins-court-order-to-block-perplexitys-ai-shopping-agent.html) ·
[Ninth Circuit reversal](https://www.engadget.com/2230471/perplexity-has-successfully-overturned-amazon-injunction-on-its-ai-shopping-bot/) ·
[Cloudflare AI traffic defaults](https://developers.cloudflare.com/changelog/post/2026-07-01-ai-traffic-options/) ·
[Cloudflare signed agents](https://blog.cloudflare.com/signed-agents/) ·
[WebRetriever real-site success rates](https://arxiv.org/pdf/2607.06118) ·
[WebArena](https://www.emergentmind.com/topics/webarena-benchmark) ·
[Browser Use benchmark](https://browser-use.com/posts/ai-browser-agent-benchmark)
