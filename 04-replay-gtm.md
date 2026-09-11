# 04 — Replay: go to market

*Version 1 — 2026-09-10. Commercial plan for the counterfactual engine described in
[02-stack.md](02-stack.md) §The counterfactual engine. The technical build and
packaging path are considered known; this document is only about finding and
convincing buyers.*

---

## What is being sold

Not "session replay" and not an eval platform. **The measurement layer beneath
model routing**: what *would* have happened at turn K under a different model,
prompt or tool, on the customer's own workload.

Three sellable shapes, in the order a customer meets them:

| Shape | The sentence | Recurring? |
|---|---|---|
| **Audit** | "800 turns went to the frontier tier last month; 730 were indistinguishable a tier down — here is the overspend" | One-off, then annual |
| **Calibration** | "Here is a routing policy fitted to *your* history, not to generic benchmarks" | Yes |
| **Drift safety** | "Cheaper at turn 12 costs you at turn 40 — here is where the saving is real and where it defers" | Yes |

The wedge that makes all three possible is **turn-K intervention**: full-session
replay costs `T × arms × N`, turn-K costs `1 × arms × N`. At 50–200 turns per
session that is one to two orders of magnitude, which is what makes N-per-cell
sampling — and therefore a defensible answer — affordable at all.

---

## Why this is an easier sale than most

1. **The buyer population is small and identifiable.** No category creation
   required; these teams already know they have this problem.
2. **The trigger is someone else's calendar.** Vendors retire models; enterprises
   migrate on the vendor's schedule. GitHub Copilot's September 2026 retirements
   already have third-party migration and regression-test guides written about
   them. Demand is not discretionary.
3. **Value is denominated in a bill they already receive.** No ROI modelling
   argument to win.
4. **Best-practice guidance already asks for the product.** Current advice for
   these migrations is *"supported model plus evidence from our own workload"* —
   a one-line description of the output, and of what nothing on the market
   provides for stateful agents.
5. **The commercial bench exists in-house.** Enterprise selling is the least
   agent-delegable function in the group (03) and is held at founder level rather
   than needing a hire.

---

## Market opportunity

`ESTIMATE` — built bottom-up from spend under management, cross-checked against the
adjacent category and against incumbent revenue. Every figure is marked; the
sensitivity analysis at the end matters more than the point estimates.

### The denominator

| Input | Figure | Source / basis |
|---|---|---|
| Enterprise frontier LLM **API** spend, 2026 | **>$35B** (~$15B in H1) | Reported; the most load-bearing number here |
| Coding as a use case | Highest penetration — ~70% of large-enterprise engineering orgs | Reported |
| Coding share of API **spend** | `ASSUMPTION` **~35%** | Coding agents are token-heavy — long contexts, many turns, thinking-heavy output — so spend share should exceed usage share |
| **Coding-agent API spend, 2026** | **~$12B** | 35B × 35% |

**Note the denominator is API spend, so per-seat tools are already excluded** —
which is correct, because a buyer paying per seat has no model-choice decision to
measure. That exclusion is load-bearing and easy to get wrong.

### From TAM to SAM

The buyer must pay per token **and** control model choice. Splitting the ~$12B:

| Holder of the spend | Share `ASSUMPTION` | Addressable? |
|---|---|---|
| Agent-product vendors' own inference (Cursor, Copilot, Devin, Factory…) | ~40% — $4.8B | **Poorly** — high in-house-build risk; these are design partners, not customers |
| Enterprises on BYO-key harnesses (Claude Code on API, OpenCode, custom) | ~40% — $4.8B | **Yes** — the best buyer |
| Consultancies and SIs | ~20% — $2.4B | **Yes** — best reachability for this group specifically |

**SAM ≈ $7.2B of spend under potential management** in 2026.

### Capture rate

The closest priced analogue is FinOps: cloud-cost platforms such as Cloudability
price at roughly **2–3% of cloud spend under management.** Two adjustments, pulling
opposite ways:

- **Downward** — FinOps manages an entire bill continuously, including allocation
  and commitment management. This is a narrower function.
- **Upward** — the savings are larger in percentage terms (routing claims 30–85%
  versus 10–20% typical for cloud), so a higher fee is defensible.

Net: **1.5–3%.**

### The numbers

| | 2026 | ~2030 at ~36% CAGR |
|---|---|---|
| Category TAM (all coding-agent spend × 2%) | **~$240M** | ~$800M |
| **SAM** (addressable holders × 2%) | **~$145M** | ~$480M |
| Realistic share for a well-executed small entrant (2–5% of SAM) | **$3–7M ARR** | $10–24M ARR |

### Two cross-checks

**Against the adjacent category.** LLM observability platforms are a **$2.69B market
in 2026** (from $1.97B in 2025, ~36% CAGR, heading for ~$9.3B by 2030). A
counterfactual-measurement sub-segment at $145–240M is 5–9% of that — plausible for
a specific function inside a broad category, and a useful reality check that this
is a segment, not a market.

