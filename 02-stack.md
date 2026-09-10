# 02 — Technical stack

*Version 3 — 2026-09-10. About the system: how the parts multiply each other and
what that implies for cost. Follows [01-portfolio.md](01-portfolio.md).*

---

## What the stack is for

Two jobs. Every call below serves one of them:

1. **Make app N+1 cheap** — and, less obviously, make app N cheaper too.
2. **Accumulate the person** — everything that compounds lives in one place and
   is joinable across every app.

A component that serves neither is a candidate for wrapping, buying, or not
existing.

---

## Definitions

The word "layer" is used throughout and was never defined. Each layer is
identified by **what it decides**, not by which repository it lives in.

| Layer | Decides | Persists |
|---|---|---|
| **Model** | Nothing on its own — raw capability everything else sits on | N/A — bought, improves under you |
| **Person** | What is known about an individual: identity, preferences, trust granted, community presence, history | **Forever.** Outlives any app |
| **Decision** | *What to do* for a person, *what to ask*, in what order, and whether to ask at all | Across sessions |
| **Form** | *How* something is presented — which pattern, which density, which primitives. Never what is said | Per interaction |
| **Execution** | How an intent becomes an effect in a system we don't own | Per action |
| **App** | The domain: business rules, content, domain-specific integrations, the surface a user signs up for | Per app |
| **Factory** | How apps get built and maintained — build agents, pipelines, verification, and the counterfactual engine that measures them | Meta: serves the group, not the end user |

Two of these are unlike the others. **Person** is the only end-user layer whose
value increases with time and with app count. **Factory** is the only layer with
no end-user surface at all — it exists solely to change the cost of everything
else, and it contains the one non-Person asset that also appreciates: the session
corpus and the engine that reads it.

---

## The system: what multiplies what

This is the part that matters. The stack's value is not in any component; it's in
a small number of loops, each of which multiplies the others. Losing any one of
them doesn't subtract — it divides.

```
    apps ──────────► person data ──────────► interaction quality
      ▲                    │                          │
      │                    ▼                          ▼
   factory ◄──── cost per app falls          retention / trust
      ▲                                               │
      └───────────── more apps affordable ◄───────────┘
```

**The loops, and their multipliers:**

| Loop | Mechanism | Multiplier |
|---|---|---|
| **Apps → person data → every app** | Each app both consumes and contributes to one profile. App #3 makes apps #1 and #2 better, retroactively | **The core multiplier.** Without it the portfolio is addition, not multiplication |
| **Community → person data** | Community reveals the person rather than the transaction — the signal class that transfers between domains | Feeds the core multiplier with data no single app could produce |
| **Factory → apps → factory** | Cheaper apps justify more apps, which justify more factory investment, which makes apps cheaper | Compounding, but slow, and gated by the commercial model (01) |
| **Person data → less runtime inference** | What is known in advance need not be computed. Latency and cost both fall as the profile deepens | Turns an accumulating asset into a performance advantage |
| **Model curve → wrapped layers** | Execution and raw capability improve on someone else's budget | **Free multiplier**, but only on layers you wrapped rather than built |
| **Specs → agent output → verification** | Better specs make agents more effective; more agent output makes verification more valuable | The factory's internal loop |
| **Sessions → replay corpus → tuned factory** | Every session captured is a future experiment; each model launch re-runs the grid and re-tunes model/context choices | Appreciates on the model curve — rare in this document |
| **Scale → operations** | One support pool, one moderation practice, one compliance posture serving N apps | Turns a linear cost into a sub-linear one — see below |

**Where the multipliers break — these are the real risks in this document:**

- **Apps don't share a user.** The core multiplier collapses to 1 and every other
  loop degrades to a per-app efficiency story. This is the single largest
  technical risk and it's a *product* decision, not an engineering one.
- **Person data is fragmented per app.** Same failure, arriving through the
  schema rather than through the product.
- **A built layer sits where a wrapped one belongs.** You pay to maintain what
  the model curve would have improved for free.
