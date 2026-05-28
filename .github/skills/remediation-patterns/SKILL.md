---
description: >
  Use when: planning or executing a remediation phase in the refactor workflow. Provides
  a reference table of common code quality findings and their recommended fix approaches.
---

# Remediation Patterns

| Pattern | Approach |
|---------|----------|
| Framework convention violation | Restructure code to follow the framework's conventions as documented in `copilot-instructions.md`. |
| Missing tests for module | Include test creation in the fix-list for implementer. |
| Hardcoded content (should be externalised) | Move data to the appropriate content/data source, update access helpers. |
| Styling convention bypass | Replace one-off values with tokens/variables from the project's design system. |
| Accessibility gaps | Add ARIA attributes, keyboard handlers, semantic HTML in fix-list. |
| Performance issues | Apply framework-specific optimisations (e.g. image optimisation, lazy loading). |
| Duplicate code | Extract shared utility or shared component. |
| Stale dependencies | Flag for user decision; do not auto-upgrade without approval. |
