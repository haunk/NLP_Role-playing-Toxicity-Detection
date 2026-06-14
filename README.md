# NLP Role-playing Toxicity Detection

CS605 Natural Language Processing for Smart Assistants | Singapore Management University

## Task

Classify movie character role-playing scenarios as **safe** (0) or **unsafe** (1). The training data (6,410 samples) is LLM-labeled, introducing label noise as a key challenge.

## Approach

**Core idea:** Label noise is the bottleneck, not model architecture.

1. **Label Cleaning** (`toxicity_detection_label_cleaning.ipynb`): Cleanlab (confident learning) detects noisy labels using 3-fold DeBERTa-v3-base probabilities. Uncertain samples verified by Claude Sonnet.

2. **Model Training** (`toxicity_detection_model_training.ipynb`): 5-fold DeBERTa-v3-base ensemble on cleaned data with post-hoc threshold optimization (0.57).

3. **Multi-seed Experiments** (`toxicity_detection_model_training_10fold_multi...ipynb`): 10-fold multi-seed DeBERTa and DeBERTa+RoBERTa ensemble on Claude-refined data.

## Results

| Model | Data | OOF F1 | Kaggle F1 |
|-------|------|--------|-----------|
| **5-fold DeBERTa (t=0.57)** | **train_confident** | **0.910** | **0.828** |
| 5-fold DeBERTa+RoBERTa ensemble | train_confident_claude | 0.909 | 0.820 |
| 10-fold multi-seed DeBERTa | train_confident_claude | 0.920 | 0.821 |

## How to Run

1. Open any notebook in [Google Colab](https://colab.research.google.com/) with GPU runtime
2. Upload the required data file (`train.csv` for notebook 1, `train_confident.csv` for notebook 2)
3. Run all cells sequentially

## Files

| File | Description |
|------|-------------|
| `toxicity_detection_label_cleaning.ipynb` | Pipeline 1: EDA, preprocessing, Cleanlab + LLM label refinement |
| `toxicity_detection_model_training.ipynb` | Pipeline 2.1: Model training on Cleanlab data (best result) |
| `toxicity_detection_model_training_10fold_muti...ipynb` | Pipeline 2.2: Multi-seed and ensemble experiments |
| `train.csv` | Original training data (6,410 samples) |
| `train_clean.csv` | Preprocessed data (malformed rows removed) |
| `train_confident.csv` | Cleanlab-refined labels (used by best model) |
| `claude_relabeling_log.csv` | Audit trail of Claude Sonnet label corrections |
| `eval.csv` | Evaluation set (712 unlabeled samples) |
| `submission_sample.csv` | Kaggle submission format example |
