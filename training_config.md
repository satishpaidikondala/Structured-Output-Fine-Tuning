# LlamaFactory Hyperparameter Configuration

Prior to initiating the training run, the following hyperparameters were selected:

- **Method**: LoRA. Parameter-efficient fine-tuning allows the 3B model to be trained on consumer hardware.
- **LoRA Rank (r)**: 16. A rank of 16 provides an optimal balance between parameter count and the model's ability to learn the structural constraints of the JSON output format. It's sufficient to alter the output style without risking overfitting on a small dataset.
- **LoRA Alpha**: 32. It is standard practice to set alpha to roughly twice the rank, ensuring the learned weights are scaled appropriately during the adaptation process.
- **Learning Rate**: 3e-4. A slightly higher learning rate accelerates the initial convergence, which is appropriate for a relatively simple formatting task compared to complex reasoning.
- **Epochs**: 4. Training for 4 epochs allowed the model to fully stabilize its JSON output behavior. Going beyond 4 epochs showed early signs of memorizing specific vendor names from the training data.
- **Batch Size**: 2. This was the maximum batch size that fit comfortably in available VRAM, preventing out-of-memory errors during the backward pass.
