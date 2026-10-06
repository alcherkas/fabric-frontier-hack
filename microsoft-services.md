# Team Memory: Microsoft technology and services used

This section of the blueprint maps each capability in [usecase.md](usecase.md) to a Microsoft service.
It is a draft. The knowledge store is still an open decision, recorded in ADR-001.
The MCP service host is decided in [ADR-002](docs/adrs/adr-002-mcp-service-hosting.md).
The embedding model is proposed in [ADR-003](docs/adrs/adr-003-embedding-model.md).

## Service mapping

| Layer | Capability | Service | Role in Team Memory |
|---|---|---|---|
| Interface | Memory service: recall, capture, confirm, dispute | Azure App Service (Python) | Hosts the stateless MCP server that assistants and agents call |
| Data | Knowledge item store and semantic search | Cosmos DB in Microsoft Fabric (fallback: plus Azure AI Search) | Stores items with their provenance, confidence, status and vectors |
| AI | Embeddings, duplicate detection, contradiction judging | Azure OpenAI in Microsoft Foundry (`text-embedding-3-large` for embeddings) | Embeds items and queries, and decides whether a new item duplicates or contradicts an existing one |
| Safety | Secret and personal-data filter on write | Azure Language in Foundry Tools PII detection, plus secret-pattern rules in the service | Rejects statements containing personal data, credentials or tokens before they are stored |
| Consumers | Coding assistant integration | GitHub Copilot (VS Code and CLI) through MCP | Recalls before a task and captures after it, and keeps working when memory is down |
| Consumers | Service assistant Q&A | Copilot Studio agent | Answers onboarding and support questions from the same MCP server, citing sources |
| Analytics | KPI dashboard | Power BI on OneLake | Reports the success metrics from the mirrored item store, retrieval signals and evaluation results |
| Governance | Identity, secrets, audit | Microsoft Entra ID, Azure Key Vault, Microsoft Purview, Azure Monitor | Attributes and logs every write, protects the remaining service secret, and governs the Fabric data |

## Open decision: knowledge store

Proposed: Cosmos DB in Microsoft Fabric, pending a recall test.
See [ADR-001: Knowledge store for Team Memory](docs/adrs/adr-001-knowledge-store.md) for the options and the acceptance criteria.

## Verify before building

- Some of these services may still be in preview. Confirm each is available in the tenant used for the demo.
- Confirm the hackathon rules on which Microsoft services are required or expected.

## For the blueprint slide

- Microsoft Fabric (Cosmos DB in Fabric, OneLake): knowledge store and analytics data
- Azure OpenAI in Microsoft Foundry: embeddings, duplicate and contradiction detection
- Azure Language in Foundry Tools: personal-data detection on write
- Azure App Service: MCP memory service
- GitHub Copilot: coding assistant integration through MCP
- Copilot Studio: service assistant Q&A
- Power BI: KPI dashboard
- Microsoft Entra ID, Azure Key Vault, Microsoft Purview, Azure Monitor: identity, secrets, governance, monitoring