- **Operations grow per-app.** The scale loop runs in reverse and eats the
  factory's savings.

---

## The cost model

v1 presented shared-vs-per-app as a binary: shared trends to zero, per-app is the
floor. That was too crude, and it neglected the biggest lever.

**Per-app cost is not fixed, and it need not grow linearly with app count.**
There are three levers, not one:

| Lever | What it does | Example |
|---|---|---|
| **1. Move left** | Reclassify per-app work as shared | Auth, payments, notifications, identity, moderation tooling |
| **2. Make it faster** | Reduce the unit cost of work that legitimately stays per-app | Generative UI collapses per-app design; agents collapse per-app implementation; content pipelines collapse seeding |
| **3. Exploit scale** | The same per-app function costs less per app as N rises | One support pool absorbing N queues; one moderation practice; one compliance posture; one GTM motion across a shared cohort |

**Lever 2 is probably the largest.** Domain modelling, app-specific content and
per-app integration work stay in the right-hand column forever — but how expensive
they are is a function of the tooling, which is what the factory is for. "It stays
per-app" and "it stays expensive" are different claims.

**Lever 2 is also the only lever with an instrument.** Levers 1 and 3 are
structural and can be reasoned about from the table. Lever 2 depends on choices —
which model, which effort level, how much context — that are currently made by
intuition and never measured. The counterfactual engine below is what turns them
into measurements, which is why it belongs in this document as infrastructure and
not merely as a licensing candidate.

**Lever 3 changes the arithmetic in 01.** That document modelled total cost as
`$500k + N × F` with F constant. If lever 3 works, **F is a declining function of
N** — later apps carry a smaller share of support, moderation, compliance and
go-to-market than earlier ones, because those functions are staffed once and
absorb marginal load. Under one shared community (see Data architecture), the
effect is strongest exactly where v1 claimed it was weakest: **moderation and
support are more scalable than domain modelling, not less.**

So the target isn't "minimise the right column." It's **drive F(N) down on all
three levers, and know which lever each cost line responds to:**

| Cost line | Responds to |
|---|---|
| Auth, payments, notifications, infra | Lever 1 — move left, once |
| UI/UX design and build | Lever 2 — generative composition |
| Model and context configuration for agent work | Lever 2 — **measured by the counterfactual engine**, not guessed |
| Implementation | Lever 2 — agent execution |
| Domain modelling | Lever 2, weakly — and lever 1 *only if apps share a cohort* |
| Content and seed data | Lever 2 — pipelines, generation |
| Support | Lever 3 — one pool, N queues |
| Community moderation | Lever 3 — strongest under one community |
| Compliance | Lever 1 and 3 — one posture, per-domain deltas |
| Go-to-market | Lever 3 — only if the apps share a cohort |

Note how many lines end in *"if the apps share a cohort"*. The cohort question
isn't only the value multiplier — **it is also the cost multiplier**, on the
lines that were supposed to be irreducible.

---

## The calls

Consequences of the above. The reasoning is the model-curve test in 01: build what
appreciates when the next model ships, wrap what depreciates.

| Layer | Call | Because |
|---|---|---|
| Model | **Buy** | Never build; assume it improves under you |
| Person | **Build, closed** | The only end-user layer that appreciates with time and app count |
| Decision | **Build, closed** | Elicitation patterns compound across all users; they set cold-start quality |
| Form | **Build, expect to open** | Commoditising — A2UI, MCP Apps. Keep it thin and the seam clean |
| Execution | **Wrap** — with a small built exception for action inside the person's own session | Arms race, identity-gated, on the model curve |
| App | **Per-app, but attack its unit cost** | Lever 2 is the whole game here |
| Factory | **Build** | The only thing that moves lever 2 — but see the split below |

**The Factory splits in two, and the halves behave oppositely.** The harness,
pipelines and tooling are conventional infrastructure: useful, and steadily
eroded by better models. The **counterfactual engine and its session corpus are
the exception in this document — the one piece of Factory that appreciates on the
model curve.**

