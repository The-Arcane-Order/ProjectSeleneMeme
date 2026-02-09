### Selene (LUNAR): Lunar Elastic Meme Coin + Open-Source Journalist Agent + Hubs + Federation (DAO of 12)
**Document type:** Technical project spec (team-shareable)  
**Status:** Concept / draft v0.2  
**Primary token:** **Selene**  
**Ticker:** **LUNAR**  

#### One-liner
Selene is a lunar-phase elastic meme coin ecosystem that funds and coordinates a real-world network of event **Hubs** (farms/cafés/warehouses/etc.), governed by a **Federation (DAO of 12)** and made legible through an **open-source journalist agent + newsletter app**. LUNAR also feeds a secondary “shares” layer that **locks/sinks LUNAR** and **cyclically mints NFTs** (trading cards) to create additional markets and long-term engagement.

#### Design goals
- **Rhythmic token mechanics:** lunar-cycle-driven rebasing creates predictable “seasons” for participation and volatility.
- **Real-world grounding:** Hubs + events give LUNAR a reason to exist beyond speculation.
- **Transparency as product:** the journalist agent explains rebases, treasury decisions, and hub outcomes with sources and audit links.
- **Bounded governance power:** the Federation is small (12) for execution speed, but constrained by policy bounds, timelocks, and reporting.
- **Economic sinks + tertiary markets:** a shares layer converts LUNAR deposits into cyclical NFT issuance, supporting secondary/tertiary market activity.
- **Open-source + forkable:** agent + newsletter pipeline + protocol policy specs are public.


### 1) System components

#### 1.1 Token: Selene (`LUNAR`) — elastic lunar currency
- **Type:** elastic supply token with rebasing behavior tied to lunar phases (“Lunar Epochs”).
- **Core mechanism:** rebasing **changes mode** depending on moon phase windows (e.g., pause / accelerate / cap).
- **Intended effects:** seasonality, gamification, and coordinated community cycles.

**Key parameters (draft)**
- `EPOCH_CADENCE`: how often rebase can occur (e.g., hourly, daily).
- `PHASE_WINDOWS`: New / Quarter / Full / other (optional: eclipses/supermoons).
- `REBASE_MODE[phase]` defines:
  - `enabled` (bool)
  - `direction` (expand / contract / neutral)
  - `max_rate_per_epoch`
  - `max_cumulative_rate_per_cycle`
- **Surgical rebase:** limited discretionary adjustment within strict on-chain bounds (see Governance).


#### 1.2 The Open-Source Journalist Agent (newsletter + “newsroom”)
**Lean app surface:** newsletter + feed that explains:
- Lunar status + upcoming phase windows
- Rebase events (automatic + any discretionary actions with rationale)
- Federation proposals/decisions + treasury movements
- Hub event calendar + recaps + impact metrics

**Two modes**
- **Journalist mode (default):** ingest, verify, cite, summarize, publish, correct.
- **Steward mode (permissioned):** propose or execute limited on-chain actions under policy constraints (e.g., scheduling an incentive campaign).

**Integrity requirements**
- Every claim should carry:
  - `sources[]` (links / hashes)
  - `confidence` (e.g., low/med/high)
  - `corrections` workflow (append-only changelog)


#### 1.3 Hubs platform (event network)
- **Hubs:** hosted venues (farms, cafés, warehouses, galleries, community spaces).
- **Events:** workshops, meetups, performances, festivals, salons.
- **LUNAR usage at hubs:**
  - attendance rewards
  - facilitator/performer subsidies
  - host incentives
  - bounties for volunteers and community work
- **Data output:** event metadata + outcomes feed the newsroom.


#### 1.4 Federation: DAO of 12 + Treasury
- **Federation:** 12-seat governance body.
- **Treasury:** receives defined inflows and allocates to:
  - Hub onboarding and event subsidies
  - Facilitator/performer programs
  - Citizen journalism bounties
  - Development + security audits
  - Marketing + community operations

**Core responsibilities**
- Set/adjust policy bounds for rebasing and incentives
- Allocate treasury funds
- Gate/upgrade agent permissions
- Trigger emergency procedures (pause discretionary actions, oracle failover)


