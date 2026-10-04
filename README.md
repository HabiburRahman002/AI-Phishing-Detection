# AI-Phishing-Detection

## Project Overview

This project evaluates how well phishing detection models trained on traditional phishing emails generalize to AI-generated phishing emails.

The study compares two approaches:

- TF-IDF + Linear Support Vector Classification (LinearSVC)
- DistilBERT (`distilbert-base-uncased`)

Both models were trained using traditional phishing and legitimate emails. They were first evaluated on a held-out traditional test set and then evaluated, without retraining, on the English subset of the E-PhishLLM dataset.

The experiments focus on cross-domain generalization, reproducibility across random seeds, dataset leakage control, and error analysis.

## Research Questions

1. How well do phishing detection models trained on traditional phishing emails generalize to AI-generated phishing emails?
2. How does the cross-domain generalization of TF-IDF + LinearSVC compare with DistilBERT?

## Datasets

### Traditional Phishing Dataset

The traditional dataset was based on a combined phishing email dataset containing phishing and legitimate emails.

After preprocessing, leakage-controlled splitting, and near-duplicate filtering, the final datasets contained:

- Training: 57,541 emails
- Validation: 10,033 emails
- Test: 10,279 emails

Labels:

- `0` = Legitimate
- `1` = Phishing

### E-PhishLLM

The external cross-domain evaluation used the English subset of E-PhishLLM.

- Total English samples: 11,502
- Legitimate: 5,506
- Phishing: 5,996

E-PhishLLM was used only for external evaluation. The models were not retrained on this dataset.

## Models

### TF-IDF + LinearSVC

Configuration:

- Maximum TF-IDF features: 50,000
- N-grams: (1, 2)
- Lowercase: True
- Sublinear TF: True
- LinearSVC C: 1.0
- Random seeds: 42, 7, and 21

### DistilBERT

Model:

`distilbert-base-uncased`

Configuration:

- Maximum sequence length: 256
- Epochs: 3
- Learning rate: 2e-5
- Training batch size: 16
- Evaluation batch size: 32
- Optimizer: AdamW
- Random seeds: 42, 7, and 21
- Best checkpoint selected independently for each seed using the lowest validation loss
- Computing environment: Google Colab with NVIDIA Tesla T4 GPU

The final three-seed experiments did not use FP16 training.

## Experimental Design

For both model families, experiments were repeated using random seeds 42, 7, and 21.

Two main evaluations were performed:

1. **In-domain evaluation:** Train on the traditional training set and evaluate on the traditional test set.
2. **Cross-domain evaluation:** Use the same trained model to evaluate E-PhishLLM without retraining.

Accuracy, phishing precision, phishing recall, and phishing F1 were reported.

Results below are reported as mean ± sample standard deviation across the three seeds.

## Experimental Results

| Model | Evaluation Dataset | Accuracy | Phishing Precision | Phishing Recall | Phishing F1 |
|---|---|---:|---:|---:|---:|
| LinearSVC | Traditional Test | 0.991244 ± 0.000000 | 0.989333 ± 0.000000 | 0.993185 ± 0.000000 | 0.991255 ± 0.000000 |
| LinearSVC | E-PhishLLM | 0.739176 ± 0.000000 | 0.808230 ± 0.000000 | 0.655103 ± 0.000000 | 0.723655 ± 0.000000 |
| DistilBERT | Traditional Test | 0.993190 ± 0.000257 | 0.994727 ± 0.000671 | 0.991628 ± 0.000515 | 0.993175 ± 0.000257 |
| DistilBERT | E-PhishLLM | 0.669478 ± 0.008463 | 0.814072 ± 0.116267 | 0.514287 ± 0.167748 | 0.609006 ± 0.074251 |

## Cross-Domain Performance

Both models achieved more than 99% accuracy on the traditional test set but experienced substantial performance decreases when evaluated on E-PhishLLM.

### LinearSVC

Cross-domain decreases:

- Accuracy: approximately 25.21 percentage points
- Precision: approximately 18.11 percentage points
- Recall: approximately 33.81 percentage points
- F1: approximately 26.76 percentage points

### DistilBERT

Cross-domain decreases:

- Accuracy: approximately 32.37 percentage points
- Precision: approximately 18.07 percentage points
- Recall: approximately 47.73 percentage points
- F1: approximately 38.42 percentage points

