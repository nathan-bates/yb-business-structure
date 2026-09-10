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

**Flat subscription, justified by measured savings. Never a share of savings.**

Share-of-savings is the obvious idea and it is a trap: it takes a percentage of a
number *we also produce*. Buyers see the conflict immediately, and it undermines
the only thing being sold, which is trustworthy measurement. Use savings to anchor
and justify the fee; never to compute it.

Anchoring: their monthly agent inference spend is known to them. Published routing
results in the wider market claim 30–85% savings, which sets the frame without
needing a promise. Price the subscription materially below the demonstrated
recoverable amount and let the diagnostic do the arguing.

Land-and-expand shape: **diagnostic → routing policy subscription → continuous
migration and regression assurance**, renewing on every model launch.

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
