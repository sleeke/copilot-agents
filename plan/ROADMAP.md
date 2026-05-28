## Unprepared requirements

### High Priority

### Medium Priority

#### Tutor
An agent which will teach a certain skill to the user. The user would request to be taught how to complete a certain task, and the tutor agent would provide step-by-step instructions on how to complete the task. The tutor agent would also be able to answer any questions the user has about the task, and provide additional resources if needed.

Example: "Tell me how to arrange a UI element so it is centered horizontally in this element". The tutor agent would then provide step-by-step instructions on how to do this, including explanations of any concepts involved, links to more information, and examples of similar concepts/structures within the codebase.

#### Documentation agent
- Make sure all md files are representative of the codebase

#### Orchestrator should be configurable 
- e.g for refactor processes

#### Reviewer agent
  - Should be able to take the recent changes (needs github access or summary), or the entire project (could be context issues)

### Low priority

## Prepared requirements

### Init agent _(implementation complete)_
- Idempotent scaffolding agent — ensures `plan/ROADMAP.md`, `plan/BUG_TRACKER.md`,
  `.github/copilot-instructions.md`, `CHANGELOG.md`, `specs/`, and `agent-output/` exist.
- Never overwrites existing content; only fills gaps.
- Invocable manually ("Initialise the project") or automatically as an orchestrator
  pre-flight step when `.github/copilot-instructions.md` is absent.
- Uses `vscode_askQuestions` to interview the user when creating `copilot-instructions.md` from scratch.
- Supports `report-only` flag to audit without creating files.
- **Delivered:** `.github/agents/init.agent.md` created; orchestrator updated with pre-flight step and routing.

## Planning-ready requirements
