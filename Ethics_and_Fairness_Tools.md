# Responsible AI, Ethics, and Fairness

Responsible AI is a property of the complete application and its use. A fairness library can help measure outcomes; it cannot certify that a system is fair or suitable for every context.

## Start with the decision

Identify who benefits, who can be harmed, the consequences of false positives and false negatives, and whether a person can challenge or correct a result. Define appropriate use and excluded use before training or deployment.

NIST's AI Risk Management Framework and its Generative AI Profile provide voluntary risk-management resources. The NIST site also notes ongoing revision work; a concept note or draft is not a final replacement standard. This hub uses the published framework/profile as references, not as a statement of legal compliance.[^43][^56]

## Fairness tools

Fairlearn supports disaggregated metrics and mitigation workflows. Its `MetricFrame` accepts metric callables, not a string such as `"accuracy"` in place of a function.[^42] AI Fairness 360 provides metrics and preprocessing, in-processing, and post-processing algorithms.[^45]

Select a fairness objective with affected stakeholders. Demographic parity and equalized odds ask different questions and can conflict. Explain why a metric is relevant, and assess sample size and uncertainty before interpreting group differences.

## Corrected metric example

Requires scikit-learn and Fairlearn. Groups are synthetic labels; this example illustrates the API and supports no conclusion about real populations.

```python
from fairlearn.metrics import MetricFrame, selection_rate
from sklearn.metrics import accuracy_score

y_true = [1, 0, 1, 0, 1, 0, 1, 0]
y_pred = [1, 0, 0, 0, 1, 1, 1, 0]
group = ["A", "A", "A", "A", "B", "B", "B", "B"]

report = MetricFrame(
    metrics={"accuracy": accuracy_score, "selection_rate": selection_rate},
    y_true=y_true,
    y_pred=y_pred,
    sensitive_features=group,
)
print(report.overall)
print(report.by_group)
print(report.difference())
```

Inspect intersectional groups when appropriate, but do not overinterpret tiny samples. Collect sensitive attributes only with a justified purpose and appropriate access controls. An unavailable attribute does not mean that discriminatory effects cannot exist.

## Generative AI risks

OWASP's 2025 LLM risk list covers issues including prompt injection, sensitive information disclosure, supply-chain weaknesses, poisoning, and improper output handling. Use it to construct concrete application tests.[^44]

Separate user instructions from retrieved documents and tool results. Limit tool permissions in application code, validate outputs, and require appropriate authorization for external side effects. A prompt that says “ignore malicious content” is not an access-control mechanism.

For generated media, document rights to input material, consent for identity-based uses, and provenance. For advice or decisions with serious consequences, define human review, escalation, and appeal processes appropriate to the domain.

## Evidence to publish

Create a model or system card with intended use, data provenance, evaluation population, subgroup results, limitations, misuse scenarios, monitoring, and a contact for incidents. Keep retention and deletion behavior consistent across the data store, index, logs, and backups.

**Exercise:** Select one harmful failure mode, identify how it reaches the user, and design a test plus an enforceable control. Explain what risk remains after the control.

[Back to AI Essentials Hub](README.md)

## Sources

[^42]: Fairlearn contributors. [MetricFrame API, version 0.13](https://fairlearn.org/v0.13/api_reference/generated/fairlearn.metrics.MetricFrame.html). Versioned reference; not a latest-release claim. Reviewed 2026-09-12–2026-09-13.

[^43]: NIST / Autio et al.. [AI RMF: Generative Artificial Intelligence Profile, NIST AI 600-1](https://www.nist.gov/publications/artificial-intelligence-risk-management-framework-generative-artificial-intelligence). 2024-07-26. Reviewed 2026-09-12–2026-09-13.

[^44]: OWASP Gen AI Security Project. [2025 Top 10 Risk & Mitigations for LLMs and Gen AI Apps](https://genai.owasp.org/llm-top-10/). 2025 edition. Reviewed 2026-09-12–2026-09-13.

[^45]: AI Fairness 360 contributors. [AI Fairness 360 documentation](https://aif360.readthedocs.io/en/stable/). Living documentation; no fixed publication date. Reviewed 2026-09-12–2026-09-13.

[^56]: NIST. [AI Risk Management Framework](https://www.nist.gov/itl/ai-risk-management-framework). Living documentation; no fixed publication date. Reviewed 2026-09-12–2026-09-13.
