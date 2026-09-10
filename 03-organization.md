# 03 — Organization

*Version 2 — 2026-09-10. v1 was an engineering org: it treated agents as
developers and never asked what it takes to actually run a line of business.
This version starts from the portfolio — what does Ecosystem, Core Tech and Apps
each require to operate, and what happens to the org as each one grows.*

---

## The question this answers

Not "who reports to whom" — with three people that's noise. The real questions:

1. **What functions must exist** to run each line, engineering being a minority
   of them?
2. **What drives the load** on each function as the business grows?
3. **Which of those functions absorb growth without absorbing people** — and
   which don't, because those are the ones that decide whether the minimal core
   survives contact with success?

---

## The human core

| | Role | Owns |
|---|---|---|
| **Nathan** | CTO *(aspirational)* | Architecture, the factory, technical direction across all three lines |
| **Agi** | COO | Operations, delivery, commercial execution, the scaling machinery |
| **Chris** | CPO *(aspirational)* | Product: cohort definition, app selection, the decision/elicitation layer, community |

**"Aspirational" is the most important word in that table.** Two of three roles
are titles the group intends rather than describes — which means Nathan and Chris
are currently absorbing work that belongs to functions which don't exist yet. The
org design's job is to name that work and route it somewhere else, in order.

`PROPOSED` — **what has to be true for each role to become actual:**

| Role | Blocked by | Cleared when |
|---|---|---|
| CTO | Doing delivery management and vendor supervision | Fixed-price contracting removes hour-by-hour oversight (01 §How engineering gets bought) |
| CPO | No cohort defined, so product decisions are per-app rather than portfolio-level | QUESTIONS Q3 answered — product becomes a portfolio question |
| COO | Operations plane doesn't exist yet, so ops is per-app improvisation | 02 §Operations built shared, before app #3 |

**Line ownership** — one accountable owner per line, because with three people a
matrix means nothing is owned:

| Line | Accountable | Contributing |
|---|---|---|
| **Ecosystem** | Nathan — architecture-shaped bet; the option-holder should hold it | Chris on product shape |
| **Core Tech** | **Split, deliberately** — Nathan on technical execution, Agi on the commercial motion | The ex-consultant bench, including the investor |
| **Apps** | Chris — app selection is the cohort question in operational form | Agi on delivery and ops, Nathan on factory |

See §Reporting structure for the functional lines beneath this, and for the one
conflict in them.

`OPEN` — react to this. The reasoning is that each line's *binding constraint*
should match its owner's remit: Ecosystem is blocked on architecture decisions,
Core Tech on commercial motion, Apps on product/cohort judgement.

---

## Reporting structure

`PROPOSED (Nathan, 2026-09-10)` — with a stated goal of **clean separation, so the
founding team is not bogged down in consulting minutiae:**

| Function | Rolls up to |
|---|---|
| Consulting — technical portions | **Nathan** |
| Sales | **Agi** |
| New products | **Chris** |

### The engineering layer beneath the CTO

`PROPOSED` — not a founder taking escalations, but **peer engineering directors,
each owning a domain**, with the CTO owning what crosses between them:

| Director domain | Focus |
|---|---|
| **Platform** *(Peer 1)* | Building the tech — the counterfactual engine, the shared platform, the factory |
| **Applied / integration** | How the tech gets applied and integrated on the consulting side |
| **Product engineering** *(Peer 2)* | How the tech supports the ecosystem and the apps |

Escalations stop at a director; the CTO holds the **envelope** — what may be
promised, what architecture is permitted, what must be reused. Envelope decisions
are low-volume and batchable; delivery escalations are neither, and keeping the two
on different people is the point.

**The axis here is build-versus-apply, not business line** — one builder, two
application surfaces. That is a better fit for this company than a line-aligned
split, and it is worth being explicit about why: the shared column of 02's cost
model (person layer, factory, ops plane, shared schema) has an owner **by
construction** rather than by exception. A line-aligned engineering org would have
left it orphaned, and the shared column is the entire cost thesis.

It also mirrors the stack's own primary seam: shared versus per-app becomes
builder versus appliers.

**It keeps the minimal-core thesis intact**, and this should hold as a design
constraint rather than by luck: a director here is *one human supervising an agent
team*, not one human supervising a department. See §Agent roles for what sits
beneath each domain.

#### The risk this axis creates instead

Not orphaned shared assets — a different pair:

