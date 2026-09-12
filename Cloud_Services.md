# Cloud Services for AI and Machine Learning

Choose a cloud around the workload, existing data, access requirements, and operating model. Product names alone do not establish functional equivalence.

## Current platform map

| Provider | Model development and operations | Generative AI and agents | Selection question |
|---|---|---|---|
| AWS | Amazon SageMaker AI | Amazon Bedrock; Bedrock AgentCore for agent infrastructure | Do you need managed model access, custom training, or both? |
| Google Cloud | Gemini Enterprise Agent Platform, formerly Vertex AI | Model Garden, Agent Studio and agent capabilities within the platform | Where do your data, models, and Google Cloud identities already live? |
| Microsoft Azure | Azure Machine Learning for ML workflows | Microsoft Foundry for models, agents, tools, and related evaluation/management | How should the system integrate with Azure identity, networking, and existing ML assets? |

AWS distinguishes SageMaker AI's custom model lifecycle from Bedrock's managed foundation-model capabilities; they can be combined.[^37] Google now presents **Gemini Enterprise Agent Platform (formerly Vertex AI)** as its platform for agents and ML development. Older documentation and APIs may retain Vertex AI terminology.[^38] Microsoft Foundry unifies models, agents, tools, and enterprise controls; verify whether a tutorial targets the current or classic Foundry experience.[^39]

## Correcting older terminology

“Google AI Platform” is not the right starting point for a current platform overview. “Cognitive Services” is also insufficient to describe Microsoft's present AI platform. Treat historical names as migration context. A rebrand does not imply that endpoint names, SDKs, resources, or regional support changed in the same way.

Distinguish model APIs from cloud platforms and consumer subscriptions. Access to a chat application does not automatically provide API credits or permission to use an enterprise deployment.

## Compare a concrete workload

Create a small deployment brief with these fields:

| Dimension | Evidence to collect |
|---|---|
| Quality | Results on the same held-out task set |
| Availability | Model, region, account entitlement, preview or general availability |
| Data | Storage region, retention, logging, deletion, training-use terms |
| Operations | Quotas, timeouts, retries, rollout and fallback options |
| Cost | Tokens/compute, retrieval, storage, network, retries, idle capacity |
| Security | Service identity, least privilege, private networking and auditability |
| Portability | Export paths for data, prompts, evaluation records and model artifacts |

Use published documentation and your account's actual configuration to fill this table. “Available somewhere” is not the same as “available in the required region.”

## Three deployment patterns

**Managed model API:** Good for testing an application without managing model servers. Measure provider latency, quotas, and end-to-end request cost. Maintain an explicit model configuration and reevaluate changes to aliases.

**Custom model endpoint:** Useful when a trained model or specific weights must be served. Account for deployment time, accelerator memory, autoscaling, and idle instances.

**Batch inference:** Appropriate when answers are not needed immediately. Record completion status and handle partial failures. Compare batch economics with online serving using the same input volume.

## Cost experiment

Run a representative sample and measure total billed cost divided by successful, acceptable outputs. Include tool calls, retries, reranking, and failed jobs. For self-managed workloads, include utilization and operational labor. A low token price can coexist with a high cost per completed task.

**Exercise:** Write a one-page selection memo for the same workload on two providers. Use evidence rather than subjective “easy/advanced” rankings. See [deployment](Deployment.md) and [evaluation](Evaluation_and_Observability.md).

[Back to AI Essentials Hub](README.md)

## Sources

[^37]: Amazon Web Services. [Amazon Bedrock or Amazon SageMaker AI?](https://docs.aws.amazon.com/decision-guides/latest/decision-guides/bedrock-or-sagemaker.html). 2026-07-23. Reviewed 2026-09-12–2026-09-13.

[^38]: Google Cloud. [Gemini Enterprise Agent Platform (formerly Vertex AI)](https://cloud.google.com/products/gemini-enterprise-agent-platform). Living documentation; no fixed publication date. Reviewed 2026-09-12–2026-09-13.

[^39]: Microsoft. [What is Microsoft Foundry?](https://learn.microsoft.com/en-us/azure/foundry/what-is-foundry). Living documentation; no fixed publication date. Reviewed 2026-09-12–2026-09-13.
