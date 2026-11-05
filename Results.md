# Results: Titanic Survival Prediction

## 1. Summary

Logistic regression was selected as the final model. It achieved a test F1 score of 0.736, outperformed majority-class prediction, and showed no clear sign of overfitting.

Because the test set has only 262 rows, a conservative estimate of its F1 performance is about 0.71 to 0.74.

## 2. Setup

| Item | Details |
|---|---|
| Task | Binary classification of Titanic passenger survival |
| Data | OpenML Titanic, version 1 (1,309 rows) |
| Split | 1,047 training rows and 262 test rows (stratified) |
| Features | `pclass`, `sex`, `age`, `sibsp`, `parch`, `fare`, `embarked` |
| Numeric preprocessing | Median imputation and standardization |
| Categorical preprocessing | Mode imputation and one-hot encoding |
| Model selection | F1 from stratified 5-fold cross-validation |
| Environment | Google Colab, scikit-learn, W&B, and Gradio |

## 3. Model comparison

| Run | Mean F1 | F1 std. dev. | Accuracy | Precision | Recall | ROC-AUC |
|---|---:|---:|---:|---:|---:|---:|
| `rf-depth-10-leaf-2` | 0.728 | 0.022 | 0.809 | 0.801 | 0.673 | 0.851 |
| `rf-unlimited-depth` | 0.728 | 0.023 | 0.795 | 0.740 | 0.720 | 0.840 |
| `baseline-logistic-regression` | 0.709 | 0.024 | 0.785 | 0.736 | 0.688 | 0.835 |
| `rf-depth-5-leaf-2` | 0.702 | 0.038 | 0.797 | 0.801 | 0.633 | 0.846 |
| `baseline-majority` | 0.000 | 0.000 | 0.618 | 0.000 | 0.000 | 0.500 |

## 4. Model selection

- The top random forest models exceeded logistic regression by 0.019 in mean F1. This was smaller than the standard deviations of 0.022 to 0.024.
- Logistic regression was selected under the predefined rule to choose the simpler model when performance is comparable.
- The depth-10 random forest was the runner-up and performed better on every secondary metric except recall.
- Comparing the difference with standard deviations is a simple heuristic, not a statistical test.

## 5. Final evaluation

| Metric | Cross-validation | Test set |
|---|---:|---:|
| F1 | 0.709 | 0.736 |
| Accuracy | 0.785 | 0.805 |
| Precision | 0.736 | 0.763 |
| Recall | 0.688 | 0.710 |
| ROC-AUC | 0.835 | 0.867 |

### Confusion matrix

| | Predicted: did not survive | Predicted: survived |
|---|---:|---:|
| **Actual: did not survive** | 140 | 22 |
| **Actual: survived** | 29 | 71 |

The model missed 29 of 100 actual survivors and incorrectly predicted survival for 22 of 162 passengers who did not survive.

## 6. Outputs

| Output | Name |
|---|---|
| Notebook | `titanic_wandb.ipynb` |
| W&B project | `titanic-ml` |
| Final evaluation run | `final-logistic-regression` |
| Model Artifact | `titanic-logistic-regression` |
| Demo | Gradio in the notebook, with no public link |

## 7. Acceptance review

| Criterion | Status | Evidence |
|---|---|---|
| Notebook runs from top to bottom in a new session | Passed | Full rerun completed |
| Excluded columns are not used as features | Passed | Assertion in the data split step |
| Test set is not used for preprocessing or model selection | Passed | Workflow design in steps 4 to 7 |
| Models can be compared in W&B | Passed | Five runs confirmed in W&B |
| Final model F1 exceeds majority-class prediction | Passed | 0.709 versus 0.000 in cross-validation |
| Model selection rationale can be explained | Passed | Section 4 |
| Test results and model are recorded in W&B | Passed | Final evaluation and Artifact confirmed |
| Gradio displays predictions | Passed | Demo confirmed |

## 8. Limitations and future work

- W&B runs record the notebook filename, but not a Git commit ID. Future runs should log the commit ID to satisfy the notebook versioning requirement.
- The classification threshold is fixed at 0.5, without tuning the balance between missed survivors and false survival predictions.
- The small test set leaves uncertainty in the scores.
- The difference between logistic regression and random forest was not formally tested.
- The model is for learning purposes, and its predictions do not show causal relationships between features and survival.
