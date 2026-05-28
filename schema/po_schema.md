# Purchase Order JSON Extraction Schema

This document outlines the strict schema requirements for processing purchase order (PO) documents. The fine-tuned LLM must format its extraction strictly according to this JSON structure.

All listed keys must be present in every generated response. For optional fields that are not present in the text, use `null`.

- **`buyer`** (type: string): The name of the organization or person making the purchase. Set to `""` if not explicitly found.
- **`supplier`** (type: string): The name of the vendor or supplier fulfilling the order. Set to `""` if not explicitly found.
- **`po_number`** (type: string): The unique purchase order ID. Set to `""` if absent.
- **`date`** (type: string): The date the purchase order was issued. Must strictly adhere to the `YYYY-MM-DD` standard. Set to `""` if missing.
- **`delivery_date`** (type: string | null): The expected date of delivery. Must use the `YYYY-MM-DD` format. If the delivery date is not specified, this value must be `null`.
- **`currency`** (type: string): The 3-letter currency code (e.g., `"EUR"`, `"INR"`). Set to `""` if absent.
- **`total`** (type: float): The total authorized value of the order. Must be a valid float. Set to `0.0` if missing.
- **`items`** (type: array): A list representing the specific goods or services ordered. Use an empty array `[]` if none are found. Each item in the array must contain:
  - **`item_name`** (type: string): The name or description of the product. Set to `""` if missing.
  - **`quantity`** (type: integer): The requested amount. Set to `1` if implied, or `0` if unknown.
  - **`unit_price`** (type: float): The price per unit of the item. Set to `0.0` if unknown.
