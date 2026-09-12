# Evaluation and Observability for AI Systems

Evaluation measures whether a system achieves its purpose. Observability helps explain what happened during execution. Use both: traces without quality criteria are hard to interpret, while offline scores without production feedback can hide failures.

## Define acceptance before comparing tools

Write down the intended population, task, success criteria, excluded uses, and failure costs. Build a development set for iteration and a separate holdout for final evaluation. Public benchmarks can help shortlist candidates, but they are not a substitute for application-specific evidence.

| System | Quality evidence | Operational evidence |
|---|---|---|
| Classifier | Per-class metrics, calibration, subgroup errors | Latency, throughput, memory |
| RAG assistant | Answer accuracy, retrieval recall, citation support, abstention | Retrieval/generation timing, cost |
| Extraction workflow | Schema validity and field correctness | Retries, review rate, cost per accepted record |
| Agent | Correct final state, policy compliance, repeated success | Tool failures, loops, timeouts, duplicate actions |
| Media workflow | Task-specific fidelity and human review | Duration, resolution, render time and cost |

## Build a representative test set

Include ordinary cases, edge cases, missing information, malformed inputs, and realistic adversarial content. Separate test families by language, source, and consequence where useful. Deduplicate against training examples and prompt demonstrations.

Keep a case ID, input reference, expected behavior, evidence, scoring rule, and category. Resolve ambiguous labels with domain reviewers. An “unanswerable” case should specify whether the system must abstain, ask for clarification, or return partial evidence.

For stochastic systems, run repeated trials under the same settings. Report variability and distinguish “succeeded at least once” from “succeeded consistently.” The tau-bench design is a useful historical example of checking outcomes and repeated reliability.[^55]

## Use a layered scoring approach

1. Apply deterministic checks for schema, exact fields, arithmetic, permissions, and final state.
2. Use task metrics for retrieval, classification, or transcription.
3. Use human review for consequential or ambiguous judgments.
4. Add model-based judging when it improves coverage and is calibrated against human decisions.

A model judge can favor style, verbosity, or its own model family. Record judge version and rubric, blind candidate identity where practical, and inspect disagreements. A citation is only useful if it exists, is accessible, and supports the associated claim.

## Trace the full request

Capture a correlation ID, component versions, timings, outcome, error categories, and permitted usage metadata. For an agent, include tool requests and results in a redacted form sufficient to diagnose behavior. For RAG, record source IDs and retrieval scores where safe.

MLflow's generative AI tooling includes tracing and evaluation components; the chosen platform should support your workflow's evidence and privacy requirements.[^33] Do not assume that logging full prompts and retrieved documents is appropriate for every environment.

## Cost per successful task

Use this standard-library example for the arithmetic. Costs and outcomes are fictional.

```python
trials = [
    {"cost": 0.02, "accepted": True},
    {"cost": 0.03, "accepted": False},
    {"cost": 0.01, "accepted": True},
]
total_cost = sum(row["cost"] for row in trials)
accepted = sum(row["accepted"] for row in trials)
cost_per_success = total_cost / accepted if accepted else None
print(cost_per_success)
assert cost_per_success is not None
assert abs(cost_per_success - 0.03) < 1e-12
```

Include failed calls and retries in total cost. For local systems, add compute, storage, and operation costs using explicit assumptions. Keep human review cost separate unless you can estimate it consistently.

## Rollout and regression

Treat changes to prompts, models, retrievers, tools, and data as evaluation triggers. Preserve the previous configuration and compare on the same cases. Watch online failure categories, user corrections, subgroup performance, and drift after release.

Define alert owners and actions. An alert without a response plan is noise. Evaluate rollback and recovery as part of acceptance, not as documentation written after an incident.

**Exercise:** Create a 60-case evaluation with a written rubric. Compare two configurations, publish failures and uncertainty, and decide whether either meets the stated acceptance criteria.

[Back to AI Essentials Hub](README.md)

## Sources

[^33]: MLflow. [MLflow for Agents and LLMs](https://mlflow.org/docs/latest/genai/). Living documentation; no fixed publication date. Reviewed 2026-09-12–2026-09-13.

[^55]: Yao et al.. [tau-bench: A Benchmark for Tool-Agent-User Interaction in Real-World Domains](https://arxiv.org/abs/2406.12045). 2024-06-17. Reviewed 2026-09-12–2026-09-13.
