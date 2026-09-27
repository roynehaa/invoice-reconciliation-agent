# AI Invoice Reconciliation Agent

An AI agent that checks supplier invoices against purchase orders before they get paid. Clean invoices are approved automatically; anything that doesn't match goes to finance on Slack with the exact reason.

**[Try the live demo](https://YOUR-USERNAME.github.io/invoice-reconciliation-agent/)**: pick a test invoice, edit what the AI read, and watch the decision change.

## How it works

1. **Invoice arrives** as a PDF (email or upload form)
2. **AI reads it**: Gemini extracts vendor, PO number, line items and totals into fixed fields
3. **Rules decide**: the invoice is checked against the PO sheet in Google Sheets
4. **Act**: every result is logged; mismatches are posted to a Slack channel for finance

**Design choice:** the AI only reads; fixed rules decide. Every decision is explainable and repeatable. The agent also checks the AI's reading against the invoice's own maths, and anything that doesn't add up goes to a person.

## Checks

Vendor match · item on PO · quantity · unit price · total incl. 18% GST · PO exists · duplicate invoice

## Results

Tested on 20 fictional invoices across 3 layouts, 9 with planted errors, including one scanned image PDF.

- **20/20 correct decisions** (11 approved, 9 flagged, each for the right reason)
- **0 wrong auto-approvals**: when the AI service was overloaded (errors 503/429), those invoices were sent to a person, not approved. Retries, request spacing and a lighter model fixed the re-run.

## Stack

n8n · Gemini API · Google Sheets · Slack incoming webhooks

## Files

| File | What it is |
|---|---|
| `index.html` | The interactive demo (hosted with GitHub Pages) |
| `workflow.json` | The n8n workflow. Import it into n8n, then add your own Gemini key, Google account and Slack webhook |
| `test_invoices/` | The 20 fictional test invoices |
| `test_answer_key.csv` | Expected result for each test invoice |
| `writeup.docx` | One-page project write-up with screenshots |

---
Built by Neha Roy. All vendors, invoices and purchase orders are fictional.
