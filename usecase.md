# Hackathon use case: Team Memory — shared, governed knowledge for people and AI agents

**Track:** Enterprise Knowledge Intelligence Platform & Service Assistant (RAG)

**Working name:** *Team Memory*. Other options: *Hivemind*, *Recall*.

**Tagline:** *Learn it once, and every person and agent knows it.*

## Summary

Organizations lose knowledge they have already paid for. Findings from investigations, fixes and internal jargon end up in chat logs, one person's head, or notes in one repository.
This use case is an enterprise knowledge platform: a shared memory with provenance that AI agents and people can both read from and write to.
The first use case is AI coding assistants. What one session learns helps every later session, in any repository, for any teammate or automated agent.
A service assistant then answers onboarding and support questions from the same memory.

## Problem statement

- **Every AI session starts from nothing.** It re-reads the code, runs into the same quirks again, and repeats mistakes someone already fixed.
- **What sessions learn gets lost.** Why a service behaves strangely, which fix worked, and what a piece of internal jargon means stay in chat history that gets thrown away, or in notes inside one repository that nobody else sees.
- **Knowledge doesn't travel.** A lesson learned in one repository, team, or by one person is invisible to everyone else.
- **Existing notes are hard to find.** A question worded differently from the note that answers it won't find it.
- **Unmanaged knowledge decays.** Without curation, duplicate, outdated and contradictory notes pile up. There is also no way to tell a confirmed fact from a guess.

**Result:** repeated investigation, slower delivery and onboarding, and AI agents that can't use what the organization already knows.

## Solution approach

A shared memory service with a small, standard interface that any assistant or agent can call, for example through MCP. It covers the full knowledge lifecycle:

- **Recall before a task.** The assistant retrieves only the knowledge relevant to the current task, repository and question.
- **Capture after a task.** The assistant records new, non-obvious knowledge, such as fixes, gotchas, conventions and glossary terms, with its source.
- **Search by meaning.** Semantic retrieval, so a differently worded question still finds the right item.
- **Share across boundaries.** Knowledge is scoped to a repository, team or the whole organization, and visible across sessions, people and agents.
- **Curate on write.** Near-duplicates are merged, contradictions are flagged, and outdated items are marked as superseded instead of piling up.
- **Provenance and trust.** Every item shows where it came from and whether it was confirmed or only assumed. Confirmations and disputes update that status.
- **Safe by default.** Secrets, credentials and personal data are detected and rejected before anything is stored.
- **Fail-open.** If the memory is unavailable, assistants keep working normally.
- **Service assistant.** A Q&A interface over the same memory for new joiners and support staff, with answers that cite their sources.

## Target users / beneficiaries

- **Developers using AI assistants** (GitHub Copilot CLI and VS Code, Claude Code): they no longer re-explain the same context in every session.
- **Teams working across many repositories:** the same problems stop getting solved more than once.
- **Autonomous AI agents:** they benefit from, and add to, what humans and other agents already learned.
- **New joiners:** they get access to the team's unwritten knowledge from day one.
- **Support and service staff:** they get sourced answers to recurring "why does X behave like Y" questions.

## Knowledge item model

Each memory is a small, self-contained, sourced statement:

| Field | Description |
|---|---|
| Statement | One concise, non-obvious fact, written to make sense out of context |
| Type | `gotcha`, `fix`, `convention`, `glossary`, `decision`, `how-to` |
| Scope | `repository`, `team` or `organization`, plus related services and components |
| Provenance | Originating session, repository, file or commit, and the author (person or agent) |
| Confidence | `confirmed` (verified, e.g. a test passed or a human approved) or `assumed` (inferred, not verified) |
| Status | `active`, `superseded` (with a link to the newer item) or `disputed` (a conflict was flagged) |
| Signals | Created and updated timestamps, how often the item was retrieved and whether it helped, confirmations and disputes |
| Relations | Links to duplicates, contradictions and related items |

Never stored: secrets, credentials, tokens, personal data, customer data, raw chat transcripts.

## Demo scenarios (for judges)

