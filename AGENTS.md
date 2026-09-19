# AGENTS.md

This file indexes the documentation maintained in this repository, for both human contributors and coding agents working in it. Its only job is to point to the right document. When a new document is added under `docs/`, or a root-level document changes purpose, update the table below in the same change.
The rules in this document must be treated as legally binding.

## Rules

These rules are mandatory for all work in this repository, regardless of language or layer. They apply in addition to whatever a specific document under `docs/` says about architecture or style.

- Doc comments document only business logic. Never use a doc comment to restate what the code already says through its own structure and naming.
- Every class and every function must have doc comments.
- Inline comments are allowed only when they document business logic. An inline comment that explains a language construct or a mechanical step is not permitted.
- Never leave TODO or similar placeholder comments. If a piece of development is undertaken, it must be completed, not marked for later.
- Never perform source control operations: no commit, stage, branch, push, or similar action. No source control actions are allowed.
- SOLID principles must be followed and documented. Where a design applies one of the five principles, state which principle and why.
- Every feature must be documented under `docs/features/`. After each implementation/code change evaluate the changes that have been performed and:
  - If the change concerns existing documentation, update it.
  - If the change is totally new, evaluate whether it needs to go under a current document or if a new one needs to be created. Ask the user if unsure.
- Never write forward-looking statements in comments. Do not write things like "did X for now, will do Y later" or "will enhance X later." A comment describes the code as it exists, not a plan for its future.
- Never mix simulated data with real API responses and data handling. A code path either works against real data end to end or is clearly and separately a simulation/test path; the two must never blend.
- Do not assume anything. If the slightest thing is unclear or ambiguous, ask questions until it is resolved before proceeding.

## Security

| Document | Description |
|---|---|
| [`docs/SECURITY.md`](docs/SECURITY.md) | Defense-in-depth security reference: identity and access (authentication, authorization, RBAC), application-layer defense (RASP, WAF), data protection (ALE, envelope encryption, key management), database integrity (RLS, atomic transactions), network/infrastructure controls, and observability/response process. |

## Frontend and backend engineering guidelines

These two documents are standalone technical references. They are not tied to this repository's current Python pipeline; they define the architecture and coding standards to follow if or when a Flutter frontend or an ASP.NET backend is built under this organization.

| Document | Description |
|---|---|
| [`docs/FLUTTER_ARCHITECTURE_AND_GUIDELINES.md`](docs/FLUTTER_ARCHITECTURE_AND_GUIDELINES.md) | Flutter frontend architecture (layered MVVM per Flutter's official app-architecture guidance), modularization patterns, advanced performance practices, and Dart code style. |
| [`docs/ASPNET_ARCHITECTURE_AND_GUIDELINES.md`](docs/ASPNET_ARCHITECTURE_AND_GUIDELINES.md) | ASP.NET Core backend architecture (Hexagonal / ports and adapters), EF Core code-first with PostgreSQL, EF Core and ASP.NET Core performance, transaction/RBAC/isolation security, and C# code guidelines. |

## Maintenance note

Keep this file to an index. Document content belongs in the linked files, not here. When a document is renamed, moved, or removed, update or remove its row in the same change so links never go stale.
