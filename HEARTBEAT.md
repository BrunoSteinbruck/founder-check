# Founder Check Runtime

## Goal

Produce a fast, evidence-based credibility read on a token's founders or team without pretending certainty where public data is thin.

Scored founder reads are only valid when backed by live X data from one of the supported collection paths:

- X API
- Apify

When a supported X source is present but degraded, the skill may return a non-scored `PARTIAL`, `UNRESOLVED`, or `BLOCKED` report. These reports may summarize verified project identity and collection failures, but must not score founder credibility.

## Workflow

### 1. Confirm Data Access

Before anything else, verify that at least one supported X data source is available:

- X API
- Apify

#### Apify Actor Priority

- `xtdata/twitter-x-scraper` (primary)
- `apify/twitter-scraper` (first fallback)
- `quacker/twitter-scraper` (second fallback)

Do not use web browser collectors for X. They are commonly blocked or degraded. Use only dedicated X/Twitter Actors.

If both are unavailable, stop immediately. Do not continue with social inference from web search alone.

If both are available, prefer:

1. `x-api`
2. `apify`

Record the source selected for this run.

#### Actor And Endpoint Health Check

Before relying on a source, record which collection modes are available:

- profile/account metadata
- timeline/posts
- search by contract, project name, or handle
- tweet detail for specific URLs
- followers/following when needed for network quality

For Apify, use dedicated X/Twitter Actors only. If an actor returns empty data for the target, test whether another allowed actor or mode can fetch the same target before declaring the source broken. If a mode fails, record the failure reason and continue only with claims supported by successful modes.

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

1. Step A: query DexScreener for token socials and links.
2. If Step A does not return a usable X handle, Step B: query Jupiter for token socials and links.
3. If Step B does not return a usable X handle, Step C: search X directly for the contract address string.
4. Stop at the first step that returns a usable handle.
5. Do not continue into websites, docs, or general web research after the handle is resolved. Those come later as supporting evidence, not primary resolution.
6. If no usable handle can be resolved after Steps A through C, return `UNRESOLVED` or `BLOCKED` depending on whether X access worked, and ask for manual founder or project handles.

Mark each resolved handle with provenance:

- `dexscreener`
- `jupiter`
- `x-search-by-ca`
- `manual input`

If the resolved handle appears to be a project or org account rather than an individual, do not stop there. Inspect its repost history to identify which individual accounts it amplifies consistently. Treat those individual accounts as founder candidates and add them to the subject list before proceeding.

If the project handle is resolved from official token metadata but X collection fails for that handle, do not discard the project identity evidence. Mark project identity as verified or partial based on official surfaces, mark founder identity as unknown, and return `PARTIAL` or `BLOCKED` without a founder score.

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

Secondary sources, X mirrors, search snippets, and third-party summaries may be used to add candidate accounts to the subject list, but only with `candidate` provenance. They may not confirm founder status or contribute to a scored founder verdict unless direct X data later verifies them.

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

Only score when direct X evidence is sufficient to evaluate founder or team credibility. If X collection is partial or broken:

- Use `PARTIAL` when project identity is verified but founder identity remains unverified.
- Use `UNRESOLVED` when founder candidates exist but cannot be verified.
- Use `BLOCKED` when the X source, credentials, actor, rate limit, or target access prevents meaningful collection.
- Set `Founder Score: N/A`.
- Include data source health and what would upgrade the report.

### 9. Return The Report

Optimize for fast trade decisions:

- Put the verdict first
- State the data source used: `x-api` or `apify`
- Make evidence scannable
- Keep unknowns explicit
- Separate strong positives from speculation
- Separate verified project identity from verified founder identity

## Source Priority

Use sources in this order:

1. Selected X data source: X API or Apify
2. Official website, docs, GitHub, and launch materials
3. DexScreener or other token metadata aggregators
4. Credible third-party accounts with direct interaction
5. Secondary summaries and X mirrors only for candidate discovery or context; never as replacement for direct X evidence

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
- Web search alone is not enough for a scored Founder Check.
- Do not merge project identity confidence into founder identity confidence.
- Use evidence labels: `observed`, `inferred`, `candidate`, and `unknown`.

## Failure Modes

Stop and return no Founder Check report when:

- No supported X data source is available

Return a non-scored Founder Check report when:

- The chosen X data source is authenticated but partially broken
- X content cannot be fetched for some or all relevant accounts, but project identity can still be verified from official surfaces
- No founder handles can be tied to the project after inspecting official surfaces, but the token/project identity is verified
- A supplied founder handle has no verifiable public presence and cannot be distinguished from a fabricated or irrelevant account

In failure or degraded mode, say what was checked, which X access path failed, which collection modes succeeded, and whether the user should connect the X API, try a different Apify actor, or provide stronger founder identifiers.
