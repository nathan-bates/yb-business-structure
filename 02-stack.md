# 02 — Technical stack

*Version 1 — 2026-09-10. Follows [01-portfolio.md](01-portfolio.md); read that
first. Sections marked `CONTINGENT` depend on open questions in
[QUESTIONS.md](QUESTIONS.md) and should not be built against until those resolve.*

---

## What the stack is for

Two jobs, and every decision below serves one of them:

1. **Make app N+1 cheap.** The portfolio strategy is unavailable if it doesn't.
2. **Accumulate the person.** Everything that compounds lives in one place and is
   joinable across every app.

A layer that serves neither is a candidate for wrapping, buying, or not existing.

---

## Layer map

| Layer | Component | Call | Why |
|---|---|---|---|
| **Person** | `library` — preference store, scope gate, profile | **BUILD — closed** | Appreciates with every model release; the only durable asset |
| **Person** | Community — identity, social graph, contributions | **BUILD — shared service** | The cross-app surface; where the person appears as a person |
| **Person** | Trust / earned-autonomy record | **BUILD — closed** | Per-user, per-capability, unreconstructible by a competitor |
| **Decision** | `arbiter` — what to ask, in what order, whether to ask | **BUILD — closed** | Elicitation patterns compound across *all* users; cold-start quality |
| **Form** | `aux` protocol, `kay`, design system, pattern library, vocabulary | **BUILD — expect to open** | Commoditising (A2UI, MCP Apps). Keep thin, keep the seam clean |
| **Execution** | `market` — C9 effect, C10 find-candidates | **WRAP** | Arms race; identity-gated; on the model curve |
| **Execution** | Co-browse — action in the person's own session | **BUILD — small** | The one execution mode that degrades gracefully under identity gating |
| **Factory** | Claude Code Cloud — agent runtime, creds, supervision | **BUILD — already load-bearing** | Directly determines the cost curve |
| **Factory** | Sextant | **EVALUATE, then decide** | Theoretical; 2–4 weeks; see 01 §Sextant |
| **Factory** | Archon / ProductLens | **PARK at good enough** | Intake and tracking for a four-person company |
| **App** | Per-app domain model, content, integrations | **PER-APP** | The irreducible part — and the floor |
| **Model** | Frontier LLMs | **BUY** | Never build; assume it improves under you |

**The rule this table encodes:** build what appreciates when the next model
ships, wrap what depreciates, and keep the seams between them clean enough that
a wrapped layer can be replaced without touching a built one.

---

## Shared vs per-app — this table *is* the cost curve

Everything in the left column is paid once. Everything in the right column is
paid every time. The floor F from 01 is the sum of the right column.

| Shared (paid once) | Per-app (paid every time) |
|---|---|
| Identity, auth, accounts | Domain model and business rules |
| Community service | Domain content and seed data |
| `library` profile + preference store | App-specific integrations not covered by the wrapper |
| `arbiter` elicitation patterns | Go-to-market, positioning, pricing |
| `kay` + design system + pattern library | Support and community moderation |
| Payments, subscriptions, billing | Compliance specific to the domain |
| Notifications, email, messaging | App-store presence where applicable |
| Observability, incident tooling | |
| Deployment pipeline, environments | |
| Agent build harness (the factory) | |

**Two implications worth acting on:**

1. **Anything discovered to be per-app that could be shared is a direct hit on
   the floor.** Reviewing this table after app #2 — what did we build twice? — is
   the highest-value retrospective available.
2. **The right column is mostly not engineering.** Content, GTM, support,
   compliance. This is the operational asymptote from 01, visible in the
   architecture: you cannot engineer your way past it, and it grows linearly with
   app count. Which makes the shared **operations plane** (below) as important as
   the shared code.

---

## The three seams that must hold

The architecture's value is in its boundaries. If these blur, the layers stop
being separable and the wrap/build distinction collapses.

| Seam | Between | Must stay true |
|---|---|---|
| **C5 / C6** | Arbiter ↔ Kay | Arbiter sends interaction shape + content; Kay decides form and only form. Content never originates in Kay |
| **C7** | Kay ↔ User Agent | Kay reads *presentation-class* preferences only. Domain facts stay in the restricted section. **This is a commercial boundary as much as an architectural one** — it's where the open layer meets the closed one |
| **C9 / C10** | Arbiter ↔ Service Agent | Effect and find-candidates are provider-agnostic. If a provider's shape leaks through, the wrapper has become a dependency |

---

## The execution wrapper — specification

`market` is an adapter, not a driver library. To be a strategy rather than a
dependency it must have:

