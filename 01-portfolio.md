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

> **Correction to an earlier draft of this document.** It claimed both halves of
> the moat were unbuilt. That was wrong: the `Youbiquity` org holds reference
> implementations for each — **`arbiter`** (the decider; owns what gets asked)
> and **`library`** (the User Agent + User KB, with a Librarian scope gate
> separating the "restricted section" of domain facts Kay may never see from the
> "open stacks" of presentation preferences). Both have `src/` and `test/`.
>
> The thin one is **`market`** — the Service Agent/Broker, holding drivers for
> external businesses and answering *what can it be asked* (C10) and *what can it
> effect* (C9). Created 2026-09-02, README only. That is the component through
> which the entire ecosystem reaches the outside world, and it is the least
> built. It is also where any Sextant-derived driver would land.

### 2. Sextant solves the standards cold-start problem, and that's its real job.

Every standards play dies the same way: nobody adopts it because nobody has
adopted it. AUX needs apps that speak it; almost no app does.

Sextant maps an application you don't control into a structured graph — without
docs, without an API, by exploration. That is a **bridge from the non-AUX world
into the AUX world**: you don't need an app to adopt the protocol if you can
derive its surface and drive it. Which makes Sextant not a testing tool, not
merely "map-making IP", but *the adoption strategy*.

There are three distinct ways to use it, and they are not equivalent:

- **(a) Internal adapter-generator.** Sextant's graph becomes a Service Agent
  driver. Concrete seam: ADR 0029/0030's **write recipes** ("authored by the
  framework, armed by a person"). **Constrained by source access — see below.**
- **(b) External integration SDK.** Give it to app owners: "run this, get an AUX
  manifest, you're a citizen." Lowers adoption cost — but it is productisation,
  with docs, support and SLAs, i.e. the human-shaped work that fights the
  minimal-core goal. Cheap for *partners and owned apps*; expensive as a
  self-serve public tool.
- **(c) Pattern-mining source.** Mapping many apps reveals recurring interaction
  patterns, feeding the Tier-3 pattern library and the Arbiter's elicitation
  patterns. Quietly the most compounding of the three.

#### The source-access constraint

Sextant today requires the codebase. Its rigor comes from things only source
grants: istanbul/nyc **coverage instrumentation**, injected `data-sextant-id`
**stable selectors**, **state seeding** via API/DB, and reading role/permission
catalogs. So "point it at any app" is not available, and the cold-start claim
needs narrowing.

What splits cleanly:

| Capability | Needs source? |
|---|---|
| Coverage measurement / completeness signal | **Yes** |
| Stable selectors | **Yes** |
| Ephemeral-user seeding, role catalogs | **Yes** |
| Exploration → state/transition graph | No, in principle |
| Recipe authoring + verification by replay | No, in principle |

The Service Agent needs the bottom two, not the top three — so this is an
engineering gap rather than an impossibility. But losing source costs three real
things, and one of them bites the thesis:

1. **No completeness signal.** Coverage is how you know exploration finished.
   Without it you cannot tell a mapped app from a partially mapped one.
2. **A brittleness treadmill.** Without stable selectors, third-party UI churn
   breaks recipes. Maintenance scales with apps × churn rate — the direct enemy
   of marginal-cost-to-zero. Recipes decay; **patterns don't**, which is the
   strongest argument for (c).
3. **No seeding.** You cannot create test users in someone else's app.
   Exploration must run live in the real user's account, side-effecting — which
   is precisely why ADR 0029 makes the first write a hold on a fixture venue.

