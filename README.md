# ParseRail for Gemini CLI

This [Gemini CLI](https://geminicli.com) extension routes financial-document work through [ParseRail](https://parserail.thecompound.tech):

- **Financial-document extraction** — invoices, receipts, bank/card statements, and embedded tables → schema-validated JSON (vendor, line items, totals, normalized transactions)
- **PII redaction** — strip names, emails, phones, SSNs, and card numbers from text before you store or log it
- **Contract review** — parties, term, renewal, obligations, and flagged risk clauses
- Plus general parsing, field extraction, classification, document comparison/splitting, and ~30 more tools from the same API

The extension uses the [`parserail-mcp`](https://www.npmjs.com/package/parserail-mcp) MCP server.

## Install

```bash
gemini extensions install https://github.com/kyisaiah47/parserail-gemini-extension
```

You'll be prompted for your ParseRail API key during install.

## Get a key

You sign up at **[parserail.thecompound.tech](https://parserail.thecompound.tech)** and buy credits through a $20 pack or a plan from $19/mo. ParseRail has no free tier. ParseRail keys look like `ksk_live_…`.

Billing is pay-per-call from a credit wallet, and **you're only charged when a call succeeds** — errors cost nothing.

## Use it

Just ask Gemini to work on a document:

```
> extract the line items from ./invoices/acme-march.pdf
> redact the PII from this support transcript before I paste it into the ticket
> review this contract and flag anything risky about renewal terms
```

Gemini routes each request through the matching ParseRail tool (`parserail_invoice`, `parserail_redact`, `parserail_contract`, …) and returns structured JSON. The bundled `GEMINI.md` tells the model which tool fits each document.

## Requirements

- Node.js 18+ (the MCP server runs via `npx`)
- A ParseRail API key (`PARSERAIL_API_KEY`)

## License

MIT
