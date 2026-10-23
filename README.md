# Clearparty plugin for Claude Code

Clearparty checks a company before your agent pays, invoices or signs a contract with it. It resolves the legal entity in GLEIF, walks its parent chain, screens every company in the chain against the OFAC, EU, UN and UK sanctions lists, and validates EU VAT numbers in VIES.

This plugin adds two things to Claude Code:

- the Clearparty MCP server, with the tools `check_counterparty`, `validate_vat` and `screen_sanctions`;
- the `clearparty-counterparty-check` skill, which tells the agent when to run a check and how to act on the result.

## Install

You need Claude Code 2.1.292 or later and a Clearparty API key.

```bash
claude plugin install clearparty --marketplace clearparty/clearparty-plugin
```

Claude Code asks for your API key during install and stores it as a sensitive setting.

## Get a key

Sign in at https://app.clearparty.dev/account/keys and create a key. It starts with `cp_live_` and is shown once.

## What is sent, and where

When the agent calls a tool, the plugin sends the tool inputs (a company name, LEI, VAT number or registry number) and your API key to `https://mcp.clearparty.dev/mcp`, and nowhere else. Clearparty's server then checks VAT numbers in the EU's VIES service, and may send the company name and candidate matches to its language-model provider (Anthropic) to resolve an ambiguous match or, when you ask for an AI summary, to write it. The plugin itself contains no code that runs on your machine.

## Other clients

Cursor, VS Code, `claude mcp add` and any Streamable HTTP client: see https://app.clearparty.dev/connect.

## Terms

Clearparty provides data lookup, not compliance decisions. See the terms at https://app.clearparty.dev/terms.

This repository is generated from Clearparty's main repository. Please open issues here; pull requests cannot be merged.
