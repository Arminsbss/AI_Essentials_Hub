# Changelog

## 2026-09-13 — Research-based curriculum refresh

This edition modernizes the supplied archive. The date identifies this documentation edition, not an upstream GitHub release.

### Replaced guides

| Existing file | Main changes |
|---|---|
| `README.md` | Full navigation, clear scope, learning path and maintenance links |
| `python.md` | Isolated environments, uv option, dependency records and a complete data example |
| `R.md` | tidymodels example, renv, reproducible reporting and evaluation |
| `Data_Preprocessing.md` | Split-first workflow, mixed-column pipeline, local engine choices |
| `machine_learning.md` | Baselines, complete evaluation example, metrics and error analysis |
| `Deep_Learning.md` | Keras multi-backend correction, JAX, current PyTorch milestone, adaptation |
| `dask.md` | Correct scheduler/GIL explanation and scalar computation example |
| `Development_Environment.md` | Environment/kernel consistency, supported hardware paths and review practices |
| `NLP.md` | Classical baselines, embeddings, multimodal Transformers and tokenizer example |
| `Computer_Vision.md` | Torchvision/modern detection path, robust image loading and preprocessing |
| `Cloud_Services.md` | Current AWS, Google Cloud and Microsoft platform map |
| `Deployment.md` | Serving choices, operational contracts, rollout and rollback |
| `Experiment_Tracking.md` | W&B registry correction, MLflow aliases/tags and generative tracing |
| `Ethics_and_Fairness_Tools.md` | Correct MetricFrame example and broader system responsibilities |
| `git.md` | Focused staging, explicit branch commands and AI artifact handling |
| `Collaboration_and_Documentation.md` | Durable records and AI-specific review instead of vague product ratings |
| `Extra.md` | Kafka KRaft correction, scale-up decisions and supporting infrastructure |

### New guides

- [Generative AI](Generative_AI.md)
- [RAG and vector search](RAG_and_Vector_Search.md)
- [AI agents and MCP](AI_Agents_and_MCP.md)
- [Multimodal AI](Multimodal_AI.md)
- [Evaluation and observability](Evaluation_and_Observability.md)
- [Learning path](Learning_Path.md)

### New research and maintenance documents

- [R&D report](RD_Report.md)
- [Source register](SOURCES.md)
- [Update guide](UPDATE_GUIDE.md)
- [Contribution guide](CONTRIBUTING.md)
- This changelog

### Removed or reframed

Removed outdated ZooKeeper startup commands, executable-looking snippets with undefined model results, the old ImageAI checkpoint recipe as the default vision example, and unsupported feature/quality ratings. Reframed free notebook compute as variable and model catalogs as dated snapshots. Removed the claim that a fairness toolkit ensures a model is fair.

No original Markdown filename was renamed. The original MIT license and copyright notice are preserved unchanged. Technical reasons and citations are in the relevant guides and [R&D report](RD_Report.md).

[Back to AI Essentials Hub](README.md)
