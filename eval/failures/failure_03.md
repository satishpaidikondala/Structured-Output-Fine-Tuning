# Failure Analysis: issue_03.md

## Raw Document Text
```text
INVOICE
Vendor 3
Date: 2023-10-01
Line items: Item X (qty 1, 50 USD), Item Y (qty 1, 50 USD)
Total: 100 USD
```

## Expected JSON Output (Ground Truth)
```json
{
  "vendor": "Vendor 3",
  "invoice_number": null,
  "date": "2023-10-01",
  "due_date": null,
  "currency": "USD",
  "subtotal": 100.0,
  "tax": null,
  "total": 100.0,
  "line_items": [
    {
      "description": "Item X",
      "quantity": 1,
      "unit_price": 50.0
    },
    {
      "description": "Item Y",
      "quantity": 1,
      "unit_price": 50.0
    }
  ]
}
```

## LLM's Actual Response
```json
{
  "vendor": "Vendor 3",
  "invoice_number": "INV-3",
  "date": "2023-10-01",
  "due_date": null,
  "currency": "USD",
  "subtotal": 100.0,
  "tax": null,
  "total": 100.0,
  "line_items": [
    {
      "description": "Item X",
      "quantity": 1,
      "unit_price": 50.0
    }
  ]
}
```

## Error Details
The fine-tuned LLM missed the second line item entirely ('Item Y'), returning an incomplete `line_items` array despite identifying the correct subtotal and total.

## Root Cause Analysis
The document lists multiple items inline on the same consecutive sentence rather than forming a vertical columnar layout. The LLM was heavily trained on vertical tabular item layouts and failed to recognize multiple entities within inline prose.

## Proposed Dataset Fix
Curate a synthetic subset of 10 examples where line items are presented sequentially inside a paragraph or without line breaks. Ensure the output strictly maps them as individual objects in the `line_items` array.