**Revised recommendation — three access tiers, matching `market`'s own driver
taxonomy ("a shop with an API, a shop that must be driven in a browser, a venue
that can hold a table"):**

- **Tier 1 — own apps** (source, build, DB): full Sextant, full rigor. Where the
  marginal-cost thesis actually gets proven.
- **Tier 2 — partners** (agreement, sandbox credentials, possibly no source):
  exploration + recipes, no coverage metric, stability notice negotiated in the
  agreement. **This is where option (b) matters more than I first credited** —
  the integration SDK is the tier-2 unlock.
- **Tier 3 — unaffiliated third parties**: do *not* attempt to map the app.
  Use co-browse (the user is genuinely present in their own session) and go
  **depth-first** — a small number of high-value capabilities per domain (buy,
  book, hold) rather than whole-app maps.

The strategic correction: cold-start is **not** solved by unilaterally mapping
the world. It is solved by depth in tier 3 and relationships in tier 2 — and
the durable asset extracted from all three tiers is the pattern library, not
the recipes.

Note also that the consulting line sells Sextant as a tool, pulling it toward
(b), while the ecosystem line wants it at (a) and (c). That tension is currently
unmanaged.

### 2b. Do not race on integration breadth — wrap it

**DECISION (proposed, Nathan 2026-09-10):** `market` is a thin adapter over
execution providers that already work, not a driver library Youbiquity builds.

The reasoning is hard to argue with: integration breadth scales with headcount
and capital, it is copyable, and OpenAI, Google and xAI are all shipping
computer-use and connector layers. Four people do not out-scan a hundred. Any
plan whose success requires winning that race is a plan to lose slowly.

The good news is that the architecture already anticipated this. `market`'s
contract is **C10 find-candidates / C9 effect** — *what can it be asked* and
*what can it effect* — not "here are our drivers". That is an interface over
execution, so making providers substitutable is a build decision, not an
architectural change. This is commoditise-your-complement: let the execution
layer get cheap and better on someone else's budget, and own the layer above it.

**What the wrapper must still contain**, or it isn't a strategy:

- **At least two real provider implementations.** An adapter with one
  implementation is not an abstraction, it's a dependency. This is also the only
  real mitigation for platform risk.
- **Outcome verification.** Did the booking actually happen? A pass-through that
  can't confirm its own effects leaves you owning the blame and none of the
  control.
- **Per-provider quality signal**, so routing can improve — and so provider
  substitution is evidence-driven rather than a rewrite.
- **The interaction record stays yours.** Providers see the action; they do not
  see the approve/edit/feedback loop. That signal is the flywheel's input and
  the thing no execution vendor can reconstruct.

**Consequence for Sextant.** If the group is not building a driver library,
Sextant's role as the AUX adoption bridge largely evaporates. What remains is
tier-1 rigor on owned apps and pattern-mining. **Sextant reverts to a line 2/3
asset** — the consulting offering and the app factory — rather than a line 1
asset. That resolves the tension flagged above in favour of line 2, and it is a
simplification worth taking.

### 2c. So what is the niche? (researched 2026-09-10)

An earlier draft of this section asserted four candidate niches from reasoning
rather than evidence. Nathan challenged it; the research says he was right and I
was wrong on the two most important points. What follows is sourced.

**Candidate 1 — "act in the person's own accounts, never intermediate the
transaction" — is NOT a niche. It is now the industry's mainstream design.**

- OpenAI **deprecated Instant Checkout in March 2026**, moving to a model where
  the agent recommends and the shopper completes on the *merchant's own site*,
  explicitly so brands keep the customer relationship, login, and loyalty
  engagement.
- The **April 2026 UCP update added Identity Linking**, so shoppers on
  UCP-integrated platforms get the same loyalty and member benefits they'd have
  logged into the retailer directly.

My reasoning — that take-rate incentives would deter incumbents from leaving the
purchase with the merchant — was simply wrong. They tried intermediated checkout
and *retreated from it*. Merchant-side checkout is currently the preferred model
across the industry.

**Aggregation is also occupied — but only partially, and the boundary is the
whole opportunity.**

**Google Universal Cart** (announced at I/O, 19 May 2026) combines purchases from
multiple merchants into a single cart across Search, Gemini, YouTube and Gmail,
with Google Wallet supplying loyalty information and merchant offers. Early
merchants: Nike, Sephora, Target, Ulta, Walmart, Wayfair, and Shopify merchants
including Fenty and Steve Madden.

So "clean multi-merchant aggregation" as a general claim is taken. **But
Universal Cart is built on UCP, and UCP is a participation protocol.** It can
only aggregate merchants who integrate. That leaves two structural gaps:

1. **Walled gardens that will never join a Google protocol.** Amazon is not
   mentioned anywhere in the Universal Cart coverage, and as a rival ecosystem
   has no reason to participate. The largest retailer is therefore structurally
   outside protocol-based aggregation.
2. **The long tail** — merchants who will not integrate anything, ever.

**The spike already reaches both**, because it drives the person's own
authenticated session rather than asking permission: add-to-cart in the person's
own Amazon session, one aggregated cart over the merchants' real carts, at
Amazon and Total Wine.

> **This is the defensible statement of the niche:** not "cleaner aggregation
> than Google" — four people do not win a UX-quality contest against Google — but
> **aggregation across merchants that protocol-based aggregation structurally
> cannot reach.** Amazon is the flagship proof. That is a permission-free
> position, and it is durable exactly because the incumbents' approach is
> permission-based by construction.

*Source discrepancy, flagged rather than resolved:* a 2026 protocol guide states
that neither ACP nor UCP specifies cross-merchant carts, and lists multi-item
carts as "coming soon" — while the May 2026 Universal Cart coverage describes
cross-merchant carts shipping. Most likely Universal Cart is a Google product
layer above UCP rather than a spec feature. Worth confirming before this is used
externally.

**Generative UI is contested too.** Google's **A2UI** (late 2025) lets agents
generate widgets inline in a conversation; **MCP Apps**, Open-JSON-UI and
CopilotKit occupy adjacent ground. AUX therefore has direct standards
competition, which reinforces the earlier conclusion: do not expect to win on
protocol design. Anthropic's **Claude Commerce Agents** (2 September 2026) even
ships an Apache-2.0 shopping-agent blueprint whose five skills include
*memory-personalization* — so personalization-as-a-feature is being commoditised
as well.

The distinction that survives: every one of these generates UI from **content
context, inline in a chat transcript**. None is driven by a durable, accumulated
model of a specific person, and none has a feedback flywheel converting each
interaction into a better-fitting interface next time. That is a narrower claim
than "generative UI" and it is the one worth defending.

**Revised niche, in one sentence:** reach the merchants protocols can't, and
present the result through an interface shaped by an accumulating per-person
preference model — with the aggregated cart as the first demonstration of both
at once.

**Sources.** [OpenAI deprecating Instant Checkout / merchant-side model + UCP
Identity Linking](https://opascope.com/insights/ai-shopping-assistant-guide-2026-agentic-commerce-protocols/) ·
[Google Universal Cart](https://www.digitalcommerce360.com/2026/05/20/google-universal-cart-for-agentic-commerce/) ·
[Grok Bot shopping / Stripe Link virtual cards](https://www.axios.com/2026/06/03/exclusive-spacexai-and-gopuff-help-you-shop-for-more-stuff) ·
[Claude Commerce Agents](https://www.marktechpost.com/2026/09/03/anthropic-released-claude-commerce-agents-an-apache-2-0-blueprint-for-shopping-and-merchant-agents-across-retail-travel-telecom-and-entertainment/amp/) ·
[Generative UI frameworks, A2UI, MCP Apps](https://www.copilotkit.ai/blog/the-developer-s-guide-to-generative-ui-in-2026)

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

1. **Standards adoption.** The hardest category to win, and 2026 is crowded.
   Winning on protocol design alone is unlikely; winning on accumulated
   preference data is more plausible.
1b. **Platform risk.** Wrapping execution providers means their terms, their
   granularity, their roadmap — and the possibility that they build the
   preference/UI layer themselves. Mitigated only by holding two live provider
   implementations and by owning the interaction signal they never see.
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
2. **Someone else's execution layer is good enough to wrap.** Replaces the
   earlier claim about Sextant mapping unfamiliar apps, which the wrapper
   decision makes moot. Testable now: the spike drives real writes at Amazon and
   Total Wine through recipes the framework authored and a person armed.
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
