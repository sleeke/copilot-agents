# Mentor Report — 2026-05-19

**Feature:** Backup and restore character sheets  
**Agents analysed:** feature-delivery, spec-expander, implementer, code-reviewer, quality-gate, scribe

---

## Summary

A standard-complexity feature was delivered end-to-end in one session.
The main friction points were:
1. Three spec revision cycles caused by UI placement ambiguity not probed upfront.
2. A filename-sanitisation regex that produced incorrect output, caught by tests.
3. A pre-existing unused variable carried through to the quality-gate lint phase.

---

## Agent findings

### spec-expander

**Issue — UI placement not probed before writing the spec.**  
The initial spec placed Download/Upload buttons inside the `CharacterSheet` header. The user corrected this twice (first revision: account dropdown; second revision: button label wording; third revision: filename convention). All three could have been prevented by a single upfront question about _where_ backup controls should live and _how_ filenames should be formatted.

**Suggestion — add a UI placement probe to the intake step:**

> Before writing the spec for any feature that introduces UI controls (buttons, links, menu items, form fields):
> 1. Ask: "Where exactly in the UI should this control appear? (e.g. page header, sidebar, navigation dropdown, floating action button)"
> 2. Ask: "Are there filename, label, or wording conventions this project already follows that the spec should honour?"
> Record the answers explicitly in the **Implementation notes** section of the spec before finalising acceptance criteria.

---

**Issue — filename conventions not extracted from the requirement.**  
The requirement said "named with the character name and the date" but did not specify the separator (`_` vs `-` vs space), invalid-character treatment, or what to do with empty names. The spec assumed defaults that did not match the user's expectation.

**Suggestion — add a filename conventions checklist to the spec template:**

> When a spec involves generating a filename from user-supplied data, the spec must document:
> - Separator between name components
> - Replacement character for filesystem-invalid characters (spaces, `\`, `/`, `:`, `*`, `?`, `"`, `<`, `>`, `|`)
> - Fallback name when the input is empty or all-invalid
> - Maximum length cap

---

### implementer

**Issue — filename-sanitisation regex `[/\\:*?"<>|]` was ambiguous.**  
The positive character class listing forbidden characters produced unexpected results when compiled: the test input with 9 special characters produced 10 underscores because the backslash in `\\` was being interpreted as an escape. The fix — switching to the negated class `[^a-zA-Z0-9._\-]` — is simpler, covers all cases, and has no escape ambiguity.

**Suggestion — prefer negated character classes for filename sanitisation:**

> When sanitising a string for use as a filename, use a negated character class that
> whitelists safe characters rather than a positive class that blacklists forbidden ones:
>
> ```ts
> // Prefer this: allow only alphanumerics, dots, hyphens, underscores
> str.replace(/[^a-zA-Z0-9._\-]/g, '_')
>
> // Avoid this: listing forbidden chars is error-prone in compiled regexes
> str.replace(/[/\\:*?"<>|]/g, '_')
> ```
>
> Reason: whitelist-based regexes are unambiguous regardless of escape handling,
> and automatically cover all control characters and Unicode edge cases.

---

**Issue — test assertions used string literals containing `\` which caused escape ambiguity.**  
Tests for backslash handling used `String.fromCharCode(92)` to avoid the `'\\'` vs `'\'` ambiguity in test assertion strings. This is correct but non-obvious.

**Suggestion — document the backslash test pattern:**

> When writing test cases that contain backslash characters, use `String.fromCharCode(92)`
> for the expected input string rather than `'\\'` to eliminate escape ambiguity in
> assertions. Note this pattern in a comment so future readers understand the intent.

---

### quality-gate

**Issue — a pre-existing unused variable (`session`) was not caught until the lint phase.**  
`const { data: session, status } = useSession()` was in the original file; `session` was never consumed. This warning existed before the feature was started. The quality-gate lint run caught it, but only after all other phases were complete.

**Suggestion — add a lint-clean precondition check:**

> At the start of Phase 0 (Pre-flight), run `npm run lint` (or equivalent) on the
> current working tree before any implementation begins. Record the baseline warning
> count. During Phase 4, require that the post-implementation warning count is ≤ the
> baseline count. Any new warnings introduced by the feature must be resolved before
> the quality gate passes.

---

### code-reviewer

No process failures. The review was thorough and correctly identified two low-severity issues (upload error never auto-cleared; no file size cap). Both are valid observations for a follow-up task.

**Suggestion — add a "upload error lifecycle" check to the review checklist:**

> When reviewing file-upload UI code, verify:
> - Does the error state clear when the user selects a new file?
> - Is there a maximum file size guard before parsing begins?
> These are common omissions in first-pass implementations.

---

### feature-delivery (self-assessment)

**Issue — spec revision cycles added unnecessary round-trips.**  
Three revision cycles for a single spec are above the expected one-to-two. The root cause was that the spec was written before enough UI placement and filename convention information was gathered.

**Suggestion — add a discovery probe before invoking spec-expander for UI features:**

> Before invoking spec-expander for any feature that adds or moves UI controls, answer
> these questions (from the requirement text, existing codebase, or by asking the user):
> 1. Where in the navigation/page hierarchy does the control live?
> 2. What existing UI patterns (icon style, menu structure) must it follow?
> 3. Are there naming or filename conventions documented anywhere in the project?
> Provide the answers as additional context to spec-expander to reduce revision cycles.

---

## Propagation hints

These suggestions should be applied to the agent definition repository
(`.github/agents/`) — not to this project's agent files.

| Target agent file | Change summary |
|---|---|
| `spec-expander.agent.md` | Add UI placement probe before writing spec; add filename conventions checklist to spec template |
| `implementer.agent.md` | Prefer negated regex classes for filename sanitisation; document `String.fromCharCode(92)` backslash test pattern |
| `quality-gate.agent.md` | Add baseline lint check in Phase 0 pre-flight; gate on no new warnings |
| `code-reviewer.agent.md` | Add upload error lifecycle and file size guard to upload review checklist |
| `feature-delivery.agent.md` | Add UI discovery probe before invoking spec-expander for UI features |
