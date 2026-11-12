---
story: <JIRA-KEY>
type: design
generated_by:                          # who produced this version (Story 1.1 AC 12, 14)
  agent: architect
  tool: <bob | gitlab-duo>
  mode: <Bob mode, or Duo chat or flow>
  model: <model name as the tool shows it>
  session: <chat or session id>
based_on:                              # the approved specs this was built from (Story 1.1 AC 12)
  - spec: <SPEC-ID>
    version: <version>
    baseline: <tag spec/iln/vN.N, or main commit id>
date: <yyyy-mm-dd>
spec_change: ../../specs/iln/stories/<JIRA-KEY>/spec-change.md
---

# Design: <JIRA-KEY> <story title>

## 1. Components and patterns

| Component | Change | Patterns to follow | Notes |
|---|---|---|---|
| <PROGRAM / copybook / job / map> | <what changes, from the spec change> | [<ONL-01>](../../architecture/iln/patterns/<ONL-01>.md) | <how the pattern applies here> |

## 2. Design decisions

### D1: <decision>

- **Options:** <A> / <B>
- **Recommendation:** <A>, because <reason>
- **Impact:** <components affected by the choice>

## 3. Data changes

<Copybook, file or table changes, with every user of them from the impact analysis. "None" if none.>

## 4. Restart, recovery and performance

<Effect on commit points, checkpoints, batch window, online response. "No change" if none.>

## 5. Open questions

| ID | Question | Owner | Status |
|---|---|---|---|
| Q-<JIRA-KEY>-D1 | <question> | <owner> | open |
