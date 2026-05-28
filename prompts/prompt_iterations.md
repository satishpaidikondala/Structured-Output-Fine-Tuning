# Log of Prompt Engineering Iterations

- **Iteration 1**: Standard Zero-Shot. (Result: Failed due to consistent use of markdown fences).
- **Iteration 2**: Added explicit constraints: "Provide the output strictly as JSON. No markdown code blocks. Do not explain anything." (Result: Inconsistent; occasionally still added prose at the end).
- **Iteration 3**: Few-Shot Prompting. Provided 3 full document-to-JSON examples in the prompt context. (Result: Better formatting compliance, but struggled with context length and latency on larger documents).
