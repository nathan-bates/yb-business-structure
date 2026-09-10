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

**And permission-free reach is not a moat either.** Cloud-browser agents already
do it: a GrokBot given its own inbox (AgentMail) and a Stripe Link virtual card
buys from Amazon on a remote Chrome instance, no merchant cooperation involved.
Any computer-use agent can take this path.

The law currently permits it — for everyone:

- Amazon sued Perplexity over Comet in November 2025; a district court granted an
  injunction in **March 2026**; the **Ninth Circuit reversed**, finding the
  claimed harms, the balance of equities and the public interest all favoured
  Perplexity. The case is not finally resolved.
- The judicial framing that matters: Comet accesses Amazon accounts **"with the
  Amazon user's permission, but without authorization by Amazon."**

Two consequences. First, "reach merchants protocols can't" is available to every
well-funded competitor, so it cannot be the moat — my previous draft was wrong
again. Second, the legal theory that survived appeal is specifically about acting
**in the user's own account, with the user's permission** — which is the
co-browse posture, and a stronger position than an agent transacting from its own
inbox and virtual card (no order history, no loyalty, no returns path, and a
weaker ToS argument).

### 2d. The uncomfortable conclusion

Three candidate niches have now failed the same test: acting in the person's own
accounts (industry standard), clean aggregation (Google Universal Cart),
permission-free reach (any computer-use agent, now legally cleared). Generative UI
has A2UI and MCP Apps; personalization ships as an Apache-2.0 blueprint.

**The evidence does not support the existence of a defensible *technical* niche
in the consumer agent layer.** Continuing to search for one is likely to keep
producing candidates that a week of research retires.

What that leaves is not nothing — it's just a different kind of advantage:

1. **Segment, not technology.** The giants are building one agent for everyone,
   monetised through ads and platform position. A four-person company wins where
   they are structurally uninterested: a specific population whose workflows are
   too small, too regulated, or too specialised to prioritise. Youbiquity already
   has an instance of exactly that — VesselHaven's marine domain, where supplier
   coordination is fragmented, high-value, and invisible to Google.
2. **Channel: B2B2C rather than a consumer front-end.** Sell the per-person
   interface layer *into* products people already use — the app portfolio first,
   partners after. The giants are competing for the consumer front door; almost
   nobody is selling app owners the ability to give each of their users a
   different interface. This also fits the minimal-core constraint, because
   B2B2C does not require consumer distribution spend.
3. **Data the giants cannot reach.** Preference and domain knowledge inside
   verticals they do not touch — vessel history, supplier relationships,
   professional workflow context.

**Recommendation:** stop positioning the ecosystem play as a consumer aggregator
competing with Gemini and ChatGPT, and position it as **the per-person interface
layer for vertical applications**, with the aggregated-cart spike retained as the
proof that the four-agent topology works end to end — a demo, not the product.

That reading also collapses a tension that has run through this whole document:
under it, lines 1 and 3 stop competing for attention and become the same motion.
The apps are how the interface layer reaches users, and the interface layer is
what makes app N+1 worth building.

> **NEEDS YOU** — This is a significant repositioning and it is *not* a decision
> I should make. It follows from the research, but it trades a large, contested
> market for a smaller, defensible one, and reasonable people would rather fight
> for the former. If you disagree, the counter-argument to make is that
> distribution can be bought or that the giants' one-size interface is worse than
> it looks — both are arguable, neither is evidenced here.

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

**Revised position, in one sentence:** the advantage is not a technical niche in
the consumer agent layer — it is a per-person interface layer sold into verticals
the giants will not prioritise, proven by the aggregated-cart spike.

