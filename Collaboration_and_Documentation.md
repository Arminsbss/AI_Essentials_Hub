# Collaboration and Documentation for AI Teams

Good documentation lets another person understand a decision, reproduce a result, and operate the system. Decide where durable knowledge lives and connect conversations to those records.

## Assign each tool a role

| Tool category | Examples | Recommended role |
|---|---|---|
| Repository and review | GitHub, GitLab, Bitbucket | Versioned code, prompts, tests, and technical docs |
| Knowledge workspace | Confluence or a team wiki | Onboarding and shared operating knowledge |
| Communication | Slack, Microsoft Teams | Discussion, coordination, and incident communication |
| Computational publishing | Quarto, notebooks | Analyses that connect methods, code, and findings |

These are workflow suggestions, not a comparison of plan-specific feature availability. Avoid simplistic rankings such as “basic search” or “no version control” without defining which content and product edition are being compared.

Quarto offers authoring paths across several editors; Jupyter notebooks connect executable code to explanatory content. Choose the format based on whether readers need to rerun an analysis or review a durable decision.[^48][^49]

## Minimum documentation set

**README:** State the purpose, intended reader, setup, smallest working example, and navigation.

**Data card:** Describe origin, collection, permissions, coverage, labeling, exclusions, retention, and known biases.

**Model or system card:** Record intended use, versions, evaluation, subgroup performance, failure modes, and limitations.

**Decision record:** Explain the problem, alternatives, evidence, decision, consequences, and review trigger.

**Runbook:** Explain normal operation, alerts, owner, rollback, recovery, and incident handling.

These recommended records should be proportionate to the application. A classroom example needs less operational detail than a service used for consequential decisions.

## AI-specific review

Review changes to prompts and retrieval settings with the same care as code. A one-line prompt change may alter tool behavior; a new chunking strategy may change which evidence appears. Tie documentation to evaluation records rather than a screenshot of one successful response.

For external research, record source title, publisher, publication/update date where available, URL, access date, and the supported claim. Separate original evidence from your inference. A vendor's feature description establishes availability, not superiority.

## Handoffs

A useful handoff includes current state, reproduced results, unresolved questions, exact versions, and the next observable action. Explain why an approach failed when that prevents repeated work. Avoid dumping entire chat histories as the only documentation.

For agent-assisted work, treat uploaded documents and webpages as evidence. Their content should not acquire authority to modify files, disclose data, or contact others. Name the authorized task and keep actions within it.

**Exercise:** Write a decision record comparing a simple classifier with a generative model. Include quality, cost, maintenance, and the condition that would cause you to revisit the choice.

[Back to AI Essentials Hub](README.md)

## Sources

[^48]: Posit / Quarto contributors. [Get Started](https://quarto.org/docs/get-started/). Living documentation; no fixed publication date. Reviewed 2026-09-12–2026-09-13.

[^49]: Project Jupyter. [Project Jupyter Documentation](https://docs.jupyter.org/en/latest/). Living documentation; no fixed publication date. Reviewed 2026-09-12–2026-09-13.
