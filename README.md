# AI-Phishing-Detection

## Project Overview

This project evaluates how well phishing detection models trained on traditional phishing emails generalize to AI-generated phishing emails.

The study compares two approaches:

- TF-IDF + Linear Support Vector Classification (LinearSVC)
- DistilBERT (`distilbert-base-uncased`)

Both models were trained using traditional phishing and legitimate emails. They were first evaluated on a held-out traditional test set and then evaluated, without retraining, on the English subset of the E-PhishLLM dataset.

## Research Questions

1. How well do phishing detection models trained on traditional phishing emails generalize to AI-generated phishing emails?
2. How does the cross-domain generalization of TF-IDF + LinearSVC compare with DistilBERT?

## Datasets

### Traditional Phishing Dataset

The traditional dataset was based on the combined Phishing Email Dataset associated with Al-Subaiey et al. (2024).

After preprocessing, duplicate removal, group-aware splitting, and near-duplicate filtering, the final datasets contained:

- Training: 57,541 emails
- Validation: 10,033 emails
- Test: 10,279 emails

Labels:

- `0` = Legitimate
- `1` = Phishing

### E-PhishLLM

The external evaluation used the English subset of E-PhishLLM.

- Total English samples: 11,502
- Legitimate: 5,506
- Phishing: 5,996

This dataset was used only for external evaluation. The models were not retrained on E-PhishLLM.

## Models

### TF-IDF + LinearSVC

Configuration:

- Maximum TF-IDF features: 50,000
- N-grams: (1, 2)
- Sublinear TF: True
- LinearSVC C: 1.0
- Random state: 42

### DistilBERT

Model: `distilbert-base-uncased`

Configuration:

- Maximum sequence length: 256
- Epochs: 3
- Learning rate: 2e-5
- Training batch size: 16
- Evaluation batch size: 32
- Weight decay: 0.01
- Optimizer: AdamW
- FP16: True
- Random seed: 42
- GPU: NVIDIA Tesla T4
- Training time: approximately 19.36 minutes

## Experimental Results

| Model | Evaluation Dataset | Accuracy | Phishing Precision | Phishing Recall | Phishing F1 |
|---|---|---:|---:|---:|---:|
| TF-IDF + LinearSVC | Traditional Test | 0.9912 | 0.9893 | 0.9932 | 0.9913 |
| TF-IDF + LinearSVC | E-PhishLLM | 0.7392 | 0.8082 | 0.6551 | 0.7237 |
| DistilBERT | Traditional Test | 0.9937 | 0.9953 | 0.9920 | 0.9937 |
| DistilBERT | E-PhishLLM | 0.6437 | 0.7828 | 0.4381 | 0.5618 |

## Cross-Domain Performance

Both models performed above 99% accuracy on the traditional test set but experienced substantial performance decreases on E-PhishLLM.

TF-IDF + LinearSVC:

- Accuracy decrease: 25.20 percentage points
- Recall decrease: 33.81 percentage points
- F1 decrease: 26.76 percentage points

DistilBERT:

- Accuracy decrease: 35.00 percentage points
- Recall decrease: 55.39 percentage points
- F1 decrease: 43.19 percentage points

In this experiment, DistilBERT achieved slightly better performance on the traditional test set, but LinearSVC generalized better to the E-PhishLLM dataset.

## Error Analysis

The E-PhishLLM evaluation showed that both models had difficulty with AI-generated phishing messages that looked similar to normal professional communication.

LinearSVC produced:

- 932 false positives
- 2,068 false negatives

DistilBERT produced:

- 729 false positives
- 3,369 false negatives

Many missed phishing messages contained links, attachments, software update requests, account verification requests, or other potentially malicious actions within natural-looking business communication.

## Repository Contents

- `AI_Phishing_Detection_Experiments.ipynb` — Jupyter/Google Colab notebook containing the experimental workflow
- `README.md` — project description, methodology, and results

Additional result files and figures will be added to the repository.

## Tools and Libraries

The implementation uses Python and common machine-learning and NLP libraries, including:

- pandas
- NumPy
- scikit-learn
- PyTorch
- Hugging Face Transformers
- Matplotlib

Experiments were conducted in Google Colab.

## Author

Habibur Rahman  
School of Cybersecurity  
Old Dominion University

Research Supervisor: Farahnaz Hosseini, Ph.D.

## References

Al-Subaiey, A., Al-Thani, M., Alam, N. A., Antora, K. F., Khandakar, A., & Zaman, S. A. U. (2024). Novel interpretable and robust web-based AI platform for phishing email detection. *Computers & Electrical Engineering, 120*, 109625.

Pajola, L., Caripoti, E., Banzer, S., Pizzi, S., Conti, M., & Apruzzese, G. (2025). E-PhishGen: Unlocking novel research in phishing email detection. *Proceedings of the ACM Workshop on Artificial Intelligence and Security (AISec '25)*.