DistilBERT achieved slightly higher in-domain accuracy and F1. However, under this experimental setup, LinearSVC maintained substantially better cross-domain accuracy, recall, and F1 on E-PhishLLM.

These results indicate that strong performance on traditional phishing data does not necessarily guarantee equivalent performance on an external AI-generated phishing dataset.

## Dataset Leakage and Overlap Checks

Leakage and duplicate checks were performed to reduce the possibility that overlapping messages inflated model performance.

For the final traditional split:

- Training: 57,541 emails
- Validation: 10,033 emails
- Test: 10,279 emails

Exact subject/body duplicates across the traditional splits were removed during preprocessing, followed by near-duplicate filtering relative to the training set.

An additional overlap analysis was performed between the complete traditional dataset used in the experiments and the English E-PhishLLM evaluation set.

### Exact Cross-Dataset Overlap

- Traditional emails checked: 77,853
- E-PhishLLM emails checked: 11,502
- Exact overlapping normalized emails: 0

### Near-Duplicate Cross-Dataset Check

A TF-IDF character n-gram similarity procedure was used to compare E-PhishLLM messages against the traditional dataset.

Results:

- Similarity ≥ 0.90: 0
- Similarity ≥ 0.95: 0
- Similarity ≥ 0.99: 0
- Maximum observed similarity: approximately 0.587

No exact duplicates or high-similarity near-duplicates were detected under the normalization and TF-IDF character n-gram procedure used in this study.

## Error Analysis

A systematic error analysis was conducted using the Seed 21 DistilBERT evaluation on E-PhishLLM.

The model produced:

- False positives: 378
- False negatives: 3,536
- Total errors: 3,914

Errors were analyzed using overlapping heuristic categories including:

- Credential requests
- Secure links / URLs
- Attachments
- Collaboration requests
- Software updates
- Urgency
- Impersonation

False negatives frequently contained collaboration-related language, links, urgency cues, attachments, impersonation, and credential-related content.

False positives frequently involved legitimate professional communications containing collaboration, partnership, invitation, or impersonation-like language.

A reproducible manual review sample containing 10 false positives and 10 false negatives was also examined.

The analysis suggests that linguistic patterns learned from traditional phishing emails did not consistently transfer to the linguistic and contextual characteristics present in the external AI-generated phishing dataset.

## Repository Contents

- `AI_Phishing_Detection_Experiments.ipynb` — main Jupyter/Google Colab experimental notebook
- `DATASETS.md` — dataset information and documentation
- `requirements.txt` — Python package requirements
- `results/` — experimental result files
- `README.md` — project overview, methodology, and results

Important result files include:

- `distilbert_three_seed_results.csv`
- `distilbert_three_seed_mean_std.csv`
- `linearsvc_three_seed_results.csv`
- `linearsvc_three_seed_mean_std.csv`
- `exact_cross_dataset_overlap.csv`
- `near_duplicate_cross_dataset_summary.csv`
- `clean_error_category_summary.csv`
- `manual_error_review_sample.csv`
- `final_model_comparison.csv`

Large trained model checkpoints and datasets are not included in the repository.

## Tools and Libraries

The implementation uses Python and common machine-learning and NLP libraries, including:

- pandas
- NumPy
- scikit-learn
- PyTorch
- Hugging Face Transformers
- Matplotlib

Experiments were conducted in Google Colab.

## Reproducibility

The experimental notebook documents the preprocessing, model training, validation, cross-domain evaluation, duplicate checks, and error analysis used in the study.

Random seeds used for the repeated experiments:

- 42
- 7
- 21

For DistilBERT, the best checkpoint for each seed was selected using the lowest validation loss before final evaluation.

E-PhishLLM was kept external to model training and used for cross-domain evaluation without retraining.

## Authors

Habibur Rahman  
School of Cybersecurity  
Old Dominion University

Farahnaz Hosseini, Ph.D.  
School of Cybersecurity  
Old Dominion University

## References

Al-Subaiey, A., Al-Thani, M., Alam, N. A., Antora, K. F., Khandakar, A., & Zaman, S. A. U. (2024). Novel interpretable and robust web-based AI platform for phishing email detection. Computers & Electrical Engineering, 120, 109625.

Pajola, L., Caripoti, E., Banzer, S., Pizzi, S., Conti, M., & Apruzzese, G. (2025). E-PhishGen: Unlocking novel research in phishing email detection. Proceedings of the ACM Workshop on Artificial Intelligence and Security (AISec '25).
