# Mentor Report — 2026-05-01

## Session Summary

**Agents involved:** Default Copilot agent (instructions from `.github/copilot-instructions.md`)  
**Task:** Implement all open bugs from `plan/BUG_TRACKER.md`  
**Outcome:** All prepared bugs implemented across three passes; one agent-instruction improvement applied mid-session.

The session had one clear pattern of friction: the agent declared a bug fixed after satisfying the headline intent, without checking each bullet point in the bug description. The user had to return twice with the remaining items before the work was complete. A guardrail was added to `copilot-instructions.md` mid-session.

---

## Agents Identified

| Agent | Instruction file |
|-------|-----------------|
| Default Copilot | `.github/copilot-instructions.md` |

No `.github/agents/` or `.github/chatModes/` files exist in this repository; the sole instruction surface is the project-level `copilot-instructions.md`.

---

## Lessons Extracted

### Dead end: Heading-level reading of bug descriptions

**What happened:** The agent was asked to "fix all open bugs." The bug entry for "Flashcard hero section should collapse" had eight bullet points. The agent implemented the headline behaviour (collapsible hero, auto-collapse on card interaction) and several bullets, but stopped before verifying every bullet was satisfied. It declared the bug done and moved the entry to `FIXED_BUGS.md`. The user had to re-open the session twice to get the remaining items addressed.

**Why it was suboptimal:** Two extra round-trips were needed. Some bullet points (toggle button placement, title visibility when collapsed, action button margin, dropdown alignment) were either deferred or missed entirely.

**The improvement:** Treat each bullet point as a distinct, independently verifiable requirement. Before closing a bug, enumerate all bullet points and confirm each is addressed. This was added to `copilot-instructions.md` — see below.

---

## Suggestions

### Suggestions for Default Copilot

**File:** `.github/copilot-instructions.md`

#### Bug Tracker Workflow

**Suggestion 1: Enumerate bullet points before closing a bug** *(already applied)*

- **Context:** The agent fixed the headline behaviour of a multi-bullet bug but missed several specific requirements, requiring two follow-up sessions.
- **Current gap:** The instructions described what constituted a "prepared bug" and how to move entries between files, but gave no guidance on how to *consume* the bullet-point list before marking work done.
- **Proposed addition:** *(Applied to the `## Working from the Bug Tracker` section added to `copilot-instructions.md`)*

```markdown
## Working from the Bug Tracker

When implementing a bug from `plan/BUG_TRACKER.md`, treat every bullet point in the bug
description as a distinct, independently verifiable requirement. Before marking a bug as done:

1. List every bullet point from the bug description.
2. Confirm each one is addressed in the implementation — do not rely on reading the heading alone.
3. Only move the entry from `BUG_TRACKER.md` to `FIXED_BUGS.md` after every bullet point is satisfied.
```

- **Rationale:** Bugs in this tracker are written as unordered lists where each item is a separate requirement. Reading only the heading caused two unnecessary user round-trips.

---

## Priority Ordering

| Priority | Suggestion | Impact |
|----------|------------|--------|
| 1 (High) | Enumerate every bullet before closing a bug | Eliminated two user round-trips; directly caused the session friction |

---

## Mentor Self-Review

- One suggestion, one root cause, one fix — no padding.
- The fix was applied mid-session (correct behaviour for Apply mode) and confirmed to work: the subsequent pass correctly identified that the remaining bullet points were already implemented in CSS.
- No over-wide scope: the suggestion is repo-agnostic as written (it describes a *process* for reading tracker entries, not specifics of any file or class).
- No instruction bloat risk: the added section is 5 lines and scoped narrowly.

---

## Propagation Hints

> These suggestions should be applied to the **agent definition repo** (the repo that owns
> the canonical `.github/agents/` files). Copy this report there and run Mentor in Apply
> mode to incorporate the changes.
>
> **Note:** This repo has no separate agent definition files — the only instruction surface
> is `copilot-instructions.md`. The fix has already been applied directly to that file.

| Agent | File | Suggestion title | Priority |
|-------|------|-----------------|----------|
| Default Copilot | `.github/copilot-instructions.md` | Enumerate bullet points before closing a bug | High |
