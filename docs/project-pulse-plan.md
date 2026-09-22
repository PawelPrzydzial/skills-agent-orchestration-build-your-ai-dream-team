# Project Pulse implementation plan

## Summary

Build Mona's Project Pulse as a lightweight, static dashboard for contributors. The first view should make active projects, owners, status, recent activity, priority or risk, and a short contributor-friendly summary easy to scan. The implementation will use semantic HTML, a single stylesheet, and a top-level `projects` array in JSON. It should run locally through the VS Code **Run Project Pulse Dashboard** configuration, serving the `app/` directory and opening `index.html` rather than exposing a directory listing.

The plan deliberately avoids a framework, build step, backend, or unnecessary client-side complexity. The data should be representative and deterministic so the dashboard is useful as a preview and easy to validate.

## Ordered implementation steps

1. **Confirm the contract and ownership.** The Orchestrator reads this plan and the Project Pulse brief, confirms the required fields and launch behavior, and gives each specialist the file scope below. No implementation begins until the data shape and visual direction are understood.
2. **Define the experience.** The Designer establishes the information hierarchy, responsive layout, accessible status and priority treatments, typography, spacing, color tokens, and interaction expectations. The Designer may provide guidance for all UI surfaces but owns the visual decisions in `app/styles.css`.
3. **Create the data fixture.** The Coder creates `app/project-data.json` with a top-level `projects` array. Each project includes `name`, `owner`, `status`, `recentActivity`, and `priority`; include a short contributor-friendly summary as an additional field if the markup needs it. Use several projects with varied statuses and priorities so all visual states can be reviewed.
4. **Build the page structure.** The Coder creates `app/index.html` with a clear Project Pulse title, concise dashboard introduction, accessible landmarks and headings, a project-card collection, and placeholders or inline rendering structure for the data. It must reference `styles.css` and `project-data.json` as required by the chosen static approach. Project cards must visibly expose name, owner, status, recent activity, priority, and summary.
5. **Apply the visual system.** The Designer implements the approved dashboard styling in `app/styles.css`, including the required `.dashboard` and `.project-card` hooks, polished rounded cards, readable spacing, contrast, status badges, priority emphasis, responsive layout, focus states, and reduced-motion consideration where relevant. The stylesheet must remain usable without a preprocessor or build command.
6. **Connect and configure the preview.** The Coder ensures the HTML, CSS, and JSON agree on paths and fields, then creates `.vscode/launch.json` as strict JSON. The configuration must be named **Run Project Pulse Dashboard**, set `cwd` to `${workspaceFolder}/app`, serve that directory with a deterministic local server, and open `index.html` directly.
7. **Review the integrated result.** The Orchestrator checks the four assigned outputs together, resolves any mismatch between Designer guidance and Coder implementation, and confirms that no unrelated application or repository files were changed.
8. **Validate and hand off.** Run the checks below, preview the dashboard through the launch configuration, record any known limitations, and hand off only when the criteria below are met.

## File assignments

| File | Owner | Assignment |
| --- | --- | --- |
| `app/index.html` | Coder, informed by Designer | Semantic page shell, title and introduction, accessible headings/landmarks, project-card markup, stylesheet link, and JSON data reference/connection. |
| `app/styles.css` | Designer | Complete visual treatment and responsive behavior, including `.dashboard`, `.project-card`, badges, priority styling, typography, spacing, contrast, shadows, rounded corners, and focus states. |
| `app/project-data.json` | Coder | Deterministic fixture with top-level `projects`; every project has `name`, `owner`, `status`, `recentActivity`, and `priority`, plus the contributor summary field used by the page. |
| `.vscode/launch.json` | Coder | Strict JSON launch configuration named **Run Project Pulse Dashboard**, with `${workspaceFolder}/app` as the working directory and `index.html` as the opened target. |

The Orchestrator owns coordination and integration review, not implementation. The Designer must not edit `index.html`, JSON, or launch configuration unless the Orchestrator explicitly changes the assignment. The Coder must not redesign the stylesheet outside the agreed visual direction; any required styling adjustment should be surfaced to the Designer or documented during integration.

## Designer responsibilities

- Turn the brief into a clear scan path: dashboard identity and summary first, then project cards with status, owner, activity, priority, and summary.
- Establish a compact visual system suitable for a static page: readable type scale, spacing, card hierarchy, restrained colors, and visible status/priority distinctions.
- Ensure status and priority are not communicated by color alone; use text labels and sufficient contrast.
- Define responsive behavior for narrow screens, including card stacking and no horizontal overflow.
- Provide accessible focus-visible states, semantic expectations, readable line lengths, and sensible support for reduced motion.
- Implement and validate only `app/styles.css` in the assigned coding phase, preserving the required `.dashboard` and `.project-card` selectors.

