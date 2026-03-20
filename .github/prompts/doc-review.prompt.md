---
name: doc-review
description: >
  Run a genuine reader simulation and quality pass on the current draft.
  The review agent operates with full context isolation — it sees only the
  finished document, finds issues, applies fixes, and returns a revised
  document with a clear report. No back-and-forth required.
agent: doc
tools: ["agent", "read", "search"]
---

Invoke the `doc` orchestrator agent to run the `/doc review` stage.

Pass only the current document and its meta (type, audience, goal) to the
`doc-review` sub-agent. The sub-agent must have no access to authoring
history, prior drafts, or context from the writing process. This isolation
is what makes the reader simulation genuine.