- **The builder builds what nobody applies.** Shared platform work drifts from
  what the two application surfaces actually need. The remedy is that the
  appliers set requirements on the builder, and the envelope arbitrates when they
  conflict; that only works if it's stated, because the builder's domain is the
  one with no external customer telling it when it's wrong.
- **The two appliers fork.** Consulting-side and product-side integration solve
  the same problem twice in incompatible ways — which quietly moves work from the
  shared column into the per-app column, reversing lever 1. Watch for the same
  capability appearing in both.

#### Timing: bets first, then assess

`DECIDED (Nathan, 2026-09-10)` — no role is filled today. **Make the bets, let
them run 1–3 months, then assess where a director level is most needed.** Which
domain comes first depends on where the bets landed and how their timelines
behaved — not on a plan made in advance.

That's consistent with §When a human gets added, and it's the right posture. One
thing it needs to work:

> **Instrument the decision now, because it can't be reconstructed later.**

In three months the question will be "where is the director needed?", and the
honest answer will come from evidence nobody is currently collecting. Three cheap
measurements, starting immediately:

| Signal | Why it decides |
|---|---|
| **Founder hours by domain** | Where attention is actually going, as opposed to where it was allocated |
| **Escalation count by domain** | Which surface generates interrupts — the direct argument for a director there |
| **Which clock is slipping** | Replay's deadline and the ecosystem's accumulation are the two things a director exists to protect |

Same argument as VesselHaven's cost decomposition: trivial to capture as it
happens, guesswork afterwards.

**And the assessment point has a natural date.** VesselHaven ships, the replay
sprint produces its first market feedback, and the community/identity decision
lands — all inside the same 1–3 months. That's one review gate with several
signals arriving together, not three separate check-ins. Worth treating it as a
single scheduled assessment.

**A director need not be a hire.** It could be a promoted contractor, a fractional
person, or a domain run by agents with a founder as nominal lead until the load
justifies otherwise. The role emerging and the role being filled are separate
decisions, and only the first is being made here.

### Scope still does more work than structure

Independent of the reporting layer:

> **Never sell bespoke work. Sell implementation of licensed product.**

Bespoke engagements generate *novel* technical problems, which escalate as far as
whoever knows the architecture — past the director, in the hard cases. Productised
implementation generates *known* problems, which stay with the director, are
documented, and route to the product backlog where they are actually useful. The
director layer bounds escalation; the scope rule reduces how much there is to
bound. Both, not either.

The entity structure reinforces it: services sit in Platform (01 §Entity shape)
with their own P&L and staff, so separation is legal and operational rather than
purely a matter of discipline.

### Sales needs a boundary, or the easy sale wins

"Sales rolls up to Agi" leaves open *sales of what*. If one function sells both
consulting and licences, the faster-closing product wins — which is precisely the
drift this group is already wary of, and the wariness is well-founded.

`PROPOSED` — a structural mechanism rather than a willpower one:

> **Consulting is sold only attached to a licence, and capped as a percentage of
> licence revenue.**

This is self-enforcing through the sales motion: every engagement must *grow*
licence revenue rather than substitute for it, and the cap moves only when
licensing moves. Far more robust than a cap on founder time, which requires
somebody to say no in the moment.

### The gap this structure leaves

**Operations.** Agi holds COO and sales. Sales is a growth function with revenue
attached; operations is a scaling function with none. Under the portfolio thesis,
**operations is the asymptote** — the shared ops plane is what makes the app tail
affordable at all (02 §Operations, lever 3). Inside one person, sales and ops
compete, and ops loses, because revenue-bearing work always does win that fight.

That is not an argument against the assignment; it is an argument for naming the
ops plane as a deliverable with a date, owned by Agi but built by agents, rather
than as something that happens when there is time. There will not be time.

### What each founder is then actually protecting

| | Owns | Must be protected from |
|---|---|---|
| **Nathan** | Technical envelope, replay execution, ecosystem architecture | Delivery escalation — the single highest-risk leak in the structure |
| **Agi** | Sales, commercial terms, operations | Letting the easier sale set the mix; letting ops slip behind revenue |
| **Chris** | New products — app tail and ecosystem product | Being pulled into delivery; it is the line with no customer demanding attention, so it loses contention silently |

---

## What it takes to run each line

Engineering is a minority of every column below. That is the point.

### Ecosystem — low volume, decision-dense