## Coder responsibilities

- Create the assigned HTML, JSON, and launch files using the Designer's structure and visual guidance.
- Keep the data schema stable and explicit; do not silently substitute missing values or hide malformed data.
- Make every required project field visible in the rendered card and keep relative file paths correct.
- Use a simple static-data connection appropriate to the final architecture; avoid adding a framework, package manifest, bundler, or backend.
- Create `.vscode/launch.json` as valid strict JSON with no comments, an exact user-facing launch name, the requested `cwd`, and a direct `index.html` URL/target.
- Test the page from an HTTP server/launch configuration rather than relying only on opening the HTML file directly, since JSON loading can be blocked by browser file-origin rules.

## Dependencies and work ordering

The brief and existing agent definitions are the source of truth for required content, file scope, and launch behavior. The Designer's hierarchy and visual decisions inform the Coder's HTML class names and data presentation; the JSON field contract informs the card markup; the page paths and server choice inform launch configuration.

The following work must be sequential:

1. Confirm requirements and agree on the data contract.
2. Designer defines the visual/semantic contract and Coder creates the JSON fixture and page structure.
3. Designer completes CSS against the agreed markup hooks; Coder connects the data and launch configuration.
4. Orchestrator integrates, previews, validates, and hands off.

## Explicit parallel-work decisions

Parallel work is allowed only where file scopes and inputs do not overlap:

- After the contract is confirmed, the Designer may develop `app/styles.css` guidance while the Coder prepares `app/project-data.json`; these are independent because the data schema is fixed by the brief.
- The Coder may draft `.vscode/launch.json` in parallel with the Designer's stylesheet work because it depends only on the static `app/` directory and agreed launch requirements.
- The Coder should not finalize `app/index.html` before the Designer has supplied the information hierarchy and required CSS hooks, and the Designer should not make markup-dependent CSS changes before the page structure is agreed.
- Integration review, browser preview, and final validation are sequential after all four files exist. No parallel edit of the same file is permitted.

## Edge cases and risks

- A JSON fetch can fail when `index.html` is opened with a `file://` URL; validate through the configured HTTP server and show an explicit, useful error state if runtime data loading is used.
- Missing or empty values must not produce misleading success-shaped cards. The fixture should be complete, and any runtime failure should be visible rather than silently replaced.
- Long project names, owner names, activity text, and summaries must wrap without breaking the card grid or causing horizontal scrolling.
- Status and priority values may vary in case or wording; use a finite, documented fixture vocabulary and ensure unknown values remain readable as text.
- Color-only status or risk cues are inaccessible; retain visible labels, adequate contrast, and keyboard focus indicators.
- Narrow viewports and zoomed layouts must stack cards cleanly and preserve readable controls/content.
- A launch configuration that serves the repository root may open a directory listing; `cwd` and the direct `index.html` target must both be checked.
- A static dashboard has no persistence, authentication, live updates, or backend error recovery; these are intentionally out of scope and should be noted at handoff.

## Validation expectations

- Confirm `app/project-data.json` parses as JSON and has a top-level `projects` array; verify every fixture object contains `name`, `owner`, `status`, `recentActivity`, and `priority`.
- Confirm `app/index.html` exists, includes the Project Pulse title, references `styles.css` and `project-data.json`, and renders project cards with all required visible information.
- Inspect `app/styles.css` for `.dashboard`, `.project-card`, rounded corners, shadows, responsive rules, contrast, and focus-visible treatment.
- Parse `.vscode/launch.json` as JSON and verify the exact **Run Project Pulse Dashboard** name, `cwd` of `${workspaceFolder}/app`, and direct `index.html` launch target.
- Preview through **Run Project Pulse Dashboard** or an equivalent local HTTP server and confirm the page opens directly, data loads, cards are readable, and no directory listing appears.
- Check desktop and narrow viewport layouts, keyboard focus visibility, text wrapping, status/priority legibility, and browser console/network errors.
- Review the final diff to ensure only the four assigned implementation files are changed during the build; this planning file is the only file created in the planning phase.

## Handoff criteria

The Orchestrator may hand off Project Pulse when all four assigned files are present, internally consistent, and validated; the dashboard opens directly from **Run Project Pulse Dashboard**; project cards visibly communicate every required field; the responsive and accessible visual checks pass; and no unresolved errors, silent fallbacks, or unrelated file changes remain. The handoff should state the static-app limitation (fixture data only, no live persistence) and identify any intentionally deferred enhancements without expanding the current implementation scope.
