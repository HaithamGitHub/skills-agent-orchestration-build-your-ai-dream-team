# Repository instructions

## Build, test, and lint

This repository is an exercise template, not the finished Project Pulse app. There is no application build, package manager, standalone test framework, or configured linter.

- Run the repository's validation checks with `./scripts/validate-exercise.sh`. It checks workflow YAML, JSON and shell syntax, exercise wording and file expectations, and some Codespace setup details. It requires Bash, Ruby, and Python 3; the Copilot CLI check is skipped when `copilot` is not installed.
- The validation script has no individual-test selector. For a focused syntax check, run the relevant command directly, for example `bash -n .devcontainer/postCreate.sh`, `python3 -m json.tool .devcontainer/devcontainer.json >/dev/null`, or `ruby -e "require 'yaml'; YAML.load_file(ARGV.fetch(0))" .github/workflows/1-step.yml`.

## Architecture

The repository packages a four-step GitHub Skills exercise that teaches learners to orchestrate a custom agent team from GitHub Copilot CLI in a Codespace. The learner-facing instructions are in `.github/steps/`; the four agent definitions in `.github/agents/` specify separate Planner, Designer, Coder, and Orchestrator roles. The expected learner outputs evolve from `docs/agent-team.md` to `docs/project-pulse-plan.md`, the static dashboard under `app/`, `.vscode/launch.json`, and `docs/final-handoff.md`.

GitHub Actions workflows implement the exercise progression: `0-start-exercise.yml` opens the exercise, then `1-step.yml` through `4-step.yml` validate each step's outputs and post the next instructions. Each step workflow is triggered by changes to its designated learner output and advances by disabling itself and enabling the next step; keep these paths and workflow progression aligned when changing exercise content. `scripts/validate-exercise.sh` is the local counterpart for key structure, content, syntax, and Codespace assumptions.

## Repository-specific conventions

- Treat expected learner files as exercise outputs, not files to add to the template. The workflow checks and local validator rely on learners creating them; the validator explicitly checks that key learner outputs are not tracked in the template.
- Exercise workflows intentionally check specific phrases, paths, and data fields with `skills/action-keyphrase-checker@v2` and `skills/exercise-toolkit/actions/file-exists@v0.9.3`. When editing a prompt or expected output, update the corresponding workflow check and local validator together.
- Keep the agent responsibilities distinct: the Orchestrator coordinates and delegates but does not implement; the Planner researches and produces plans without writing code; the Designer owns design guidance and assigned design work; the Coder implements within its assigned file scope. The Orchestrator should specify file assignments and avoid overlapping scopes in parallel phases.
- The Project Pulse deliverable is a static frontend: `app/index.html` presents project data from `app/project-data.json`, styled by `app/styles.css`; `.vscode/launch.json` serves the `app` directory and opens `index.html`. Its expected project fields and CSS/markup hooks are asserted in the Step 3 workflow and validator.
- Preserve the learner-facing prompts' required literal names, paths, and key phrases because automated checks depend on them. Prompt examples in `.github/steps/` use quoted blockquotes and wrapped prompt lines; the validator enforces those formatting details.
- Codespace setup installs or verifies Copilot CLI in `.devcontainer/postCreate.sh`; `.vscode/tasks.json` starts the CLI on folder open via `.devcontainer/postStart.sh`. Keep setup instructions and validation checks consistent if changing this entry flow.
- `.github/mcp.json` configures the Playwright MCP server for browser-based inspection of the learner-built dashboard. Copilot CLI loads this shared project configuration after repository trust is granted.