**Against incumbent revenue, which is the sobering one.** LangChain — one of the
best-known names in the category, LangSmith included — reported **~$16M ARR in 2025
at a $1.3B valuation.** The *leaders* in LLM observability are at low tens of
millions. Any model that outputs $50M+ ARR for a niche entrant is wrong.

**Bottom-up, independently.** A consultancy spending $2M/yr on agent inference
yields $40–60k/yr at 2–3% capture. Fifty customers at $50k average = **$2.5M ARR**;
a hundred at $75k = $7.5M. Thirty to eighty customers via warm network over 2–4
years is plausible. That lands in the same band as the top-down figure, which is
the main reason to believe either.

### Sensitivity — where this breaks

| If… | Then |
|---|---|
| Coding is 20% of API spend, not 35% | Halve everything. SAM ~$80M, realistic $1.5–4M ARR |
| Capture is 1%, not 2% | Halve again |
| Agent-product vendors turn out to be buyers rather than builders | SAM rises ~65% to ~$240M |
| Per-seat pricing dominates coding agents | Denominator collapses; the model-choice decision sits with vendors, not buyers |

The $35B API-spend figure and the 35% coding share are the two assumptions doing
the most work. Both are secondary-source estimates and both should be revisited
before this number is used externally.

### What this means for the portfolio

**A $3–7M ARR business in 3–4 years, with $15–25M as a strong-execution ceiling.**

Two conclusions, and they point in opposite directions on purpose:

1. **Against this group's cost structure, that is excellent.** Three to four
   founders plus agents, against apps costing ~$250k to build: $3–5M ARR funds the
   ecosystem bet indefinitely and needs no further capital. The metered model
   compounds it — revenue grows with customers' token volume, which is itself
   growing ~40%/yr, so a *fixed* customer base grows revenue without new logos.
2. **It is not a venture-scale outcome on its own**, which is exactly what 01 says
   it is for. **The market sizing confirms the portfolio structure rather than
   challenging it:** replay is the funding mechanism that buys time for the
   ecosystem bet, not the large outcome. If it were a $1B category it would deserve
   to *be* the company, and this analysis says it isn't.

**Proportionality of effort**, since that was the question behind the question:

- The analysis so far has cost conversation and one document. Proportionate.
- The next step — drift pilot at ~$300 plus engineering time — is proportionate to
  a $3–8M opportunity.
- **Not proportionate**: building a full product, hiring sales, or letting it
  displace the ecosystem bet. Anything beyond the 90-day plan should be gated on
  actual revenue, not on this estimate.

