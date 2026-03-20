---
name: doc-draft
description: >
  Generate a complete first draft from the context gathered in /doc start.
  The drafting agent simulates brainstorming, curation, and self-editing
  internally — returning a full polished document in a single response.
  Optionally paste clarifying question answers or additional context below.
agent: doc
tools: ["agent", "read", "search"]
---

Invoke the `doc` orchestrator agent to run the `/doc draft` stage.

Use the context summary and structure already established in this session.
If the user has added any clarifying question answers or additional context
after `/doc start`, include that in the Draft Package passed to the
`doc-draft` sub-agent.

If no prior `/doc start` context exists in this session, ask the user to
provide their document type, audience, goal, and any context they have —
then proceed directly to drafting without requiring a separate `/doc start`.
