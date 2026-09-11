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

- `kynth_invoice` — invoices → vendor, dates, PO refs, tax, totals, line items
- `kynth_receipt` — receipts → merchant, items, totals, payment method, expense category
- `kynth_statement` — bank/card statements → account, period, balances, normalized transactions
- `kynth_tables` — any document → every table as clean headers + rows
- `kynth_resume` — resumes/CVs → structured candidate profile
- `kynth_contract` — contracts → parties, term, renewal, obligations, flagged risk clauses
- `kynth_parse` — unknown document type (accepts an optional `docType` hint)
- `kynth_split` — a multi-document scan bundle → classified segments with boundaries
- `kynth_compare` — two versions of a document → material changes + risk notes

## Text and data tools

- `kynth_redact` — strip PII/PHI (names, emails, phones, SSNs, cards) before storing or logging text; optional `types` to limit what gets redacted, optional `placeholder`
- `kynth_extract` — pull a caller-defined field list out of any text
- `kynth_structure` — messy input + YOUR JSON Schema → validated output with a `valid` flag
- `kynth_classify` / `kynth_categorize` — label text (single or batched) against your taxonomy
- `kynth_summarize` / `kynth_minutes` — summaries, key points, action items
- `kynth_normalize` / `kynth_match` — clean messy records; entity-match two record sets
- `kynth_sentiment`, `kynth_triage`, `kynth_reply`, `kynth_review_reply`, `kynth_moderate`
- `kynth_chargeback`, `kynth_po_match`, `kynth_fraud_flag`, `kynth_dunning`, `kynth_quote`
- `kynth_enrich`, `kynth_research`, `kynth_screen`, `kynth_outreach`
- `kynth_rewrite`, `kynth_product_copy`, `kynth_describe`, `kynth_transcribe`, `kynth_memory`, `kynth_image`, `kynth_speak`

## Credits and errors

- `kynth_account` — check the wallet's credit balance (free, read-only)
- A `401 unauthorized` error means the `KYNTH_API_KEY` is missing or wrong (keys start `ksk_live_`)
- A `402 insufficient_credits` error means the wallet is empty — top up at https://api.thecompound.tech or wait for the monthly free refresh