**Sources.** [Enterprise LLM API spend](https://presenc.ai/research/enterprise-llm-adoption-statistics-june-2026) ·
[LLM observability market size](https://www.giiresearch.com/report/tbrc1981334-large-language-model-llm-observability-platform.html) ·
[LangChain revenue](https://getlatka.com/companies/langchain) ·
[FinOps percentage-of-spend pricing](https://holori.com/20-best-finops-and-cloud-cost-management-tools-in-2025/)

---

## Buyer segments

Ranked by acuteness of pain, not by market size.

### 1. Agent product companies — design partners, not the revenue base

Companies whose product *is* an agent (coding tools, SWE agents, AI QA, autonomous
PR bots). Inference is COGS; model choice **is** gross margin. Buyer: CTO or Head
of Model Infra. Short cycles, technical enough to value the methodology, logos that
buy credibility.

**Risk:** the most likely segment to build it in-house. Use them as design
partners — cheap or free, for feedback and reference — and monetise elsewhere.

### 2. Large consultancies and systems integrators — the revenue base

Reachable warm through the founders' network. Agent inference is **cost of
delivery**, so routing savings convert directly into gross margin on work already
sold. Many client codebases means high variance, which is where routing pays most,
and they face many simultaneous retirements.

Buyer titles: Head of AI Engineering, Practice Lead, Head of Delivery Excellence,
or whoever owns delivery margin. Secondary: the person who must justify tooling
choices to clients, for whom the evidence artifact has procurement value beyond
the savings.

**This is the segment with the best ratio of pain to reachability, and the one the
group is uniquely positioned for.**

### 3. Enterprise platform / DevEx teams — biggest, slowest

VP Platform Engineering, Head of Developer Productivity, AI Enablement Lead. They
already measure engineering effectiveness (DORA/SPACE), so measurement culture and
budget line both exist. Long procurement, InfoSec involved.

### 4. Teams that already bought a router — natural attach

Anyone running LiteLLM, Portkey, Martian or similar has the router and no way to
prove it works. The qualifying question writes itself.

### 5. Model vendors — partnership, not a sale

They benefit when migration friction falls. Long cycle; note and defer.

---

## The proof problem, and why public data solves it

**The concern:** the corpus is ~936 sessions from one harness, one team. Proving
the method generalises appears to require customer data — which creates a security
risk for them, and shipping a runner pre-purchase creates a reverse-engineering
risk for us.

**The resolution:** generalisation risk here is not about model behaviour. It is
about **harness and environment diversity** — transcript formats, lockfile and
build conventions, monorepo versus polyrepo, languages whose dependency graphs
don't behave like npm's. All of that is testable on public code, with third-party
data, without asking anyone for anything.

### Public corpora with both code and reconstruction materials

| Dataset | Scale | Harness |
|---|---|---|
| [nvidia/Open-SWE-Traces](https://huggingface.co/datasets/nvidia/Open-SWE-Traces) | 207,489 trajectories, **9 languages**, from 20k real PRs | **OpenHands + SWE-agent** |
| [nvidia/SWE-Zero-openhands-trajectories](https://huggingface.co/datasets/nvidia/SWE-Zero-openhands-trajectories) | 318k | OpenHands |
| [nebius/SWE-agent-trajectories](https://huggingface.co/datasets/nebius/SWE-agent-trajectories) | 80,036, multiple models | SWE-agent |
| [nebius/SWE-rebench-openhands-trajectories](https://huggingface.co/datasets/nebius/SWE-rebench-openhands-trajectories) | Qwen3-Coder-480B | OpenHands |
| [nvidia/SWE-Hero-openhands-trajectories](https://huggingface.co/datasets/nvidia/SWE-Hero-openhands-trajectories) | 34k | OpenHands |

**Open-SWE-Traces is the first pick** — nine languages across two harnesses is
exactly the breadth claim, and it is someone else's data, which is what makes it
impartial.

**Why the reconstruction materials are genuinely present.** Trajectories supply
turn-by-turn actions and observations; the underlying task datasets (SWE-bench,
SWE-bench-extra, SWE-rebench) supply a **pinned repo commit, a dockerised
environment, and install/test commands**. Join on instance ID and you have an exact
initial state plus the action sequence, so state at any turn K is *regenerable* —
no third-party snapshots needed.

> **Public benchmark environments are deterministic by construction; real sessions
> are not.** The mutation-guard problem — the hardest correctness issue in the
> design — largely vanishes on this corpus. Public data is *cleaner* substrate for
> demonstrating the method than our own sessions are.

The two corpora do different jobs, and should be described that way: **the public
corpora prove breadth; the 936 real sessions prove it survives real-world mess.**

**One worry that can be dropped:** it does not matter which model generated these
trajectories. Counterfactual replay needs a valid starting state and an action
sequence; the original model is substrate. Intervene at turn K with whatever panel
matters.

### Caveats to state before relying on this

1. **Benchmark-shaped, not product-shaped.** One GitHub issue, short and uniform,
   versus 50–200-turn product development. Breadth, not depth — say so explicitly.
2. **Adapter work is real.** OpenHands and SWE-agent trajectory formats are not
   ours. It is also the same work required to serve any customer on those
   harnesses, so it is a requirement surfaced early rather than a detour.
3. **Licences must be checked** before any commercial claim rests on them.
4. **Grading noise is documented in this exact setting** — see the ["lucky pass"
   problem in SWE-agent evaluation](https://arxiv.org/pdf/2605.12925), where tests
   pass for the wrong reason. Grading on tests inherits it; an argument for the
   blinded-judge path, and worth citing rather than rediscovering.

### Prior art that bears on positioning

- **SWE-Replay** (arXiv 2601.22129) is not what the name implies: it is test-time
  scaling that recycles prior trajectories by **branching at critical intermediate
  steps** to improve agent performance. Useful twice over — it validates the
  mechanism's feasibility, and it establishes mid-trajectory branching as published
  art, so **differentiation rests on the measurement framing rather than on
  branching itself.** No dataset released.
- **Causal Agent Replay** (arXiv 2606.08275) releases code
  (`github.com/jaineet17/causal-agent-replay`) and applies do-operations to a step
  for *failure root-cause attribution*, validated on synthetic causal models. Same
  primitive, third purpose, no filesystem. Read their intervention formalism before
  finalising ours.

---

## The staged trust model

The security risk and the reverse-engineering risk are not a trade-off — they
belong at different stages. **Never use the strongest instrument before there is a
contract.**

| Stage | Instrument | Their data | Our code ships | Gate |
|---|---|---|---|---|
| **0. Public proof** | Published study, public corpora, multiple harnesses and languages | None | None (a paper) | Free |
| **1. Qualify** | **Metadata-only estimate** | Metadata only — no code, no prompts | None | Free |
| **2. Pilot** | Full engine, self-hosted or in their VPC | Never leaves | Runner only | **Paid** |
| **3. Deploy** | Continuous monitoring and policy | Never leaves | Runner only | Subscription |

**Stage 1 is the unlock.** A qualifying estimate needs no source code: turn counts,
token counts, tool-call distribution, model used, elapsed time, cost. That is
enough for *"roughly X% of your turns look like cheaper-tier candidates; expected
recoverable range Y."* It is deliberately weaker than a true counterfactual — it
only has to be credible enough to justify a paid pilot — and it carries **zero data
risk and zero IP risk**.

### Runner / grader split

An architectural requirement that comes from the commercial plan rather than from
the technology:

> **Ship the runner. Keep the grader.**

Snapshot, restore and re-drive are plumbing and must run in their environment. The
**experimental design and grading is the moat** and can operate on derived
artifacts — action traces, structured diffs, metrics — served from our side. If a
runner is disassembled, what is obtained is the commodity half.

Two things make the residual risk acceptable:

- **Publishing the methodology dissolves most of it.** You cannot lose by
  disclosure what has already been disclosed; what remains is implementation
  quality, calibration and the corpus.
- **Reconstructing a working system from a shipped artifact is slower than
  building it from a design you understand.** The realistic threat is a funded
  competitor deciding to build, and they would start from the paper. Against a
  12–18 month window, moving slowly to protect the artifact costs more than the
  artifact is worth.

Middle ground where a customer prefers it: **we deploy and operate inside their
VPC** — they get isolation, we keep operational control, and no artifact sits on
anyone's laptop indefinitely.

---

## Pricing

`PROPOSED` — **metered, per token of the session analysed.** Not subscription, and
not a share of savings.

**Why metered rather than subscription.** A subscription has to clear a
procurement threshold: at 40% savings, justifying $50k/yr at 3× ROI needs roughly
**$375k/yr of customer inference spend**, which prices out everyone below the
largest buyers. A $500 analysis needs no procurement cycle at all. Metering also
degrades gracefully — when a routing policy stabilises and usage falls to periodic
re-checks, revenue dips instead of churning to zero, which is the honest answer to
"is this a one-time purchase wearing a subscription costume."

**Why per *analysed* token rather than per *consumed* token.** This distinction
decides whether the business survives falling model prices:

| Basis | If inference drops 95% |
|---|---|
| Price per token consumed running the replay (cost-plus) | **Revenue falls 95%** alongside COGS — the customer's savings shrink and so does the fee |
| **Price per token of the customer's analysed session** | Revenue tracks **their workload volume**, which grows, while COGS falls — margin expands |

Anchor the rate against replay COGS (~$1.59 per turn across a five-arm panel at
N=3, before batch — see §Budget) at a healthy multiple, then decouple it from
inference price by denominating in *their* tokens.

**Never a share of savings.** It takes a percentage of a number we also produce.
Buyers see the conflict immediately, and it undermines the only thing being sold.

Land-and-expand becomes usage growth rather than seat negotiation: a first
diagnostic, then continuous analysis as the customer's agent volume rises.
*(Trade-off to accept: metering needs metering and billing infrastructure, and
usage-based pricing implies at least partial self-serve, which carries a docs and
support load — the human-shaped work 01 warns about. Budget for it deliberately.)*

## Budget

Rates are Anthropic first-party, per MTok, as of 2026-09-10. Cache **write** is
1.25× (5-min TTL) or 2× (1-hour); **Batch API is 50% off** and is the right shape
for offline grids.

| Model | Input | Output | Cache read |
|---|---|---|---|
| Haiku 4.5 | $1 | $5 | 0.1× |
| Sonnet 5 | $2 | $10 | 0.1× |
| Opus 5 / 4.8 | $5 | $25 | 0.1× |
| Fable 5 / 5.1 | $10 | $50 | **0.025×** ($0.25) |

### Cost per replayed turn

`ASSUMPTION` — 25k input tokens at turn K (benchmark-shaped session), 2k output
with thinking, 4k for Fable since thinking is always on. N=3 samples per cell
sharing one cached prefix.

| Arm | Per turn |
|---|---|
| Haiku 4.5 | $0.07 |
| Sonnet 5 | $0.13 |
| Opus 5 | $0.33 |
| Fable 5.1 | $0.93 |
| Control (fresh replay of the original condition) | $0.13 |
| **Five-arm total** | **$1.59** |

### Study totals

| Grid | Tokens | With batch |
|---|---|---|
| Lean — 300 turns, 4 arms, N=3 | $199 | **~$100** |
| Full — 300 turns, 5 arms, N=3 | $476 | **~$238** |
| Publishable — 500 turns, 5 arms, N=5 | $1,125 | **~$563** |
| Grading (judge on the ~30% of cells tests can't settle) | $188 | ~$94 |
| Latency subsample — ~100 turns, **synchronous, no batch discount** | ~$160 | ~$160 |

**The publishable study is roughly $800 in tokens.** At 6× these token
assumptions it is ~$4k. **Tokens are not the constraint on this project;
engineering time and compute are.**

### The drift experiment costs more than the grid

Correcting an earlier estimate of "a few hundred dollars": the drift experiment
runs **forward to completion** from turn K, which is precisely the expensive
full-session replay the product exists to avoid. Each run is 10–30 turns rather
than one.

- **Pilot** — 40 sessions × 2 K-positions × 2 arms × N=3 ≈ 480 runs at ~15 turns:
  **~$300 with batch.** Enough to see whether a relationship exists.
- **Conclusive** — 100 sessions × 3 K-positions × 2 arms × N=3: **~$1–2k.**

### Cost levers, in order

1. **Batch API — 50%**, for everything except the latency arms.
2. **Cache the prefix and run samples within a cell sequentially.** A cache entry
   is only readable once the first response begins streaming, so **N parallel
   requests with the same prefix all pay full price** — parallelism forfeits the
   0.1× reads and roughly doubles input cost.
3. **Prefer objective grading.** SWE-bench-family instances ship tests, so most
   cells grade on compute rather than tokens. Reserve the blinded judge for
   genuinely ambiguous cells.
4. **Hold effort fixed in the first grid.** A five-level effort sweep multiplies
   arms by up to 5 *and* raises output tokens — a 5–8× jump. Second study, on a
   subsample.

### Subscriptions vs API keys

Claude Max, ChatGPT Pro and Grok subscriptions are fine for exploration. Use
**metered API keys for the study and anything commercial**, for three reasons:

- **Batch's 50% is API-only** — that alone pays for the metered path.
- **Rate limits.** A grid is bursty and sustained; interactive quotas will throttle it.
- **The commercial product needs metered billing anyway**, and customer-side runs
  use the customer's keys.

Measuring in **tokens rather than dollars** removes attribution as a concern, so
that is not an argument against the subscriptions — the three above are.

`OPEN` — API keys with metered billing may not exist yet for all four vendors.
This is a prerequisite, not a detail.

### Not tokens

**Compute.** Docker builds and test runs across a publishable grid are real CPU —
on the order of $100–300 and considerably more wall-clock than the inference.
Route it through `compute-run` rather than a session container.

---

## Distribution

1. **Publish a finding, not a product.** Run the grid on public corpora and
   publish: *"We replayed N real coding-agent sessions across M models and K
   languages — here is what routing would have saved, and where quality actually
   regressed."* Findings get forwarded; product pages do not. It doubles as the
   methodology-authority play, and it is agent-assistable, so it costs no
   headcount.
2. **Structure the warm network.** Ask each of the two co-founders and the investor
   for ten names who own delivery margin or AI platform spend. Thirty warm intros
   is more than enough to find the first five customers, and it beats every
   outbound channel available to a three-person company.
3. **Free metadata diagnostic as the qualifier.** The output *is* the sales
   artifact; the conversation becomes "here is your number" rather than "let me
   explain the product."
4. **Routers, gateways and model-agnostic harnesses as channel.** They sell
   switchability and cannot prove it. Co-marketed integration reaches their
   installed base. Slower to negotiate; high leverage.
5. **Time publication to retirement announcements**, when buyers are actively
   searching.

**Do not:** volume SEO/content (needs a team), paid acquisition (needs a funnel),
conference booths (low yield at this stage), or a self-serve product (needs docs
and support at scale). All four require the humans the minimal-core thesis exists
to avoid.

---

## First 90 days

| Weeks | Action | Produces |
|---|---|---|
| 1–2 | Run the grid on public corpora (Open-SWE-Traces first) plus our own 936 | The finding — nothing else can start without it |
| 3–5 | Publish. In the same window, collect the 30 names | Authority + pipeline |
| 4–8 | Five metadata diagnostics: two agent-product design partners, three consultancies | Qualified pipeline; and the diagnostics reveal what the product must be |
| 8–12 | Convert two to paid pilots, self-hosted | First revenue; first security review passed |

The gate at week 12 coincides with VesselHaven's launch and the identity-model
decision, which is the single assessment point 03 anticipates for the director
question.

---

## Risks, ranked

1. **Security friction on data access.** Slows every deal. Mitigated by
   self-hosting from day one and by never needing data before stage 2.
2. **In-house build by technical buyers.** Precisely why agent-product companies
   are design partners and consultancies are the revenue base.
3. **The 12–18 month window.** Workspace reconstruction is a roadmap item for an
   incumbent, not a research project. Argues for starting with the publication
   rather than with more building.
4. **Generalisation genuinely failing** on other harnesses — see kill criteria.
5. **Drift into bespoke consulting**, because this segment will ask for it and the
   commercial bench is good at it. 03's scope rule applies here first: sell
   implementation of a licensed product, never bespoke work.

## Counter-arguments — what would make this insolvent

Distinct from the kill criteria below, which cover *execution* failure. These are
ways the market could be wrong.

| Argument | Status after review | Cheapest test |
|---|---|---|
| **Model prices collapse ~95%**, so savings are immaterial *and* full-session replay becomes affordable, removing turn-K's efficiency advantage | **Live, and the deepest one — but survivable on the speed axis.** Partial rescue: Jevons — unit price falls, usage rises, aggregate spend holds. And the optimisation target moves from *which model* to *which configuration finishes in fewest turns*, which is a **speed** question that cheap tokens do not touch. **Same engine, different pitch** | None; pivot when it lands. Hold the cost framing loosely and keep speed measured from day one |
| **Frontier families converge on quality**, so there is nothing left to tune | **Weakest of the set once speed is in scope.** Convergence on quality does not imply convergence on tokens/second, and effort trades against time regardless | Track the tier-to-tier spread on our own workload — we are positioned to measure our own market |
| **Harness vendors ship it natively** — Cursor, Claude Code, Copilot, Factory own the session format, workspace, user and telemetry | **The most likely killer.** Defence is genuine: they compare models *within* their harness and will never show that a competitor's configuration wins. Cross-vendor, cross-harness neutrality is structurally unavailable to them — the same claim routing products are built on | Watch release notes; ask design partners what they expect their vendor to add |
| **Model vendors remove the migration trigger** with long support windows and free assurance tooling | **Weak.** Vendor assurance answers *"is it safe to move"*, not *"which family and effort level"* — and vendors have **negative incentive** on the second, since the honest answer is often "use a cheaper tier." They will not build the thing that reduces their own revenue | — |
| **Savings below the cost of buying** — $20k saved doesn't justify a procurement cycle and a security review | **Fair, and it repriced the product.** Resolved by metering rather than subscription (§Pricing), which removes the threshold and puts the mid-market back in scope | The five metadata diagnostics in weeks 4–8 |
| **Turn-K counterfactuals don't predict policy outcomes** — trajectories are path-dependent, so a better decision at K can yield a worse session | **Live and unresolved. Needs real data.** This is the falsification test for the entire product, not a feature: if drift is large and unpredictable, the measurement is precise and irrelevant | **The drift experiment, weeks 1–2, a few hundred dollars — and it must precede publication** |
| **One-time purchase wearing a subscription costume** — a fitted policy holds for months | **Rejected.** Tuning is continuous regardless of churn: new efforts, families, workload mixes. Metering lets the customer decide when it is worth running, and revenue dips rather than churning when a policy stabilises | Ask the first two customers what a renewal is *for* |
| **Open-source commoditisation** | **Withdrawn.** Reasoned by analogy rather than from evidence. Publishing a methodology is not publishing an implementation, and the implementation — workspace reconstruction, mutation guard, harness adapters, grading — is the hard part, not weekend OSS. The realistic version of this fear is the harness-vendor row above | — |

### Solvency conditions

1. Aggregate agent spend keeps rising as unit price falls, so *something* stays
   worth optimising.
2. **Drift is bounded and turn-K predicts policy outcomes.**
3. Cross-vendor neutrality has visible commercial value against vendor tooling.
4. Metered pricing clears at small deal sizes — no procurement cycle required.
5. Configuration tuning remains a live question — i.e. the
   **quality / cost / speed** spread between families and effort levels stays
   material.

**Speed is the axis that makes conditions 1 and 5 robust**, and it deserves its own
statement because it survives both lethal scenarios:

- **Against price collapse:** if tokens approach free, latency and elapsed time
  remain hard constraints. Agent throughput is bounded by wall-clock, not by bill.
- **Against quality convergence:** even where families converge on quality, they
  differ sharply in tokens/second, and effort levels trade directly against time.
  There is still something to tune when there is nothing left to tune on quality.

It also reaches a **different buyer**. Cost savings interest whoever owns the bill;
speed interests whoever owns delivery throughput — engineering leadership, and
delivery margin in a consultancy. That converts the pitch from *"spend less"*, a
cost-centre conversation, to *"ship faster"*, which historically sells at higher
prices and with less scrutiny.

And the two framings converge on one measurement: **turns-to-completion**. A model
that is slower per turn but needs half as many turns is faster end to end — which
is exactly the "which configuration finishes in fewest turns" metric the
price-collapse pivot lands on. The axis that survives cheap tokens and the axis
that survives converged quality are the same axis.

### Measuring speed costs more than measuring cost

Three practical consequences, because latency is not deterministic the way token
counts are:

1. **Latency is confounded by infrastructure** — API load, region, time of day,
   rate limiting. **Randomise arm order within a cell across time.** Running all
   Haiku samples at 03:00 and all Opus at 09:00 produces a speed result that is
   really a measurement of the provider's diurnal load.
2. **Fast mode is a distinct cell, not an effort level.** On Opus 5 / 4.8 it runs
   the same model at up to ~2.5× output tokens/second at premium pricing
   ($10/$50). Speed is purchasable, so the frontier is genuinely
   three-dimensional and fast mode belongs in the grid as its own arm.
3. **Batch runs cannot measure latency.** The Batch API's 50% discount is
   asynchronous by construction, so **the speed-measuring arms have to run
   synchronously at full price.** Budget a smaller latency subsample — on the
   order of 100 turns — at full rate, and put the quality/cost grid through
   batch. That is a modest addition to the totals in §Budget, but it is not free
   and it is easy to overlook until the numbers don't reconcile.

Two of these are testable in the first month for under $1k, and one of them —
**drift** — should be tested before anything is published, because a study built
on an invalid premise is worse than no study.

## Kill criteria

Set in advance so the decision is evidence rather than attachment:

- **Stage 0 fails** if the public-corpus study cannot produce a credible,
  defensible savings figure across at least two harnesses and three languages.
  Then the generalisation claim is false and nothing downstream is worth building.
- **Stage 1 fails** if metadata-only estimates cannot be made credible enough to
  convert to a paid pilot. Then the sales motion requires data access up front,
  and the funnel economics change fundamentally.
- **Stage 2 fails** if two design partners cannot pass their own InfoSec review on
  a self-hosted deployment within a quarter. Then the buyer is wrong, not the
  product.

## Open questions

1. Which asset gets licensed first — this or Claude Code Cloud? (QUESTIONS Q13)
2. Who owns the commercial motion day to day, given the split ownership in 03?
3. Dataset licences: cleared for commercial use?
4. Does the metadata-only diagnostic actually persuade? Testable in week 4 with
   one warm intro, before anything is built for it.

---

## Handoff to an execution session

*What a fresh bridge session needs in order to produce real results rather than
more analysis. Everything below is operational.*

### The first task, and it is not the grid

**Run the drift experiment before anything else.** It is the falsification test
for the product (see §Counter-arguments): if an intervention at turn K does not
predict end-of-session outcome, the measurement is precise and irrelevant, and
publishing a study built on it would be worse than publishing nothing.

**Spec:**

| | |
|---|---|
| **Corpus** | A public set with an objective end-state grader — SWE-bench-family, so tests decide pass/fail |
| **Sessions** | 40 for the pilot; short trajectories preferred (10–40 turns) |
| **K positions** | ~25%, ~50%, ~75% of turn count, so distance-to-end varies |
| **Arms** | Control (fresh replay, original condition) + one cheaper tier |
| **N** | 3 per cell |
| **Run** | Forward to completion from K, then grade end state with the instance's tests |
| **Record** | Per-turn action match at K; end-of-session pass/fail; distance from K to end; wall-clock; tokens; cost |
| **The question** | Does agreement at turn K predict the end-state outcome, and does the effect decay or compound with distance to end? |
| **Kill result** | No usable relationship between the turn-K signal and end-state outcome |
| **Budget** | ~$300 with batch. Hard ceiling: $600 |

### Where things are

| | |
|---|---|
| Design of record | `REPLAY_DESIGN.md`, `REPLAY_WALKTHROUGH.md` (repo root of `llm-slack-channel-bridge`) |
| Implementation | `packages/cloud-worker/src/replay-*.ts` — `replay-cli`, `replay-harness`, `replay-capture`, `replay-dispatch`, `replay-perturbation`, `replay-git-snapshot`, `replay-truncate`, `replay-reconstruct`, `replay-index` |
| Own corpus | ~936 sessions on EFS, never deleted, four models in use |
| Public corpora | `nvidia/Open-SWE-Traces` (9 languages, OpenHands + SWE-agent), plus the datasets in §Public corpora |
| Heavy commands | `compute-run` — not the session container |

### Prerequisites to clear before running anything

1. **Metered API keys** for each vendor in the panel, stored in Secrets Manager
   (not subscription OAuth). Blocking.
2. **Dataset licences** confirmed for commercial use. Blocking for publication,
   not for the pilot.
3. **A results file convention**, so findings accumulate on disk rather than in a
   transcript. Every run appends: session id, K, arm, model, effort, N index,
   tokens in/out, `cache_read_input_tokens`, wall-clock, grade, cost.
4. **A hard budget ceiling with a kill switch** — an `.abort` file checked between
   items, so a runaway grid stops with partial results saved rather than burning
   the ceiling.

### Measurement conventions — fix these before the first run

- **A "turn" is the unit the capture layer already snapshots.** Do not redefine it
  mid-study.
- **Verify caching is actually working**: `usage.cache_read_input_tokens > 0` on
  the second sample of every cell. If it is zero, something in prompt assembly is
  invalidating the prefix and input cost is ~10× what it should be. This fails
  silently.
- **Record wall-clock even in the batch arms**, but never *compare* speed across
  batch and synchronous runs.
- **Randomise arm order across time** in the latency subsample.

### Known traps

| Trap | Consequence |
|---|---|
| Parallel samples over one prefix | All pay full price; no cache reads |
| Batch used for latency arms | Speed numbers meaningless |
| Comparing effort labels across tiers | Not comparable; sweep effort as an axis. Haiku 4.5 has no effort parameter |
| Passing thinking blocks cross-model | Origin-locked on Fable; normalise prefix thinking uniformly so every arm thinks fresh at K |
| Grading only on tests | Inherits the ["lucky pass"](https://arxiv.org/pdf/2605.12925) problem — tests passing for the wrong reason |
| Judge is a contestant | Model prefers its own output. Blind it, randomise order, third-party arbiter on ties |
| Baseline is the recorded outcome | Confounds every result with prompt-regeneration drift. Baseline must be a **fresh replay of the original condition** |
| Replaying turns with external side effects | Skip them — readable off `tool_log`. Write-guard and egress denied regardless |

### Decision gates

| Gate | Result | Action |
|---|---|---|
| Drift pilot | Relationship exists | Proceed to the quality/cost/speed grid |
| Drift pilot | No relationship | **Stop.** Report it; do not publish |
| Drift pilot | Ambiguous | Scale to the conclusive design (~$1–2k) before proceeding |
| Grid | Defensible figures across ≥2 harnesses and ≥3 languages | Write the publication |
| Grid | Fails that bar | Generalisation claim is false — see §Kill criteria |

### Deliverable

Not a summary in a transcript. **A results file, plus a written finding** with the
grid, the confidence intervals, and the methodology. That document is
simultaneously the scientific artifact, the marketing artifact and the sales
artifact — which is why it is worth writing properly the first time.

### Negative results are the deliverable too

**Stated explicitly because an execution-focused session will otherwise optimise
toward a publishable positive.** That drift is the falsification test means a
negative or weak result is a *successful* outcome of the work, not a failure of it.

This is not only a matter of integrity. **The product being sold is trustworthy
measurement.** A study that overstates will be dismantled by exactly the technical
buyers being targeted, and the cost lands on methodology authority — the single
asset the whole strategy rests on (§Runner/grader split, §Distribution). There is
no version of this business that survives being caught flattering its own results.

**Mechanisms, because an instruction is not a control:**

1. **Pre-register before running.** Write the hypothesis, the metric, the
   threshold and the decision rule to the results file *first*. A pre-registered
   threshold cannot be reinterpreted after the numbers arrive.
2. **Write both abstracts in advance** — the positive finding and the negative one.
   If the negative version is unwritable, the experiment isn't designed to fail,
   which means it isn't a test.
3. **Report the whole grid, never the favourable slice.** Confidence intervals,
   not point estimates.
4. **Log every exclusion with a reason and a count.** Cells dropped for
   uninteresting causes — non-replayable turns, harness errors, timeouts — are part
   of the result. Silent exclusion is the most common way an honest study becomes a
   dishonest one.
5. **State n, excluded n, and power.** A result that cannot distinguish its
   hypothesis from noise is an ambiguous result, not a positive one.
6. **If ambiguous, scale the experiment — do not reframe it.** The conclusive
   design is $1–2k (§Budget). That is cheaper than publishing something
   unfalsifiable.

### What each drift outcome means — so a negative result is actionable

| Outcome | Interpretation | Product consequence |
|---|---|---|
| **Turn-K signal predicts end-state, effect decays with distance** | The premise holds | Proceed as planned. Distance-to-end becomes a confidence weight in the routing policy |
| **Predicts end-state, effect compounds** | Premise holds, and drift is the more valuable measurement | Lead with drift safety rather than with savings — it is the thing nobody else can offer |
| **Bounded but weak** | Turn-K measures *local* quality only | Narrower product: "which turns are safe to cheapen", with a conservatism margin. Still sellable, priced lower |
| **Large and unpredictable** | Per-turn counterfactuals do not support policy | **Pivot, don't persist.** The surviving question is turns-to-completion — the speed framing in §Solvency conditions, measured end-to-end rather than per turn. Same engine, different unit of analysis |
| **No measurable relationship at all** | The premise is false | Stop. Report it internally. The engine retains internal value for tuning our own factory (02 §The counterfactual engine) |

Note that **only one of five outcomes kills the commercial thesis**, and three of
them change the product rather than ending it. Worth knowing before the numbers
arrive, because it removes most of the incentive to lean on them.

### What this session should *not* do

- Build product packaging. Not until the drift result exists.
- Approach customers. Stage 1 needs the finding first.
- Extend the grid to effort sweeps. Second study.
- Optimise the runner. Correctness of reconstruction beats speed of reconstruction
  at this stage.
