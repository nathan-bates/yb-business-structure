# 01 — Portfolio

> **Revision note.** An earlier draft treated the agent factory as the flagship.
> It isn't — it's a building block. This version is restructured around the
> ecosystem play as the centre of gravity, with three revenue lines beneath it.

## The governing idea

Youbiquity is **three revenue lines and one option.**

Two of the lines earn money on a knowable timescale and generate evidence. The
third — the ecosystem play — is the only one with a venture-scale outcome, and
is also the most likely to fail. The structure's job is to let the first two
fund and de-risk the third without consuming the attention it needs.

Stated for the investor conversation, because this is the whole pitch in two
sentences:

> We are not asking you to fund a moonshot with no revenue. We are asking you to
> fund an **option** on a large outcome, where the other two lines pay for the
> option premium and produce the technology the option depends on.

## The three lines

| | **1. Ecosystem** | **2. Core tech** | **3. Apps** |
|---|---|---|---|
| **What** | Aggregation/orchestration platform: acts across many third-party apps on the user's behalf, recommends by preference, then executes (scheduling; shopping) | Sell/lease the agent stack — bridge, ProductLens, Sextant, aux — as a consulting offering | VesselHaven, App #2, #3… vertical SaaS built by the factory |
| **Assets** | `project-k`, `aux` | `llm-slack-channel-bridge`, Sextant, Archon/ProductLens | VesselHaven |
| **Horizon** | 3–5 yr | Now | 1–2 yr |
| **Risk** | High — unproven, contested space | Low | Medium |
| **Scales with** | Network effects | **Humans (linear)** | Factory maturity (sub-linear) |
| **Role** | The outcome | Runway + credibility | Proof of the cost curve |

The middle column is the one to watch. Consulting is the only line whose costs
scale linearly with headcount — it is simultaneously the nearest revenue and the
most direct threat to the minimal-core goal. It should be **capped deliberately**,
not grown opportunistically.

> **NEEDS YOU** — Is consulting meant to fund the ecosystem play, or is it a
> parallel bet you'd grow on its own merits? If the former, it needs a stated
> ceiling (engagements/yr, or % of founder time) written into the operating model
> in 03. Left uncapped, it wins by default — it's the only line with customers
> asking for more of it today.

## Why these are one company and not four hobbies

This is the part an investor will probe, so it's worth stating precisely. The
ecosystem play needs three capabilities. Youbiquity has already built all three,
for unrelated-looking reasons:

1. **A map of apps you don't control.** To act inside a third-party app, an agent
   must understand its structure without documentation or an API. That is exactly
   what Sextant does — autonomous exploration producing a graph where design,
   code and behaviour are one artifact. Built as a testing tool; **it is actually
   a map-making tool.** This is the strongest and least obvious asset in the group.
2. **Hands.** Something that executes inside a real, authenticated session when
   no API exists. That's aux and the co-browse work.
3. **A runtime.** Somewhere agents live, hold credentials, and are supervised.
   That's Claude Code Cloud.

Apps (line 3) then serve two purposes: they prove the factory's cost curve, and
they become the **first citizens** of the ecosystem — the reference
implementations for the interfaces and patterns the platform wants to dictate.

> **ASSUMPTION** — This reading of `project-k` and `aux` comes from your
> description; I have no read access to either repo (the App isn't installed on
> the `Youbiquity` org — see 02 open items). Correct anything I've mischaracterised
> before this argument gets used externally.

## The differentiator worth testing early

"Interacts with other apps through whatever means necessary" has an unavoidable
problem: **credential custody.** An agent acting on your behalf across dozens of
services normally has to hold your logins. That is a serious security surface, a
regulatory burden, and the single most common reason users refuse this category
of product.

The co-browse model appears to sidestep it — execution happens **in the user's
own authenticated browser session**, so the platform never holds the credentials.
If that holds up, it isn't just an implementation detail; it's a defensible
market position against better-funded competitors who will be asked "so you have
all my passwords?" in every enterprise review.

> **NEEDS YOU** — Is that an accurate reading of the aux/co-browse execution
> model? If yes, it deserves to be a stated pillar of the ecosystem strategy
> rather than a technical footnote.

## Asset inventory

| Asset | Line | Class | Status |
|---|---|---|---|
| **project-k** | 1 | Flagship (pre-product) | PoC |
| **aux** | 1 (+ possible standalone) | Execution substrate — the "hands" | PoC; responsive-surface / true-send working |
| **Sextant** | 1 + 2 | Map-making IP; consulting asset | Working, unproductised |
| **Claude Code Cloud** | 1 + 2 | Agent runtime; the factory | Load-bearing, runs daily |
| **Archon / ProductLens** | 2 | Work layer | Mid-consolidation |
| **VesselHaven** | 3 | Revenue vertical; first ecosystem citizen | Live |

## Proposed entity shape

