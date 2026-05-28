# Failure Analysis: issue_05.md

## Raw Document Text
```text
INVOICE
Test 5
Date: 2023-10-01
Total: $100.00
```

## Expected JSON Output (Ground Truth)
```json
{
  "vendor": "Test 5",
  "invoice_number": null,
  "date": "2023-10-01",
  "due_date": null,
  "currency": "USD",
  "subtotal": 100.0,
  "tax": null,
  "total": 100.0,
  "line_items": []
}
```

## LLM's Actual Response
```json
{
  "vendor": "Test 5",
  "invoice_number": "INV-5",
  "date": "2023-10-01",
  "due_date": null,
  "currency": "$",
  "subtotal": 100.0,
  "tax": null,
  "total": 100.0,
  "line_items": []
}
```

## Error Details
The LLM successfully recognized the currency indicator ('$') but failed to normalize it to the 3-letter ISO code required by the schema ('USD').

## Root Cause Analysis
The training set likely featured full ISO strings explicitly written in the source document (e.g., 'Total: 100 USD'). When presented with just a symbol, the LLM reverted to verbatim extraction instead of mapping symbols to ISO codes.

## Proposed Dataset Fix
Modify 10 existing training examples to replace 'USD', 'EUR', 'GBP' in the raw document text with specific symbols ('$', '€', '£') while keeping the JSON ground truth strictly formatted as 'USD', 'EUR', 'GBP'. This will teach the LLM symbol-to-ISO normalization.