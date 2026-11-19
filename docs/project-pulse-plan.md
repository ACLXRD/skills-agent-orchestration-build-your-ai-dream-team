# Project Pulse Implementation Plan

## Summary

Build a polished, dependency-free static dashboard in `app/` that presents Project Pulse project records. The dashboard will use a semantic HTML shell, JSON-backed project data, responsive CSS, and a VS Code launch configuration that serves `app/` on port 5500 and opens `index.html`.

The work follows the repository's specialist model:

- **Planner** defines scope, dependencies, ownership, and validation.
- **Designer** owns UI/UX direction and accessibility; the Coder implements styling within the assigned files.
- **Coder** implements the HTML, data, stylesheet, and launch configuration.
- **Orchestrator** coordinates phases and verifies the integrated result.

## Ordered Implementation Steps

### 1. Establish the project data contract

**Owner:** Coder

Create `app/project-data.json` with a top-level `projects` array and a small set of representative projects. Each project should use consistent, predictable field names:

- `name`
- `owner`
- `status`
- `recentActivity`
- `priority`

Use stable status and priority values so the UI can apply deterministic badge treatments.

**Why first:** Markup and rendering depend on this JSON shape.

### 2. Build the dashboard structure

**Owner:** Coder

Create `app/index.html` with:

- An exact visible heading and document title of **Project Pulse**
- Semantic page structure for the dashboard
- Project card markup or a JavaScript rendering target
- A reference to `styles.css`
- A fetch or equivalent browser loading path for `project-data.json`
- Explicit loading and data-error states
- Status, priority, owner, and recent activity presentation for every rendered project

Use `.dashboard` for the dashboard container and `.project-card` for each project card. Keep DOM hooks predictable for the Designer-guided stylesheet.

### 3. Apply the visual system and responsive behavior

**Owner:** Designer for decisions; Coder for implementation

**Designer responsibilities**

- Define the information hierarchy, card layout, typography, spacing, and color system.
- Specify clear status badges and priority treatments.
- Design visible rounded-corner cards with shadows and readable contrast.
- Define responsive behavior for desktop, tablet, and mobile widths.
- Define keyboard focus states and semantic or screen-reader considerations.
- Avoid decorative choices that reduce usability or accessibility.

**Coder responsibilities**

- Implement the approved design in `app/styles.css`.
- Keep CSS selectors deterministic and maintainable.
- Adjust only markup in `app/index.html` if needed for the agreed visual or accessibility structure.
- Do not introduce build tooling or a framework unless explicitly assigned.

### 4. Add the runnable VS Code configuration

**Owner:** Coder

Create `.vscode/launch.json` as strict JSON with no comments. Add a configuration named exactly **Run Project Pulse Dashboard** that:

- Runs `python3 -m http.server 5500`
- Sets `cwd` to `${workspaceFolder}/app`
- Uses `serverReadyAction` to open `http://localhost:<port>/index.html`
- Opens the dashboard entry page instead of a directory listing

Preserve existing repository VS Code conventions where compatible.

### 5. Integrate and validate

**Owner:** Orchestrator

Coordinate a cross-file review after implementation. Confirm that the JSON contract, markup, CSS hooks, and launch behavior agree. Hand off only an integrated result with validation results and any remaining risk.

## File Assignments

| File | Primary owner | Responsibility |
|---|---|---|
| `app/project-data.json` | Coder | Project data contract and representative records |
| `app/index.html` | Coder | Dashboard structure, entry page, and project rendering |
| `app/styles.css` | Designer decisions; Coder implementation | Polished responsive cards, badges, typography, contrast, focus states |
| `.vscode/launch.json` | Coder | Port 5500 preview configuration and direct `index.html` browser launch |
| `docs/project-pulse-plan.md` | Planner | Implementation plan produced for the Orchestrator |

## Dependencies

1. `app/project-data.json` defines the field contract.
2. `app/index.html` depends on the JSON field names and stable CSS hooks.
3. `app/styles.css` depends on the final card and dashboard markup selectors.
4. `.vscode/launch.json` depends on `app/index.html` existing as the app entry point.
5. Integration validation depends on all four implementation files.

There are no required third-party packages or build steps. Python 3 is assumed to be available for previewing the static app.

## Parallel Work Decisions

### Can run in parallel

- The Coder may begin `app/project-data.json` while the Designer establishes the dashboard hierarchy.
- After the JSON field contract and markup hooks are agreed, the Coder may implement `app/index.html` and `app/styles.css`.
- `.vscode/launch.json` may be prepared once `app/index.html` is confirmed as the entry page.
- Static JSON and launch configuration checks may run after their respective files exist.

### Must run sequentially

- The data contract must be agreed before project rendering is implemented.
- The markup and CSS hooks must be stable before final stylesheet work is complete.
- The app entry page must exist before launch behavior is validated.
- Cross-file integration review must occur after implementation.

Each specialist owns a non-overlapping file scope. The Designer should not implement markup or JSON; the Coder should not change design-only decisions after approval. No agent should stage, commit, or push changes.

## Edge Cases

- Invalid or unavailable `project-data.json` should produce an explicit error state, not a blank dashboard.
- Empty project arrays should produce a clear empty state.
- Missing optional or required fields should not cause silent rendering gaps.
- Status and priority values should have fallback styling.
- Long project names or owner names should wrap without breaking card layout.
- Narrow viewports should reflow cards without horizontal overflow.
- Fetching JSON must work when the app is served from `app/`; opening `index.html` directly from the filesystem is not guaranteed to support fetch behavior.

## Validation Expectations

### Static validation

- Parse `app/project-data.json` successfully as JSON.
- Confirm every project has `name`, `owner`, `status`, `recentActivity`, and `priority`.
- Parse `.vscode/launch.json` as strict JSON with no comments.
- Confirm all four implementation files exist.
- Confirm `app/index.html` references `styles.css` and loads or imports `project-data.json`.
- Confirm `.dashboard` and `.project-card` CSS hooks exist.
- Confirm launch configuration name, command, `cwd`, port, and browser URL are exact enough to open the dashboard.

### Browser validation

- Start **Run Project Pulse Dashboard** from VS Code.
- Confirm it runs `python3 -m http.server 5500` from `${workspaceFolder}/app`.
- Confirm the browser opens `http://localhost:<port>/index.html`.
- Confirm the page shows the exact Project Pulse heading rather than a directory listing.
- Confirm project cards render with name, owner, status, recent activity, and priority.
- Confirm status and priority treatments are visually distinct and readable.
- Resize to representative desktop, tablet, and mobile widths and confirm responsive reflow.
- Verify visible keyboard focus and sensible semantic headings and regions.
- Simulate JSON failure or malformed data and confirm an explicit error state appears.
- Stop the preview server after validation.

## Open Questions

- Whether the final dashboard should use static card markup or JavaScript-driven JSON rendering.
- The exact status and priority values to ship in the initial sample data.
- Whether additional interactions, such as filtering or sorting, are required beyond the requested dashboard view.

Unless a later user instruction adds scope, implementation should remain static and dependency-free.