#### 1.5 Secondary layer: “Lunar Shares” + Cyclical NFT Mint (economic sink)
A secondary market layer acts like “shares of LUNAR”:
- Users deposit/lock LUNAR into a **Shares Vault**.
- In return they receive a receipt/share token (e.g., `sLUNAR`) representing their vault position.
- The system **cyclically mints NFTs** (trading cards) to participants based on:
  - time held / epoch participation
  - share-weighted allocation
  - lunar phase modifiers (rarity curves)

This creates:
- A **sink** (LUNAR is locked, time-locked, or partially burned/redirected).
- A **secondary market** (`sLUNAR` positions can be tradable if desired).
- A **tertiary market** (NFT cards can be traded/used for perks).


### 2) Token flow & value generation (perspective model)

#### 2.1 Primary flows
**A) Treasury inflows (examples; choose 1–3 for MVP)**
- Rebase participation fee (small skim to treasury)
- Transaction fee (optional; careful with UX)
- NFT sink contract revenue (a portion of deposits or mint fees)
- Hub platform fees (ticketing/service fees)
- Voluntary contributions / sponsorships

**B) Treasury outflows**
- **Hub subsidies:** event grants, host onboarding grants, equipment microgrants
- **Facilitator/performer subsidies:** guaranteed minimums + bonuses for attendance/impact
- **Citizen journalism payments:** bounties for high-quality reports into the newsroom
- **Developer/security budget:** audits, bug bounties, infra


#### 2.2 Hub + event subsidies (how LUNAR “does work”)
A recommended structure:
- **Event Grant:** Federation approves a budget for an event.
- **Distribution:**
  - fixed base pay to facilitators/performers (predictable support)
  - variable bonus pool tied to verified attendance and/or outcomes
- **Verification:** QR check-in + host attestation + optional community review.


#### 2.3 Shares/NFT sink loop
- User deposits LUNAR → receives `sLUNAR`.
- Deposited LUNAR is:
  - locked for `LOCK_PERIOD`, and/or
  - partially burned, and/or
  - partially routed to Treasury (configurable)
- Each lunar cycle (or phase window), a **Mint Epoch** occurs:
  - NFTs are minted and distributed to eligible `sLUNAR` holders.
  - rarity and card sets change with lunar phases.

**Outcome:** reduces circulating pressure (sink), creates new collectible markets, and adds long-term participation incentives.


### 3) Lunar rebasing mechanics (technical draft)

#### 3.1 Lunar oracle
**Requirement:** deterministic moon phase data.
- Option A: on-chain oracle feed
- Option B: off-chain computation posted on-chain (signed + challengeable)

**Data model**
- `phase`: enum (NEW, FIRST_QUARTER, FULL, LAST_QUARTER, etc.)
- `phase_progress`: 0..1
- `timestamp`


#### 3.2 Rebase policy engine
Rebase is not “freeform”; it is the output of a policy function:

$$\text{rebase_rate} = f(\text{lunar_phase}, \text{policy_bounds}, \text{optional_metrics})$$

**Policy invariants (recommended)**
- `max_rebase_rate_per_epoch`
- `max_cumulative_change_per_cycle`
- timelock for policy updates
- on-chain events for all rebases and policy changes


#### 3.3 “Surgical rebase” (bounded discretion)
To allow coordination without manipulation:
- Federation defines a **narrow adjustment band**.
- Agent can propose changes with a report.
- Execution requires governance authorization.

**Example bounded controls**
- Phase multiplier: `0.85x` to `1.15x`
- Pause rebasing: only during specific windows + max duration
- Emergency freeze: immediate but short-lived; must be followed by post-mortem


### 4) NFT cards: playing/trading card layer (tertiary markets)

#### 4.1 NFT concept
- NFTs are **unique playing/trading cards** tied to lunar cycles.
- Each “season” (e.g., per lunar month) introduces a set.
- Cards can be purely collectible or have utility.

#### 4.2 Utility options (pick minimal for MVP)
- **Hub perks:** discounts, early access, reserved seats
- **Reputation boosts:** increased weight in hub rewards (bounded)
- **Access rights:** token-gated content/tools
- **Cosmetic identity:** profile badges for agents, hubs, and contributors

