# 02 — Technical stack

*Version 2 — 2026-09-10. Rewritten: v1 was a component catalogue that
over-indexed on individual pieces. This version is about the system — how the
parts multiply each other, and what that implies for cost. Follows
[01-portfolio.md](01-portfolio.md).*

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
| **Factory** | How apps get built and maintained — build agents, pipelines, verification, work tracking | Meta: serves the group, not the end user |

Two of these are unlike the others. **Person** is the only layer whose value
increases with time and with app count. **Factory** is the only layer with no
end-user surface at all — it exists solely to change the cost of everything else.

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

**Lever 2 is the one v1 ignored and it is probably the largest.** Domain
modelling, app-specific content, and per-app integration work all stay in the
right-hand column forever — but how expensive they are is a function of the
tooling, and that's exactly what the factory is for. "It stays per-app" and "it
stays expensive" are different claims.

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

Consequences of the above, stated compactly. The reasoning is the model-curve
test in 01: build what appreciates when the next model ships, wrap what
depreciates.

| Layer | Call | Because |
|---|---|---|
| Model | **Buy** | Never build; assume it improves under you |
| Person | **Build, closed** | The only layer that appreciates with time and app count |
| Decision | **Build, closed** | Elicitation patterns compound across all users; they set cold-start quality |
| Form | **Build, expect to open** | Commoditising — A2UI, MCP Apps. Keep it thin and the seam clean |
| Execution | **Wrap** — with a small built exception for action inside the person's own session | Arms race, identity-gated, on the model curve |
| App | **Per-app, but attack its unit cost** | Lever 2 is the whole game here |
| Factory | **Build** | It is the only thing that moves lever 2 |

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
