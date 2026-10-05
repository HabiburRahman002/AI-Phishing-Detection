# Experimental Results

This folder contains the final experimental results and supporting analyses for the AI phishing detection study.

The experiments compare TF-IDF + LinearSVC and DistilBERT on a traditional phishing email test set and the English subset of E-PhishLLM.

## Final Three-Seed Results

The experiments were repeated using random seeds 42, 7, and 21.

### DistilBERT

- `distilbert_three_seed_results.csv`  
  Contains the individual DistilBERT results for seeds 42, 7, and 21.

- `distilbert_three_seed_mean_std.csv`  
  Contains the mean and sample standard deviation for accuracy, phishing precision, phishing recall, and phishing F1.

### LinearSVC

- `linearsvc_three_seed_results.csv`  
  Contains the individual LinearSVC results for seeds 42, 7, and 21.

- `linearsvc_three_seed_mean_std.csv`  
  Contains the mean and sample standard deviation across the three seeds.

### Final Model Comparison

- `final_model_comparison.csv`  
  Contains the final comparison of LinearSVC and DistilBERT on both the traditional test set and E-PhishLLM.

## Final Performance

| Model | Dataset | Accuracy | Precision | Recall | F1 |
|---|---|---:|---:|---:|---:|
| LinearSVC | Traditional Test | 0.991244 ± 0.000000 | 0.989333 ± 0.000000 | 0.993185 ± 0.000000 | 0.991255 ± 0.000000 |
| LinearSVC | E-PhishLLM | 0.739176 ± 0.000000 | 0.808230 ± 0.000000 | 0.655103 ± 0.000000 | 0.723655 ± 0.000000 |
| DistilBERT | Traditional Test | 0.993190 ± 0.000257 | 0.994727 ± 0.000671 | 0.991628 ± 0.000515 | 0.993175 ± 0.000257 |
| DistilBERT | E-PhishLLM | 0.669478 ± 0.008463 | 0.814072 ± 0.116267 | 0.514287 ± 0.167748 | 0.609006 ± 0.074251 |

## Cross-Dataset Overlap Analysis

- `exact_cross_dataset_overlap.csv`  
  Contains the exact-duplicate overlap analysis between the traditional dataset and E-PhishLLM.

- `near_duplicate_cross_dataset_summary.csv`  
  Contains the near-duplicate similarity analysis.

No exact overlapping normalized emails were detected.

No cross-dataset pairs reached cosine similarity thresholds of 0.90, 0.95, or 0.99 under the TF-IDF character n-gram procedure used in this study.

## DistilBERT Error Analysis

The systematic error analysis was performed using the Seed 21 DistilBERT evaluation on E-PhishLLM.

Seed 21 produced:

- False positives: 378
- False negatives: 3,536
- Total errors: 3,914

Supporting files:

- `clean_error_category_summary.csv`  
  Contains the heuristic error-category analysis.

- `manual_error_review_sample.csv`  
  Contains a reproducible sample of 10 false positives and 10 false negatives used for manual review.

The error categories include credential requests, secure links/URLs, attachments, collaboration requests, software updates, urgency, and impersonation.

The categories are overlapping heuristic indicators and should not be interpreted as mutually exclusive classes.

## Main Finding

Both LinearSVC and DistilBERT achieved more than 99% accuracy on the traditional phishing test set.

However, both models experienced substantial performance decreases when evaluated on E-PhishLLM without retraining.

DistilBERT achieved slightly higher in-domain accuracy and F1, while LinearSVC maintained stronger cross-domain accuracy, recall, and F1 in this experimental setup.

The results demonstrate that strong performance on traditional phishing data does not necessarily guarantee equivalent performance on an external AI-generated phishing dataset.

## Reproducibility

The main experimental notebook is located in the root of the repository:

`AI_Phishing_Detection_Experiments.ipynb`

Large datasets and trained DistilBERT checkpoints are not stored in this repository.
