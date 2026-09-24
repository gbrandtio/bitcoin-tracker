# AGENTS.md

This file indexes the documentation maintained in this repository, for both human contributors and coding agents working in it. Its only job is to point to the right document. When a new document is added under `docs/`, or a root-level document changes purpose, update the table below in the same change. The rules in this document must be treated as legally binding.

## Rules

These rules are mandatory for all work in this repository, regardless of language or layer. They apply in addition to whatever a specific document under `docs/` says about architecture or style.

- Doc comments document only business logic. Never use a doc comment to restate what the code already says through its own structure and naming. Every class and every function must have doc comments.
- Inline comments are allowed only when they document business logic. An inline comment that explains a language construct or a mechanical step is not permitted.
- Never leave TODO or similar placeholder comments. If a piece of development is undertaken, it must be completed, not marked for later.
- Never write forward-looking statements in comments. Do not write things like "did X for now, will do Y later" or "will enhance X later." A comment describes the code as it exists, not a plan for its future.
- Never leave something "to be done later". Never write that "something will be done in the future" or similar.
- Never perform source control operations: no commit, stage, branch, push, or similar action. No source control actions are allowed.
- SOLID principles must be followed and documented. Where a design applies one of the five principles, state which principle and why.
- Never use mdashes. Whatever is being written must be written in a simple, understandable, neutral language.
- Every feature must be documented under `docs/features` on a separate .md document. If documentation exists for a current feature, it must be updated if the feature has been updated.
- Never mix simulated data with real API responses and data handling. A code path either works against real data end to end or is clearly and separately a simulation/test path; the two must never blend.
- Do not assume anything. If the slightest thing is unclear or ambiguous, ask questions until it is resolved before proceeding.
- Strict GDPR compliance is mandatory. Every change that collects, stores, derives, logs, caches, sends or displays personal data, adds a third-party service or SDK, or touches ingested third-party text or any LLM layer must follow the binding rules in `docs/GDPR.md` (section 3) and pass its checklist (section 20) before it is considered done.
- Strict security measures are mandatory. Never bypass security and strictly follow `docs/SECURITY.md`.
- Test coverage must be equal or greater than 90%.
- The actual code implementation must follow strictly what the project's documentation is defining.

## Index

Before proceeding with any action, you must gather context from the following files. Keep the index updated always if new files / subdirectories / file structures are introduced.

- `docs/ASPNET_API_ARCHITECTURE.md`: Generic architecture and coding-standards rules for the backend API.
- `docs/FLUTTER_ARCHITECTURE.md`: Generic architecture and coding-standards rules for the frontend.
- `docs/GDPR.md`: GDPR rules that must be followed.
- `docs/SECURITY.md`: Cybersecurity rules that must be followed.
- `docs/features/`: Documentation of all the features relevant to the project.
  -
- `docs/uiux/`: Documentation of the UI/UX guidelines relevant to the project.

## Maintenance note

Keep this file to an index. Document content belongs in the linked files, not here. When a document is renamed, moved, or removed, update or remove its row in the same change so links never go stale.
