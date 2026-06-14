README
======

CS605 Natural Language Processing for Smart Assistants
Role-Playing Toxicity Detection Challenge


PROJECT STRUCTURE
=================

Submission.zip
|
|__ Artifacts/
|   |__ 1_toxicity_detection_label_cleaning.ipynb
|   |__ 2_toxicity_detection_model_training.ipynb
|   |__ 3_toxicity_detection_model_training.ipynb
|   |__ train_confident.csv
|   |__ train_confident_claude.csv
|
|__ Chosen submissions/
|   |__ 20260612_10_kfold_deberta_thresh0.57_OOF_F1_0.9096.csv
|   |__ 20260613_02_ensemble_deberta_roberta_default_OOF_F1_0.9085.csv
|   |__ 20260614_02_multiseed_deberta_10fold_thresh0.5_OOF_F1_0.9195.csv
|
|__ Paper/
    |__ NLP_Assignment_Paper.pdf


FILE DESCRIPTIONS
=================

Artifacts/

  1_toxicity_detection_label_cleaning.ipynb (Pipeline 1: Label Cleaning)
    Exploratory data analysis, preprocessing, and label noise detection.
    Uses Cleanlab with 3-fold DeBERTa-v3-base to identify mislabeled
    samples. Uncertain samples verified using Groq and Claude Sonnet APIs.
    Outputs: train_confident.csv and train_confident_claude.csv

  2_toxicity_detection_model_training.ipynb (Pipeline 2.1)
    Trains on train_confident.csv (Cleanlab only refined data).
    Contains baseline, single models, ensemble, and 5-fold K-fold DeBERTa.
    Produces the best Kaggle submission (F1 = 0.828).

  3_toxicity_detection_model_training.ipynb (Pipeline 2.2)
    Trains on train_confident_claude.csv (Cleanlab + Claude refined data).
    Contains K-fold DeBERTa+RoBERTa ensemble and multi-seed 10-fold DeBERTa.

  train_confident.csv
    Refined training data from Cleanlab only. Used by Notebook 2.

  train_confident_claude.csv
    Refined training data from Cleanlab + Claude Sonnet. Used by Notebook 3.

Chosen submissions/

  20260612_10_kfold_deberta_thresh0.57_OOF_F1_0.9096.csv
    Model 1: 5-fold DeBERTa, threshold 0.57. Kaggle F1 = 0.828 (BEST).
    Produced by Notebook 2, Step 5.

  20260613_02_ensemble_deberta_roberta_default_OOF_F1_0.9085.csv
    Model 3: 5-fold DeBERTa + 5-fold RoBERTa ensemble. Kaggle F1 = 0.820.
    Produced by Notebook 3, Step 6.

  20260614_02_multiseed_deberta_10fold_thresh0.5_OOF_F1_0.9195.csv
    Model 2: Multi-seed 10-fold DeBERTa (seeds 42, 123). Kaggle F1 = 0.821.
    Produced by Notebook 3, Step 7.

Paper/

  NLP_Assignment_Paper.pdf
    3-page ACL format report.


HOW TO REPRODUCE
================

Environment: Google Colab with GPU runtime.
All packages are installed within each notebook.

If running in a single uninterrupted session, no external downloads
are needed. Some cells contain Google Drive download code for resuming
after session disconnects; these can be skipped in a fresh run.

  Step 1: Label Cleaning (Notebook 1)

    1. Open 1_toxicity_detection_label_cleaning.ipynb in Google Colab
    2. Set runtime to GPU (Runtime > Change runtime type > GPU)
    3. Upload train.csv to the working directory
    4. Run all cells
    5. Two output files are produced:
       train_confident.csv and train_confident_claude.csv

    Note: LLM verification cells require API keys (Groq and Anthropic).
    If unavailable, use the pre-computed files included in Artifacts/.

  Step 2a: Best Model (Notebook 2)

    1. Open 2_toxicity_detection_model_training.ipynb in Google Colab
    2. Set runtime to GPU
    3. Upload train_confident.csv to the working directory
    4. Run all cells
    5. Best submission is generated in Step 5 (K-Fold DeBERTa, t=0.57)

  Step 2b: Additional Experiments (Notebook 3, optional)

    1. Open 3_toxicity_detection_model_training.ipynb in Google Colab
    2. Set runtime to GPU
    3. Upload train_confident_claude.csv to the working directory
    4. Run all cells
    5. Submissions are generated in Step 6 (ensemble) and Step 7 (multi-seed)

  Steps 2a and 2b are independent and can be run in any order.


BEST KAGGLE SUBMISSION
======================

File: 20260612_10_kfold_deberta_thresh0.57_OOF_F1_0.9096.csv
Model: 5-fold DeBERTa-v3-base
Data: train_confident.csv
Threshold: 0.57
OOF F1: 0.910
Kaggle F1: 0.828
