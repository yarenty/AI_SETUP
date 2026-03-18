# WORK.md: Execute the Current Task (AGENTS.md-Driven)

Use this file as the starting prompt for the current work item.

## Current Task Description

Fill in the placeholders before you start:

- `TASK_DESCRIPTION`: {TASK_DESCRIPTION}
- `DELIVERABLES`: {DELIVERABLES}
- `SUCCESS_CRITERIA`: {SUCCESS_CRITERIA}
- `CONSTRAINTS / NON-GOALS`: {CONSTRAINTS}

## For Complex Tasks (follow the workflow)

1. Create `task.md` using the provided template (`tools/task_template.md`).
2. Break work into manageable phases and update phase statuses as you go.
3. Track decisions and errors systematically (capture rationale, not just outcomes).
4. Update progress regularly in `task.md` so the work is recoverable.

## For AI Agents (how to execute safely)

1. Read the relevant `AGENTS.md` guidance before making changes.
2. Read component-specific `AGENTS.md` files first (whenever a change touches a specific component).
3. Follow the 3-strike error protocol when something fails.
4. Use the 2-action rule: after every two view/browser/search operations, immediately write key findings to text files.
5. Maintain clear documentation of changes (update the task plan/progress and any relevant docs).

## Best Practices During This Work

Before starting development:

1. Review the project's `AGENTS.md` file.
2. Read component-specific documentation relevant to the touched areas.
3. Understand existing code patterns before implementing anything new.

During development:

1. Follow established architectural patterns from the documentation.
2. Log errors and resolutions (do not hide failures; record attempts and outcomes).
3. Test incrementally rather than doing one large verification at the end.
4. Document decisions and rationale so future work matches current intent.

After development:

1. Update relevant documentation to match what you implemented.
2. Mark task phases as complete.
3. Review and clean up temporary artifacts.
4. Share learnings with the team (record key takeaways in `task.md`).

## Tool Reminders (use these while implementing)

- `tools/solid_principles_guide.md`: use for deeper SOLID examples and guidance.
- `tools/solid_principles_quick_reference.md`: use for daily SOLID checklists while designing/refactoring.
- `tools/rust_coding_conventions.md`: use for Rust-specific style and implementation conventions.

## Direct Instructions for the Next Action

1. Start by reading `AGENTS.md`.
2. Create `task.md` with a short goal statement and an initial phase breakdown.
3. Identify which components this task touches, then read each component's `AGENTS.md`.
4. Proceed with small, testable increments, updating `task.md` after each phase.
