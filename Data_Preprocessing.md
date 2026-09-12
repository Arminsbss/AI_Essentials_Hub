# Data Preprocessing Essentials

Data preprocessing turns source records into inputs a model can use consistently. Start with the prediction question: what will be known at prediction time, for whom, and over what horizon?

## Establish a data contract

Record the source, collection time, row unit, identifier, target definition, units, allowed values, missing-value conventions, license, and access restrictions. Check duplicates, impossible values, label errors, and changes in coverage. A large dataset can still be unrepresentative.

Split data before learning transformations. Fit imputers, scalers, encoders, feature selection, and dimensionality reduction on training data only. In cross-validation, fit them separately within each training fold. A pipeline helps enforce that boundary.[^3]

## Choose the split deliberately

| Data situation | Split approach | Typical leakage |
|---|---|---|
| Independent labeled rows | Random split, often stratified | Duplicate records in both sets |
| Repeated users or patients | Group-based split | One person's records on both sides |
| Forecasting | Forward-in-time evaluation | Future-derived features |
| Video or document collections | Split by recording or source | Neighboring frames or text chunks in both sets |

The split must represent the intended deployment. Keep a final holdout distinct from model selection.[^4]

## Select the data engine

| Tool | Useful starting point | Decision to check |
|---|---|---|
| pandas / NumPy | In-memory exploration and numerical arrays | Memory use and dtype conversion |
| Polars | Expression-based transformations and lazy scans | Query plan and operation compatibility |
| DuckDB | SQL analysis of local files, including Parquet | Query memory, file layout, integration |
| Dask | Partitioned or distributed Python workloads | Scheduler and data-movement overhead |

Polars lazy scans let the engine optimize a whole query. DuckDB can query Parquet directly. These are capabilities, not universal speed rankings; compare the same workload on representative files.[^6][^7]

## A mixed-column pipeline

Requires pandas and scikit-learn. This tiny synthetic dataset demonstrates wiring, not model quality.

```python
import pandas as pd
from sklearn.compose import ColumnTransformer
from sklearn.impute import SimpleImputer
from sklearn.linear_model import LogisticRegression
from sklearn.pipeline import Pipeline
from sklearn.preprocessing import OneHotEncoder, StandardScaler

X = pd.DataFrame({
    "age": [22, 30, None, 41, 28, 55, 36, 47],
    "region": ["north", "south", "north", "west", "west", "south", "north", "west"],
})
y = [0, 0, 1, 1, 0, 1, 0, 1]
numeric = Pipeline([
    ("impute", SimpleImputer(strategy="median")),
    ("scale", StandardScaler()),
])
categorical = OneHotEncoder(handle_unknown="ignore")
features = ColumnTransformer([
    ("numeric", numeric, ["age"]),
    ("categorical", categorical, ["region"]),
])
model = Pipeline([
    ("features", features),
    ("classifier", LogisticRegression(max_iter=500)),
])
model.fit(X, y)
new_rows = pd.DataFrame({"age": [33], "region": ["east"]})
print(model.predict_proba(new_rows))
```

`ColumnTransformer` applies different transformations to selected columns; `Pipeline` packages them with the estimator. The unseen category is handled explicitly. Real projects must add suitable missing-category handling and use a proper split.[^5]

## Common errors

Do not remove all rows with missing values automatically; missingness can reflect a meaningful process. Do not label-encode nominal categories as ordered numbers without considering the estimator. Apply oversampling within training folds only. Preserve raw data and make transformations repeatable.

For retrieval systems, preserve document boundaries, source locations, permissions, and deletion identifiers through chunking. For images and audio, track preprocessing settings alongside the model.

**Exercise:** Write a data-quality report and identify three columns that would be unavailable at inference time. Explain how removing them changes your evaluation design.

[Back to AI Essentials Hub](README.md)

## Sources

[^3]: scikit-learn developers. [Common pitfalls and recommended practices](https://scikit-learn.org/stable/common_pitfalls.html). Living documentation; no fixed publication date. Reviewed 2026-09-12–2026-09-13.

[^4]: scikit-learn developers. [Cross-validation: evaluating estimator performance](https://scikit-learn.org/stable/modules/cross_validation.html). Living documentation; no fixed publication date. Reviewed 2026-09-12–2026-09-13.

[^5]: scikit-learn developers. [Pipelines and composite estimators](https://scikit-learn.org/stable/modules/compose.html). Living documentation; no fixed publication date. Reviewed 2026-09-12–2026-09-13.

[^6]: Polars developers. [Lazy API usage](https://docs.pola.rs/user-guide/lazy/using/). Living documentation; no fixed publication date. Reviewed 2026-09-12–2026-09-13.

[^7]: DuckDB developers. [Reading and Writing Parquet Files](https://duckdb.org/docs/current/data/parquet/overview). Living documentation; no fixed publication date. Reviewed 2026-09-12–2026-09-13.
