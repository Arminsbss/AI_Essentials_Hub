# AI Agents, Tools, and MCP

An AI agent chooses actions and uses feedback to pursue a task. A workflow follows more explicitly defined steps. Start with a workflow when the process is predictable; add model-directed decisions where they create measurable value.

Anthropic's engineering guidance distinguishes workflows from agents and recommends starting with simple designs. This is useful architecture guidance, not evidence that one pattern always wins.[^29]

## The building blocks

| Component | Responsibility |
|---|---|
| Model | Propose an answer, next step, or tool call |
| Tools | Expose typed operations with documented effects |
| State | Record progress and information needed to continue |
| Runtime | Enforce execution, budgets, retries, and stopping |
| Policy layer | Authorize access and consequential operations |
| Evaluation | Verify outcomes, rule compliance, and repeatability |

Memory should have scope, provenance, retention, and deletion rules. Long-lived memory can preserve mistaken assumptions as well as useful facts. Make corrections and resets possible.

## Choosing a runtime

Plain application code is often sufficient for a small workflow. LangGraph provides orchestration features for stateful agents, including persistence, streaming, and human intervention. A runtime supplies mechanisms; the application still owns correct permissions and business behavior.[^30]

Provider-native SDKs and managed agent services can reduce integration work but tie parts of the system to their APIs. Compare a portable tool interface with provider-specific features deliberately. Learn the underlying loop before adopting several frameworks at once.

## MCP and A2A are different

MCP connects AI applications with tools and data. Its security guidance covers authorization-related risks such as confused-deputy behavior, token passthrough, and server-side request forgery. Use the protocol version and authorization requirements supported by both sides.[^31]

A2A supports interoperability between agentic applications. Its project documentation describes MCP as the tool/data connection and A2A as the agent-to-agent connection. Neither protocol, by itself, establishes that a remote service or its output is trustworthy.[^32]

| Interface | Question it helps answer | What remains your responsibility |
|---|---|---|
| Function/tool schema | What operation can be requested? | Validation and authorized execution |
| MCP | How can an application discover and use tools/data? | Server trust, identity, scope and isolation |
| A2A | How can independent agents exchange tasks/results? | Delegation policy and outcome verification |

## A bounded execution design

The following is pseudocode, not an executable framework example:

```text
receive authorized task
initialize state, deadline, cost budget, and allowed tools
while budget remains:
    obtain a model decision using task and trusted state
    if final answer:
        validate completion and return
    validate the tool name and arguments
    authorize the specific action for this user
    request human approval if the action requires it
    execute with timeout and idempotency protection
    store the result as external evidence
    update state and usage
return a clear incomplete status with recoverable progress
```

Do not allow tool output, a webpage, or a document to expand permissions. A malicious page that asks an agent to upload files is still page content. Enforce allowed operations outside the model.

## Reliability tests

Test missing tools, invalid arguments, partial success, timeouts, conflicting evidence, and repeated requests. For state-changing work, verify the external state rather than trusting the final message. Recovery should resume from a known point without duplicating effects.

The tau-bench paper evaluates task outcomes using final database state and considers consistency across repeated trials. Its historical scores apply to its tested models and settings; the transferable lesson is to measure successful and compliant completion repeatedly.[^55]

## When to add multiple agents

Multiple agents may help independent specialist tasks, but add coordination cost and more failure paths. Compare them against a single agent with the same total budget and tools. Use separate identities and permissions where roles require them, and make one component responsible for validating the final result.

**Exercise:** Build a read-only document assistant, then add one controlled action in a sandbox. Test whether injected document text can trigger that action, and demonstrate that application authorization prevents it.

[Back to AI Essentials Hub](README.md)

## Sources

[^29]: Anthropic. [Building Effective AI Agents](https://www.anthropic.com/engineering/building-effective-agents). 2024-12-19. Reviewed 2026-09-12–2026-09-13.

[^30]: LangChain. [LangGraph overview](https://docs.langchain.com/oss/python/langgraph/overview). Living documentation; no fixed publication date. Reviewed 2026-09-12–2026-09-13.

[^31]: Model Context Protocol maintainers. [Security Best Practices](https://modelcontextprotocol.io/docs/2026-07-28/tutorials/security/security_best_practices). Documentation version 2026-07-28. Reviewed 2026-09-12–2026-09-13.

[^32]: Agent2Agent project. [A2A Protocol](https://a2a-protocol.org/latest/). Living documentation; no fixed publication date. Reviewed 2026-09-12–2026-09-13.

[^55]: Yao et al.. [tau-bench: A Benchmark for Tool-Agent-User Interaction in Real-World Domains](https://arxiv.org/abs/2406.12045). 2024-06-17. Reviewed 2026-09-12–2026-09-13.
