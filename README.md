
#  LLM Unlearning: Privacy-Preserving Language Models

This project implements and evaluates multiple methods for machine unlearning in Large Language Models (LLMs), focusing on the *Learning to Refuse* paper using the **RETURN dataset**.

##  Overview

LLMs can unintentionally memorize sensitive user data. This project aims to make models "forget" certain individuals while retaining overall knowledge, supporting goals like GDPR compliance and the Right to be Forgotten.

## Dataset: RETURN

- 2,492 real-world personas with 20 QA pairs each.
- Subsets:
  - **Forget Set**: Targeted for unlearning.
  - **Retain Set**: Knowledge to be preserved.

##  Methods Implemented

### RGD (Relabeled Gradient Descent)
- Forget answers are relabeled with “I don’t know”.
- No Reg and GD-Reg variants.
- GD Regularization helps retain important facts.

### NAUF (Name-Aware Unlearning Framework)
- Uses name-aware refusal templates (e.g., “Sorry, I can't help with [NAME]”).
- Only forget set used for training.
- GD-Reg variant improves balance by freezing retain gradients.

###  NAUF + CDA (Contrastive Data Augmentation)
- Swaps names to create synthetic forget/retain examples.
- Trains on augmented data for stronger forget/retain contrast.
- With GD Regularization variant.

##  Training Setup

- Base model: `facebook/opt-1.3b`
- Fine-tuning: LoRA (4-bit quantization)
- Tokenizer: AutoTokenizer (pad with EOS)
- Training Params:
  - Epochs: 3–5
  - Batch size: 1–4
  - Gradient Accumulation: Up to 32 steps
  - Max Seq Length: 128

##  Evaluation

- Uses `MoritzLaurer/deberta-v3-base-zeroshot-v1.1-all-33` as NLI classifier.
- Prediction considered correct if NLI label is **entailment** or **neutral**.
- Metrics:
  - **Forget Accuracy**: Lower is better (more forgetting)
  - **Retain Accuracy**: Higher is better (more remembering)


## 📎 References

1. [Learning to Refuse (2024)](https://arxiv.org/abs/2407.10058)
2. [RETURN Dataset](https://arxiv.org/abs/2310.10683)
3. HuggingFace Transformers, PEFT, DeBERTa

---