| Function | Nature |
|---|---|
| Architecture and product-shape decisions | Irreducibly founder work; cannot be delegated |
| Competitive and technical research | Continuous, not episodic — the six retired niches in 01 all died to research |
| Decision record — ADRs, contradictions, stale decisions | Mechanical; agent-suitable |
| Standards stewardship, if AUX opens | Issue triage, RFC summaries, compatibility review |
| Preference-data governance | Privacy, consent, portability — becomes mandatory the moment the profile is shared |
| Partnerships | Execution providers, design partners — relationship work |
| Legal posture | IP, ToS, the co-browse question (QUESTIONS Q8) |

**Load scales with:** decisions and surface area, **not** users. This line does
not grow headcount as it succeeds — it grows *commitments*, which is a different
and slower burden.

### Core Tech — scales with licensees, not with hours

The licensing model (01 §Line 2) only pays off if the functions below are built
to absorb licensees without absorbing people.

| Function | Nature |
|---|---|
| Packaging and release engineering | Turning internal tools into installable products — the gate on licensing at all |
| Documentation | The single largest recurring licensing cost, and highly agent-suitable |
| Tiered licensee support | Tier 1 agent-answerable from docs and code; tier 2 human |
| Versioning, compatibility, deprecation policy | Constrains the architecture — see commitment cap |
| Pipeline and qualification | Prospect research, briefs — agent-suitable |
| Negotiation and closing | **Least agent-delegable function in the business** |
| Contract administration | Licences, renewals, redlines against a playbook |
| Billing, invoicing, collections | Mechanical |
| Security review responses | Underrated: enterprise licensees send questionnaires, and they are relentless. Maintain an evidence base once, answer from it forever |
| Roadmap commitment management | What you promised whom, and what that forecloses |

**Load scales with:** number and *enterprise-ness* of licensees. A single large
licensee can generate more support, security-review and compliance load than ten
small ones — worth knowing before choosing the first.

### Apps — scales with apps × users

| Function | Nature |
|---|---|
| App selection and product definition | Founder work — it's the cohort question applied |
| Domain expertise acquisition | Per app; partly buyable, partly agent-researchable |
| Build and review | Agent-executed with human review (path B, still unproven) |
| Verification and QA | Agent-suitable; see the Sextant evaluation |
| Content and seed data | Per-app, agent-generable — a lever-2 target |
| Go-to-market: positioning, pricing, launch | Per app, but shared if the apps share a cohort |
| Acquisition and growth analytics | Agent-suitable reporting; human judgement on response |
| Support | Shared pool across apps — lever 3 |
| Community moderation | Shared under one community — the strongest lever-3 line |
| Trust and safety | Policy set by humans, enforcement largely agent |
| Payments, refunds, chargebacks | Mechanical, rising with users |
| Incident response | Agents first, humans on escalation |
| App-store and platform compliance | Per surface, not per app |

**Load scales with:** apps × users per app. This is the line that can break the
minimal core, because its load is a product of two growing numbers.

### Group — cross-cutting

Finance (bookkeeping, contractor payments, cash, investor reporting, tax across
entities), legal and entity administration (intra-group IP licensing, transfer
pricing, DPAs), vendor management (SOWs, milestone verification — materially
easier under fixed-price), security and data protection, investor relations,
and the decision record itself.

**Load scales with:** entities, contracts, and jurisdictions — step functions,
not curves. Each new entity or country is a discrete jump.

---

## Agent roles across the business

v1's agent roster was developers with different hats. The functions above suggest
a much wider set. Each has a **default action it may take without asking** —
that's what makes it an org role rather than a tool — and a condition-based
escalation.