---

## The counterfactual engine

*The measurement instrument for everything in the cost model above. Without it,
levers 1–3 are asserted rather than known.*

### What exists

`llm-slack-channel-bridge`, `REPLAY_DESIGN.md`, Phase 1 building. Per-turn capture
of transcript *and* source workspace; deps reconstructed from the lockfile via a
content-addressed depcache; build output re-derived. The primitive:

```
replay(session_id, from_turn=K, P)
    P = do_policy(model/effort) | do_context(prompt/tool) | do_resample(same)
```

Two supporting assets do more work than the replay loop itself. **The mutation
guard**: a turn that changed non-repo state in a way the snapshot can't explain —
hand-patched `node_modules`, a `--no-save` install, an opaque artifact — is
classified and **fails loudly** rather than rebuilding a different tree. Without
it a replay grades a fiction. **The corpus**: ~936 sessions on EFS, never deleted,
four models already in use across them, offline and batchable.

### Why turn-K intervention is the product

Filesystem reconstruction is the enabler — a necessary condition. **Intervening at
a single turn is the value**, and it changes the economics, the validity and what
can be sold.

Full-session replay costs `T × arms × N`. Turn-K costs `1 × arms × N`. At 50–200
turns per coding session that is **one to two orders of magnitude** — the
difference between a grid you run on every model launch and one you cost out and
abandon. Three things follow:

1. **Granularity buys statistical validity.** N-per-cell sampling with confidence
   intervals is only affordable if a cell is one turn. Full-session replay forces
   n=1 by economics — which is the industry default of "we tried it and it seemed
   better". Granularity and rigour are one insight, not two features.
2. **Attribution.** A whole-session re-run yields an end-to-end delta that could
   have originated at turn 3 or turn 47. Intervening at one turn isolates the
   effect *at that decision point* — a causal claim rather than a correlation.
3. **A routing policy is only derivable from per-turn counterfactuals.** "Which
   model is better overall" is one-time procurement advice. "Which *kinds of turns*
   does the cheap tier handle indistinguishably" is a policy — recurring,
   measurable, worth a share of a known inference bill.

It also yields a measurement nobody currently offers: **the drift curve.** Vary K
and measure how far downstream an intervention persists — does a cheap model at
turn 12 cost you at turn 40? In stateful sessions that is *the* routing question.

### Where it connects to the rest of the stack

- **Cost model, lever 2.** Model choice and context configuration are the largest
  controllable inputs to agent-executed work, and today they are set by intuition:
  the design doc's own example is a ~7.7KB global preamble on *every turn of every
  session*, never once measured. Replay converts lever 2 from a hope into an
  instrument.
- **The Factory loop.** Each session captured is a future experiment; each model
  launch re-runs the grid and re-tunes the configuration. The corpus deepens
  either way.
- **Person layer, by analogy not by dependency.** Both are accumulated assets that
  a competitor can design but not copy. The corpus is to the Factory what the
  profile is to the product.
- **Performance.** Same principle as ADR 0026 — what is known in advance need not
  be inferred at runtime. Replay is how you learn what can be known in advance.

### What is genuinely differentiated

Two adjacent categories already exist, and neither covers this. Stating both in
one table rather than claiming the whole territory:

| | Trace / eval platforms | Model routers | **Counterfactual engine** |
|---|---|---|---|
| Examples | LangSmith, Langfuse, Braintrust, Roark | Martian, RouteLLM, Not Diamond, OpenRouter, Entelligence, Cursor | — |
| Replay a request against a new model | Yes | n/a | Yes |
| Prompt / context perturbation | Yes | No | Yes |
| Per-turn decisions for coding agents | No | **Yes — already shipping** | Yes |
| **Workspace + filesystem reconstruction** | **No** | No | **Yes** |
| **Mid-session intervention at turn K** | No | n/a | **Yes** |
| **Observes the counterfactual** | Partially (whole request) | **Structurally never** | **Yes** |
| Guard when state isn't reconstructable | No | n/a | **Yes** |

