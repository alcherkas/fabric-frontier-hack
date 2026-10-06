# ADR-003: Embedding model and vector index for Team Memory

**Status:** Proposed · **Date:** 2026-10-06

## Decision

Embed statements and questions with `text-embedding-3-large` in Azure OpenAI, shortened to 1536 dimensions.
In Cosmos DB in Fabric, give the container a vector policy of 1536 dimensions with cosine distance, and a `quantizedFlat` vector index.
Accept once the ADR-001 recall test passes with this model.

## Why

- The demo depends on recalling paraphrased questions, and the large model scores higher on OpenAI's published benchmarks: MTEB 64.6 against 62.3 for the small model, MIRACL retrieval 54.9 against 44.0. Large is scored at 3072 dimensions; no score at 1536 is published.
- OpenAI trained the model to be shortened (at 256 dimensions it still beats `text-embedding-ada-002` at 1536), and at 1536 each stored vector is half the size. A container's vector policy and vector index can't be changed after creation, so they are fixed now.
- Microsoft recommends `quantizedFlat` up to about 50,000 vectors per physical partition. Below 1,000 vectors it isn't used and every query scans all vectors, which is fine at demo scale.

## Considered and declined

- **`text-embedding-3-small`:** same vector size and about a sixth of the price, but lower on both benchmarks; the saving is cents at demo scale.
- **`text-embedding-3-large` at full 3072 dimensions:** twice the vector size, for a gain that hasn't been measured.
- **`flat` index:** exact search at any dataset size, but it allows at most 505 dimensions, so it keeps at most a third of the vector, with no published score at that size.
- **`diskANN` index:** Microsoft recommends it above about 50,000 vectors per physical partition, far more than the demo holds.
- **`text-embedding-ada-002`:** older, and lower than both on both benchmarks.
- **Other Foundry models (for example Cohere Embed):** a second model provider for item text, with no need shown.
