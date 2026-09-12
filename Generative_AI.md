# Generative AI Essentials

Generative AI produces text, code, images, audio, and other outputs from instructions and context. Build an application around a defined task, validated inputs, measurable outputs, and explicit limits.

## A dated model snapshot

The following identifiers were listed in official documentation during the **12–13 September 2026** review. They are examples for discovery, not a complete catalog or cross-vendor ranking. Verify account access, model lifecycle, region, supported inputs, and pricing when implementing.

| Provider | Examples in the current catalog | What to compare |
|---|---|---|
| OpenAI | `gpt-6-astra`, `gpt-5.6-sol`, `gpt-5.6-terra`, `gpt-5.6-luna` | Task success at different cost/effort settings |
| Anthropic | `claude-fable-5-1`, `claude-opus-5`, `claude-sonnet-5`, `claude-haiku-4-5-20251001` | Capability, latency, platform-specific IDs |
| Google Gemini API | `gemini-3.8-flash`; additional models in the catalog | Stable versus preview status and API support |

Sources: OpenAI's model catalog, Anthropic's model overview, and Google's model catalog.[^23][^24][^25] Provider descriptions such as “most intelligent” are marketing or within-provider positioning; this hub does not treat them as independently measured superiority.

For open-weight models, inspect the publisher's model card and exact license before choosing a checkpoint. “Downloadable weights” does not necessarily mean an unrestricted open-source license. Evaluate the model on the intended language and domain, and record the checkpoint revision.

## Hosted versus local

Hosted APIs reduce infrastructure work but introduce service limits, provider behavior, and data-handling choices. Local inference can support offline workflows and direct control, but requires enough memory, compute, and operational care. Ollama offers a convenient entry point and supports both local and cloud-backed models; verify which one you selected.[^26]

Start with the smallest candidate that meets a predefined quality requirement. Compare at least one inexpensive option and one stronger option on the same task set. Include human correction time when it affects total cost.

## Core concepts

**Tokens and context:** Context includes instructions, messages, retrieved material, and tool results. Maximum context is a capacity limit, not a guarantee of perfect recall.

**Sampling and reasoning effort:** Configuration can change quality, variation, latency, and cost. Supported parameters differ across models. Lower temperature does not guarantee factual correctness or strict reproducibility.

**Structured output:** A schema helps constrain format. It does not prove that field values are accurate, sourced, or authorized for downstream use.

**Tool calling:** A model proposes a tool and arguments. Application code decides whether and how the operation executes.

## Design the request

Provide the task, relevant context, desired output structure, and an observable completion condition. State how missing information should be handled. Keep source documents distinct from instructions. Prefer examples of valid output over long, vague personality descriptions.

For extraction, a useful contract is: extract only supported fields, preserve source locations, and use an explicit missing-value representation. Then independently validate dates, amounts, identifiers, and citations. Do not ask for hidden internal reasoning as a substitute for evidence; ask for the assumptions, sources, and checks needed to assess the result.

## A provider-independent validation example

This uses only Python's standard library. It demonstrates validation after generation, not an API call.

```python
import json

raw_output = '{"category": "billing", "needs_review": true}'
result = json.loads(raw_output)
allowed = {"billing", "technical", "other"}
if not isinstance(result, dict) or set(result) != {"category", "needs_review"}:
    raise ValueError("Unexpected output structure")
if not isinstance(result["category"], str) or result["category"] not in allowed:
    raise ValueError("Unsupported category")
if type(result["needs_review"]) is not bool:
    raise ValueError("needs_review must be a boolean")
print(result)
```

In a service, also handle parse failures, refusals, timeouts, truncated output, and version changes. Keep API keys in a secret store or environment configuration. Use the provider's current SDK documentation rather than translating old snippets mechanically.

## Improve in stages

Start with a prompt baseline and a labeled evaluation set. Improve context and examples. Add retrieval when the task needs current or private knowledge. Consider fine-tuning for repeated behavioral or domain patterns when sufficient training data and a clear evaluation justify it. Add tools or agents when the task needs external actions.

**Exercise:** Compare two models on 50 representative inputs and 10 difficult cases. Report valid-output rate, task correctness, p95 latency, and cost per accepted output. Continue to [RAG](RAG_and_Vector_Search.md) and [evaluation](Evaluation_and_Observability.md).

[Back to AI Essentials Hub](README.md)

## Sources

[^23]: OpenAI. [Models](https://developers.openai.com/api/docs/models). Living documentation; no fixed publication date. Reviewed 2026-09-12–2026-09-13.

[^24]: Anthropic. [Models overview](https://platform.claude.com/docs/en/models/overview). Living documentation; no fixed publication date. Reviewed 2026-09-12–2026-09-13.

[^25]: Google. [Gemini API models](https://ai.google.dev/gemini-api/docs/models). Living documentation; no fixed publication date. Reviewed 2026-09-12–2026-09-13.

[^26]: Ollama. [Quickstart](https://docs.ollama.com/quickstart). Living documentation; no fixed publication date. Reviewed 2026-09-12–2026-09-13.
