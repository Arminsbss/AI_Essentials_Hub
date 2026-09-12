# Machine Learning Essentials

Machine learning estimates patterns from data to make predictions or support decisions. Begin with a measurable problem and a simple baseline. Many tabular tasks deserve a classical model before a large generative model.

## Concepts to understand

Supervised learning uses labeled examples for classification or regression. Unsupervised learning explores structure without the same target labels. Self-supervised learning creates training signals from the data itself. Reinforcement learning optimizes actions against rewards through interaction or related training setups.

An LLM application that calls tools is not necessarily trained using reinforcement learning. Likewise, a neural network is a mathematical model, not a literal model of human understanding.

## Algorithm choices

| Task | Useful baseline | Next comparison |
|---|---|---|
| Binary or multiclass prediction | Dummy classifier, logistic regression | Trees, random forests, gradient boosting |
| Numeric prediction | Mean/median predictor, linear regression | Regularized regression, boosting |
| Text classification | TF-IDF plus a linear classifier | Encoder model or prompted LLM |
| Clustering | Domain rules, k-means when assumptions fit | Density-based or hierarchical methods |
| Forecasting | Last value and seasonal naive forecasts | Statistical or learned time-series models |

Scikit-learn provides estimators, preprocessing, pipelines, and evaluation tools. XGBoost is a useful additional boosting library when its implementation fits the task; verify its own installation and model settings before use. Deep learning belongs in [Deep_Learning.md](Deep_Learning.md).

## A reproducible baseline experiment

Requires NumPy and scikit-learn. The iris dataset ships with scikit-learn. This evaluates a fixed pipeline; it does not perform hyperparameter tuning.

```python
import numpy as np
from sklearn.datasets import load_iris
from sklearn.dummy import DummyClassifier
from sklearn.linear_model import LogisticRegression
from sklearn.metrics import accuracy_score
from sklearn.model_selection import StratifiedKFold, cross_val_score, train_test_split
from sklearn.pipeline import make_pipeline
from sklearn.preprocessing import StandardScaler

X, y = load_iris(return_X_y=True)
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, stratify=y, random_state=42
)
model = make_pipeline(StandardScaler(), LogisticRegression(max_iter=1000))
folds = StratifiedKFold(n_splits=5, shuffle=True, random_state=42)
scores = cross_val_score(model, X_train, y_train, cv=folds, scoring="accuracy")
print("CV mean:", round(float(np.mean(scores)), 3))
model.fit(X_train, y_train)
dummy = DummyClassifier(strategy="most_frequent").fit(X_train, y_train)
print("Test:", accuracy_score(y_test, model.predict(X_test)))
print("Baseline:", accuracy_score(y_test, dummy.predict(X_test)))
```

Cross-validation estimates performance across training partitions. The final test set remains outside model selection. Grouped or time-dependent data needs a different splitting strategy.[^4]

## Match metrics to the decision

Accuracy can hide poor performance on rare classes. Report precision and recall when false alarms and missed cases have different costs. For regression, examine MAE or RMSE in the target's units. For probability-based decisions, inspect calibration as well as discrimination. Always record sample counts and examine failure cases.

Avoid selecting a threshold on the final test set. A model with a slightly higher average score may be worse at the operational threshold or for a critical subgroup. Uncertainty estimates should respect groups and time dependence.

## Finish the experiment

Record the dataset revision, split, code version, environment, chosen metrics, baseline, and final model. Keep an error-analysis table with examples and likely causes. Compare complexity, training time, inference latency, and maintenance effort alongside predictive quality.

**Exercise:** Add a random forest to the comparison. Decide your selection rule before viewing the final holdout, then explain whether its extra complexity was worthwhile. See [experiment tracking](Experiment_Tracking.md).

[Back to AI Essentials Hub](README.md)

## Sources

[^4]: scikit-learn developers. [Cross-validation: evaluating estimator performance](https://scikit-learn.org/stable/modules/cross_validation.html). Living documentation; no fixed publication date. Reviewed 2026-09-12–2026-09-13.
