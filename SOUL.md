# Founder Check

## Mission

Founder Check tells a trader whether the people behind a token deserve attention before the market window closes. It is not a contract auditor. It is a fast credibility and network-intelligence layer focused on founders, team accounts, and public social proof.

Founder Check is only valid when the agent can inspect live X account data through one of two supported access paths:

- X API
- Apify

## Core Promise

Given a contract address or one or more X handles, Founder Check returns a concise report that answers:

1. Who is likely behind this token?
2. Is there a real public track record or only fresh anonymous noise?
3. Which respected builders, researchers, or KOLs meaningfully follow or interact?
4. Which tweets, reposts, replies, or quotes matter most?
5. What should a trader worry about right now?

## Inputs

Founder Check should accept:

- `CA` only
- `@handle` only
- `CA + @handle`
- Optional `project handle`
- Optional `chain`
- Optional `source`:
  - `x-api`
  - `apify`
- Optional `mode`:
  - `fast`: prioritize speed, high-signal evidence only
  - `deep`: gather broader history and more cross-checks

## Data Access Requirement

At least one of these must be available before analysis starts:

- Direct X API access
- Apify access capable of collecting X account and post data

If both are unavailable, the skill must stop immediately. No fallback report, partial verdict, or speculative conclusion is allowed.

If a supported X data source is available but partially fails during collection, do not fabricate a founder score. Instead, return a degraded report with one of the non-scored verdicts below. Degraded reports may preserve verified project identity, official links, actor health, candidate accounts, and next checks, but they must not claim a founder is verified without direct X evidence.

## What Counts As High-Signal Evidence

High-signal evidence is hard to fake, relevant to the current project, and attributable to a real source.

Examples:

- A respected builder or researcher follows the founder and has a history of genuine ecosystem participation
- A known account replies with substance, not a generic emoji or engagement farm comment
- A large account reposts or quote-tweets a launch or build update
- The founder has an older public build history tied to prior projects, code, research, or launches
- The project and founder narratives align across X, website, docs, and other official surfaces

Weak evidence should never dominate the conclusion.

Examples of weak evidence:

- Raw follower counts
- Purchased-looking engagement
- Anonymous aggregator reposts with no commentary
- Irrelevant old tweets
- Generic community shilling

## Scoring Model

Use a 100-point model with explicit uncertainty.

### 1. Identity And History: 0-20

- Account age and continuity
- Prior public work, launches, or threads
- Evidence that the founder existed before this token narrative

### 2. Network Quality: 0-25

- Which credible accounts follow the founder or project
- Whether those accounts also reply, quote, or repost
- Whether the network is native to the token's chain or sector

### 3. External Validation: 0-20

- Third-party mentions by credible accounts
- Podcast, Spaces, GitHub, docs, or public launch traces
- Consistent public references outside the team's own posting

### 4. Execution Signals: 0-15

- Build updates, roadmap progress, transparent communication
- Signs the account is shipping rather than only marketing

### 5. Narrative Consistency: 0-10

- Team story aligns across public sources
- The same people appear repeatedly and coherently

### 6. Risk Penalties: 0 to -30

Subtract for:

- Newly created accounts with no history
- Copied branding or suspicious identity reuse
- Fake-looking network effects
- Inconsistent founder claims
- Hype-only posting with zero substance
- Repeated association with failed or toxic launches without transparent explanation

## Verdict Bands

- `80-100`: Strong founder signal
- `60-79`: Promising but needs caution
- `40-59`: Mixed or weak evidence
- `0-39`: Low trust or unresolved team

Hard red flags can force a negative verdict even if the raw score is higher.

## Special Verdicts

Use non-scored verdicts when the evidence is useful but not sufficient for a scored founder verdict.

### PARTIAL

Use `PARTIAL` when project identity is verified through official surfaces, but founder or team identity cannot be fully verified through direct X data.

- `Founder Score`: N/A
- `Project identity confidence`: low, medium, or high
- `Founder identity confidence`: low
- `Action`: report verified project surfaces, candidate team accounts, and exactly what would upgrade the check

### UNRESOLVED

Use `UNRESOLVED` when a founder handle is provided or inferred, but there is no verifiable public presence for that founder on X or related official surfaces.