| Domain | Agent | Standing authority | Escalates when |
|---|---|---|---|
| **Ecosystem** | Research | Publish sourced findings into the docs | A finding contradicts a recorded decision |
| | Decision-record | Maintain ADRs; flag contradictions and stale decisions | Two live decisions conflict |
| | Standards triage | Label, summarise and route external issues/PRs | A change would break a licensee commitment |
| **Core Tech** | Documentation | Keep docs in sync with every release | Behaviour changed without a decision record |
| | Licensee support (tier 1) | Answer from docs, code and prior tickets | Anything implying a commitment, refund, or roadmap promise |
| | Release/compatibility | Changelogs, deprecation notices, migration guides | A breaking change lacks a migration path |
| | Security-questionnaire | Answer from the maintained evidence base | A question the evidence base can't support |
| | Pipeline research | Prospect briefs and qualification | — |
| | Contract first-pass | Redline against the playbook | Any deviation from the playbook |
| **Apps** | Build | Implement to spec; open PRs | Spec ambiguity; cross-seam change |
| | Review | Adversarial review; block merge | Unresolved after two rounds |
| | Content/seed | Generate and stage domain content | Factual claims needing domain sign-off |
| | Support (tier 1) | Answer across all apps from known material | Commitments, refunds, distress |
| | Community moderation | Enforce published policy; remove, warn, escalate | Novel case; policy gap; appeal |
| | Trust and safety | Act on defined categories | Anything not in the policy |
| | Growth analytics | Produce cohort, funnel and experiment readouts | A metric moves past a defined threshold |
| | Ops/incident | Triage, diagnose, mitigate, roll back | Customer-visible impact or data risk |
| **Group** | Finance ops | Categorise, invoice, chase, report cash | Anything unbudgeted or contested |
| | Compliance | Handle data-subject requests; collect audit evidence | A request that can't be satisfied as-is |
| | Vendor management | Track SOWs; verify milestones against fixed-price deliverables | Milestone disputed or missed |
| | Investor reporting | Assemble the metrics pack | Narrative judgement |

**The two rules that make this an organization rather than a tool shelf:**

1. **Standing authority is the whole point.** An agent that must ask before acting
   becomes an item in a founder's queue — which is the constraint being optimised
   away. Every role above has a named default action.
2. **Escalation is by condition, not by discomfort.** The right-hand column is a
   contract. Escalation outside it means the boundary is wrong and should be
   changed deliberately, not eroded case by case.

---

## How the org scales

The central question. For each growth driver: what load rises, and does it
consume people?

| Growth driver | Load rises in | Absorbs how | Human cost |
|---|---|---|---|
| **More apps** | Build, QA, content, GTM, support, moderation, compliance, incident | Levers 1–3 (02): shared services, agent execution, one ops pool | **Sub-linear** if the ops plane exists; linear if it doesn't |
| **More users per app** | Support, moderation, trust & safety, infra spend | Strongest agent leverage in the business; genuine economies of scale | **Sub-linear** |
| **More licensees** | Docs, support, security reviews, contracts, billing, commitments | Agent-first, with humans on negotiation and escalation | **Sub-linear, except negotiation** |
| **More consulting deals** | Delivery, account management, embedded staff | Nothing absorbs it | **Linear — this is why it needs a cap** |
| **Bigger ecosystem surface** | Standards stewardship, compatibility, partnerships | Agent triage; founder decisions | **Step-wise** |
| **More entities/jurisdictions** | Finance, legal, tax, compliance | Advisors, not employees | **Step-wise** |

**Read the right-hand column, because it is the whole org strategy.** Every line
is sub-linear or step-wise except one. Consulting is the only growth vector that
converts revenue into headcount at a fixed ratio — which is why 01 pushes line 2
toward licensing, and why the cap exists. **The org scales precisely to the
degree the business avoids that row.**

**The second-order risk:** most of the sub-linear rows are only sub-linear
*because* something shared exists — the ops plane, the support pool, one
community, one profile. Those are all things that must be built *before* the
growth arrives. Built after, they're rewrites under load, and the row silently
becomes linear.

---

## When a human gets added

No headcount plan. Trigger conditions instead, so a hire is a response to
evidence rather than a feeling of being busy.

**Add a human when all four hold:**

1. The function is load-bearing — something breaks without it.
2. It is **not agent-delegable**, and that's been tested rather than assumed.
3. The load is recurring, not a spike.
4. It is otherwise consuming a founder's protected attention.

**By that test, the functions most likely to demand a human first** — stated as
functions, not as roles to fill:

- **Commercial negotiation and closing.** The least agent-delegable function in
  the business, and the gate on line 2 scaling at all.
- **Operations ownership**, once apps × users makes incident and support load
  continuous rather than occasional.
- **Domain product judgement**, if the apps *don't* share a cohort — because then
  each app needs its own product thinking, which is precisely the failure mode
  01 warns about. **A hire triggered by this reason is a signal the portfolio
  thesis is failing, not a sign of growth.**

---

## Sourcing: 1P vs consultants, onshore vs offshore

Two axes usually conflated — **employment relationship** (first-party employee vs
contractor vs agency) and **cost geography** (onshore vs offshore at ~3–4×) — plus
a third this document has already established: **agent-executed vs
human-executed.** The third one changes the answer to the first two.

