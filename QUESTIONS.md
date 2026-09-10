# Open questions register

Consolidates the `NEEDS YOU` markers scattered through [01-portfolio.md](01-portfolio.md),
ordered by how much the answer changes what gets built. Updated 2026-09-10.

Sequencing note: under the conclusion in §2f — *distribution in a vertical →
accumulated data → compounding advantage* — the stack is **downstream** of the
vertical choice. Writing `02-stack.md` before Q1–Q3 are settled risks specifying
architecture for a business that hasn't been chosen yet.

---

## Q0 — Do the apps share a user?

*(Replaces the earlier Q0, "is marine the domain you compound in" — that question
assumed VesselHaven was a strategic relationship. It isn't: it's one app of many,
pre-revenue, built to prove the factory.)*

The portfolio compounds only if the same person uses more than one app. If they
do, the preference layer spans the portfolio and app selection should be driven
by *same person, different need*. If they don't, the group holds N disjoint small
datasets and is a software shop with excellent tooling.

What would decide it: name the **cohort**, not the market. Who is the person the
next three apps all serve? If that person can't be named in a sentence, the
cross-app preference layer is a hope rather than a plan.

## Q0b — One community, or one per app?

Community is the cross-app surface (§2i), which makes this an identity-model
decision, not a feature decision — and identity models calcify. *One community,
many apps* is the only version in which the portfolio compounds; *many apps, each
with a community* is §2h's failure mode. Decide before VH's community ships.

## Q1 — What is the wedge, and what did app #1 actually cost?

Per §2g, a wedge is not a moat and is allowed to depreciate. Why does the first
cohort choose this over asking Gemini? Community is a strong candidate: it is the
one thing a generalist agent cannot supply, because it is other people.

**Now with a number attached.** VesselHaven is ~$250k to release. The decomposition
— how much non-recurring factory capability vs. how much VH-specific — is the
highest-value measurement available right now (§2j), and it is far cheaper at
launch than reconstructed later. State app #2's cost target *before* building it,
or the curve is a story rather than a test.

Per §2g, a wedge is not a moat and is allowed to depreciate. Why does the first
cohort choose a VesselHaven — or app #2 — over asking Gemini? "Fit" is a real
answer; it just has to be specific enough to name.

Corollary, cheap and time-sensitive: **capture VesselHaven's true build cost at
launch** — human hours, elapsed weeks, agent spend. It is the first point on the
cost curve the whole line-3 thesis depends on, and it is much harder to
reconstruct later.

## Q1b — What is the cost floor, and how much of VH's 11 months was waiting?

Two sub-questions from §2k, both cheap to answer and both decisive:

- **Name F**, the per-app floor, and what sits in it. If halving held forever the
  entire portfolio would cost ~$500k; it won't, and F alone determines how many
  apps the portfolio can hold. The likely floor is operational (support, GTM,
  compliance), not engineering.
- **Of 11 months, how much was waiting on a human decision?** If a large share,
  the time target is an org problem rather than an engineering one, and agents
  alone will not reach it.

## Q2 — Is the consumer aggregator accepted as a demo rather than the product?

§2d recommends repositioning from consumer aggregator to per-person interface
layer for verticals. That is a significant call and explicitly yours to make.
If accepted, continued investment in the cart spike needs a **stated purpose and
a stop condition** — it earns its place by generating preference signal and
proving the four-agent topology, not by extending merchant coverage.

## Q3 — What is the consulting cap?

Open since the three-line framing. Under §2f it is more dangerous than it looked:
consulting is a relationship business conducted in *other people's* domains, so
it generates no compounding data for you while consuming the attention that
would. State a ceiling — engagements per year, or % of founder time — or it wins
by default.

---

## Shapes the build

## Q4 — What is the smallest thing that produces real preference signal from real users?

The actual critical path. Not "build the preference store" — a preference layer
with no users is a schema. Which users, which interaction, generating which
durable signal, by when?

## Q5 — Which execution providers do you wrap, and are there two?

§2b's rule: an adapter with one implementation is a dependency. Concrete
sub-question with a concrete answer available — does wrapping a **signed** agent
provider inherit its access under the Cloudflare/Shopify identity gating
described in §2e? If yes, that is a strong practical argument for the wrapper
independent of the strategic one.

## Q6 — What is the legal posture on co-browse?

It is the one execution mode that degrades gracefully as identity gating
tightens, because it isn't agent traffic. It is also the one a merchant could
characterise as routing around its controls. The Ninth Circuit reversal helps;
the Amazon case is unresolved. This should be a deliberate posture with legal
input, not an emergent property of the architecture.

## Q7 — Monetisation, given no take rate

Subscription, B2B licensing to app owners, or per-vertical SaaS? §2c established
that leaving the transaction with the merchant forecloses transaction revenue.
The answer shapes both the entity structure and what "app #2" should even be.

---

## Structural — for 03-organization.md

## Q8 — Jurisdiction and existing entities

Open since the first draft. Jurisdiction-neutral so far; the intra-group IP
licensing mechanics are not, and transfer pricing will shape them.

## Q9 — Where does the prospective employee sit?

Platform, on the current reading — so consulting delivery doesn't consume founder
attention. Confirm or correct.

## Q10 — What are the agent roles, and what is the supervision ratio?

The substance of `03`. Three to four people directing agent-executed engineering
across three lines is the organisational bet; it needs to be stated concretely
enough to be wrong.

---

## Answered / retired

- **Sell the factory?** Reframed — consulting, not productisation (§2). Cap still open (Q3).
- **Is `market` a driver library?** No — a provider wrapper (§2b, decided 2026-09-10).
- **Sextant's role?** Line 2/3 asset; its AUX-adoption role evaporated with §2b.
- **Direct subsidiary or grandchild for VesselHaven?** Grandchild — keeps verticals individually disposable.
- **Six candidate niches** — all retired by research (§2c–§2f) as *moats*.
  Partially rehabilitated as *wedges* by §2g.
- **"Is marine the domain to compound in?"** — retired: VesselHaven is one app of
  many, not a relationship. Replaced by Q0 above.
