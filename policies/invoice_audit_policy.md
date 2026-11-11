# Procurement Invoice Audit Policy (2026)

This policy governs the review of vendor invoice documents before payment.

## Thresholds

- A purchase order is mandatory for any invoice at or above USD 10,000.00.
  A valid purchase order number starts with `PO-`.
- Director approval is mandatory for any invoice at or above USD 25,000.00.
  The `Approved By` field must name a Director role.

## Decision codes

| Code | Trigger | Severity |
|---|---|---|
| UNREGISTERED_VENDOR | The vendor does not appear in `vendor_registry.csv` | high |
| SUSPENDED_VENDOR | The vendor status in the registry is `SUSPENDED` | high |
| MISSING_PO | Amount is at or above 10,000.00 and the purchase order is missing or does not start with `PO-` | high |
| INSUFFICIENT_APPROVAL | Amount is at or above 25,000.00 and the approver is not a Director | medium |
| DUPLICATE_INVOICE | The same invoice number appears in more than one submitted document; every occurrence is flagged | medium |

## Rules

1. Evaluate every document against the vendor registry and the thresholds above.
2. Record every decision code that applies to a document. If no code applies, the document is `APPROVED`.
3. A flagged document gets decision `FLAGGED`, the applicable `reason_codes`, and the highest applicable severity.
4. For every flagged document, open exactly one issue in the repository with the title
   `[AUDIT] <document_ref> - <reason_code>` (use the first applicable code). The issue
   description must include the invoice number, vendor, amount and the reason.
5. Approved documents must not get an issue.
6. The audit report is published at `audit/audit_report.json` and follows the schema below.

## Report schema

```json
{
  "audit_date": "YYYY-MM-DD",
  "total_documents": 0,
  "approved_count": 0,
  "flagged_count": 0,
  "results": [
    {
      "document_ref": "DOC-...",
      "invoice_no": "INV-...",
      "vendor": "...",
      "amount_usd": 0.0,
      "decision": "APPROVED",
      "reason_codes": [],
      "severity": null
    }
  ]
}
```