- **Two live provider implementations from the start.** One is not an
  abstraction. This is also the only real hedge against platform risk.
- **Outcome verification.** Did the effect actually happen? A pass-through that
  can't confirm its own results owns the blame and none of the control.
- **Per-provider quality and cost signal**, so routing improves and substitution
  is evidence-driven rather than a rewrite.
- **The interaction record retained locally.** Providers see the action; they
  never see the approve/edit/feedback loop. That's the flywheel's input.

`OPEN` — which two providers, and does wrapping a **signed** agent provider
inherit its access under the Cloudflare/Shopify identity gating? If yes, that's a
strong practical argument for the wrapper independent of the strategic one.

**Co-browse sits beside the wrapper, not inside it.** It's the fallback for
merchants no provider reaches, it's what the cart spike already demonstrates, and
its properties are different enough (person present, own session, own account)
that collapsing it into the provider interface would lose what makes it valuable.

---

## Data architecture

The single most consequential technical decision in the portfolio:

> **One identity, one profile, one community — joinable across every app.**

If a person is a different record in each app, the preference layer never
compounds and the portfolio is a software shop. This must be true at the schema
level from app #2 onward, and retrofitting it is expensive.

Concretely:

- One identity provider across all apps; app-level accounts are projections of it.
- `library` holds the profile; apps read it through the scope gate, never
  duplicate it.
- Preference entries carry provenance (which app, which interaction, when) so the
  profile can be reasoned about and audited.
- The trust/autonomy record is per-person-per-capability, not per-app.

`CONTINGENT` on the one-community-vs-per-app decision. *One community, many apps*
is the only version where this architecture pays off.

---

## Performance

`aux` ADR 0026 already has Kay **selecting** rather than generating — a pure
exhaustive switch resolving the same shape to the same pattern, with
model-assisted composition behind a flag.

**Latency is what you pay for missing knowledge.** The more the system knows about
the person and the scenario, the less it infers at runtime. So:

- Composition stays deterministic by default.
- The model-assisted flag carries an **explicit latency budget**, not just a
  feature toggle. Ship the budget with the flag.
- Cache and pre-compose per (scenario, preference-profile) offline, where the
  flywheel's signal is available and nobody is waiting.

A 30-second think is fatal to this product. Treat latency as a product
requirement with a number attached, not a non-functional aspiration.

---

## Open vs closed

| Open | Closed |
|---|---|
| `aux` protocol and types | `library` — profile, preference model, accumulation loop |
| Reference renderer, design system, pattern library | `arbiter` elicitation patterns |
| Adapter interfaces (C9/C10 shapes) | Trust/autonomy record |
| | Community data |

Rationale: standards are worth little unadopted, and adoption usually requires
giving them away — the `aux` README already anticipates packages lifting out to
OSS. So the question isn't whether AUX gets adopted, it's **what remains
proprietary when it is.** The answer is everything on the right, and the C7 seam
is the line between them.

---

## The operations plane

Named separately because 01 identifies operations as the likely asymptote, and
because it's the part usually left until it hurts.

Running N apps with a minimal core needs, shared from the start:

- One observability and alerting surface across all apps, not one per app.
- One incident process, with agents as first responders and humans on escalation.
- One support intake, routed per app but staffed once.
- One compliance and privacy posture — especially given the profile is shared,
  which makes data-subject requests a cross-app operation by construction.
- One release pipeline and environment model.

**Anything here built per-app raises F directly**, and F decides how many apps
the portfolio can hold.

---

## What not to build

Explicitly, to save the argument later:

- **A driver library.** Wrapped.
- **Frontier model capability.** Bought; assume it improves under you.
- **A general-purpose generative-UI framework for other people.** That's
  productisation — human-shaped work that fights the minimal core.
- **Anything justified by "models can't do this reliably yet."** That's a
  depreciating asset; if it's needed as a *wedge*, time-box it and say so.
- **A second work-management system.** Archon/ProductLens is parked at good enough.

---

## Sequencing

Ordered by what unblocks what, not by what's interesting.

1. **Community as an extractable service, before VH ships.** It's an identity-model
   decision and those calcify. Highest-urgency item in this document.
2. **Shared identity + profile schema**, in place before app #2 starts.
3. **`market` wrapper with two providers**, replacing any bespoke driver work.
4. **Shared operations plane**, before app #3 rather than after.
5. **`arbiter` elicitation patterns**, as the second app supplies cross-user signal.
6. **Sextant** — evaluate, then decide. Nothing here depends on it.