`ASSUMPTION` — illustrative rates, adjust to your actuals: onshore senior
~$150–200k fully loaded (~$100–150/hr contract); offshore ~$40–60k (~$25–40/hr).
VesselHaven's ~$250k over 11 months is consistent with roughly 3–4 offshore FTE;
the same work onshore would have been $750k–$1m.

### The observation that decides most of it

**Offshore is an arbitrage on hours. The company's entire strategy is to reduce
hours.** Optimising the unit price of the thing you are trying to eliminate is
the sourcing version of betting against the model curve.

Run the numbers at your own 3–4× assumption:

| | Cost/yr | Bends the cost curve? |
|---|---|---|
| 4 offshore engineers, hourly | ~$200k | **No** — and under T&M they're structurally opposed to it (01) |
| 1 onshore senior + agent spend | ~$180k + ~$30k | **Yes** — the senior's job is to drive the automation |

Roughly the same money. So the real question is: **does one senior driving agents
outproduce three-to-four juniors?**

That is not a staffing question — **it is the portfolio thesis restated.** If the
factory works, senior-and-few wins. If it doesn't, cheap-and-many wins, but then
there is no cost curve, no portfolio, and the strategy in 01 fails anyway. The
sourcing decision and the company thesis stand or fall together, which means they
should not be decided by different logic.

### When you pay for output, geography stops mattering

The onshore/offshore question only has force **in an hourly world.** Under
fixed-price, you buy a deliverable and the vendor's cost structure is their
problem. That's a further argument for the contracting shift in 01 — it makes the
3–4× question moot for everything bought as output.

`ASSUMPTION` — a fixed-price offshore bid is *not* ¼ of onshore, more like ½,
because the vendor prices in the risk they're now absorbing. That premium is the
cost of transferring delivery risk, and it is usually worth paying.

### The rule: staff what compounds, buy what doesn't

This falls straight out of 02's shared-vs-per-app table:

> **First-party for the shared column. Bought as deliverables for the per-app
> column.**

Because shared work *accumulates* — the factory, the platform, the ops plane, the
preference layer — and in a company whose thesis is that app N makes app N+1
cheaper, **letting that learning walk out at the end of every engagement is
directly value-destroying.** Per-app work doesn't accumulate, so rent it.

| Work | Source | Why |
|---|---|---|
| Architecture, product, negotiation, commitments | **1P, senior** | Judgement-dense, low-volume, irreducible. The founders today |
| Factory, platform, ops plane, preference layer | **1P** | Compounds; the learning must stay |
| App delivery, end to end | **Fixed-price vendor** — geography irrelevant | Bought as output; risk transfers |
| High-volume, low-judgement: content ops, QA sweeps, moderation triage, tier-1 support | **Agents first** | Offshore only for what agents can't do *yet* — and time-box it against the model curve |
| Specialist bursts: security audit, legal, compliance, design | **Onshore consultants, project-priced** | You're buying a credential and a signature, not hours |
| Domain expertise for a new vertical | **Fractional/advisor, in-market** | Cheapest high-leverage input available |

### The tension your two targets create

Offshore taxes the metric 03 identifies as the binding constraint. **Decision
latency drives elapsed time**, and a team with two hours of overlap adds latency
to every decision in the chain.

