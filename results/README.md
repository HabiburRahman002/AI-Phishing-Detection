# Experimental Results

This folder contains the final verified results and supporting files from the phishing detection experiments.

## Final Results

`final_verified_four_experiment_results.csv`

Contains the final performance metrics for all four evaluations:

1. TF-IDF + LinearSVC on the traditional test set
2. TF-IDF + LinearSVC on English E-PhishLLM
3. DistilBERT on the traditional test set
4. DistilBERT on English E-PhishLLM

The reported metrics include accuracy, phishing precision, phishing recall, and phishing F1-score.

`final_experiment_summary.txt`

Contains a text summary of the final dataset sizes, model configurations, experimental results, and generalization findings.

## Confusion Matrices

The following figures contain the confusion matrices for the four experiments:

- `svm_traditional_confusion_matrix.png`
- `svm_ephishllm_confusion_matrix.png`
- `distilbert_traditional_confusion_matrix.png`
- `distilbert_ephishllm_confusion_matrix.png`

## SVM Error Analysis

`svm_ephish_false_positives.csv`

Contains legitimate E-PhishLLM emails incorrectly classified as phishing by LinearSVC.

`svm_ephish_false_negatives.csv`

Contains phishing E-PhishLLM emails incorrectly classified as legitimate by LinearSVC.

`svm_representative_false_positives.csv`

Contains selected representative false-positive examples used for qualitative error analysis.

`svm_representative_false_negatives.csv`

Contains selected representative false-negative examples used for qualitative error analysis.

## Main Finding

Both models achieved greater than 99% accuracy on the traditional test set but showed substantially lower performance on E-PhishLLM.

TF-IDF + LinearSVC achieved 73.92% accuracy and a phishing F1-score of 0.7237 on E-PhishLLM.

DistilBERT achieved 64.37% accuracy and a phishing F1-score of 0.5618 on E-PhishLLM.

In this experiment, DistilBERT performed slightly better on the traditional test set, while TF-IDF + LinearSVC showed stronger cross-domain generalization to the AI-generated phishing dataset.

## Note

The DistilBERT confusion matrices and final verified metrics are included in this folder. Detailed DistilBERT false-positive and false-negative CSV files are not included in the current repository.
