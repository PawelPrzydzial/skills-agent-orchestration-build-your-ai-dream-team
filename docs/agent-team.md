# Agent team for Mona's Project Pulse dashboard

We are using the custom GitHub Copilot CLI agent team in a Codespace to orchestrate the work for Mona's Project Pulse dashboard.

## Team members

- Planner — model: Claude Opus 4.7 (copilot)
  - Responsibility: researches the repository, reads the relevant files and docs, identifies risks and edge cases, and produces the implementation plan the orchestrator can turn into execution phases.
  - Definition: `.github/agents/planner.agent.md`

- Orchestrator — model: Claude Opus 4.7 (copilot)
  - Responsibility: breaks the plan into phases, assigns work to specialized agents, coordinates parallel and sequential tasks, and verifies the integrated result before reporting progress and completion.
  - Definition: `.github/agents/orchestrator.agent.md`

- Designer — model: Gemini 3.1 Pro (copilot)
  - Responsibility: focuses on the dashboard's UI/UX, accessibility, information hierarchy, interaction flow, and polished visual design for the Project Pulse experience.
  - Definition: `.github/agents/designer.agent.md`

- Coder — model: GPT-5.5 (copilot)
  - Responsibility: implements the code changes, fixes bugs, and completes the assigned logic within the file scope given by the orchestrator while validating the result before completion.
  - Definition: `.github/agents/coder.agent.md`

## Summary

This agent team gives us a clear separation of concerns: the Planner researches and plans, the Orchestrator coordinates execution, the Designer shapes the user experience, and the Coder implements the technical work. The definitions all live under the repository's `.github/agents/` folder, and we are running the workflow through the GitHub Copilot CLI inside a Codespace.

Orchestrator, Planner, Coder, and Designer.
The model assigned to each agent.
The responsibility of each agent.
The .github/agents/ file for each agent.
How the team will work together to build Project Pulse.