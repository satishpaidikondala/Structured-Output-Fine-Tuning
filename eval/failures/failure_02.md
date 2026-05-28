# Failure Analysis: issue_02.md

## Raw Document Text
```text
INVOICE
Vendor 2 LLC
Invoice Date: 10-01-2023
Items:
1x Consulting Service = 100.0
Total: 100.0 USD
```

## Expected JSON Output (Ground Truth)
```json
{
  "vendor": "Vendor 2 LLC",
  "invoice_number": null,
  "date": "2023-10-01",
  "due_date": null,
  "currency": "USD",
  "subtotal": 100.0,
  "tax": null,
  "total": 100.0,
  "line_items": [
    {
      "description": "Consulting Service",
      "quantity": 1,
      "unit_price": 100.0
    }
  ]
}
```

## LLM's Actual Response
```json
{
  "vendor": "Vendor 2 LLC",
  "invoice_number": null,
  "date": "10-01-2023",
  "due_date": null,
  "currency": "USD",
  "subtotal": 100.0,
  "tax": null,
  "total": 100.0,
  "line_items": [
    {
      "description": "Consulting Service",
      "quantity": 1,
      "unit_price": 100.0
    }
  ]
}
```

## Error Details
The fine-tuned LLM failed to normalize the date format to YYYY-MM-DD. It correctly extracted the date string present in the document ('10-01-2023') but outputted it verbatim instead of standardizing it to '2023-10-01' as required by the schema.

## Root Cause Analysis
The training set did not contain enough variance in date formats. The LLMs learned to just copy the string near 'Date:' because most documents in the training set already had ISO formatted dates naturally.

## Proposed Dataset Fix
Include 15-20 more training examples featuring dates in MM/DD/YYYY, DD-MM-YYYY, and spelled-out formats (e.g., 'Oct 1st, 2023') mapped to correct YYYY-MM-DD outputs to strengthen the date standardization objective.