**Sources.** [OpenAI deprecating Instant Checkout / merchant-side model + UCP
Identity Linking](https://opascope.com/insights/ai-shopping-assistant-guide-2026-agentic-commerce-protocols/) ·
[Google Universal Cart](https://www.digitalcommerce360.com/2026/05/20/google-universal-cart-for-agentic-commerce/) ·
[Grok Bot shopping / Stripe Link virtual cards](https://www.axios.com/2026/06/03/exclusive-spacexai-and-gopuff-help-you-shop-for-more-stuff) ·
[Claude Commerce Agents](https://www.marktechpost.com/2026/09/03/anthropic-released-claude-commerce-agents-an-apache-2-0-blueprint-for-shopping-and-merchant-agents-across-retail-travel-telecom-and-entertainment/amp/) ·
[Generative UI frameworks, A2UI, MCP Apps](https://www.copilotkit.ai/blog/the-developer-s-guide-to-generative-ui-in-2026) ·
[Amazon v. Perplexity injunction](https://www.cnbc.com/2026/03/10/amazon-wins-court-order-to-block-perplexitys-ai-shopping-agent.html) ·
[Ninth Circuit reversal](https://www.engadget.com/2230471/perplexity-has-successfully-overturned-amazon-injunction-on-its-ai-shopping-bot/) ·
[CFAA analysis of agent access](https://nohacks.co/blog/amazon-perplexity-cfaa-agent-visitor-rights)

### 2e. Testing the "reach is universal" hypothesis (researched 2026-09-10)

Nathan's hypothesis: *there probably isn't a meaningful set of merchants that
aren't already accessible to the latest bots.* Broadly correct for consumer
retail — but the research surfaces two qualifications, and the first inverts the
question.

**Reach is being re-gated by identity, not by capability.**

- **Cloudflare, from 15 September 2026:** new domains get Training- and
  Agent-classified bots **blocked by default on pages that display ads**; Search
  stays allowed. Existing domains keep their configuration. Cloudflare replaced
  the blunt block-AI-bots switch with per-category control in July 2026.
- **Shopify, 7 May 2026:** stricter rate limits on any bot or agent hitting the
  Storefront API or Shopify-hosted pages **without a signed identity**. The
  ChatGPT, Perplexity and Copilot agents that Agentic Storefronts supports are
  signing their requests.
- Akamai reports AI bot traffic up **more than 300%** between 2025 and early 2026,
  which is what is driving the clampdown.

So the risk is not "merchants nobody can reach." It is **"merchants only *they*
can reach"** — access allocated by signed-agent programmes whose members are the
incumbents. For a four-person company this is worse than a coverage gap: it is a
gate that closes quietly, and it further strengthens the decision to wrap a
recognised provider rather than to be an unrecognised agent.

**Co-browse is the exception, and it degrades gracefully.** Execution inside the
person's own browser is not agent traffic to classify — same session, same
fingerprint, same account, person present. As the signed-agent regime tightens,
that property becomes *more* valuable, not less.
*Stated honestly: this is also a position that a merchant could characterise as
routing around its controls. The Ninth Circuit's reversal helps, the Amazon case
is unresolved, and this should be a deliberate posture with legal input rather
than an accident of architecture.*

**Reach is not the binding constraint anyway — reliability is.** On real
websites (WebRetriever), agents average **21.1%** success on basic navigation,
29.2% with operational documentation, and best-in-class Gemini-2.5-Pro in
computer-use mode reaches 45.2%. End-to-end tasks combining navigation with
extraction average **11.8%**. WebArena's best single agent is 61.7% against 78%
for humans. Failure modes are mundane: dynamic UI elements cause 73% of reading
failures, CAPTCHAs 36% of handling failures.

Against that, Browser Use Cloud reports **80.0%** for claude-fable-5 in June 2026.
The spread between ~12% and 80% is not model quality — it is **scaffolding**. And
scaffolding is precisely what the group already builds: a framework-authored,
person-armed, replay-verified **recipe** is deterministic where live navigation is
probabilistic, which makes it both more reliable and faster. The same principle
as ADR 0026: what you know in advance, you do not have to infer at runtime.

**Conclusion.** The hypothesis holds, and it reinforces rather than weakens the
repositioning in 2d. Reach is commoditised; what is not yet commoditised is
*completing the task reliably*, and that is an engineering-scaffolding problem
rather than an integration-breadth problem — which is the one kind of race a
small team can enter, because it is won by design rather than by headcount.

**Sources.** [Cloudflare AI traffic options and Sept 15 defaults](https://developers.cloudflare.com/changelog/post/2026-07-01-ai-traffic-options/) ·
[Cloudflare signed agents](https://blog.cloudflare.com/signed-agents/) ·
[WebRetriever real-website success rates](https://arxiv.org/pdf/2607.06118) ·
[WebArena](https://www.emergentmind.com/topics/webarena-benchmark) ·
[Browser Use benchmark](https://browser-use.com/posts/ai-browser-agent-benchmark)

### 2f. The model-curve test

The reliability argument in 2e does not survive contact with the trajectory.
80% was `claude-fable-5` in June 2026, already a generation back. Frontier labs
are working directly on exactly this number, and any advantage measured as *"the
model can't do X reliably yet"* is an advantage with an expiry date set by
someone else's release schedule.

That retires the last candidate. Across this document, six have now failed:
acting in the person's own accounts, clean aggregation, permission-free reach,
generative UI mechanics, personalisation-as-a-feature, and reliability
scaffolding. They failed for the same underlying reason, which is worth naming
once and then applying as a rule:

> **The model-curve test.** For any proposed investment, ask: *does this get
> more valuable or less valuable when the next frontier model ships?*
> Capability gaps depreciate — you are betting against the curve. Accumulated
> assets appreciate — a better model makes them worth more.

Sorted by that test:

| Asset | Next model ships → | Verdict |
|---|---|---|
| Reliability scaffolding, recipes | Subsumed | **Depreciating** |
| Drivers / reach / integrations | Subsumed, and identity-gated besides | **Depreciating** |
| Generative UI mechanics | Subsumed (A2UI, MCP Apps, native composition) | **Depreciating** |
| Per-person preference data | Worth *more* — better model, better use of it | **Appreciating** |
| Domain data the giants don't hold | Worth more | **Appreciating** |
| Earned-autonomy / trust record | Worth more | **Appreciating** |
| Customer relationships in a vertical | Unaffected | **Durable** |

**Build what appreciates. Wrap what depreciates. Time-box anything whose value
assumption is "models won't do this well soon."**

Applied concretely: build `library` (the preference store), the autonomy/trust
record, and domain-data capture inside the verticals. Wrap `market`, the model,
and the reliability scaffolding. Keep the aggregated-cart spike **only for what
it generates** — preference signal and proof the topology works end to end — not
as a capability demo, because the capability is on the curve.

**And the honest last turn of the screw.** The giants accumulate per-person
preference too, across hundreds of millions of users. So "own the preference
layer" is not safe *in general* either — it is safe only where you hold the
relationship and they do not. Which means:

> **The moat is not a layer. It is a relationship in a domain.**
> Every layer in this stack is reachable by a better-funded competitor. What is
> not reachable is being the system of record for a specific population's
> specific decisions, in a domain where the giants have no relationship and
> insufficient reason to build one.

This is the same conclusion 2d reached from the market side, arrived at
independently from the technology side, which is some evidence it is right.

It also fixes the sequencing, and the order is the opposite of the instinct:
**distribution in a vertical → accumulated data → compounding advantage.** Not
technology → users. A preference layer with no users does not compound; it is
just a schema.

### 2g. Wedge, not moat — correcting a category error

VesselHaven is **not** a domain relationship. It is one app, nearly ready to
launch, pre-revenue, built to prove the factory and develop muscle memory — one
line item among many. Treating it as the strategic relationship (as §2f's closing
paragraph did) was wrong.

Removing that premise exposes a category error running through §2c–§2f. Those
sections asked *what is durable in five years* and correctly retired six answers.
But a company with no customers does not yet need a moat:

> **A moat protects a position you already hold. A wedge is why the first cohort
> shows up at all.** They are different questions and they have different
> answers. Moats are late-game and made of accumulated data. Wedges are
> early-game and usually made of *fit* — solving one workflow so specifically
> that the generalist is annoying by comparison.

This matters because it rehabilitates assets the model-curve test disqualified.
A wedge is **allowed** to depreciate; that is what wedges are for. You use a
depreciating advantage to buy a relationship, and the relationship compounds
after. What is fatal is investing in a depreciating asset and *expecting it to be
the moat* — which is the actual error the model-curve test protects against, and
a narrower claim than the one §2f made.

So the model-curve test survives, with a correction: **apply it to what you
expect to defend with, not to what you expect to win the first cohort with.**

### 2h. The portfolio is the distribution strategy

If no single app is the relationship, and the factory makes app N+1 progressively
cheaper, then the strategy is not "pick a vertical and compound in it". It is:
**many small apps, each cheap, with one preference layer spanning all of them.**
The compounding assets are the cross-app preference profile and the cost curve
itself — neither of which lives in any single app.

That is coherent, it matches the stated goal that each next app costs less, and
it is a genuinely unusual position: the giants build one interface for everyone,
and vertical SaaS companies build one app for one market. Nobody much is building
*many* small apps sharing one accumulating model of the person.

**But it has a single point of failure, and it is the question the portfolio now
turns on:**

> **Do the apps share a user?**
>
> If the same person uses several of them, the preference layer compounds across
> the portfolio and app selection should be driven by *same person, different
> need*. If they don't, you have N disjoint tiny datasets rather than one
> compounding asset — and the group is a software shop with excellent tooling,
> which is a fine business but not this one.

This inverts how apps get chosen. Not "which market is most attractive" assessed
independently per app, but "which app deepens the profile of a person we already
serve". Marine yacht owners, for instance, are also high-net-worth individuals
with property, travel, insurance and concierge needs — that is a coherent person,
and it would make VesselHaven the first probe of a cohort rather than a market
entry. Whether that is the actual intended cohort is Nathan's to say.

**Immediate, cheap action while it's fresh:** VesselHaven's real cost — human
hours, elapsed weeks, agent spend — is the first datapoint on the cost curve that
the entire line-3 thesis rests on. Capture it at launch. Without app #1's number,
app #2's number means nothing.

### 2i. Community is the cross-app surface — so it isn't a VesselHaven feature

Nathan: the marine functionality doesn't overlap much with other apps, but **the
upcoming community feature does.**

That is the most useful structural fact in this document, because it identifies
where the person shows up *as a person* rather than as a boat owner. Berth
booking teaches you about a vessel. Community teaches you about a human — what
they care about, whose opinion they trust, what they respond to, how they like to
be addressed. That is precisely the class of signal that transfers across
domains, and precisely what the `library` preference store exists to hold.

Two consequences follow, and both are decisions rather than observations:

1. **Community is a platform primitive that happens to launch inside VesselHaven
   first.** It should be built to be *extracted* — shared identity, shared
   profile, shared social graph, sitting beside `library` — not as a VH module
   that gets copy-pasted into app #2. The cost of getting this wrong is paid
   twice: once rebuilding it, and once discovering the profiles don't join.
2. **One community, or one community per app?** This is a genuine fork and it
   decides whether the portfolio compounds:
   - *One community, many apps* — the same people, entering through different
     apps. The cohort is real, the profile joins, network effects accrue once.
   - *Many apps, each with a community* — N disjoint communities, N cold starts,
     N thin datasets. This is the failure mode from §2h wearing a different hat.

   The first is much harder to bootstrap and is the only version where the
   strategy works. Worth deciding explicitly before the VH community ships,
   because it is an identity-model decision and those calcify fast.

Community is also plausibly the **wedge** in §2g's sense: it is the one thing a
generalist agent cannot supply, because it isn't a capability — it's other people.
ChatGPT cannot give a yacht owner the other yacht owners.

### 2j. The cost curve is not a metric — it is the strategy

**VesselHaven will cost roughly $250k to release.** That is the first real number
in this document and it changes the shape of the plan.

At $250k per app, "many small apps sharing a preference layer" is **capital-bound
before it is anything else.** Ten apps is $2.5m of build alone, before support,
hosting or marketing. So the cost curve isn't a nice-to-have that demonstrates
the factory works — it is the gating constraint on whether the portfolio strategy
is available at all. If app #2 is not dramatically cheaper, the strategy collapses
back to one or two apps, which is a different and weaker company than the one
described here.

**The decomposition matters more than the total.** Of the $250k:

- How much was **non-recurring** — building factory capability, shared services,
  patterns and muscle memory that app #2 inherits free?
- How much was **VesselHaven-specific** — domain modelling, marine integrations,
  design work that transfers to nothing?

If the split is 80/20 toward non-recurring, there is a steep curve and the thesis
is live. If it's 20/80, there is no curve, only a per-app price. **This
decomposition is the single highest-value measurement available to the group
right now**, and it is much cheaper to do at launch than to reconstruct later.

**State app #2's target explicitly, in advance.** A curve claimed after the fact
is a story; a target set before the fact is a test. If the thesis is real,
something like ≤$100k for app #2 and ≤$50k for app #3 should be nameable now,
along with *which specific capabilities* make the difference.

**The payback bar this sets.** $250k with no take rate (§2c) needs recovering
through subscription or licensing. At, say, $20/month that is roughly a thousand
sustained subscribers per app for a year merely to break even on build — before
any contribution to the group. That number should be sanity-checked against the
realistic size of each target cohort *before* app #2 is chosen, because it may
rule out small verticals entirely and push toward apps with larger addressable
cohorts. It is also the strongest argument yet for resolving Q7 (monetisation)
early rather than late.

### 2k. The cost curve, made testable

**Targets (Nathan, 2026-09-10):** halve per app, on cost *and* time, until an
asymptotic wall.

| | Cost | Elapsed |
|---|---|---|
| VesselHaven (actual) | ~$250k | ~11 months |
| App #2 (target) | ≤$125k | ≤5.5 months |
| App #3 (target) | ≤$62k | ≤3 months |
| Floor | **?** | **?** |

**The floor is the only number that matters.** If halving held forever, every app
Youbiquity will ever build would cost $250k ÷ (1 − 0.5) = **$500k in total** —
the whole infinite portfolio for the price of two VesselHavens. It won't, because
there's a floor F, and past the first few apps the cost of N apps is roughly
$500k + N × F. So F alone decides how many apps the portfolio can hold, and
therefore whether "many small apps sharing a preference layer" is a real strategy
or a slogan. **Name F, and name what's in it.** That is a more useful exercise
than hitting any individual halving target.

For contrast: at a 20%-per-app improvement rather than 50%, the series doesn't
meaningfully converge — app #6 still costs $82k. The difference between "halves"
and "improves a lot" is the difference between a portfolio and a software shop.

**What has to compress, and what won't:**

| Component | Compresses? | Mechanism |
|---|---|---|
| UI/UX design + build | **Strongly** | Kay/AUX — the single biggest lever, and the one being built |
| Auth, payments, notifications, infra | **Strongly** | Shared services; one-time |
| Integrations | Yes | The §2b wrapper decision |
| QA / verification | Yes | Sextant, on owned apps (tier 1) |
| Domain modelling | **Weakly** | Only compresses if app #2 serves the *same cohort* — another argument for §2h |
| Content/data seeding, go-to-market, support | **No — and grows with app count** | |

That last row is the likely location of the wall, and it means **the asymptote is
probably operational, not engineering.** Build cost falls; the cost of *operating*
N apps — support surfaces, app-store presences, compliance, incident response —
rises roughly linearly. A portfolio strategy dies of operations long before it
dies of engineering. Worth modelling in `02`/`03` rather than discovering.

**Time and cost have different floors, and this is the part I'd most want tested.**
Cost falls with automation. Elapsed time falls only if the *critical path*
shortens — and on an 11-month project with a 3–4 person core, a large share of the
critical path is usually not build throughput but **decision latency**: waiting for
review, for a design call, for a founder to choose between two options.

If, say, half of VH's 11 months was waiting rather than building, then agents
doubling build throughput yields ~8 months, not 5.5 — and the halving target is
unreachable by engineering means alone. **To halve elapsed time you have to attack
decision latency**: fewer decision points, pre-committed defaults, more delegation
to agents with standing authority. Which is the same scarce resource as the
attention-allocation problem — meaning the cost curve and the org design are one
problem, not two. `03` should treat them together.

> **NEEDS YOU** — Of VH's 11 months, roughly what fraction was *waiting on a
> human decision* versus *work in progress*? A rough split is enough. It
> determines whether the time target is an engineering problem or an org problem,
> and those have completely different remedies.

**Define the measurement before app #2 starts**, or the numbers won't be
comparable and the curve will be unfalsifiable:

- What counts in the money — contractors, founder time (at what notional rate?),
  agent/model spend, infrastructure, design, third-party licences?
- What counts as "released" — feature-complete, in-store, first paying user?
- What counts as the start — first commit, or first decision to build?

**Pre-commit the failure threshold.** If app #2 lands above ~$150k or beyond ~7
months, treat it as evidence against the portfolio strategy rather than as a
one-off overrun, and revisit before starting app #3. A threshold set in advance is
a test; one set afterwards is a rationalisation.

### 2l. Why VesselHaven isn't a clean baseline — and the adoption problem

The acceleration tooling was built *during* VH, and adoption was painful and
remains contested. Three consequences.

**1. VH's $250k / 11 months is not a control measurement.** It bundles building
the factory with using it, plus the disruption of adopting tools mid-flight.
That cuts both ways: the number overstates what an app costs *with* a factory,
and understates how much of the spend was one-time. It also means the
recurring/non-recurring split asked for in §2j is harder to recover than it
looked — but more valuable, because **app #2 is the first real measurement the
group will ever have.** Instrument it accordingly.

**2. The incentive under the adoption problem is structurally misaligned, and it
is not a personality issue.** The thesis requires work done during app #1 to make
app #2 cheaper. But the people building app #1 are, in the main, contracted for
app #1. They bear the full cost of adopting unfamiliar tooling and capture none
of the benefit, which accrues to an app they may never touch. Resistance under
those conditions is the rational response, not a failure of will — and no amount
of advocacy fixes an incentive.

Remedies, roughly in order of how structural they are:

- **Move factory work out of app delivery.** The core owns and builds the tools;
  delivery consumes them. Contractors are then asked to *use* something, not to
  *invest* in something.
- **Contract for it explicitly.** If factory contribution is a deliverable with
  scope and payment attached, it stops being an unfunded ask.
- **Measure delivery on app-N+1 cost, not app-N throughput** — only works where
  the same people continue across apps, which is worth knowing either way.

**3. The minimal-core thesis offers a third path, and it should be named
honestly.** Agents adopt tooling instantly and without politics. If the build
loop contains few humans, the adoption problem largely dissolves — which makes
"fewer humans in the build loop" a *remedy* for tool contention rather than only
a cost strategy.

That is convenient enough to deserve suspicion. VesselHaven was built by people.
There is no evidence yet that the agent-executed path works at whole-app scale,
and "replace the humans who resist the tools" is a strategy that fails badly if
the agents can't actually do the work. So it should be tested, not assumed.

**The two paths to the time target are different bets, and blending them hides
which one is working:**

| | Path A — human team + tools | Path B — agent-executed + human review |
|---|---|---|
| Requires | Solving adoption: incentives, scope, ownership | Agents performing at whole-app scale |
| Known cost | Management overhead, ongoing friction | Unproven; VH gives no evidence either way |
| Failure mode | Tools built, not used; curve doesn't bend | Quality/throughput collapse; humans re-enter late and expensively |

**Make app #2 the experiment, and instrument it to answer *why* the number moved.**
The single most informative metric available is not dollars: it is **the fraction
of merged work that was agent-originated**, tracked per app. Cost tells you
whether the curve bent; that ratio tells you which path bent it — and therefore
whether the result generalises to app #3.

> **NEEDS YOU** — Which path is app #2 actually testing? If the honest answer is
> "mostly A with some B", say so explicitly and set the ratio in advance, because
> a blended result with no instrumentation will be unreadable.

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
