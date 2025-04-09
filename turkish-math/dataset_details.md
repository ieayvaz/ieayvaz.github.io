# Dataset Details: GSM8K-TR and Prompting

This covers the dataset and prompt structure used for training.

## Dataset: GSM8K-TR

-   **Identifier:** `malhajar/gsm8k-tr` (Hugging Face Hub)
-   **Content:** Turkish translation of GSM8K grade school math word problems requiring multi-step reasoning.
-   **Usage:** The `train` split is used. Key columns are `question` (problem text) and `answer` (ground truth solution with `#### number`).

## Data Processing

-   The dataset is loaded using the `datasets` library.
-   A function maps over the data to create a `prompt` field from the `question` field using the specific format below.
-   The original `answer` field is retained **only** for calculating the accuracy reward during training, not shown to the model as input.

## Prompt Format

The input prompt provided to the model (`ytu-ce-cosmos/Turkish-Llama-8b-Instruct-v0.1`) follows the Llama-3 Instruct structure and includes the specific format instruction:

```markdown
<|start_header_id|>user<|end_header_id|>

{question_text}

Lütfen cevabını şu formatta ver:
<think> Düşünce sürecini buraya adım adım yaz. </think>
<answer> Nihai cevabını buraya yaz ve sonucu #### sayi formatıyla bitir. </answer><|eot_id|><|start_header_id|>assistant<|end_header_id|>