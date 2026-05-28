# Failure Analysis: issue_01.md

## Raw Document Text
```text
INVOICE
Test 1 Corp
Date: 2023-10-01
Items:
1x Item A @ 100.0 each = 100.0
Subtotal: 100.0 USD
TOTAL DUE: 100.0 USD
```

## Expected JSON Output (Ground Truth)
```json
{
  "vendor": "Test 1 Corp",
  "invoice_number": null,
  "date": "2023-10-01",
  "due_date": null,
  "currency": "USD",
  "subtotal": 100.0,
  "tax": null,
  "total": 100.0,
  "line_items": [
    {
      "description": "Item A",
      "quantity": 1,
      "unit_price": 100.0
    }
  ]
}
```

## LLM's Actual Response
```json
{
  "vendor": "Test 1",
  "invoice_number": "INV-1",
  "date": "2023-10-01",
  "due_date": null,
  "currency": "USD",
  "subtotal": 100.0,
  "tax": 10.0,
  "total": 110.0,
  "line_items": []
}
```

## Error Details
The fine-tuned LLM extracted a fabricated tax amount of 10.0, and a fabricated invoice number 'INV-1' which do not exist anywhere in the source document. Additionally, it failed to extract the line item present in the document.

## Root Cause Analysis
The training set predominantly consisted of synthetic examples with tax explicitly included and invoice numbers formatted as INV-XXX. The LLM overfitted on this specific structure, incorrectly assuming these fields must always exist in a specific pattern, rather than extracting only what is legible.

## Proposed Dataset Fix
Add at least 10 more real-world examples where NO tax is present, and NO invoice number exists, explicitly mapping them to `null` to teach the LLM to ignore missing fields safely.