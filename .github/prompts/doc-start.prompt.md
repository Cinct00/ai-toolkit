---
name: doc-start
description: >
  Start a new document co-authoring session. Presents a structured intake form,
  processes your context, and returns a clear document structure with assumptions
  and optional clarifying questions — all in a single response.
agent: doc
tools: ["agent", "read", "search"]
---

Invoke the `doc` orchestrator agent to run the `/doc start` stage.

The user is beginning a new document co-authoring session. Present the context
intake form and process their input through the `doc-context` sub-agent when
they respond.
