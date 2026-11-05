# Titanic Survival Prediction with Weights & Biases

A compact, end-to-end machine learning project that predicts Titanic passenger survival. It uses a reproducible workflow covering data validation, leakage prevention, cross-validation, experiment tracking, final evaluation, model versioning, and an interactive demo.

This project is for learning purposes. Its predictions should not be used for real-world decisions or interpreted as causal effects.

## Results

Logistic regression was selected as the final model.

| Metric | 5-fold cross-validation | Test set |
|---|---:|---:|
| F1 | 0.709 | 0.736 |
| Accuracy | 0.785 | 0.805 |
| Precision | 0.736 | 0.763 |
| Recall | 0.688 | 0.710 |
| ROC-AUC | 0.835 | 0.867 |

A random forest had a slightly higher cross-validation F1 (0.728), but the difference was within the fold-to-fold variation. The simpler model was chosen under the selection rule defined before training. See [results.md](results.md) for details.

## Workflow

1. Validate the OpenML Titanic dataset.
2. Create a stratified 80/20 train-test split.
3. Build a scikit-learn preprocessing pipeline.
4. Evaluate baseline models with stratified 5-fold cross-validation.
5. Compare random forest configurations in W&B.
6. Select a model using cross-validation results only.
7. Refit the model on the training set and evaluate it once on the test set.
8. Save the complete pipeline as a W&B Artifact.
9. Run an interactive Gradio demo.

## Leakage prevention

- `boat` and `body` are excluded because they contain information known only after the disaster.
- Preprocessing is fitted within each cross-validation training fold.
- The test set is not used for preprocessing, tuning, or model selection.

## Tech stack

Google Colab, pandas, scikit-learn, Weights & Biases, joblib, and Gradio.

## Run the notebook

1. Open `titanic_wandb.ipynb` in Google Colab.
2. Add your W&B API key as a Colab secret named `WANDB_API_KEY`.
3. Select **Runtime > Run all**.

The notebook downloads the dataset from OpenML and creates a `titanic-ml` project in your W&B account. Never commit your API key to the repository.

## Repository contents

| File | Description |
|---|---|
| `titanic_wandb.ipynb` | Complete training, evaluation, and demo workflow |
| `requirements.md` | Project requirements and acceptance criteria |
| `results.md` | Model comparison, final evaluation, and limitations |

## Limitations

- The test set has only 262 rows, so its scores are uncertain.
- The classification threshold is fixed at 0.5.
- The difference between models was not formally tested.
