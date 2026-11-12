---
story: <JIRA-KEY>
type: spec-change
generated_by:                          # who produced this version (Story 1.1 AC 12, 14)
  agent: specification
  tool: <bob | gitlab-duo>
  mode: <Bob mode, or Duo chat or flow>
  model: <model name as the tool shows it>
  session: <chat or session id>
date: <yyyy-mm-dd>
approval:
  product_owner: <name>          # filled in when approved
  date: <yyyy-mm-dd>
  merge_request: <link>
---

# Spec change: <JIRA-KEY> <story title>

## 1. Why

<The business reason, from the story.>

## 2. Specs changed

| Spec | Version before | Version after | Kind of change |
|---|---|---|---|
| [<PROGRAM>](../../programs/<PROGRAM>.md) | <1.2.0> | <1.3.0> | <rule changed / rule added / data changed / flow changed> |

## 3. Changes

### <BR-PROGRAM-004>: <rule name> (<new | modified | removed>)

| | Text |
|---|---|
| **Before** | Given <...> When <...> Then <...> |
| **After** | Given <...> When <...> Then <...> |

- **Story acceptance criterion:** <which one this satisfies>
- **Spec:** [<PROGRAM> §5](../../programs/<PROGRAM>.md)

<!-- Repeat for every changed rule, data item, flow or error. -->

## 4. Story acceptance criteria covered

| Acceptance criterion | Covered by |
|---|---|
| <AC text or number> | <BR-... / FLOW-... / DATA-...> |

## 5. Not changed on purpose

<Behaviour someone might expect to change but doesn't, and why.>

## 6. Open questions

| ID | Question | Owner | Status |
|---|---|---|---|
| Q-<JIRA-KEY>-0n | <question> | <owner> | open |
