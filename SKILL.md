---
name: founder-check
description: Analyze the credibility of crypto project founders and teams from a contract address or X handles, but only when the agent has live Twitter/X access through either the X API or Apify. Use this skill when a trader needs founder intelligence backed by direct X account data before trading a newly launched token.
---

# Founder Check

Founder Check is a team-intelligence skill for web3 trading workflows. It complements contract and rug-checking tools by evaluating whether the people behind a token look credible, connected, and consistent in public.

This skill requires live X access. The agent must use one of these sources:

- X API
- Apify

If neither source is available, the agent must stop and refuse to produce a Founder Check report.

## Use This Skill When

- The user wants to assess a token team's legitimacy before trading.
- The input is a contract address and the agent must resolve official socials first.
- The user already has one or more founder X handles and wants a fast credibility check.
- The goal is to find meaningful signals such as respected builders following, replying, quoting, or reposting the founders or project account.
- The agent has access to either the X API or Apify.

## Inputs

Founder Check accepts any of these:

- A contract address
- One or more X handles
- A contract address plus one or more X handles
- Optional project handle, chain, or urgency mode

## Hard Requirement

Founder Check should only run when at least one live X data source is available:

- X API
- Apify

If both are unavailable, unauthenticated, rate-limited, or otherwise unable to fetch the relevant accounts and posts, the skill must return no verdict and explicitly ask the user to connect or fix one of those integrations first.

## Operating Files

- Product identity, scoring model, and report schema: `SOUL.md`
- Runtime workflow, evidence rules, and fallback logic: `HEARTBEAT.md`
- Invocation patterns and operating modes: `AGENTS.md`
- User-facing positioning copy: `README.md`

Read `SOUL.md` and `HEARTBEAT.md` before running the skill.
