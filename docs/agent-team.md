# Agent team

I will use this custom agent team to build Mona's Project Pulse dashboard:

| Agent | Target model | Responsibility | Definition |
| --- | --- | --- | --- |
| Orchestrator | Claude Opus 5.5 (copilot) | Coordinates the specialists, assigns clear file scopes, manages work dependencies and parallel phases, and verifies the integrated result. | `.github/agents/orchestrator.agent.md` |
| Planner | Claude Opus 5.5 (copilot) | Researches the repository and requirements, then creates an implementation plan with steps, file assignments, dependencies, edge cases, and validation expectations. | `.github/agents/planner.agent.md` |
| Coder | GPT-5.5 (copilot) | Implements application logic and runnable-app support, keeping changes explicit, testable, and within the assigned scope. | `.github/agents/coder.agent.md` |
| Designer | Gemini 3.7 Flash (copilot) | Shapes the dashboard's user experience, visual hierarchy, accessibility, interaction flow, and responsive design. | `.github/agents/designer.agent.md` |

I am using GitHub Copilot CLI in a Codespace to orchestrate the work.
