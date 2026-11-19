# Agent team

I will use GitHub Copilot CLI in a Codespace to orchestrate the custom agent team for Mona's Project Pulse dashboard.

- **Orchestrator** — **Claude Opus 5.5 (copilot)**. Coordinates the Planner, Coder, and Designer; turns the implementation plan into phased, non-overlapping assignments and verifies the integrated result without implementing it directly. Defined at `.github/agents/orchestrator.agent.md`.
- **Planner** — **Claude Opus 5.5 (copilot)**. Researches the repository and requirements, identifies dependencies and edge cases, and produces an ordered implementation plan with file assignments and validation expectations. Defined at `.github/agents/planner.agent.md`.
- **Coder** — **GPT-5.5 (copilot)**. Implements the dashboard code and support configuration with predictable structure, testable behavior, and explicit error handling. Defined at `.github/agents/coder.agent.md`.
- **Designer** — **Gemini 3.7 Flash (copilot)**. Owns UI/UX, accessibility, information architecture, interaction flow, responsive behavior, and the polished visual design of the dashboard. Defined at `.github/agents/designer.agent.md`.
