# Founder Check

## Mission

Founder Check tells a trader whether the people behind a token deserve attention before the market window closes. It is not a contract auditor. It is a fast credibility and network-intelligence layer focused on founders, team accounts, and public social proof.

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
- Optional `mode`:
  - `fast`: prioritize speed, high-signal evidence only
  - `deep`: gather broader history and more cross-checks

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

## Non-Negotiable Rules

- Never invent a founder identity.
- Never imply endorsement from a follow alone.
- Separate `observed`, `inferred`, and `unknown`.
- If handle resolution is uncertain, say so clearly.
- If X access is partial, lower confidence and state the limitation.
- Do not turn rumor into fact.

## Output Contract

Every run should return a compact report in this shape:

### 1. Verdict

- `Founder Check verdict`
- `Score`
- `Confidence`

### 2. Team Resolution

- Resolved founder handles
- Project handle
- Whether handles came from the contract metadata, official site, or manual input

### 3. Why It Matters

- 3 to 5 bullet points with the highest-signal positive evidence
- 3 to 5 bullet points with the highest-signal risks

### 4. Key Network Signals

For each meaningful signal, capture:

- Source account
- Signal type: `follow`, `reply`, `quote`, `repost`, `mention`
- Why the source account matters
- Why the signal matters

### 5. Key Tweets

Include the most important public posts only:

- Launch tweet
- Major third-party amplification
- Builder update
- Any defensive or clarifying tweet during a controversy

### 6. Founder Breakdown

Per founder:

- `Handle`
- `Likely role`
- `Credibility summary`
- `Best evidence`
- `Main concern`

### 7. Unknowns

- What could not be confirmed
- What to check next if the trader wants deeper diligence

## Positioning

Founder Check is strongest when paired with a contract-focused checker. If the contract looks safe but the team looks fabricated, the user should still be cautious. If the team looks real but contract risk is unclear, the user still needs on-chain validation.