```
                    Youbiquity Group  (umbrella / holdco)
                    - investor equity sits here
                    - owns all platform IP, licenses it down
                    - employs the human core
                    - INCUBATES the ecosystem play (no entity yet)
                              |
              +---------------+---------------+
              |                               |
     Youbiquity Platform            Youbiquity Apps  ("App Portfolio")
     (core tech + consulting)       (holding co for verticals)
     - Sextant, bridge,                       |
       Archon/ProductLens               +-----+-----+
     - now REVENUE-generating,          |           |
       not a pure cost centre      VesselHaven   App #2, #3...
```

Two changes from the earlier draft:

**The ecosystem play stays inside the Group for now.** Pre-product, it has
nothing to protect and nothing to sell. It gets its own entity at the first of:
outside capital specific to it, a filing worth protecting, or a partner
requiring a counterparty. Creating it earlier is administrative cost with no
corresponding benefit.

**Platform is no longer a pure cost centre.** Consulting revenue lands there,
which makes the intra-group IP licensing arrangement more important, not less —
Platform will be both earning externally and charging internally.

**VesselHaven remains a grandchild**, for the reason given before: it keeps each
vertical individually disposable without touching platform IP. That reasoning
strengthens under the ecosystem thesis — a vertical may be worth more to an
acquirer *because* it's a working citizen of the platform, and you want to be
able to sell one without selling the standard.

## Attention allocation — the actual scarce resource

Three lines, three horizons, 3–4 people. Capital isn't the binding constraint;
founder attention is. A structure that doesn't say how attention splits will
default to whatever is loudest, which is always the line with paying customers.

Proposed rule, to be made concrete in 03:

- **Lines 2 and 3 are agent-delivered by default.** Any founder hour spent on
  them is a bug to be automated, not a cost of doing business.
- **Line 1 gets protected founder attention** — it is the only line that cannot
  be delegated to agents, because it's the one where the product doesn't exist yet.
- **Shift triggers stated in advance**, so reallocation is a decision rather than
  a drift: e.g. app #2 launching under a stated human-hour budget; a consulting
  engagement exceeding the cap; a `project-k` milestone slipping two quarters.

## Risks specific to the ecosystem play

Named plainly, because an investor will raise them and pre-empting is stronger
than answering:

1. **Counterparty risk.** You are operating inside apps you don't control. ToS
   restrictions, bot detection, and deliberate blocking are permanent structural
   risks, not bugs to fix once. Mitigation is partly the co-browse model (the
   user is genuinely present) and partly commercial (be a demand source apps want).
2. **Platform competition.** Well-funded incumbents are moving on
   agents-that-act-across-apps, and standardisation (MCP and successors) may
   commoditise the integration layer. Differentiation must be preference
   modelling, execution reliability, or trust — not merely having connectors.
3. **The aggregation trap.** Aggregators need supply *and* demand before either
   is valuable. VesselHaven-style owned apps are a partial answer to cold start;
   whether one vertical is enough is unproven.
4. **Trust and data.** Preference data is the moat and the liability
   simultaneously. Get the custody model right early — retrofitting it is
   expensive, as the credential-handling work in the bridge already demonstrates.

## Keep / kill / park

| Asset | Call | Change | Reasoning |
|---|---|---|---|
| project-k | **Invest — protected** | — | The option. Everything else is instrumental to it. |
| aux | **Invest** | ⬆ *reversed from "kill or absorb"* | Not a stray feature of the factory — the execution substrate for line 1, and possibly a product. |
| Sextant | **Invest — reframe** | ⬆ | Stop describing it as a testing tool internally. It's the map-making layer. |
| Claude Code Cloud | **Keep — invest** | — | Runtime for lines 1 and 2. |
| VesselHaven | **Keep — as proof and as citizen** | — | Cost-curve evidence plus ecosystem reference implementation. |
| Archon / ProductLens | **Park at "good enough"** | — | Still the call most likely to be wrong; it's now also line-2 consulting inventory, which argues for a little more polish than "freeze". |

## What has to be true

Falsifiable claims, ordered by how much rests on them:

1. **An agent can reliably act inside apps it wasn't built for.** The entire
   ecosystem play. Sextant's graph is the evidence base; it has so far been
   demonstrated mainly against VesselHaven, which co-evolved with it.
2. **Users will delegate real decisions** — scheduling, spending — to a
   recommendation layer. Product risk, not technical.
3. **A second vertical launches without adding humans.** Line 3's thesis; still untested.
4. **Consulting can be capped.** Organizational discipline, and historically where
   companies in this shape fail: the cash line quietly becomes the company.
5. **The human core can supervise across three lines at this ratio.** The bet 03 must make concrete.

## Open questions

1. Consulting: fund-the-ecosystem, or parallel bet? (needs a cap either way)
2. Is the co-browse "never hold credentials" reading correct?
3. VesselHaven — paying customers? Whose IP?
4. Jurisdiction and existing entities.
5. Where does the prospective employee sit? Under this structure the answer
   almost writes itself — Platform, so consulting delivery doesn't consume
   founder attention — but that's yours to confirm.
6. Repo access to `Youbiquity/project-k` and `Youbiquity/aux` so this document
   can be grounded in what's actually built.
