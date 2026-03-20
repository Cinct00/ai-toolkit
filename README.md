# AI Toolkit

A collection of agent orchestration systems, prompts, and tools for building structured AI-assisted workflows.

---

## Overview

AI Toolkit is an open-source repository of reusable components for agent-based AI workflows. The goal is to provide well-structured, composable building blocks that handle common patterns: multi-stage pipelines, stateless sub-agents, context management, and user-facing skill interfaces.

Components are organized around `.github/agents/` and `.github/prompts/` — conventions supported natively by Claude Code.

---

## Contents

### Agents

Agents are stateless, single-purpose units invoked by orchestrators or directly by users. Each agent has a clearly scoped role and defined input/output contract.

| Agent                                                        | Description                                                                                                                                                            |
| ------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [doc-orchestrator](.github/agents/doc-orchestrator.agent.md) | Coordinates the document co-authoring pipeline. Routes `/doc start`, `/doc draft`, and `/doc review` commands to the appropriate sub-agents and manages session state. |
| [doc-context](.github/agents/doc-context.agent.md)           | Processes raw user input into a structured context package — document type, audience, goals, assumed structure, and clarifying questions.                              |
| [doc-draft](.github/agents/doc-draft.agent.md)               | Generates complete documents from a structured context package. Supports initial drafting and iterative refinement rounds.                                             |
| [doc-review](.github/agents/doc-review.agent.md)             | Simulates a genuine reader with zero authoring context. Identifies structural issues, redundancy, and gaps — then applies fixes and returns a revised document.        |

### Prompts

Prompts are user-invocable skill definitions that activate agent workflows. They serve as the entry point for end users.

| Prompt                                                 | Description                                                                                                                                     |
| ------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------- |
| [doc-coauthor](.github/prompts/doc-coauthor.prompt.md) | Activates the document co-authoring system. Guides users through context intake, drafting, and review using the three-stage sub-agent pipeline. |

---

## Systems

### Document Co-Authoring

A multi-agent pipeline for producing high-quality structured documents (RFCs, PRDs, decision docs, architecture specs, technical proposals). See [docs/doc-coauthor.md](docs/doc-coauthor.md) for full documentation.

---

## Getting Started

This toolkit is designed for use in AI tools like IDEs and CLIs. Agents in `.github/agents/` and prompts in `.github/prompts/` are automatically available when working within this repository.

1. Clone the repo into your project or use it as a reference for your own `.github/` directory.
2. Open your project in the repository root.
3. Invoke a skill prompt (e.g. `doc-coauthor`) or reference an agent directly.

---

## Contributing

Contributions are welcome. The goal is to keep each component focused, well-documented, and composable. When adding new agents or prompts, follow the existing conventions:

- Agents live in `.github/agents/` with a `.agent.md` suffix.
- Prompts live in `.github/prompts/` with a `.prompt.md` suffix.
- Each component should have a clear, single responsibility.

---

## License

[MIT License](LICENSE)