Two observations carry the position:

> **Trace platforms model an agent as a sequence of LLM calls.** A coding agent is
> a stateful process mutating a filesystem; replaying it faithfully means
> reconstructing that state.

> **Routers are predictive and forward-only.** They choose, and the road not taken
> is never driven. No router can say what would have happened had it chosen
> differently — on your workload, in your repository, at that turn.

Routing is mature and well-capitalised — reported valuations near $1.3B, 30–85%
savings claimed in real deployments, per-turn routing already shipping for coding
agents. **Building a router is a funded fight. Being the measurement layer beneath
one is not**, and it converts routers and model-agnostic harnesses from
competitors into channels: they all sell savings they cannot prove on a customer's
own workload.

That yields three sellable shapes: **audit** ("800 turns went to the frontier
tier; 730 were indistinguishable a tier down — here is the overspend"),
**calibration** (fit a policy to the customer's own history rather than to generic
benchmarks), and **drift safety**.

### The moat is the methodology, not the snapshot

Anyone can capture state in a quarter. Getting a *valid* answer out of a
heterogeneous panel, where trajectories diverge the moment you intervene, is a
research problem — and these choices are the asset:

| Design choice | What it prevents |
|---|---|
| Baseline is a fresh replay of the original condition, **not the recorded outcome** | System-prompt regeneration drift confounding every result |
| Normalise prefix thinking across the panel | Silently handicapping a model whose thinking is origin-locked and drops cross-model |
| Sweep effort as an axis; labels aren't cross-tier comparable | Comparing a model with no effort parameter against one with thinking always on |
| Blinded, order-randomised judge, never a contestant; ties → third-party arbiter | A model preferring its own output — the commonest silent bias in LLM-graded evals |
| N per cell, action-match rate with CIs | n=1 conclusions |
| Grade on the cost / quality / time frontier | Optimising quality into a bill nobody will pay |
| Mutation guard fails loudly | Grading a reconstructed tree that differs from the one the turn actually had |

**In a measurement business, being right is the moat — the buyer cannot verify the
answer themselves, which is why they are buying it.**

### Commercial shape

| | |
|---|---|
| **Trigger** | Vendor-forced model retirements — calendar-driven, recurring, budgeted as risk rather than tooling. Current best-practice guidance for these migrations already asks for *"evidence from our own workload"*, which is a one-line description of the output |
| **Buyer** | Platform / AI-infra teams running coding agents at material spend; secondarily procurement, which needs a defensible artifact |
| **Pricing** | Share of measured savings or subscription, renewing on every model launch — not a per-migration project sale, because a routing policy is recurring where migration advice is not |
| **Distribution** | Routers and model-agnostic harnesses as channel; they cannot prove their own value proposition |

**Risks.** The window is **12–18 months**, not open-ended: workspace
reconstruction is a roadmap item for an incumbent, not a research project, once
coding agents become the dominant workload — time matters more for this asset than
for anything else in the portfolio. Our corpus proves the method and calibrates
grading, but it is not a moat against a buyer's own data; position it as
credibility. And the category is crowded, so the constraint is attention rather
than capability.

`PROPOSED` — **publish the methodology, sell the implementation.** In measurement,
the product is trust, and trust accrues to whoever defines how the measurement
should be done. The hard parts — implementation, integration surface, calibration
— don't leave with a paper. It converts copyability into authority: competitors
implementing your design validate it. And it suits the group's constraints, being
agent-assistable and needing no sales headcount, unlike buying attention in a
crowded category.

`OPEN` — this is a strong candidate for **first asset licensed** (01 §Line 2, and
QUESTIONS Q13): it works, it is horizontal, its value is denominated in a bill the
customer already receives, and it needs no strategic buy-in.

---

## Seams

Boundaries are what keep the multipliers separable — a wrapped layer can only be
replaced if nothing has leaked across its seam.

| Seam | Must stay true | If it blurs |
|---|---|---|
| Decision ↔ Form | Decision sends interaction shape and content; Form decides form only | Content originates in the presentation layer and can't be reused elsewhere |
| Form ↔ Person | Form reads *presentation-class* preferences only; domain facts stay behind the scope gate | The open layer acquires knowledge of the closed one — **this is a commercial boundary as much as a technical one** |
| Decision ↔ Execution | Effect and find-candidates stay provider-agnostic | The wrapper has become a dependency and provider substitution becomes a rewrite |
| App ↔ Person | Apps read the profile through the gate; they never hold their own copy | The core multiplier dies quietly, one app at a time |

---

## Data architecture

The precondition for every multiplier above:

> **One identity, one profile, one community — joinable across every app.**

If a person is a different record in each app, the core loop never runs and the
portfolio is addition. This has to be true at the schema level from app #2
onward; retrofitting it is expensive and usually partial.

- One identity provider; app accounts are projections of it.
- The profile lives once; apps read it through the scope gate.
- Preference entries carry provenance — which app, which interaction, when — so
  the profile can be reasoned about, audited, and (per Q8/Q11) exported or deleted.
- The trust/autonomy record is per-person-per-capability, not per-app.

`CONTINGENT` on one-community-vs-per-app (QUESTIONS Q1). One community is also
what makes lever 3 work on moderation, which is the second reason to decide it
before VesselHaven ships.

---

## Operations

Named separately because 01 identifies operations as the likely asymptote, and
because lever 3 lives here. Running N apps with a minimal core needs, shared from
the start: one observability and alerting surface; one incident process with
agents as first responders and humans on escalation; one support intake routed
per app but staffed once; one compliance and privacy posture — mandatory anyway
once the profile is shared, since data-subject requests become cross-app by
construction; one release pipeline and environment model.

**Anything here built per-app converts lever 3 from an advantage into a
liability.**

---

## Performance

Latency is what you pay for missing knowledge. The deeper the profile, the less
has to be inferred at runtime — which is why the Person layer is a performance
asset and not only a strategic one.

- Composition stays deterministic by default; the model-assisted path stays
  behind a flag.
- **That flag ships with an explicit latency budget**, not just a toggle.
- Pre-compose per (scenario, profile) offline, where nobody is waiting.

A 30-second think is fatal to this product. Treat latency as a product
requirement with a number attached.

---

## What not to build

- **A driver library.** Wrapped.
- **Frontier model capability.** Bought.
- **A general-purpose generative-UI framework for other people.** That's
  self-serve productisation — human-shaped work that fights the minimal core.
  *(Licensing to negotiated customers is different — see 01 §Line 2.)*
- **Anything justified by "models can't do this reliably yet"** — unless it's an
  acknowledged wedge, in which case time-box it and say so.
- **A second work-management system.**

---

## Sequencing

Ordered by what unblocks what.

1. **Community as an extractable shared service, before VesselHaven ships.**
   Identity-model decisions calcify, and it gates both the core multiplier and
   lever 3.
2. **Shared identity and profile schema**, in place before app #2 starts.
3. **Execution wrapper with two live providers**, replacing bespoke driver work.
   Two, not one — otherwise it's a dependency, not an abstraction, and it must
   retain outcome verification, a per-provider quality signal, and the
   interaction record.
4. **Attack lever 2 on the largest per-app line.** After app #2, the highest-value
   retrospective available is: *what did we build twice, and what stayed
   expensive that shouldn't have?*
5. **Shared operations plane**, before app #3 rather than after.
6. **Verification** — evaluate, then decide. Nothing above depends on it.

**Running alongside, on its own clock:** the counterfactual engine. It is not
sequenced with the list above because it doesn't block any of it — but its
external window is 12–18 months, which is shorter than every other deadline in
these documents.
