# Project Pulse Dashboard Plan

## Goal

Build a lightweight static dashboard that helps contributors scan project ownership, status, recent activity, priority or risk, and a short contributor-friendly summary. It must display the dashboard UI—not a directory listing—when launched from VS Code.

## Responsibilities and file assignments

- **Orchestrator:** Coordinate the work, agree on the shared data/markup contract, delegate the tasks below, and reconcile the result.
- **Planner:** Plan-only; identify phases, dependencies, ownership, edge cases, and validation. Does not implement files.
- **Designer:** Own `app/styles.css` and visual/accessibility decisions: responsive card layout, hierarchy, spacing, status badges, and readable styling. Include `.dashboard`, `.project-card`, `border-radius`, and `box-shadow`.
- **Coder:** Own `app/index.html`, `app/project-data.json`, and `.vscode/launch.json`. Implement the page and data loading, and configure the launch target.

## Ordered phases and dependencies

1. **Agree on the contract.** The Orchestrator, Designer, and Coder align on the top-level `projects` array and project fields (`name`, `owner`, `status`, `recentActivity`, `priority`), plus the markup hooks the stylesheet will target. Include a short summary for contributors in the dashboard content.
2. **Implement in parallel after the contract is agreed.** The Designer builds the stylesheet in `app/styles.css`. The Coder builds `app/index.html`, `app/project-data.json`, and `.vscode/launch.json`; the HTML must use the exact title `Project Pulse`, reference `styles.css` and `project-data.json`, and render visible `.project-card` cards showing status, recent activity, and priority.
3. **Integrate and review.** The Orchestrator checks the stylesheet and markup contract together, resolves mismatches, and confirms all required paths and launch settings are present.
4. **Validate the dashboard.** Check the files and JSON, then manually preview using the launch configuration and confirm the Project Pulse UI opens.

## Parallel work decisions

After agreeing on the shared JSON and markup contract in phase 1, Designer and Coder can work independently: Designer owns only `app/styles.css`; Coder owns `app/index.html`, `app/project-data.json`, and `.vscode/launch.json`. Integration review depends on both tracks being complete.

## Validation expectations

- Confirm these required paths exist: `app/index.html`, `app/styles.css`, `app/project-data.json`, and `.vscode/launch.json`.
- Confirm `app/index.html` has the exact title `Project Pulse`, references both `styles.css` and `project-data.json`, and renders `.project-card` cards with each project's status, `recentActivity`, and priority.
- Confirm `app/styles.css` includes `.dashboard`, `.project-card`, `border-radius`, and `box-shadow`, with a responsive, readable layout.
- Confirm `app/project-data.json` is valid JSON with a top-level `projects` array; each project has `name`, `owner`, `status`, `recentActivity`, and `priority`.
- Confirm `.vscode/launch.json` is strict JSON and includes the exact launch name `Run Project Pulse Dashboard`. It must run `python3 -m http.server 5500` with working directory `${workspaceFolder}/app` and open `http://localhost:%s/index.html`.
- Manually start **Run Project Pulse Dashboard** and verify the browser shows the dashboard from `index.html`, not a server directory listing. Stop the preview server afterward.
- The Step 3 workflow assertions provide additional checks for file presence, required keyphrases, JSON validity, card fields, CSS selectors, launch name, and `index.html` target.

## Edge cases

- Handle an empty `projects` array with a useful empty state rather than a blank dashboard.
- If loading or parsing `project-data.json` fails, show a clear, contributor-friendly error state; do not leave the dashboard silently empty.
- Ensure status and priority remain understandable without color alone (for example, visible text labels or icons with accessible names).
- Keep content readable on narrow screens and when project names or activity summaries are long.