- The **cost** target (≤$125k for app #2) favours offshore.
- The **time** target (≤5.5 months) favours overlapping hours and fewer handoffs.

At 3–4× cheaper but plausibly 30–50% slower in elapsed time, **the two targets
pull in opposite sourcing directions**, and the time target may be unreachable
offshore regardless of tooling. `OPEN` — which are you optimising? If both, the
honest middle is nearshore with real overlap, or fixed-price delivery where
elapsed time is contractually the vendor's problem rather than yours.

### Recommended combination

`PROPOSED`:

1. **1P, onshore, very few, senior.** Founders now; the first hire when the
   trigger conditions below fire. They own everything that compounds and they
   supervise agents.
2. **Agents as the default executor** for volume work across every function, not
   just engineering.
3. **Fixed-price vendors for per-app delivery**, chosen on price and track record
   rather than geography.
4. **Onshore specialists, project-priced**, for credentialed bursts.
5. **Offshore hourly: minimise, and treat any of it as a time-boxed position**
   against the model curve — it is the category most likely to be obsoleted by
   the next capability release, and the one whose incentives fight the factory.

**The trap to avoid:** offshore hourly is the cheapest-looking option on a
spreadsheet and the only one that structurally resists the thing the company is
trying to build. It optimises the line item and defeats the strategy.

---

## Decision rights and latency

Attention binds twice — as allocation, and as latency. On an 11-month build, much
of the critical path is waiting for a human to decide, so agents can double
throughput and barely move elapsed time.

- **One accountable owner per line decides within it.** Cross-line decisions —
  capital allocation, commercial model, entity changes — are founder-level. The
  investor is consulted on capital and structure, informed otherwise.
- **Pre-committed defaults** for recurring decision classes; agents proceed unless
  the case is genuinely novel.
- **Decision SLAs.** A request unanswered in N working days executes the recorded
  default. Uncomfortable, and the mechanism that actually works — without it the
  queue is unbounded.
- **Batched review windows** rather than continuous interruption; it protects the
  Ecosystem attention block, which is the only one with no external party
  demanding it.
- **Fewer decision points by design.** A spec naming one approach beats one
  offering three.

`OPEN` — of VesselHaven's 11 months, what fraction was waiting versus working?
It decides whether the app #2 time target is an engineering problem or this one.

---

## Attention allocation

- **Apps and Core Tech are agent-delivered by default.** A founder hour spent
  there is a bug to be automated.
- **Ecosystem gets a protected block** — the only line with no customer demanding
  attention, and therefore the only one that loses by default.
- **Two caps, for two different models.** Embedded consulting: a time cap.
  Licensing: a **commitment** cap, because what you promise licensees constrains
  what the architecture can still change.

| Shift trigger | Response |
|---|---|
| App #2 exceeds ~$150k or ~7 months | Stop; revisit the portfolio strategy before app #3 |
| A consulting engagement breaches the cap | Decline, subcontract, or convert to a licence |
| A licence commitment would freeze a moving interface | Refuse it, or license a stabilised subset |
| Two apps ship with no shared users | Revisit the cross-app thesis |
| A function escalates outside its stated conditions repeatedly | The boundary is wrong — redraw it deliberately |

---

## Measurement

| Metric | Answers | Cadence |
|---|---|---|
| Humans per revenue line | Is the structure working at all | Quarterly |
| Cost and elapsed time per app | The portfolio thesis | Per app |
| Agent-originated share of merged work | Which path bent the curve, and whether it generalises | Monthly |
| Support and moderation cost per user, per app | Whether lever 3 is real | Monthly |
| Cross-app users | The core multiplier | Monthly, from app #2 |
| Median age of open decision requests | The latency floor | Weekly |
| Licensees per commercial FTE-equivalent | Whether licensing scales as claimed | Quarterly |

---

## Failure modes

1. **Consulting grows because it closes faster than licensing does.** The only
   linear row in the scaling table wins by default, and it arrives disguised as
   good news. **This is now a founding-team risk, not only a market one:** with
   three ex-consultants on the commercial side, consulting deals will feel easy
   and licensing will feel slow — an instinct earned in a business model whose
   economics were the opposite of this one's.
2. **Shared machinery built after the growth instead of before.** The sub-linear
   rows quietly become linear, and the fix is a rewrite under load.
3. **Agents without standing authority.** Every agent action becomes a founder
   decision and the org gets *slower* with more agents.
4. **Aspirational roles stay aspirational.** Nathan and Chris keep absorbing
   functions that were never assigned anywhere, and the Ecosystem block — the
   only unprotected line — is what gets spent.
5. **A line without an accountable owner.** It will be Ecosystem, because it's the
   one with no customers asking.
6. **Commercial model unchanged**, so the cost curve never bends and Apps grows
   linearly in people regardless of how good the factory is.
7. **Bespoke consulting sold at all.** Novel technical problems escalate past a
   director to whoever holds the architecture — which is the person carrying both
   clocked bets. The director layer bounds escalation; the scope rule reduces it.
8. **The two application surfaces fork**, solving the same integration problem
   twice — which moves work from the shared column back into the per-app column
   and reverses lever 1.
9. **The platform domain drifts from what the appliers need**, because it is the
   only domain with no external customer to tell it when it is wrong.
10. **The ops plane never gets built**, because it sits with the founder who also
    carries revenue, and revenue always wins the week.
11. **The three-month assessment runs on memory**, because nothing was
    instrumented — and the loudest domain wins rather than the most constrained one.
