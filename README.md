# Copilot Agents

A reusable, generic set of GitHub Copilot agents that form an automated development
pipeline. These agents can be adapted to **any** repository and technology stack —
they discover the project's language, framework, tooling, and conventions at runtime.

## What's included

A complete three-tier agent hierarchy:

| Tier | Agents | Role |
|------|--------|------|
| **Tier 1** | Orchestrator | Intent router — parses requests and delegates to workflows |
| **Tier 2** | Feature Delivery, Refactor, Release Manager | Workflow coordination — end-to-end pipelines with handoffs |
| **Tier 3** | Spec Expander, Implementer, Code Reviewer, Architect, Quality Gate, Designer, Scribe, Deployer, Mentor | Specialist execution — each does exactly one thing |

## Getting started

1. Copy the `.github/agents/` directory into your repository.
2. Create a `.github/copilot-instructions.md` file documenting your project's
   architecture rules, conventions, and constraints.
3. (Optional) Create a `plan/ROADMAP.md` with prepared requirements.
4. Start using the agents — `@orchestrator Add a caching layer to the API`.

For detailed adaptation instructions, see **[ADAPTING.md](ADAPTING.md)**.

## Documentation

- **[.github/agents/README.md](.github/agents/README.md)** — Agent overview, data flow,
  and invocation examples.
- **[ADAPTING.md](ADAPTING.md)** — How to adapt these agents to your repository, including
  a suggested agent prompt for merging them into an existing team.
- **[plan/ROADMAP.md](plan/ROADMAP.md)** — Feature roadmap and prepared requirements.
- **[plan/BUG_TRACKER.md](plan/BUG_TRACKER.md)** — Bug tracking template.
