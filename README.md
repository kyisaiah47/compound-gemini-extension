# ParseRail for Gemini CLI

A [Gemini CLI](https://geminicli.com) extension that gives Gemini native tools for financial-document work, powered by [ParseRail](https://api.thecompound.tech):

- **Financial-document extraction** — invoices, receipts, bank/card statements, and embedded tables → schema-validated JSON (vendor, line items, totals, normalized transactions)
- **PII redaction** — strip names, emails, phones, SSNs, and card numbers from text before you store or log it
- **Contract review** — parties, term, renewal, obligations, and flagged risk clauses
- Plus general parsing, field extraction, classification, document comparison/splitting, and ~30 more tools from the same API

Under the hood it wires up the [`@compound/api-mcp`](https://www.npmjs.com/package/@compound/api-mcp) MCP server (also in the official MCP registry as `studio.compound/core`).

## Install

```bash
gemini extensions install https://github.com/kyisaiah47/compound-gemini-extension
```

You'll be prompted for your Compound API key during install.

## Get a key

Sign up at **[api.thecompound.tech](https://api.thecompound.tech)** — free, no card, **500 credits every month**. Keys look like `ksk_live_…`.

Billing is pay-per-call from a credit wallet, and **you're only charged when a call succeeds** — errors cost nothing.

## Use it

Just ask Gemini to work on a document:

```
> extract the line items from ./invoices/acme-march.pdf
> redact the PII from this support transcript before I paste it into the ticket
> review this contract and flag anything risky about renewal terms
```

Gemini routes each request through the right Compound tool (`compound_invoice`, `compound_redact`, `compound_contract`, …) and returns structured JSON. The bundled `GEMINI.md` teaches the model which tool fits which document.

## Requirements

- Node.js 18+ (the MCP server runs via `npx`)
- A Compound API key (`COMPOUND_API_KEY`)

## License

MIT
