# Delivery records

One folder per Jira story, holding everything the agents produced for it apart from the specification itself
(which lives in `specs/`, including the story's impact analysis and spec change).

```
delivery/
  <JIRA-KEY>/
    status.md            Jira agent: steps done, approvals, open questions, merge requests
    design.md            Architect agent: patterns and design decisions for the story
    code-change.md       Coding agent: members changed and the spec item each implements
    test-results.md      Testing agent: tests chosen, run and their results
    verification.md      Verification agent: every check, pass or fail, with evidence
```

Templates: `agents/templates/`. The order of work and the approval points: `agents/README.md`.
