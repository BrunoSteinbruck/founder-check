# Founder Check Agent Interface

## Primary Commands

Use any of the following patterns:

- `founder-check <CA>`
- `founder-check <CA> @founder1 @founder2`
- `founder-check @founder1 @founder2`
- `founder-check @founder1 @founder2 --project @project`
- `founder-check <CA> --chain solana`
- `founder-check <CA> --source x-api`
- `founder-check <CA> --source apify`
- `founder-check <CA> --mode fast`
- `founder-check <CA> --mode deep`

## Input Rules

- Treat any `0x...` or base58-style token input as a contract address candidate.
- Treat any `@...` token as an X handle.
- If both are present, use the contract address to validate that the provided handles match official project surfaces.
- If multiple handles are supplied, assume they are founder or team handles unless clearly labeled otherwise.
- Require at least one live X access path before running: `x-api` or `apify`.
- If the source is unspecified, prefer `x-api` when available and fall back to `apify`.
- If neither source is available, stop and ask the user to connect one of them.

## Operating Modes

### Fast Mode

Use when the trader needs a quick read during a live launch.

Deliver:

- One verdict
- One score
- The best positive signals
- The main risks
- The most relevant tweets or interactions

### Deep Mode

Use when the trader wants more history before sizing up.

Add:

- Older founder history
- Prior project links
- More cross-checks across docs, GitHub, podcasts, or public launch materials
- More explicit unknowns

## Good Fit

Use Founder Check when the user asks:

- "Who is behind this token?"
- "Does this team look real?"
- "Any serious accounts following or interacting with them?"
- "What are the key founder signals before I ape?"

## Access Requirement

Scored Founder Check verdicts only work when the agent can fetch live X data through:

- X API
- Apify

Without one of those, do not produce a report.

If one of those exists but collection is degraded, produce a non-scored report instead of guessing:

- `PARTIAL`: project identity is verified, founder identity is not verified
- `UNRESOLVED`: founder candidates exist but cannot be verified
- `BLOCKED`: X source or actor failed before meaningful founder collection

Always set `Founder Score: N/A` for these verdicts.

## Bad Fit

Do not use Founder Check as the only tool when the user needs:

- Contract security analysis
- Honeypot detection
- Holder concentration analysis
- Liquidity lock validation

Those belong to a contract or rug checker.

## Response Style

- Lead with the verdict and score
- State the source used: `x-api` or `apify`
- Keep the report dense and skimmable
- Use short evidence bullets
- Mark unknowns explicitly
- Avoid overstating weak signals
- Separate project identity confidence from founder identity confidence
- Include data source health when X collection is partial, empty, or broken
