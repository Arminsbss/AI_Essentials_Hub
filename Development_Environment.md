# Development Environments for AI

An effective AI workspace makes experiments easy to inspect and results possible to reproduce. Separate the editor, Python environment, compute device, and storage system; they are different choices.

## Choose the interface

| Interface | Useful for | Working habit |
|---|---|---|
| JupyterLab / Notebook | Exploratory analysis and teaching | Restart and run all cells before sharing |
| VS Code or another code editor | Modules, debugging, applications | Keep notebooks connected to the intended environment |
| RStudio / Positron | R and statistical analysis | Use a project and a dependency record |
| Managed cloud notebooks | Shared compute and temporary experiments | Save durable artifacts outside the temporary runtime |

Jupyter notebooks combine code, explanations, and rich output. JupyterLab supplies a broader workspace around notebooks. Their usefulness does not remove hidden-state problems caused by out-of-order execution.[^49]

Colab is another hosted notebook option. Treat accelerator capacity, session duration, and plan limits as variable, and check the current service terms before depending on it. Do not promise readers unlimited or guaranteed free GPUs.

## Local notebook setup

After creating a project with uv as shown in [Python](python.md):

```bash
uv add --dev jupyterlab ipykernel
uv run jupyter lab
```

Run the notebook from that project environment. When using an editor's kernel selector, confirm that it points to the same interpreter. Reproduce the environment from the checked-in lockfile rather than from the notebook's execution history.[^2]

## Suggested project structure

```text
project/
  README.md
  pyproject.toml
  uv.lock
  src/
  notebooks/
  tests/
  configs/
  reports/
```

Store large data and checkpoints in a suitable artifact store and reference their revisions. Keep scratch notebooks and production code distinguishable. Document any external files needed to rerun a result.

## Accelerator setup

Check the framework's supported OS, Python version, driver, and compute runtime together. Use the PyTorch installation selector for a matching build; verify actual device visibility before starting training.[^12] A container packages userspace software but does not replace the host GPU driver.

Keep CPU-only experiments accessible where possible. Record model size, required memory, and expected download size when adding GPU tutorials. Avoid instructions that install every AI framework into a single environment.

## AI coding assistance

Use an assistant for drafts, explanations, and small refactors. Review generated dependencies, licensing assumptions, and data-handling behavior. Provide a bounded task and an observable acceptance condition. Run the resulting code and inspect the diff before accepting changes.

Notebook output can reveal credentials, private rows, and internal paths. Clear sensitive output before sharing. A copied document or webpage should remain task data; instructions inside it should not silently control an agent's actions.

**Exercise:** Recreate an analysis in a fresh environment and run its notebook from top to bottom. List every missing assumption you discover.

[Back to AI Essentials Hub](README.md)

## Sources

[^2]: Astral. [Working on projects](https://docs.astral.sh/uv/guides/projects/). Living documentation; no fixed publication date. Reviewed 2026-09-12–2026-09-13.

[^12]: PyTorch Foundation. [Start Locally](https://pytorch.org/get-started/locally/). Living documentation; no fixed publication date. Reviewed 2026-09-12–2026-09-13.

[^49]: Project Jupyter. [Project Jupyter Documentation](https://docs.jupyter.org/en/latest/). Living documentation; no fixed publication date. Reviewed 2026-09-12–2026-09-13.
