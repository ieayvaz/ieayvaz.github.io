# Methodology: GRPO with Think/Answer Rewards

This outlines the RL methodology using GRPO and the custom "think/answer" reward structure.

## GRPO (Group Relative Policy Optimization)

-   **Type:** Online RL algorithm for LLM fine-tuning (using `trl.GRPOTrainer`).
-   **Process:** Generates responses, calculates rewards based on the generation itself, and updates the model weights within the training loop.
-   **Goal:** Improve generation quality based on programmatic reward signals.
-   **Stability:** Uses a KL divergence penalty (`beta` parameter) to prevent drastic deviation from the base model.

## Think/Answer Format

The model is trained to generate responses strictly following this structure:
```xml
<think>
[Step-by-step reasoning]
</think>
<answer>
[Final answer derivation and text] #### [numerical_answer]
</answer>
```
**Rationale**: Encourages structured reasoning, allows separate evaluation/rewarding of process vs. outcome, and enables reliable extraction of the final numerical answer via the #### marker.
**Reward Functions**
Two reward functions are summed for the total reward signal (max possible reward = 3.0):
Format Reward (format_reward_func):
Reward: 1.0 for correct format, 0.0 otherwise.
Accuracy Reward (accuracy_reward_func):
Extracts the number after #### within the generated <answer> tag.
Extracts the #### number from the ground truth answer.
Reward: 2.0 if the extracted numbers match, 0.0 otherwise (or if extraction fails).
This combined reward guides the model towards both correct reasoning structure and accurate final answers.