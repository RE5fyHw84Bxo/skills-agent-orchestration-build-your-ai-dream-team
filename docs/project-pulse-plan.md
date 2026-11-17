# Project Pulse Dashboard Implementation Plan

## Goal

Build a lightweight static dashboard that lets Mona's contributors quickly see
which projects are active, who owns them, each project's status, recent
activity, and priority or risk. The interface should feel polished, readable,
accessible, and useful on both desktop and smaller screens. The VS Code launch
configuration must serve the app from `app/` and open its `index.html`, rather
than displaying a directory listing.

## Repository context and constraints

- `.github/project-pulse-brief.md` defines the required app files and project
  data fields: `name`, `owner`, `status`, `recentActivity`, and `priority` in a
  top-level `projects` array.
- `.github/steps/3-step.md` further requires a visible Project Pulse frontend,
  project cards, the exact title `Project Pulse`, references to both CSS and
  JSON, the `.dashboard` and `.project-card` CSS hooks, and a
  **Run Project Pulse Dashboard** launch configuration.
- `.github/agents/designer.agent.md` assigns visual hierarchy, accessibility,
  and responsive design decisions to Designer. The Coder definition permits
  creating the assigned VS Code launch support.
- The repository currently has no dashboard implementation under `app/` and no
  `.vscode/launch.json`. Existing `.vscode/tasks.json` is for the exercise
  terminal; leave it unchanged.
- Keep this work to the four assigned deliverables below. Do not stage, commit,
  or push files.

## Roles and file ownership

- **Orchestrator:** Use this plan to coordinate the specialists, keep their
  file scopes explicit, resolve integration questions, and validate the
  completed app.
- **Designer:** Define the information hierarchy, card layout, status and
  priority visual treatments, accessible color/contrast and focus states, and
  responsive behavior. Own `app/styles.css`; give Coder markup and interaction
  guidance before implementation.
- **Coder:** Implement the static page and data loading, provide representative
  project data, and configure the VS Code preview. Own `app/index.html`,
  `app/project-data.json`, and `.vscode/launch.json`. Integrate Designer's
  decisions without changing the assigned CSS file unless the Orchestrator
  explicitly reassigns it.

| File | Owner | Deliverable |
| --- | --- | --- |
| `app/styles.css` | Designer | Polished responsive dashboard styles, including `.dashboard` and `.project-card`, readable spacing and typography, rounded cards, shadows, and clear status/priority treatments. |
| `app/project-data.json` | Coder | Valid JSON with a top-level `projects` array and multiple realistic sample projects; every item has `name`, `owner`, `status`, `recentActivity`, and `priority`. |
| `app/index.html` | Coder | Semantic, accessible page titled exactly `Project Pulse`, links `styles.css`, loads `project-data.json`, and renders a visible `.project-card` per project with the required details. |
| `.vscode/launch.json` | Coder | Strict JSON containing **Run Project Pulse Dashboard**, serving from `${workspaceFolder}/app` with `python3 -m http.server 5500` and opening `http://localhost:%s/index.html`. |

## Ordered implementation phases

### 1. Agree on the UI and data contract

Designer and Coder align on the card content order and display formats for
status and priority. Confirm that labels are human-readable while preserving
the required data fields. Orchestrator confirms the deliverables and ownership
above.

**Dependency:** This agreement precedes page/data integration. Designer may
begin a style direction independently, but final CSS should align with the
agreed markup hooks.

### 2. Produce independent foundations (parallel)

- **Designer — `app/styles.css`:** Create the responsive visual system and
  styles for the agreed semantic elements and class hooks. Include visible
  keyboard focus, sufficient contrast, and distinct but text-labeled status
  and priority treatments.
- **Coder — `app/project-data.json`:** Create parseable, representative sample
  data with more than one project and all required fields. Keep values
  consistent with the statuses and priorities supported by the UI.
- **Coder — `.vscode/launch.json`:** Add the assigned preview configuration as
  strict JSON. Set `cwd` to `${workspaceFolder}/app`, start
  `python3 -m http.server 5500`, and configure `serverReadyAction` to open
  `http://localhost:%s/index.html`.

