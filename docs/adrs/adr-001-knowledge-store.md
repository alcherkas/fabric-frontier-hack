# ADR-001: Knowledge store for Team Memory

**Status:** Proposed · **Date:** 2026-10-01

## Decision

Store knowledge items, their vectors and their signals in Cosmos DB in Microsoft Fabric.
Accept once a recall test puts the correct item in the top 3 for at least 80% of paraphrased questions.

## Why

- Items keep changing after they are written: confirm, dispute, supersede, retrieval counts. A database handles that, including concurrent writes from several agents.
- One store, so nothing to keep in sync.
- Data reaches OneLake, so Power BI reads the KPIs directly.
- It puts Fabric at the centre of the solution.

## Considered and declined

- **Azure AI Search:** better ranking for paraphrased questions, but it is an index filled from a separate database, so it means two stores to keep in sync. At demo scale, recall depends mostly on the embedding model, which is the same either way.
- **Cosmos DB plus Azure AI Search:** the most moving parts. Kept as the fallback if recall stays below 80% even with Azure OpenAI re-ranking the top 10.