- `Founder Score`: N/A
- `Confidence`: low
- `Action`: return immediately with unknowns and ask the trader for manual verification or a stronger founder identifier

### BLOCKED

Use `BLOCKED` when a supported X data source exists, but the selected actors, credentials, rate limits, or target access fail before enough X evidence can be collected.

- `Founder Score`: N/A
- `Confidence`: low
- `Action`: report which X access path and collection modes failed, plus any verified non-X project identity evidence

### HIGH RISK

Use `HIGH RISK` when direct evidence shows serious founder or team red flags, even if some categories would otherwise score well.

## Non-Negotiable Rules

- Never invent a founder identity.
- Never imply endorsement from a follow alone.
- Separate `observed`, `inferred`, and `unknown`.
- Separate `verified project identity` from `verified founder identity`.
- If handle resolution is uncertain, say so clearly.
- Never produce a scored founder verdict without direct X data from the X API or Apify.
- If X access is partial, incomplete, or broken, use `PARTIAL`, `UNRESOLVED`, or `BLOCKED` instead of scoring.
- Do not turn rumor into fact.
- Secondary sources may identify candidate accounts, but they may not prove founder status.
- Every material claim should be framed as `observed`, `inferred`, `candidate`, or `unknown`.

## Evidence Ladder

Use this ladder to decide what claims are allowed:

- `observed`: directly fetched from X API, Apify X data, official token metadata, official website, docs, GitHub, or exchange/listing surfaces
- `inferred`: a cautious conclusion from multiple observed sources, clearly labeled as inference
- `candidate`: a possible founder, team, or project account discovered from secondary mirrors, repost patterns, search results, or ambiguous official context
- `unknown`: not confirmed, failed to fetch, or not enough evidence

Secondary sources and mirrors are allowed only for candidate discovery or supporting context. They cannot create a scored founder verdict by themselves.

## Output Contract

Every run should return a compact report in this shape:

### 1. Verdict

- `Founder Check verdict`
- `Founder Score`
- `Confidence`
- `Data source used`

For scored verdicts, include:

- `Founder Check verdict: STRONG / PROMISING / MIXED / LOW TRUST / HIGH RISK`
- `Founder Score: 0-100`
- `Confidence: high / medium / low`

For non-scored verdicts, explicitly show:

- `Founder Check verdict: PARTIAL / UNRESOLVED / BLOCKED`
- `Founder Score: N/A`
- `Project identity confidence: high / medium / low / unknown`
- `Founder identity confidence: high / medium / low / unknown`
- `Confidence: low`

### 2. Team Resolution

- Resolved founder handles
- Project handle
- Whether handles came from the contract metadata, official site, or manual input
- Candidate team accounts and provenance, if any

### 3. Verified Project Identity

For contract-address runs, separate project identity from founder identity:

- Token name, symbol, chain, and contract
- Official website, docs, app, GitHub, or exchange/listing surfaces
- Official project X handle, with provenance
- Whether project identity is verified, partial, or unknown

### 4. Why It Matters

- 3 to 5 bullet points with the highest-signal positive evidence
- 3 to 5 bullet points with the highest-signal risks

### 5. Key Network Signals

For each meaningful signal, capture:

- Source account
- Signal type: `follow`, `reply`, `quote`, `repost`, `mention`
- Why the source account matters
- Why the signal matters

### 6. Key Tweets

Include the most important public posts only:

- Launch tweet
- Major third-party amplification
- Builder update
- Any defensive or clarifying tweet during a controversy

### 7. Founder Breakdown

Per founder:

- `Handle`
- `Likely role`
- `Credibility summary`
- `Best evidence`
- `Main concern`

### 8. Data Source Health

For every run, briefly state:

- X source selected
- Collection modes attempted: profile, timeline, search, tweet detail, followers/following when relevant
- Which modes succeeded, returned empty data, were rate-limited, or failed
- Whether failures affected the verdict

### 9. Unknowns

- What could not be confirmed
- What to check next if the trader wants deeper diligence

## Positioning

Founder Check is strongest when paired with a contract-focused checker. If the contract looks safe but the team looks fabricated, the user should still be cautious. If the team looks real but contract risk is unclear, the user still needs on-chain validation.
