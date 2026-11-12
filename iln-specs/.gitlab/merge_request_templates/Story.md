## Story

- Jira: <JIRA-KEY>, <story title>
- Feature branch: `fe/<FEATURE-KEY>`
- Status and steps: `delivery/<JIRA-KEY>/status.md`

## Chain (agents/README.md)

- [ ] 1. Impact analysis committed first: `specs/iln/stories/<JIRA-KEY>/impact.md`
- [ ] 2. Spec change: `specs/iln/stories/<JIRA-KEY>/spec-change.md`, versions raised
- [ ] Product Owner approved the spec change on <date> (before design and coding)
- [ ] 3. Design: `delivery/<JIRA-KEY>/design.md`
- [ ] 4. Code change: `delivery/<JIRA-KEY>/code-change.md`, iln commits <ids>
- [ ] 5. Tests and results: `delivery/<JIRA-KEY>/test-results.md`
- [ ] 6. Verification: `delivery/<JIRA-KEY>/verification.md`, result <PASS / FAIL>

## Reviewer checks

- [ ] Every changed component is in the impact analysis
- [ ] Every spec item maps to code and to at least one test
- [ ] Every commit message starts with `<JIRA-KEY> <agent>:`
- [ ] No secrets, user ids, account numbers or production data anywhere in the diff

