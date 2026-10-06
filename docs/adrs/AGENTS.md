# Architecture decision records

Rules for writing ADRs in this directory.

## Keep it really short

An ADR answers three questions and nothing else:

1. **Decision:** what was decided, in one or two sentences. If it still needs validating, add the condition for accepting it.
2. **Why:** the reasons, as short bullets.
3. **Considered and declined:** each alternative and why it was declined, one bullet each.

Leave out context essays, consequences sections and repeated pros and cons.
If a point doesn't support the decision or explain a rejection, it doesn't belong.

## Format

- **File name:** `adr-NNN-short-slug.md`, using the next number after the highest existing ADR.
- **Title:** `# ADR-NNN: <decision topic>`.
- **Header line:** `**Status:** Proposed | Accepted | Superseded by ADR-NNN · **Date:** YYYY-MM-DD`.
- **Changing a decision:** write a new ADR and mark the old one as superseded, rather than rewriting it.

See [adr-001-knowledge-store.md](adr-001-knowledge-store.md) for an example.
