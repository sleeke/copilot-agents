# Agent Improvements - Bug Tracking

This document tracks open bugs in the agents. Bugs are listed here until tests are created to verify the fix, at which point they should be removed.

## Bugs needing additional information

Please ignore bugs in this section until the remaining information has been filled out:

### Mentor agent doesn't have access to agents when used in the user scope
- When developing the mentor agent, it was observed that when the mentor agent is used in the user scope, it doesn't have access to the agents. This limits its functionality and prevents it from effectively mentoring other agents.
- When working from the user scope, I would like the mentor agent to be able to provide a report which could be consumed by the mentor in this team to determine how to improve the agents. This would allow the mentor agent to fulfill its purpose of mentoring other agents and providing valuable insights for improvement.
- The report should be output to agent-output in the host repo so that the results can be transferred somehow to the mentor in this team. This would enable the mentor agent to share its findings and recommendations with the mentor in this team, facilitating collaboration and improvement across teams.
- if there are known ways to incorporate the mentor agent's insights into the mentor in this team, please share them here. This would help in finding a solution to the issue and ensuring that the mentor agent can effectively contribute to the mentoring process.
- Make suggestions to the user which might help in this process. Chat and discuss; brainstorm, until the desired end goal is achieved.
- It would also be useful to determine a way to differentiate the agents in the user scope from those within the workspace for the user. 

## Prepared bugs

(none)

## Active Bugs

> Fixed bugs are tracked in [FIXED_BUGS.md](FIXED_BUGS.md).

(none)

---

## Bug Report Template

When adding a new bug, use this template:

```markdown
### N. Bug Title

**Status**: Open  
**Severity**: Low/Medium/High/Critical  
**Date Reported**: YYYY-MM-DD

**Reproduction Steps**:
1. Step 1
2. Step 2
3. Step 3

**Expected Behavior**: What should happen

**Actual Behavior**: What actually happens

**Details**: Additional context or investigation notes

**Affected Files**:
- File 1
- File 2
```

---

## Severity Levels

- **Critical**: App crashes or data loss
- **High**: Feature doesn't work at all
- **Medium**: Feature works but with incorrect behavior
- **Low**: Minor issue, cosmetic or rarely encountered
