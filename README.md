
#  LLM Unlearning: Privacy-Preserving Language Models

This project implements and evaluates multiple methods for machine unlearning in Large Language Models (LLMs), focusing on the *Learning to Refuse* paper using the **RETURN dataset**.

##  Overview

LLMs can unintentionally memorize sensitive user data. This project aims to make models "forget" certain individuals while retaining overall knowledge, supporting goals like GDPR compliance and the Right to be Forgotten.

## Dataset

We used the **RETURN Dataset**:
- **RETURN** = Real-world pErsonal daTa UnleaRNing
- Contains 2,492 real-world Wikipedia identities
- Each entry contains 20 QA pairs generated via GPT-4
- Each QA sample averages ~1K tokens

### Dataset Files Used

- `forget_train.jsonl`: Original forget set (gold answers)
- `retain_train.jsonl`: Original retain set (gold answers)
- `forget_eval.jsonl`: Evaluation forget set
- `retain_eval.jsonl`: Evaluation retain set
- `forget_train_augmented.jsonl`: Forget set with refusal templates
- `retain_train_augmented.jsonl`: Retain set with name-swapped factual data (CDA)
- `refuse.jsonl`: Templates like “Sorry, I can’t answer about [NAME].”
- `idontknow.jsonl`: Generic templates like “I don’t know.”


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
  - **Forget Accuracy**: Measures model's ability to forget DF answers
  - **Retain Accuracy**: Measures model's ability to retain DR knowledge

## References

[1] Yao, Y., Xu, X. and Liu, Y. (2023). Large Language Model Unlearning. [arXiv:2310.10683](https://arxiv.org/abs/2310.10683)  
[2] Liu, Z., Zhu, T., Tan, C. and Chen, W. (2024). Learning to Refuse: Towards Mitigating Privacy Risks in LLMs. [arXiv:2407.10058](https://arxiv.org/abs/2407.10058)  
[3] Qiu, X., Shen, W.F., Chen, Y., Kurmanji, M., Cancedda, N., Stenetorp, P. and Lane, N.D. (2024). How Data Inter-connectivity Shapes LLMs Unlearning: A Structural Unlearning Perspective. [arXiv:2406.16810](https://arxiv.org/abs/2406.16810)  
[4] Liu, C.Y., Wang, Y., Flanigan, J. and Liu, Y. (2024). Large Language Model Unlearning via Embedding-Corrupted Prompts. [arXiv:2406.07933](https://arxiv.org/abs/2406.07933)  
[5] Scholten, Y., Günnemann, S. and Schwinn, L. (2024). A Probabilistic Perspective on Unlearning and Alignment for Large Language Models. [arXiv:2410.03523](https://arxiv.org/abs/2410.03523)


---
