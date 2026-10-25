# Clearparty plugin for Claude Code

Clearparty checks a company before your agent pays, invoices or signs a contract with it. It resolves the legal entity in GLEIF, walks its parent chain, screens every company in the chain against the OFAC, EU, UN and UK sanctions lists, and validates EU VAT numbers in VIES.

This plugin adds two things to Claude Code:

- the Clearparty MCP server, with the tools `check_counterparty`, `validate_vat` and `screen_sanctions`;
- the `clearparty-counterparty-check` skill, which tells the agent when to run a check and how to act on the result.

## Install

You need Claude Code 2.1.292 or later and a GitHub account.

```bash
claude plugin install clearparty --marketplace clearparty/clearparty-plugin
```

## Sign in

The first tool call opens a browser: Sign in with GitHub and allow Claude Code. You can also run `/mcp` in Claude Code and choose Authenticate. There is no API key to copy. See and revoke connected apps at https://app.clearparty.dev/account/apps.

Updating from 1.0.0: the stored API key is no longer used, so sign in once after the update. To keep using a key instead, add the server with `claude mcp add` and an `Authorization` header (see https://app.clearparty.dev/connect).

## What is sent, and where

When the agent calls a tool, the plugin sends the tool inputs (a company name, LEI, VAT number or registry number) and your sign-in token to `https://mcp.clearparty.dev/mcp`, and nowhere else. Clearparty's server then checks VAT numbers in the EU's VIES service, and may send the company name and candidate matches to its language-model provider (Anthropic) to resolve an ambiguous match or, when you ask for an AI summary, to write it. The plugin itself contains no code that runs on your machine.

## Other clients

Cursor, VS Code, `claude mcp add` and any Streamable HTTP client: see https://app.clearparty.dev/connect.

## Terms

Clearparty provides data lookup, not compliance decisions. See the terms at https://app.clearparty.dev/terms.

This repository is generated from Clearparty's main repository. Please open issues here; pull requests cannot be merged.
