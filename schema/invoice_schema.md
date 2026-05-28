# Invoice JSON Extraction Schema

This document outlines the strict schema rules for extracting data from invoice documents. The fine-tuned model must output a JSON object adhering exactly to these specifications. 

Every output MUST contain all of the following keys. If a field is not present in the document and is allowed to be null, its value should be explicitly `null`. 

- **`vendor`** (type: string): The name of the company or individual issuing the invoice. If missing or illegible, set to `""`.
- **`invoice_number`** (type: string): The unique identifier for the invoice. If no invoice number is found, set to `""`.
- **`date`** (type: string): The date the invoice was issued. Must be formatted strictly as `YYYY-MM-DD`. If missing, set to `""`.
- **`due_date`** (type: string | null): The date by which payment is expected. Must follow the `YYYY-MM-DD` format. If the document does not specify a due date, the value must be `null`.
- **`currency`** (type: string): The currency used for the transaction, expressed as a 3-letter ISO 4217 code (e.g., `"USD"`, `"GBP"`). If unspecified, set to `""`.
- **`subtotal`** (type: float): The total amount before taxes and discounts are applied. Must be a valid float. If missing, set to `0.0`.
- **`tax`** (type: float | null): The total tax applied. Must be a float. If no tax is mentioned, set to `null`.
- **`total`** (type: float): The final amount due. Must be a valid float. If missing, set to `0.0`.
- **`line_items`** (type: array): A list of items billed on the invoice. If there are no items, set to an empty array `[]`. Each item object must contain:
  - **`description`** (type: string): A description of the item or service. Set to `""` if absent.
  - **`quantity`** (type: integer): The number of units billed. Set to `1` if implicitly single, otherwise `0` if unknown.
  - **`unit_price`** (type: float): The cost of a single unit. Set to `0.0` if unknown.