These deliverables can be prepared in parallel after the contract is agreed:
the JSON and launch configuration are independent, and Designer can develop
the CSS while Coder prepares them. Avoid two agents editing the same file.

### 3. Implement and integrate the page

**Coder — `app/index.html`:** Build a semantic page with the exact title
`Project Pulse`, a clear heading and concise context, and a project-list
container using `.dashboard`. Reference `styles.css`; load `project-data.json`
and render one `.project-card` per project. Show name, owner, status,
`recentActivity`, and priority in accessible text, not color alone. Handle
loading and failure states in the page.

**Dependencies:** The agreed markup hooks and data schema from Phase 1 are
prerequisites. The page consumes the CSS and JSON from Phase 2, so the final
integration check follows their completion. Coder can implement against the
agreed contract before those files are finished.

### 4. Integrate and validate sequentially

Orchestrator reviews the four assigned files together, checks consistency
between data values, markup, and styles, then runs the checks below. Fix
integration issues in the owning file and repeat affected checks. Do not
modify unrelated exercise files.

## Dependencies and parallel-work decisions

- The project data schema and card information hierarchy are shared contracts;
  agree on them before parallel implementation to prevent mismatched fields or
  status values.
- `app/project-data.json` and `.vscode/launch.json` can be implemented in
  parallel with Designer's `app/styles.css`.
- `app/index.html` depends on the agreed contract and must integrate the CSS
  hooks and JSON fields. It can be coded while CSS/data are being prepared,
  but the integrated app cannot be validated until all three app files exist.
- The launch configuration can be created independently, but its end-to-end
  check is sequential: start it, wait for the server/browser action, and confirm
  the page is served from `app/`.
- Only one owner edits each file at a time; Orchestrator handles review and
  integration rather than making overlapping edits.

## Edge cases and risks

- Empty `projects` array: show a clear empty-state message instead of a blank
  dashboard.
- Missing, malformed, or unavailable JSON: show a useful error state; do not
  silently leave the project list blank. Confirm the page is served over HTTP,
  since browser `fetch()` of JSON may fail when opening the HTML directly via
  `file://`.
- Incomplete or unexpected project values: render safe fallbacks for missing
  owner/activity and avoid assuming every status or priority is a known value.
- Long project names, summaries, or activity text: ensure cards wrap without
  horizontal overflow.
- Status and priority must be distinguishable without relying on color alone;
  preserve readable contrast and keyboard focus visibility.
- Port `5500` may already be occupied, and a Python 3 executable may be
  unavailable. Surface these as preview setup issues; keep the required launch
  command and port unless the exercise requirements are changed.
- A launch configuration may require the VS Code Python debug adapter for
  `serverReadyAction` behavior. Verify the setup in the target Codespace and
  report any extension prerequisite rather than silently changing the required
  preview behavior.

## Validation expectations

1. Parse `app/project-data.json` with `python3 -m json.tool`; verify it has a
   top-level `projects` array and each project has all five required fields.
2. Parse `.vscode/launch.json` with `python3 -m json.tool`; confirm the
   configuration is named **Run Project Pulse Dashboard**, uses the required
   working directory and command, and opens
   `http://localhost:%s/index.html`.
3. Inspect `app/index.html` for the exact document title, CSS and JSON
   references, accessible semantic structure, loading/error/empty states, and
   rendered project cards with the required fields.
4. Inspect `app/styles.css` for `.dashboard`, `.project-card`, responsive
   layout, readable spacing, `border-radius`, `box-shadow`, contrast, and
   focus treatment.
5. Run **Run Project Pulse Dashboard** from VS Code. Confirm the server serves
   `app/`, the browser opens `/index.html` rather than a directory listing,
   project cards appear from JSON, and the layout remains usable at narrow
   viewport widths. Stop the preview server after checking.
6. If a browser/UI check is unavailable, state that limitation and complete
   the static JSON/configuration and source checks instead.

## Open questions

- What real project names, owners, activity summaries, statuses, and priority
  levels should replace the representative sample data?
- Should status and priority use a fixed vocabulary (for example, Active /
  At risk / Blocked and Low / Medium / High), or should the data support
  additional labels?
- Is the VS Code Python debug adapter available in the target Codespace, or
  should the exercise provide/install it for the required
  `serverReadyAction` launch flow?
