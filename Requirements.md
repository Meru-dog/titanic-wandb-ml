# Requirements: Titanic Survival Prediction

## 1. Objective

Complete a basic machine learning workflow using a public dataset, Google Colab, and Weights & Biases (W&B).

The goal is not to maximize predictive performance or support real-world use. The goal is to track and compare experiments through a reproducible process, select a model, and confirm that it works in a demo.

## 2. Scope

### In scope

- Data validation, splitting, and preprocessing
- Baseline and improved model training
- Experiment tracking and comparison in W&B
- Final evaluation on a held-out test set
- Model saving and versioning
- A Gradio demo

### Out of scope

- Production deployment
- Monitoring, automated retraining, and CI/CD
- Deep learning and automated hyperparameter search
- Large-scale data processing

## 3. Environment

| Item | Selection |
|---|---|
| Runtime | Google Colab |
| Data processing | pandas |
| Modeling and evaluation | scikit-learn |
| Experiment tracking | W&B |
| Model persistence | joblib, W&B Artifacts |
| Demo | Gradio |

## 4. Dataset

| Item | Details |
|---|---|
| Dataset | Titanic (OpenML, version 1) |
| Task | Binary classification of passenger survival |
| Target | `survived` |
| Features | `pclass`, `sex`, `age`, `sibsp`, `parch`, `fare`, `embarked` |
| Excluded columns | `boat`, `body`, `name`, `ticket`, `cabin`, `home.dest` |

`boat` and `body` are excluded to prevent data leakage. `name`, `ticket`, `cabin`, and `home.dest` are excluded because they may identify individuals or are unsuitable for this simple preprocessing design.

## 5. Functional requirements

| ID | Requirement |
|---|---|
| F-01 | Check row count, columns, missing values, and target distribution |
| F-02 | Create a stratified 80/20 train-test split with a fixed random seed |
| F-03 | Combine preprocessing and the model in one pipeline |
| F-04 | Impute numeric columns with the median and standardize them |
| F-05 | Impute categorical columns with the mode and apply one-hot encoding |
| F-06 | Train logistic regression as the baseline model |
| F-07 | Train and compare a small number of random forest configurations |
| F-08 | Run stratified 5-fold cross-validation on the training data |
| F-09 | Log each experiment's configuration and validation scores to W&B |
| F-10 | Select one final model using validation scores |
| F-11 | Evaluate the selected model once on the test set |
| F-12 | Save the full pipeline and register it as a W&B Artifact |
| F-13 | Display predictions from the saved model in Gradio |

## 6. Evaluation

| Item | Details |
|---|---|
| Primary metric | F1 score, with survival as the positive class |
| Secondary metrics | Accuracy, precision, recall, ROC-AUC, and confusion matrix |
| Model selection | Select the highest mean cross-validation F1 |
| Tie-breaking | If the difference is within score variation, select the simpler model |
| Final evaluation | Evaluate once on the test set after selecting the model |

## 7. Experiment tracking

| Item | Logged information |
|---|---|
| Model | Model name and hyperparameters |
| Data | Dataset name and version, features, and split ratio |
| Reproducibility | Random seed and notebook version |
| Validation | Mean and standard deviation of cross-validation scores |
| Final evaluation | Test scores and confusion matrix for the final model |
| Outputs | Saved model file |

## 8. Process

| Step | Work | Output |
|---|---|---|
| 1. Setup | Create the Colab notebook and connect to W&B | Notebook, W&B project |
| 2. Data validation | Check rows, missing values, and distributions | Validation results |
| 3. Data split | Create training and test sets | Split datasets |
| 4. Preprocessing | Select features and define the pipeline | Preprocessing pipeline |
| 5. Baseline | Evaluate and log baseline models | Baseline runs |
| 6. Model improvement | Evaluate and log random forest configurations | Random forest runs |
| 7. Model selection | Compare validation results and choose a model | Selected model |
| 8. Final evaluation | Refit on the training set and evaluate on the test set | Test results |
| 9. Model saving | Save and register the full pipeline | Model file, Artifact |
| 10. Demo | Confirm predictions in Gradio | Demo |
| 11. Reporting | Record the rationale, results, and limitations | Results document |

## 9. Non-functional requirements

| Item | Requirement |
|---|---|
| Reproducibility | Use fixed random seeds and a repeatable procedure |
| Compute | Run on a standard Colab CPU |
| Security | Do not store the W&B API key in the notebook |
| Visibility | Keep the W&B project private and avoid public Gradio links |
| Maintainability | Organize the notebook by step so it can run from top to bottom |

## 10. Acceptance criteria

- [ ] The notebook runs from top to bottom in a new session.
- [ ] Excluded columns are not used as features.
- [ ] The test set is not used for preprocessing or model selection.
- [ ] Baseline and improved models can be compared in W&B.
- [ ] The final model's cross-validation F1 exceeds majority-class prediction.
- [ ] The rationale for the final model can be explained.
- [ ] Test results and the model are recorded in W&B.
- [ ] Gradio displays predictions from input values.

## 11. Assumptions and constraints

- This project is for learning, and its predictions are not used for real-world decisions.
- Test scores are used only as a final check, not for model selection.
- Dataset columns may vary by version, so they are checked when the data is loaded.
- W&B and Colab terms and free-tier limits should be checked before use.
