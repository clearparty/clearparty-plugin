---
name: clearparty-counterparty-check
description: "Use Clearparty to verify a company before money or a contract changes hands: before paying a new payee, onboarding a supplier or customer, signing a contract, or when a payee's bank details change. Do not use for a repeat payment to a payee already checked within the last 30 days with unchanged details, even when the request just says to pay them again."
---

# Clearparty counterparty check

Clearparty is a data lookup, not a compliance decision. It tells you what public registries and sanctions lists say about a company.

## When to check

- First payment to a new payee.
- Onboarding a supplier or customer.
- Signing a contract.
- Any change of a payee's bank details.

Skip it for a repeat payment to a payee already checked within the last 30 days, unless the payee's details changed.

## Tools

- `check_counterparty`: the default. Takes a name (with a country hint), LEI, VAT number or registry ID, and returns identity, ownership chain, sanctions screening, VAT status and flags.
- `validate_vat`: only to confirm one EU VAT number.
- `screen_sanctions`: only to screen a bare name against sanctions lists.

Prefer `check_counterparty`, except when the user only asks whether a name is sanctioned and gives no country or ID: then use `screen_sanctions`.

Pass one identifier per company, in one call: the LEI if you have it, else the VAT number, else the registry ID with its country, else the name. Do not call twice for the same company, unless the user gives a new identifier (for example the LEI or VAT number you asked for after a `LOW_CONFIDENCE_MATCH` or `NO_MATCH`): then check again with it.

Answer follow-up questions from the result you already have. Do not call another tool just to elaborate on it.

## Acting on flags

- `SANCTIONS_HIT_SUBJECT` or `SANCTIONS_HIT_PARENT`: stop. Do not pay or sign. Quote the hit and ask a human.
- `LOW_CONFIDENCE_MATCH` or `NO_MATCH`: do not guess. Ask the user for the company's LEI or VAT number, then check again.
- `CHAIN_UNREPORTED`, `CHAIN_TRUNCATED` or `NO_LEI`: tell the user the ownership chain could not be fully screened.
- `DATA_STALE` or `VAT_UNCHECKED`: mention it and do not treat the result as complete.
- `LEI_LAPSED`, `ENTITY_INACTIVE` or `VAT_INVALID`: report it plainly and let the user decide.

If a tool reports the usage limit is reached, the quota applies per account to all three tools. Tell the user the limit and the reset date. Do not retry and do not switch to another tool to get around it.

## Limits to state

- The parent chain reflects accounting consolidation, which is not the same as ownership. It can be incomplete, and a clean chain does not prove nobody sanctioned owns the company.
- A result is a data lookup and not a compliance decision. Always include the disclaimer from the tool result and say a human should verify before acting.