1. **Cross-repository recall with different wording (core scenario)**
   - Repository A: a developer finds that the payments client fails with a vague timeout when a region setting is missing. Their assistant records a `gotcha` with a `fix`, marked `confirmed` and linked to the commit.
   - Repository B, a week later: another developer asks, *"Why does my checkout integration hang on startup?"*. The words overlap very little with the original note.
   - The assistant recalls the item first, cites where it came from, and applies the fix without re-investigating.
   - The same task is then shown without memory, and the two runs are compared on steps, time and tokens.
2. **Curation on write**
   - An agent tries to record a near-duplicate of an existing item, and the items are merged rather than duplicated.
   - Another agent records a statement that contradicts an existing one. The conflict is flagged, both items are marked `disputed`, and a human resolves it. The losing item becomes `superseded`.
3. **Service assistant and onboarding Q&A**
   - A new joiner asks, *"What does 'blue lane' mean here, and why do deploys to it need approval?"*
   - The answer is built from `glossary` and `decision` items and cites their sources. It shows which parts are confirmed and which are assumed.
4. **Safety and resilience**
   - An attempt to store a statement that contains an API key is rejected.
   - The memory service is stopped, and the assistant finishes the task anyway with a warning that memory is unavailable.

## Business and technical value

- Less repeated investigation and fewer repeated mistakes.
- Tasks that were solved before get done faster.
- Knowledge is shared between people and AI agents.
- Onboarding is faster, because unwritten team knowledge becomes available.
- Assistants use less context and fewer tokens, because they get only what's relevant.
- Knowledge can be trusted, because every item shows its source, confidence and status.

## Real-world application

Engineering organizations with many repositories and teams that adopt AI coding assistants and autonomous agents at scale.
The same memory serves internal developer support, onboarding, and service desks that answer recurring technical questions.

## Hackathon MVP scope

**In scope**
- A memory service with recall, capture, confirm and dispute operations, exposed through MCP.
- Semantic search with filtering by scope.
- Knowledge item model with provenance, confidence and status.
- Duplicate detection and contradiction flagging on write.
- Secret and personal-data filter on write.
- Fail-open behavior in the client.
- Integration with at least one coding assistant, plus a simple service-assistant Q&A interface.
- A seeded dataset and an evaluation harness for the success metrics below.

**Out of scope (future work)**
- Fine-grained per-user permissions and SSO beyond scoping by team.
- Bulk import of existing wikis, tickets and chat history.
- Automatic re-validation of old knowledge against current code.
- Production-grade scaling, retention policies, multi-region deployment.
- Analytics dashboards beyond the success metrics below.

## Success metrics / KPIs

| KPI | How it's measured | MVP target |
|---|---|---|
| Recall of paraphrased questions | Share of paraphrased test questions where the correct item is in the top 3 results | ≥ 80% |
| Effort saved on a repeated task | Tool calls, time and tokens with memory vs. without, on scripted tasks | ≥ 30% reduction |
| Duplicates caught | Share of seeded near-duplicates merged or linked instead of stored again | ≥ 90% |
| Conflicts flagged | Share of seeded contradictions marked `disputed` | ≥ 80% |
| Secret and personal-data leakage | Seeded secrets or personal data that got stored | 0 |
| Resilience | Assistant task completes when memory is down | 100% |
| Source coverage | Items with complete provenance and confidence | 100% |

## Future extensibility

- More consumers: CI agents, code review bots, incident and on-call assistants.
- Knowledge sources beyond sessions: merge requests, incident reports, tickets and design docs, with provenance.
- Automatic expiry and re-validation when the linked code changes.
- Richer governance: approval workflows, audit trails, access control per scope.
- Graph view of how knowledge relates across services and teams.
- Relevance learning from retrieval feedback (which items actually helped).

## Constraints

- No secrets, credentials or personal data may be stored.
- Only approved, internal AI services and infrastructure may be used.
- Assistants must keep working when the memory is unavailable.
- Knowledge is visible only within its declared scope. All writes are attributable and auditable.
