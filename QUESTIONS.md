# Open questions register

Consolidates the `NEEDS YOU` markers scattered through [01-portfolio.md](01-portfolio.md),
ordered by how much the answer changes what gets built. Updated 2026-09-10.

Sequencing note: under the conclusion in §2f — *distribution in a vertical →
accumulated data → compounding advantage* — the stack is **downstream** of the
vertical choice. Writing `02-stack.md` before Q1–Q3 are settled risks specifying
architecture for a business that hasn't been chosen yet.

---

## Q0 — The question everything now rests on

**Is marine the domain you compound in — or just the one you happen to have?**

The conclusion of the whole analysis is that the moat is a relationship in a
domain. VesselHaven is the only relationship in the portfolio. That makes this
question load-bearing in a way it wasn't three drafts ago, and it deserves a
real answer rather than a default.

What would decide it:

- **Decision cadence.** How often does a user in this domain make a
  high-consideration decision the platform could mediate — charter, berth,
  maintenance, provisioning, insurance, crew? A flywheel needs repetition. Four
  decisions a year takes a decade to compound; forty a year takes a season.
- **Fragmentation.** Are suppliers numerous, un-integrated and un-aggregated?
  That is what makes an aggregation layer valuable *and* what keeps Google out.
- **Value per decision.** High enough to justify a subscription in the absence of
  a take rate?
- **Relationship ownership.** See Q1.

If marine scores badly, the honest answer may be that the right vertical is one
you don't currently serve — which is a much bigger decision than a doc revision,
and better made deliberately than by inertia.

## Q1 — Who owns VesselHaven, and does it have paying users?

Open since the first draft, now critical rather than administrative. If
VesselHaven is a client's IP, **you do not own the relationship**, and the
strategy in §2d/§2f has no substrate. The entity shape also changes: it becomes
services revenue rather than a portfolio company.

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
- **Six candidate niches** — all retired by research (§2c–§2f). Superseded by
  "the moat is a relationship in a domain."
