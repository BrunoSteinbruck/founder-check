# Founder Check

Founder Check is a web3 team-intelligence skill for traders using AI agents. It resolves founders from a contract address or accepts X handles directly, then evaluates the people behind a token through public history, network quality, and meaningful social proof.

## Required X Access

Founder Check only works with live X account access through one of these paths:

- X API
- Apify

If neither is available, the skill should stop and return no verdict.

## What It Looks For

- Founder account history and continuity
- Relevant builder or KOL follows
- Real replies, quote tweets, and reposts from accounts that matter
- Key launch or builder tweets
- Narrative consistency across X and official project surfaces

## What It Avoids

- Treating follower count as trust
- Confusing hype with credibility
- Turning rumor into fact
- Replacing a contract security check
- Pretending web search alone is enough to validate X behavior

## Example Positioning

Use Founder Check alongside a rug checker:

- Rug checker asks: `Is the contract dangerous?`
- Founder Check asks: `Do the people behind this look real and worth trusting?`

## Example Output

`Founder Check verdict: Promising but needs caution`

- `Score: 68/100`
- `Confidence: medium`
- `Data source used: x-api`
- `Resolved handles: @founder, @project`
- `Why it matters: two credible ecosystem builders replied to build updates, but founder history before this token is thin`
- `Main risk: account is new and most validation still comes from the project's own network`
