# Founder Check Agent Interface

## Primary Commands

Use any of the following patterns:

- `founder-check <CA>`
- `founder-check <CA> @founder1 @founder2`
- `founder-check @founder1 @founder2`
- `founder-check @founder1 @founder2 --project @project`
- `founder-check <CA> --chain solana`
- `founder-check <CA> --mode fast`
- `founder-check <CA> --mode deep`

## Input Rules

- Treat any `0x...` or base58-style token input as a contract address candidate.
- Treat any `@...` token as an X handle.
- If both are present, use the contract address to validate that the provided handles match official project surfaces.
- If multiple handles are supplied, assume they are founder or team handles unless clearly labeled otherwise.

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

## Bad Fit

Do not use Founder Check as the only tool when the user needs:

- Contract security analysis
- Honeypot detection
- Holder concentration analysis
- Liquidity lock validation

Those belong to a contract or rug checker.

## Response Style

- Lead with the verdict and score
- Keep the report dense and skimmable
- Use short evidence bullets
- Mark unknowns explicitly
- Avoid overstating weak signals
