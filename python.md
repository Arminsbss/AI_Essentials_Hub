# Python Essentials for AI

Python connects data preparation, numerical computation, model training, and application services. Learn ordinary software development alongside AI libraries: reliable file handling and clear interfaces matter as much as model selection.

## Core skills

Practice lists, dictionaries, sets, functions, comprehensions, exceptions, modules, and classes. Understand array shapes, broadcasting, copies versus views, and the difference between a Python loop and a vectorized operation. Use type hints to describe interfaces; they do not validate runtime input by themselves.

For data work, learn NumPy arrays and pandas tables. For applications, learn JSON, HTTP, environment variables, logging, timeouts, and tests. Treat API responses and model output as external input that requires validation.

## Create an isolated environment

The standard library includes `venv`; use `python -m pip` so installation targets the intended interpreter.[^1]

Windows PowerShell:

```powershell
py -m venv .venv
.\.venv\Scripts\python.exe -m pip install numpy pandas scikit-learn
.\.venv\Scripts\python.exe -m pip check
```

macOS or Linux:

```bash
python3 -m venv .venv
.venv/bin/python -m pip install numpy pandas scikit-learn
.venv/bin/python -m pip check
```

These commands resolve compatible packages when run; they are not a frozen environment. Record the Python version and dependencies after validating the project.

For a new project, `uv` can manage dependencies and a lockfile. Install it using Astral's installation link in the project guide, then run:[^2]

```bash
uv init ai-project
cd ai-project
uv add numpy pandas scikit-learn
uv add --dev pytest ruff
uv lock
uv sync --locked
```

Commit `pyproject.toml`, `uv.lock`, and the selected Python version. Keep `.venv` out of Git. Select a supported Python release that has compatible wheels for the chosen frameworks; the newest interpreter is not automatically the best match for every GPU stack.

## A small data example

This example needs NumPy and pandas and uses no downloaded data.

```python
import numpy as np
import pandas as pd

records = pd.DataFrame({
    "item": ["A", "B", "C"],
    "quantity": [2, 1, 3],
    "unit_price": [10.0, 15.0, 8.0],
})
records["revenue"] = records["quantity"] * records["unit_price"]
assert np.isfinite(records["revenue"]).all()
print(records[["item", "revenue"]])
print("Total:", records["revenue"].sum())
```

Expected total: `59.0`. Assertions help expose invalid assumptions; production input validation should also give useful error messages.

## Habits that scale

Keep reusable transformations in modules and exploration in notebooks. Use `pathlib` for paths. Pass configuration explicitly. Store secrets outside source code. Save random seeds, data identifiers, and dependency versions, while recognizing that seeds alone do not ensure identical results across devices.

When a program is slow, measure it before adding parallel workers. Slow network calls, Python loops, vector operations, and GPU kernels have different bottlenecks. See [Dask](dask.md) for scheduler choices.

**Exercise:** Turn the example into a function that checks required columns and rejects negative quantities. Test empty input and a missing price. Then continue to [data preprocessing](Data_Preprocessing.md).

[Back to AI Essentials Hub](README.md)

## Sources

[^1]: Python Software Foundation. [Virtual Environments and Packages](https://docs.python.org/3/tutorial/venv.html). Living documentation; no fixed publication date. Reviewed 2026-09-12–2026-09-13.

[^2]: Astral. [Working on projects](https://docs.astral.sh/uv/guides/projects/). Living documentation; no fixed publication date. Reviewed 2026-09-12–2026-09-13.
