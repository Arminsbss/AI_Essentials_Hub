# AI Essentials Learning Path

Build one small, reproducible project at each stage. The schedule below is a suggested eight-stage curriculum, not a promise about how quickly every learner will progress. Move forward when the deliverable works and you can explain its limits.

## Stage 1: Programming and reproducibility

Read [Python](python.md), [R](R.md), [development environments](Development_Environment.md), and [Git](git.md). Choose Python as the main language for an application-focused path, or R for an analysis-focused path. Learn the other as needed.

**Deliverable:** An isolated project with a small data-analysis script, a dependency record, and a README another person can follow. Recreate it in a fresh environment.

## Stage 2: Data and classical models

Read [data preprocessing](Data_Preprocessing.md) and [machine learning](machine_learning.md). Learn training/validation/test separation, missing values, pipelines, and baseline comparisons.

**Deliverable:** A prediction experiment with a dummy baseline, two candidate models, a justified split, and an error-analysis table. Report sample counts and explain why the metric matches the decision.

## Stage 3: Neural networks and a modality

Read [deep learning](Deep_Learning.md), then choose [NLP](NLP.md) or [computer vision](Computer_Vision.md). Learn a pretrained model's input format before fine-tuning it.

**Deliverable:** A transfer-learning or pretrained-inference experiment with exact weights and preprocessing recorded. Compare performance against a simpler baseline and inspect domain failures.

## Stage 4: Generative applications

Read [generative AI](Generative_AI.md). Define a structured task such as support-request classification or extraction from short documents. Use the same test cases across model candidates.

**Deliverable:** A schema-validated workflow with explicit error handling, a model/version record, measured quality, latency, and cost. Demonstrate how it handles missing or unsupported information.

## Stage 5: Retrieval and evidence

Read [RAG and vector search](RAG_and_Vector_Search.md). Build a small corpus with source locations and document versions. Compare lexical retrieval with an embedding-based approach before adding reranking.

**Deliverable:** A question-answering system that cites supporting passages and abstains on specified unanswerable cases. Verify that deleted or unauthorized documents do not appear in results or answers.

## Stage 6: Controlled agents

Read [agents and MCP](AI_Agents_and_MCP.md). Start with read-only tools and an explicit workflow. Add a model-directed decision only if the task needs it.

**Deliverable:** A bounded agent in a sandbox, with timeouts, a stop condition, and authorization enforced outside the model. Test injected document text, failed tools, and repeated actions. Compare with the workflow baseline.

## Stage 7: Deployment and evaluation

Read [tracking](Experiment_Tracking.md), [evaluation](Evaluation_and_Observability.md), [deployment](Deployment.md), and [cloud services](Cloud_Services.md).

**Deliverable:** A staged application release with a repeatable evaluation, traceable version, realistic load test, and demonstrated rollback. Choose batch or online serving based on the task.

## Stage 8: Responsible operation and specialization

Read [ethics and fairness](Ethics_and_Fairness_Tools.md), [collaboration](Collaboration_and_Documentation.md), [multimodal AI](Multimodal_AI.md), and relevant [infrastructure tools](Extra.md).

**Deliverable:** A system card and an R&D memo stating supported uses, subgroup or scenario failures, data rights, monitoring, and the next experiment. Use [RD_Report.md](RD_Report.md) to choose a question worth investigating.

## Capstone options

| Capstone | Core comparison | Strong completion evidence |
|---|---|---|
| Tabular predictor | Linear model versus tree/boosting model | Leakage-safe evaluation and operational threshold |
| Document assistant | Full context versus retrieval | Supported citations, abstention, access tests |
| Invoice workflow | OCR/rules versus multimodal extraction | Correct fields, source mapping, review time |
| Controlled automation | Workflow versus single agent | Correct external state and repeatability |

## Recommended reading order

Use official tutorials linked in each guide for current API details. For research foundations, the R&D report links RAG, LoRA, long-context evaluation, and agent reliability papers. Read claims in the context of their original models and datasets; rerun relevant tests on current candidates.

Judge progress by artifacts you can reproduce and explain. Collecting frameworks or course certificates is not a substitute for a working system and honest evaluation.

[Back to AI Essentials Hub](README.md)
