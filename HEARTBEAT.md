# Founder Check Runtime

## Goal

Produce a fast, evidence-based credibility read on a token's founders or team without pretending certainty where public data is thin.

The read is only valid when it is backed by live X data from one of the supported collection paths:

- X API
- Apify

## Workflow

### 1. Confirm Data Access

Before anything else, verify that at least one supported X data source is available:

- X API
- Apify

Preferred Apify Actor for X data:

- `apify/twitter-scraper` (primary)
- `quacker/twitter-scraper` (fallback)

Do not use generic web browser collectors for X. They are commonly blocked or degraded. Use only dedicated X/Twitter Actors.

If both are unavailable, stop immediately. Do not continue with social inference from web search alone.

If both are available, prefer:

1. `x-api`
2. `apify`

Record the source selected for this run.

### 2. Normalize The Input

Detect whether the input contains:

- A contract address
- One or more X handles
- A project handle
- A chain hint
- A mode hint such as `fast` or `deep`

If the user provides only a contract address, attempt social resolution first. If the user already provides handles, do not block on resolution.

### 3. Resolve Official Socials

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

If the resolved handle appears to be a project or org account rather than an individual, do not stop there. Inspect its repost history to identify which individual accounts it amplifies consistently. Treat those individual accounts as founder candidates and add them to the subject list before proceeding.

### 4. Build The Subject List

Create a list of accounts to inspect:

- Founder handles
- Project handle
- At most 3 related accounts if they materially help attribution

Do not bloat the report with every community account.

Distinguish clearly between:

- `project handle`: the org, brand, or token account
- `founder handle`: the individual behind the project

Never treat a project handle as a founder handle without explicit confirmation.

If only a project handle is resolved, inspect its repost history, replies, and repeated amplifications to identify which individual accounts it consistently boosts. Treat those as founder candidates, not confirmed founders, until stronger evidence exists.

### 5. Research Each Founder

For each founder handle, inspect with the selected X data source:

- Bio and stated role
- Apparent account age and continuity
- Prior project references
- Posting cadence
- Evidence of building, shipping, or researching
- Signs of recycled identity or abrupt narrative pivots

Prefer recent evidence plus one or two older signals proving history.

### 6. Map Meaningful Network Signals

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

### 7. Extract Key Tweets

Capture only tweets that materially change the trader's understanding:

- The main launch or announcement tweet
- A builder progress update
- A major repost, quote, or reply from a credible third party
- A tweet that addresses a risk, controversy, or exploit

Avoid cluttering the result with generic hype posts.

### 8. Score And Classify

Use the scoring model in `SOUL.md`. Report:

- Raw score
- Confidence level: `high`, `medium`, or `low`
- Clear reasons for the verdict

Confidence should fall when:

- Social resolution is uncertain
- The selected X data source is incomplete
- Most evidence comes from the project itself
- The signal set is too new or too thin

If the X data source fails before the analysis is complete, abort the run and return no verdict.

### 9. Return The Report

Optimize for fast trade decisions:

- Put the verdict first
- State the data source used: `x-api` or `apify`
- Make evidence scannable
- Keep unknowns explicit
- Separate strong positives from speculation

## Source Priority

Use sources in this order:

1. Selected X data source: X API or Apify
2. Official website, docs, GitHub, Mirror, and launch materials
3. DexScreener or other token metadata aggregators
4. Credible third-party accounts with direct interaction
5. Secondary summaries only when they support primary X evidence rather than replace it

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
- Web search alone is not enough for a valid Founder Check.

## Failure Modes

Stop and return no Founder Check verdict when:

- No supported X data source is available
- The chosen X data source is unauthenticated, rate-limited, or broken
- X content cannot be fetched for the relevant accounts
- No founder handles can be tied to the project after inspecting official surfaces
- A supplied founder handle has no verifiable public presence and cannot be distinguished from a fabricated or irrelevant account

In failure mode, say what was checked, which X access path failed, and whether the user should connect the X API or Apify.
