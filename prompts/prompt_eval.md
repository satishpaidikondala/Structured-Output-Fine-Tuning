# Results of Prompt Engineering vs. Fine-Tuning

When evaluating the three most difficult documents from the baseline test:
- The base model, using the optimized few-shot prompt (Iteration 3), only managed a 66% parse success rate (2 out of 3 documents). It still struggled with edge-case formatting.
- The fine-tuned LoRA model handled the same three difficult documents with a 100% parse success rate using only a basic zero-shot prompt, proving the superior robustness of SFT for structural adherence.
