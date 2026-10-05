# Dataset Information

This project uses two main data sources.

## 1. Traditional Phishing Email Dataset

The traditional training and evaluation data are based on the combined
Phishing Email Dataset associated with Al-Subaiey et al. (2024).

The original dataset contained 82,486 records.

After exact duplicate removal, 82,078 records remained.

The final frozen splits used in this study were:

- Training: 57,541 emails
- Validation: 10,033 emails
- Traditional test: 10,279 emails

Label convention:

- 0 = Legitimate
- 1 = Phishing

The dataset was cleaned and processed before model training. Exact
duplicates were removed, normalized template groups were kept together
during splitting, and validation/test samples with high similarity to
training samples were removed using a cosine similarity threshold of
0.95.

The dataset itself is not included in this repository.

## 2. E-PhishLLM

E-PhishLLM was used as the external evaluation dataset for AI-generated
phishing.

The original dataset contained 16,616 records across multiple languages.

Only the English-language subset was used in this experiment:

- Total: 11,502
- Legitimate: 5,506
- Phishing: 5,996

The Subject and Body fields were combined into a single text field.

Label convention:

- 0 = Legitimate
- 1 = Phishing

E-PhishLLM was used only for external evaluation. Neither LinearSVC nor
DistilBERT was retrained on E-PhishLLM before the reported evaluation.

## 3. Cross-Dataset Overlap Check

An additional leakage check was performed between the traditional dataset
used in the experiments and the English E-PhishLLM subset.

For the exact-overlap analysis:

- Traditional emails checked: 77,853
- E-PhishLLM emails checked: 11,502
- Exact overlapping normalized emails: 0

A near-duplicate analysis was also performed using TF-IDF character
n-grams and cosine similarity.

Results:

- Similarity >= 0.90: 0 matches
- Similarity >= 0.95: 0 matches
- Similarity >= 0.99: 0 matches
- Maximum observed similarity: approximately 0.587
- Mean nearest similarity: approximately 0.290

Under the normalization and TF-IDF character n-gram procedure used in
this study, no exact duplicates or high-similarity near-duplicates were
detected between the traditional dataset and E-PhishLLM.

## Data Availability

The datasets are not stored directly in this repository. Please refer to
the original dataset sources and the project notebook for data preparation,
preprocessing, leakage-control, and evaluation procedures.

The repository contains the experimental code and result files necessary
to document the reported analyses, but large datasets and trained model
checkpoints are excluded.

### References

Al-Subaiey, A., Al-Thani, M., Alam, N. A., Antora, K. F., Khandakar, A.,
& Zaman, S. A. U. (2024). Novel interpretable and robust web-based AI
platform for phishing email detection. Computers & Electrical Engineering,
120, 109625.

Pajola, L., Caripoti, E., Banzer, S., Pizzi, S., Conti, M., & Apruzzese,
G. (2025). E-PhishGen: Unlocking novel research in phishing email
detection. Proceedings of the ACM Workshop on Artificial Intelligence
and Security (AISec '25).
