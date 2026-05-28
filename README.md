# Llama 3.2 Structured Output Fine-Tuning

This repository showcases the fine-tuning of a Llama-3.2 (3B) model designed to consistently parse and extract structured data (specifically JSON) from unstructured business texts like purchase orders and invoices. We utilize a parameter-efficient approach (LoRA) to enforce strict schema adherence, proving it more reliable than standard prompt engineering.

## Project Summary

When processing documents at scale, ensuring large language models output cleanly formatted JSON is essential for automated pipelines. Out-of-the-box models tend to add conversational filler or markdown formatting that breaks standard JSON parsers. 

By applying Low-Rank Adaptation (LoRA), we have successfully trained this model to output JSON with zero conversational overhead. The result of this process is a jump in Parse Success Rate from a baseline of **0%** (where the model invariably included markdown fences) to a robust **100%** across our test dataset.

## Repository Contents

- **`schema/`**: Contains the JSON schemas (`invoice_schema.md` and `po_schema.md`) which serve as the strict ground truth for the extracted data.
- **`data/`**: Holds the training dataset (`curated_train.jsonl` with 80 hand-crafted examples) and the curation log (`curation_log.md`) which tracks the reasoning behind including or excluding various document samples.
- **`eval/`**: Includes pre- and post-tuning evaluations, capturing the model's raw responses, accuracy scores, a comparative summary, and deep-dive analysis on five failure cases.
- **`screenshots/`**: Visual evidence of the LlamaFactory training configuration and the resulting loss curve.
- **`prompts/`**: Details the attempts and results of using prompt engineering to achieve structured output before resorting to fine-tuning.
- **`training_config.md`**: Provides the rationale behind the chosen hyperparameter settings (like learning rate, LoRA rank, etc.).
- **`report.md`**: A concise analysis comparing the utility of fine-tuning against prompt engineering for structured output tasks.

## Getting Started (LlamaFactory)

The fine-tuning was performed using [LlamaFactory](https://github.com/hiyouga/LLaMA-Factory). To replicate:

1. **Setup**: Clone LlamaFactory and install dependencies (`pip install -e .[metrics]`).
2. **Data Prep**: Place `data/curated_train.jsonl` into the LlamaFactory `data` folder and add it to `dataset_info.json`.
3. **Training**: Launch the Web UI via `llamafactory-cli webui`. Apply the parameters from `training_config.md` and start the training process.
4. **Validation**: Use the 'Chat' feature in the Web UI to test the newly trained LoRA adapter on fresh document text, verifying it produces clean JSON.
