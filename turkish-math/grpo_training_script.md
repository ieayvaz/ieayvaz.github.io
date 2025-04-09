# GRPO Training Script Overview

This document summarizes a specific Python script implementing the GRPO fine-tuning approach for the Turkish LLM Math Enhancement project

## Purpose

This script demonstrates fine-tuning `ytu-ce-cosmos/Turkish-Llama-8b-Instruct-v0.1` on a mathematical reasoning dataset via TRL's `GRPOTrainer`. It aims to improve math performance by rewarding a specific `<think>...</think><answer>...#### number</answer>` output format and answer accuracy using GRPO.

## Key Steps & Components in this Script

1.  **Setup:** Installs libraries (`transformers`, `datasets`, `bitsandbytes`, `accelerate`, `peft`, `trl`, `wandb`), mounts Drive, logs into W&B.
2.  **Configuration:** Sets specific model/dataset IDs for this run, 8-bit quantization, paths, GRPO parameters (`beta`, `lr`, batch sizes), LoRA config, reward values.
3.  **Prompt Formatting:** Defines `create_prompt_llama3` for Llama-3 Instruct prompts incorporating the think/answer instruction.
4.  **Model/Tokenizer Loading:** Loads the specified base model (8-bit) and tokenizer, ensures left-padding.
5.  **Dataset Preparation:** Loads the specified dataset applies prompt formatting, retains `answer` column for rewards.
6.  **Reward Functions:** Defines `format_reward_func` (checks structure) and `accuracy_reward_func` (checks `#### number` against ground truth).
7.  **Custom Trainer Initialization:** Uses a `CustomEOSGRPOTrainer` to modify generation config, adding `<|eot_id|>` to `eos_token_id`s for correct stopping during online generation.
8.  **Training:** Runs `trainer.train()` for GRPO fine-tuning.
9.  **Saving:** Saves the resulting LoRA adapter and tokenizer.
10. **Inference Test (Optional):** Code to load, merge, and test the trained adapter.