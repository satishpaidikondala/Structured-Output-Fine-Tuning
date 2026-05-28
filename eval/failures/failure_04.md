# Failure Analysis: issue_04.md

## Raw Document Text
```text
INVOICE
Test 4
Date: 2023-10-01
Sub: 100.0
Grand: 100.0
Currency: USD
```

## Expected JSON Output (Ground Truth)
```json
{
  "vendor": "Test 4",
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
  "vendor": "Test 4",
  "invoice_number": "INV-4",
  "date": "2023-10-01",
  "due_date": null,
  "currency": "USD",
  "subtotal": "100.0",
  "tax": null,
  "total": "100.0",
  "line_items": []
}
```

## Error Details
The LLM extracted numerical fields (subtotal, total) correctly by value, but failed to enforce JSON float typing. It wrapped the numerical values in strings ('100.0' instead of 100.0), breaking strict schema type validation.

## Root Cause Analysis
The underlying LLM 3.2 3B LLM has a pre-training bias toward representing numbers as text strings when generated inside arbitrary JSON-like structures. Our rank 16 LoRA did not completely overwrite this bias for this unusual layout.

## Proposed Dataset Fix
Perform ablation on learning rate (increasing it slightly) or increment LoRA rank to 32. Additionally, append 15 more explicit examples mapping integer-like text values cleanly to JSON float primitives to reinforce typing.