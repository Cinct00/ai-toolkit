# Document Co-Authoring System

A multi-agent pipeline for producing structured, high-quality documents through a three-stage workflow: context intake, drafting, and reader simulation review.

---

## Overview

The document co-authoring system separates the writing process into three isolated stages, each handled by a dedicated sub-agent. The orchestrator manages state and routes commands — sub-agents have no memory of prior invocations and work only with what they are given.

This isolation is intentional: the review stage simulates a reader who has never seen the authoring process, which produces more genuine feedback than a reviewer with full context.

**Supported document types:** Decision docs, RFCs, PRDs, Architecture specs, Technical specs, Proposals

---

## Architecture

```
User
 │
 ▼
doc-coauthor (prompt / skill entry point)
 │
 ▼
doc-orchestrator (state manager + router)
 ├── /doc start  ──▶  doc-context  (context processing)
 ├── /doc draft  ──▶  doc-draft    (document generation)
 └── /doc review ──▶  doc-review   (reader simulation + QA)
```

### Components

| Component | File | Role |
|-----------|------|------|
| Entry point | [doc-coauthor.prompt.md](../.github/prompts/doc-coauthor.prompt.md) | User-facing skill that activates the orchestrator |
| Orchestrator | [doc-orchestrator.agent.md](../.github/agents/doc-orchestrator.agent.md) | Routes commands, holds session state |
| Stage 1 | [doc-context.agent.md](../.github/agents/doc-context.agent.md) | Parses raw input into structured context |
| Stage 2 | [doc-draft.agent.md](../.github/agents/doc-draft.agent.md) | Generates and refines the document |
| Stage 3 | [doc-review.agent.md](../.github/agents/doc-review.agent.md) | Reader simulation and structural QA |

---

## Workflow

### Stage 1 — `/doc start`

The user provides raw context about what they want to write. The orchestrator passes this to `doc-context`, which returns:

- **Context Summary** — a 2-4 sentence briefing on the document's purpose
- **Assumed Document Structure** — numbered sections with rationale for each
- **Key Assumptions** — inferences made from incomplete input
- **Risks and Edge Cases** — topics the document should address
- **Clarifying Questions** — up to 5 genuine gaps that inference cannot fill

The user reviews the output and either answers the clarifying questions or proceeds directly to drafting.

### Stage 2 — `/doc draft`

The orchestrator assembles a Draft Package (context summary, assumptions, structure, and any additional user input) and passes it to `doc-draft`. The agent generates a complete document in a single pass using a disciplined process:

1. Generate 10-15 candidate points per section
2. Cull to the 4-6 strongest based on audience and goal
3. Check for redundancy with adjacent sections
4. Draft the section
5. Self-edit pass (remove filler, strengthen weak sentences)
6. Cross-document pass (flow, contradictions, tone consistency)

The output includes:
- Full document in clean markdown
- "What I decided" — 3-5 bullets on structural and content choices

**Refinement rounds** are supported between draft and review. The user provides feedback; the orchestrator passes the current draft and feedback to `doc-draft` for a targeted revision pass.

### Stage 3 — `/doc review`

The orchestrator passes the current draft to `doc-review` with zero authoring context. The agent:

1. Generates 8-10 realistic reader questions
2. Attempts to answer each using only the document content
3. Marks each as ✓ (clear), △ (partial), or ✗ (missing/misleading)
4. Runs structural quality checks:
   - Internal contradictions
   - False assumptions about reader knowledge
   - Redundancy across sections
   - Filler sentences
   - Inconsistent tone or register
   - Weak or missing transitions
   - Sections that don't earn their place
5. Fixes all issues it can resolve without author knowledge
6. Escalates only issues requiring factual input or unresolved decisions

The output includes:
- Reader simulation table (questions + answer quality)
- Issues found and fixed (what changed and why)
- Escalations (only if necessary)
- Full revised document in clean markdown

---

## Session State

The orchestrator tracks the following across the session:

```
doc_type         — type of document being written
audience         — intended readers
goal             — what the document needs to achieve
constraints      — length, format, scope limits
context_summary  — output from doc-context stage
assumptions      — inferences doc-context made
structure        — agreed section outline
current_draft    — most recent version of the document
review_complete  — boolean, set after /doc review runs
```

Sub-agents are stateless — they receive only what they need for their stage and nothing more.

---

## Design Principles

**Isolation over convenience.** Each sub-agent receives a scoped context package rather than the full conversation history. This prevents context bleed and keeps each stage predictable.

**No orchestrator authoring.** The orchestrator routes and stores — it never writes content, brainstorms, or analyzes. All creative and analytical work is delegated to sub-agents.

**Verbatim output passing.** The orchestrator presents sub-agent output verbatim. It does not summarize, compress, or editorialize.

**Genuine reader simulation.** The review agent is explicitly isolated from the authoring context so its reader simulation reflects what an actual reader would experience.

---

## Usage Example

```
> Use doc-coauthor

[Orchestrator activates, explains workflow]

> /doc start
I need to write an RFC for replacing our current job queue with a Kafka-based system.
The audience is the platform team. Main concern is migration risk.

[doc-context returns structured summary, assumed structure, questions]

> /doc draft

[doc-draft returns full RFC draft]

> Make the migration risk section more concrete, add a rollback plan

[doc-draft applies refinement]

> /doc review

[doc-review returns reader simulation + revised document]
```
