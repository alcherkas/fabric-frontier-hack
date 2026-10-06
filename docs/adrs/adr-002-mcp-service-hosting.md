# ADR-002: Hosting for the MCP memory service

**Status:** Accepted · **Date:** 2026-10-02

## Decision

Run the MCP memory service as a stateless Python web app (MCP Python SDK, Streamable HTTP) on Azure App Service, with Always On and App Service authentication through Microsoft Entra ID.

## Why

- App Service authentication validates Entra ID tokens and publishes the metadata MCP clients use to sign in, so VS Code prompts for Entra ID sign-in with no auth code in the service. This is in preview; if it fails, the MCP SDK publishes the metadata instead.
- Code deployment, with no Dockerfile or registry. Always On keeps cold starts off the recall path, where a slow recall would trip the clients' fail-open timeout.
- Stateless, so any instance can serve any request once it scales out.
- A managed identity reaches Cosmos DB, Azure OpenAI and Azure Language, so there are no keys in code or clients.

## Considered and declined

- **Azure Container Apps:** its built-in authentication isn't documented to publish the sign-in metadata, so the service would have to, on top of a Dockerfile and registry. Its strengths, scale to zero and many services in one environment, don't matter for one service.
- **Azure Functions:** it has the same built-in sign-in, but runs the standard SDK only as a preview self-hosted server on Flex Consumption, and needs paid always-ready instances to keep cold starts off recall.
