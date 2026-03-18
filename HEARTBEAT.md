# Founder Check Runtime

## Goal

Produce a fast, evidence-based credibility read on a token's founders or team without pretending certainty where public data is thin.

## Workflow

### 1. Normalize The Input

Detect whether the input contains:

- A contract address
- One or more X handles
- A project handle
- A chain hint
- A mode hint such as `fast` or `deep`

If the user provides only a contract address, attempt social resolution first. If the user already provides handles, do not block on resolution.

### 2. Resolve Official Socials

For `CA` input:

1. Query DexScreener or equivalent token metadata source for `x`, `telegram`, `website`, and related links.
2. If X handles exist there, treat them as candidate official accounts, not guaranteed founders.
3. If socials are missing, inspect the official website, docs, link hub, Telegram pinned posts, and launch thread.
4. If no credible handle can be resolved, stop early with a limited result and ask for manual handles.

Mark each resolved handle with provenance:

- `contract metadata`
- `official website`
- `manual input`
- `other public source`

### 3. Build The Subject List

Create a list of accounts to inspect:

- Founder handles
- Project handle
- At most 3 related accounts if they materially help attribution

Do not bloat the report with every community account.

### 4. Research Each Founder

For each founder handle, inspect:

- Bio and stated role
- Apparent account age and continuity
- Prior project references
- Posting cadence
- Evidence of building, shipping, or researching
- Signs of recycled identity or abrupt narrative pivots

Prefer recent evidence plus one or two older signals proving history.

### 5. Map Meaningful Network Signals

Prioritize interactions that are difficult to fake:

- Credible follows
- Replies with substance
- Quote tweets with commentary
- Reposts from accounts that matter in the same ecosystem
- Appearances in Spaces, podcasts, or launch threads with known operators

For each signal, answer:

1. Who is the source account?
2. Why does that source account matter?
3. Is the interaction meaningful or just noise?
4. Is it relevant to this project, or only to an older context?

### 6. Extract Key Tweets

Capture only tweets that materially change the trader's understanding:

- The main launch or announcement tweet
- A builder progress update
- A major repost, quote, or reply from a credible third party
- A tweet that addresses a risk, controversy, or exploit

Avoid cluttering the result with generic hype posts.

### 7. Score And Classify

Use the scoring model in `SOUL.md`. Report:

- Raw score
- Confidence level: `high`, `medium`, or `low`
- Clear reasons for the verdict

Confidence should fall when:

- Social resolution is uncertain
- X access is incomplete
- Most evidence comes from the project itself
- The signal set is too new or too thin

### 8. Return The Report

Optimize for fast trade decisions:

- Put the verdict first
- Make evidence scannable
- Keep unknowns explicit
- Separate strong positives from speculation

## Source Priority

Use sources in this order:

1. Official X accounts and original posts
2. Official website, docs, GitHub, Mirror, and launch materials
3. DexScreener or other token metadata aggregators
4. Credible third-party accounts with direct interaction
5. Secondary summaries only when primary sources are inaccessible

## Search Patterns

Use focused searches such as:

- `"<project>" site:x.com`
- `"<handle>" site:x.com`
- `"<handle>" founder OR builder OR launched`
- `"<project>" quote tweet`
- `"<project>" repost`
- `"<project>" founder interview`

Adapt terms to the chain or niche, for example `solana`, `base`, `ai agent`, `launchpad`, or `memecoin`.

## Evidence Hygiene

- Do not claim a follow if it cannot be verified.
- Do not infer founder status from a casual mention.
- Treat engagement-farm accounts as low quality.
- A large account repost without commentary is weaker than a thoughtful reply.
- If the same signal repeats across many screenshots or mirrors, count it once.

## Failure Modes

Return a degraded report instead of hallucinating when:

- No founder handles can be tied to the project
- The project social graph is mostly anonymous noise
- X content is unavailable or rate-limited

In degraded mode, say what was checked, what failed, and what manual input would unblock the next pass.