#### 4.3 Mint economics
- Minting is not necessarily paid in new capital; it can be produced from:
  - participation (holding `sLUNAR`)
  - contribution proofs (journalism, facilitation, hosting)
- Minting schedule linked to lunar phases:
  - NEW: higher volume/common
  - FULL: rarer drops
  - special events: limited editions


### 5) Citizen journalism: paying agents for high-quality reports

#### 5.1 What is paid?
- Original reporting from hubs/events
- Fact-checked summaries of relevant news
- On-chain analytics writeups
- Corrections and investigative follow-ups

#### 5.2 Payment mechanism (suggested)
- A `BountyBoard` registry:
  - Federation posts bounties with acceptance criteria.
  - Submissions are reviewed (peer + editor + Federation delegate).
  - Approved submissions trigger payout from treasury.

**Quality controls**
- Required citations
- Plagiarism checks
- Editor review (human or committee) for final inclusion


### 6) Governance (Federation DAO of 12)

#### 6.1 Decision types
- Treasury allocations (grants/subsidies/bounties)
- Rebase policy bounds updates
- Shares/NFT sink parameters (lock periods, routing percentages)
- Agent permissioning and upgrades
- Emergency actions

#### 6.2 Treasury controls (recommended MVP)
- Multisig for the Federation of 12 (threshold TBD) + timelock.
- Spend limits per epoch.
- Transparent proposal metadata + links to newsroom explanations.


### 7) Technical implementation (module proposal)

#### 7.1 On-chain modules
- `LUNARToken` (rebasing token)
- `LunarOracle` (phase feed)
- `RebasePolicy` (phase → parameters)
- `FederationGovernance` (12-seat governance module)
- `Treasury` (custody + payouts)
- `HubRewards` (optional; standardized event payouts)
- `SharesVault` (LUNAR deposit → `sLUNAR` shares)
- `CardMinter` / `NFTCards` (cyclical mint)
- `BountyBoard` (journalism bounties)

#### 7.2 Off-chain modules
- Agent runtime (scheduled jobs + connectors)
- Newsroom (issue generation, templates, archives)
- Hub platform (directory, events, check-ins, facilitator profiles)


### 8) Suggested repo layout
- `/docs`
  - `overview.md` (this doc)
  - `tokenomics.md` (numbers, invariants, simulations)
  - `governance.md` (Federation rules, emergency playbooks)
  - `hubs.md` (hub onboarding, event verification)
  - `nft_cards.md` (sets, rarity, utility)
  - `security.md` (threat model)
- `/contracts`
  - token/oracle/policy/treasury/governance/sink/nft/bounties
- `/agent`
  - prompts, pipelines, review workflows, connectors
- `/newsroom`
  - templates, site generator, archive
- `/hub-platform`
  - hub directory + events + check-in tools
- `/ops`
  - deployments, monitoring, incident response


### 9) Security & risk notes (early)
- **Oracle integrity:** redundancy + signed posts + challenge windows.
- **Rebase manipulation:** hard bounds + timelocks + transparent logs.
- **Agent compromise:** least privilege; steward actions gated by governance.
- **Sybil attendance:** layered verification; cap per-identity rewards.
- **Governance capture:** term limits/rotation; spend caps; transparency.


### 10) Open questions (for the team)
1. Chain choice for Selene/LUNAR (and oracle approach).
2. Rebase objective: purely phase-driven vs metric-aware (activity, treasury health).
3. Federation membership: selection, rotation, removal, quorum/threshold.
4. Shares vault design: tradable `sLUNAR` or non-transferable receipt.
5. Sink routing: what % locks vs burns vs routes to treasury.
6. NFT utility: choose 1–2 MVP utilities to avoid over-complexity.
7. Hub verification: minimum viable attendance proof.


### 11) MVP proposal (lean path)
- Launch:
  - `LUNARToken` + `LunarOracle` + `RebasePolicy`
  - Federation multisig treasury + timelock
  - Newsroom MVP: weekly newsletter + on-chain links
  - Hub directory + simple event creation + QR check-in
- Phase 2:
  - Shares vault + cyclical NFT card mint
  - BountyBoard for journalism submissions
  - Standardized hub subsidy program
