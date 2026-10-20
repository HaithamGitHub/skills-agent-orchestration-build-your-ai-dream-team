# Agent team

I will use GitHub Copilot CLI in a Codespace to orchestrate this team to plan, design, and build Mona's Project Pulse dashboard.

| Agent | Model | Responsibility | Definition |
|---|---|---|---|
| Orchestrator | Claude Opus 5.5 (copilot) | Coordinates the work, gets a plan from the Planner, assigns specialists clear file scopes and phases, integrates their work, and reports the result. It delegates rather than implementing. | `.github/agents/orchestrator.agent.md` |
| Planner | Claude Opus 5.5 (copilot) | Researches the repository, identifies requirements, risks, dependencies, and validation expectations, and produces an actionable implementation plan. It does not write code. | `.github/agents/planner.agent.md` |
| Designer | Gemini 3.7 Flash (copilot) | Guides Project Pulse usability, accessibility, information hierarchy, responsive behavior, and visual design, including the dashboard's card-based presentation. | `.github/agents/designer.agent.md` |
| Coder | GPT-5.5 (copilot) | Implements assigned dashboard and support files, follows the plan and design direction, and validates the result. | `.github/agents/coder.agent.md` |

The Orchestrator will use the Planner's file assignments and dependencies to sequence work, delegate design and implementation to the Designer and Coder, then review the integrated dashboard.
