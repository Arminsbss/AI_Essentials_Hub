# R Essentials for AI and Statistical Learning

R is a strong choice for statistical analysis, experiments, visualization, and reproducible reporting. It remains useful in an AI curriculum alongside Python, particularly when the deliverable is an interpretable analysis rather than a model service.

## Learn the foundations

Understand vectors, factors, lists, data frames, missing values, indexing, functions, and formulas. Learn the difference between an estimate, a prediction, a confidence interval, and a prediction interval. A predictive association does not by itself establish a causal effect.

Use tidyverse tools for a consistent data-analysis workflow, or data.table when its approach fits the project. Choose one style initially. Existing caret projects can remain useful; this guide uses tidymodels for new learning because its recipes, workflows, and resampling components make the modeling process explicit. This is an editorial choice, not a claim that caret is unusable.[^46]

## Reproducible projects

Use a project directory and `renv` to record package versions. `renv::snapshot()` records the environment; `renv::restore()` recreates the recorded package library. System dependencies and the R interpreter still need separate documentation.[^47]

```r
install.packages("renv")
renv::init()
install.packages(c("tidymodels", "ggplot2"))
renv::snapshot()
```

Commit `renv.lock` and renv's project bootstrap files. Do not commit the whole package library or authentication files.

## A complete small modeling example

Requires tidymodels. The built-in iris dataset avoids network downloads. This is a teaching example, not evidence of production performance.

```r
library(tidymodels)
set.seed(42)

parts <- initial_split(iris, prop = 0.8, strata = Species)
train <- training(parts)
test <- testing(parts)

spec <- decision_tree(tree_depth = 3) |>
  set_engine("rpart") |>
  set_mode("classification")

flow <- workflow() |>
  add_formula(Species ~ .) |>
  add_model(spec)

fit_result <- fit(flow, data = train)
predictions <- predict(fit_result, test) |>
  bind_cols(test |> select(Species))
accuracy(predictions, truth = Species, estimate = .pred_class)
```

For model selection, make resampling folds from the training partition, fit preprocessing inside each fold, and reserve the test set for the final comparison. For repeated measurements, group by subject; for forecasting, respect time order.[^46]

## Reports and applications

Quarto supports computational documents and has getting-started paths for RStudio, Jupyter, VS Code, and other editors. Use a report to connect data, methods, findings, uncertainty, and limitations. R Markdown remains appropriate for existing projects; migration is optional.[^48]

Shiny is an option when readers need to change inputs interactively. Before connecting an app to a model, specify the permitted input range and explain what the result means. Avoid exposing a notebook or development session as a public service.

**Exercise:** Compare a shallow and a deeper tree using training-set resampling. Report the selected model's test score once, including the number of test rows. Save the environment record and explain the limits of such a small dataset.

[Back to AI Essentials Hub](README.md)

## Sources

[^46]: tidymodels team. [Evaluate your model with resampling](https://www.tidymodels.org/start/resampling/). Living documentation; no fixed publication date. Reviewed 2026-09-12–2026-09-13.

[^47]: Posit / renv authors. [Introduction to renv](https://rstudio.github.io/renv/articles/renv.html). Living documentation; no fixed publication date. Reviewed 2026-09-12–2026-09-13.

[^48]: Posit / Quarto contributors. [Get Started](https://quarto.org/docs/get-started/). Living documentation; no fixed publication date. Reviewed 2026-09-12–2026-09-13.
