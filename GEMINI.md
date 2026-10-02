# ParseRail

This extension connects you to ParseRail (https://parserail.thecompound.tech), an API that turns documents and messy text into schema-validated JSON. Every tool calls the hosted API and burns credits from the account's wallet — but only on success. A failed call costs nothing, so it is always safe to try.

## Passing documents

The document tools accept exactly one input source:

- `fileUrl` — a public URL to a PDF or image
- `text` — raw text, if you already have it
- `fileBase64` + `fileMimeType` — inline file bytes (base64) with their MIME type, for local files

For a local file, read it and base64-encode it, then pass `fileBase64` with the correct `fileMimeType` (e.g. `application/pdf`, `image/png`).

## Picking the right tool for a document

Prefer the specialized tool when the document type is known — the output schema is richer and the call is cheaper than generic parsing:

- `parserail_invoice` — invoices → vendor, dates, PO refs, tax, totals, line items
- `parserail_receipt` — receipts → merchant, items, totals, payment method, expense category
- `parserail_statement` — bank/card statements → account, period, balances, normalized transactions
- `parserail_tables` — any document → every table as clean headers + rows
- `parserail_resume` — resumes/CVs → structured candidate profile
- `parserail_contract` — contracts → parties, term, renewal, obligations, flagged risk clauses
- `parserail_parse` — unknown document type (accepts an optional `docType` hint)
- `parserail_split` — a multi-document scan bundle → classified segments with boundaries
- `parserail_compare` — two versions of a document → material changes + risk notes

## Text and data tools

- `parserail_redact` — strip PII/PHI (names, emails, phones, SSNs, cards) before storing or logging text; optional `types` to limit what gets redacted, optional `placeholder`
- `parserail_extract` — pull a caller-defined field list out of any text
- `parserail_structure` — messy input + YOUR JSON Schema → validated output with a `valid` flag
- `parserail_classify` / `parserail_categorize` — label text (single or batched) against your taxonomy
- `parserail_summarize` / `parserail_minutes` — summaries, key points, action items
- `parserail_normalize` / `parserail_match` — clean messy records; entity-match two record sets
- `parserail_sentiment`, `parserail_triage`, `parserail_reply`, `parserail_review_reply`, `parserail_moderate`
- `parserail_chargeback`, `parserail_po_match`, `parserail_fraud_flag`, `parserail_dunning`, `parserail_quote`
- `parserail_enrich`, `parserail_research`, `parserail_screen`, `parserail_outreach`
- `parserail_rewrite`, `parserail_product_copy`, `parserail_describe`, `parserail_transcribe`, `parserail_memory`, `parserail_image`, `parserail_speak`

## Credits and errors

- `parserail_account` — check the wallet's credit balance (free, read-only)
- A `401 unauthorized` error means the `PARSERAIL_API_KEY` is missing or wrong (keys start `ksk_live_`)
- A `402 insufficient_credits` error means the wallet is empty. Buy a credit pack or a plan at https://parserail.thecompound.tech. There is no free tier and no monthly refresh.
