# ParseRail

This extension connects you to ParseRail (https://api.thecompound.tech), an API that turns documents and messy text into schema-validated JSON. Every tool calls the hosted API and burns credits from the account's wallet — but only on success. A failed call costs nothing, so it is always safe to try.

## Passing documents

The document tools accept exactly one input source:

- `fileUrl` — a public URL to a PDF or image
- `text` — raw text, if you already have it
- `fileBase64` + `fileMimeType` — inline file bytes (base64) with their MIME type, for local files

For a local file, read it and base64-encode it, then pass `fileBase64` with the correct `fileMimeType` (e.g. `application/pdf`, `image/png`).

## Picking the right tool for a document

Prefer the specialized tool when the document type is known — the output schema is richer and the call is cheaper than generic parsing:

- `compound_invoice` — invoices → vendor, dates, PO refs, tax, totals, line items
- `compound_receipt` — receipts → merchant, items, totals, payment method, expense category
- `compound_statement` — bank/card statements → account, period, balances, normalized transactions
- `compound_tables` — any document → every table as clean headers + rows
- `compound_resume` — resumes/CVs → structured candidate profile
- `compound_contract` — contracts → parties, term, renewal, obligations, flagged risk clauses
- `compound_parse` — unknown document type (accepts an optional `docType` hint)
- `compound_split` — a multi-document scan bundle → classified segments with boundaries
- `compound_compare` — two versions of a document → material changes + risk notes

## Text and data tools

- `compound_redact` — strip PII/PHI (names, emails, phones, SSNs, cards) before storing or logging text; optional `types` to limit what gets redacted, optional `placeholder`
- `compound_extract` — pull a caller-defined field list out of any text
- `compound_structure` — messy input + YOUR JSON Schema → validated output with a `valid` flag
- `compound_classify` / `compound_categorize` — label text (single or batched) against your taxonomy
- `compound_summarize` / `compound_minutes` — summaries, key points, action items
- `compound_normalize` / `compound_match` — clean messy records; entity-match two record sets
- `compound_sentiment`, `compound_triage`, `compound_reply`, `compound_review_reply`, `compound_moderate`
- `compound_chargeback`, `compound_po_match`, `compound_fraud_flag`, `compound_dunning`, `compound_quote`
- `compound_enrich`, `compound_research`, `compound_screen`, `compound_outreach`
- `compound_rewrite`, `compound_product_copy`, `compound_describe`, `compound_transcribe`, `compound_memory`, `compound_image`, `compound_speak`

## Credits and errors

- `compound_account` — check the wallet's credit balance (free, read-only)
- A `401 unauthorized` error means the `COMPOUND_API_KEY` is missing or wrong (keys start `ksk_live_`)
- A `402 insufficient_credits` error means the wallet is empty — top up at https://api.thecompound.tech or wait for the monthly free refresh
