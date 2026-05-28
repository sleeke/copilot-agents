---
description: >
  Use when: a delegate reports a blocker you cannot immediately resolve. Provides a
  reference table of standard blocker types and recovery actions for all Tier 2
  workflow contexts (feature-delivery, bug-fix, refactor).
---

# Intervention Protocols

## Feature delivery blockers

| Blocker type | Action |
|---|---|
| Spec ambiguity (implementer unsure how to proceed) | Read spec + source, amend spec or re-invoke spec-expander with the specific question. Restart from implementation phase. |
| Dependency missing (package, env var, binary) | Install or configure directly via terminal, then re-invoke the blocked agent. |
| Conflicting requirements vs `copilot-instructions.md` | The architecture doc wins. Amend the spec to align, note the change, and restart from implementation. |
| Persistent quality-gate failure (retries exhausted) | Report to user: failing test name, assertion, actual vs expected, diagnosis. |
| Code review finds architectural violation | Fix via implementer before proceeding to quality-gate. |
| Deploy failure after green quality-gate | Invoke quality-gate to re-verify, then retry deploy. |

## Bug-fix blockers

| Blocker type | Action |
|---|---|
| Cannot reproduce from description | Ask the user for reproduction steps using `vscode_askQuestions` before proceeding. |
| Root cause spans multiple unrelated systems | Fix the primary failure point. Open a new bug entry for each secondary issue. |
| Fix requires architectural change | Do not proceed autonomously. Present the finding to the user; route to feature-delivery or refactor if the user confirms scope. |
| Implementer cannot isolate fix without side-effects | Present the trade-off using `vscode_askQuestions` before choosing an approach. |
| Persistent quality-gate failure unrelated to the fix | Report the pre-existing failure to the user separately. Do not block the bug-fix summary. |

## Refactor / analysis blockers

| Blocker type | Action |
|---|---|
| Architect or code-reviewer produces empty/incomplete report | Re-invoke with explicit scope and feedback about what is missing. |
| Implementer cannot resolve a finding | Escalate to the user with the finding details and implementer's diagnosis. |
| Conflicting findings between architect and code-reviewer | Prefer the finding with higher severity. Note the conflict in the report. |
| Remediation introduces more issues than it fixes | Halt remediation for that stage. Report findings to the user. |
| Persistent quality-gate failure (3 retries exhausted) | Report to user with full diagnostics. |
| Scope parameter invalid (e.g. target file doesn't exist) | Report to user immediately; do not guess. |
