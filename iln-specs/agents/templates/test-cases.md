---
type: test-cases
scope: <PROGRAM | module | JIRA-KEY>
generated_by:                          # who produced this version (Story 1.1 AC 12, 14)
  agent: testing
  tool: <bob | gitlab-duo>
  mode: <Bob mode, or Duo chat or flow>
  model: <model name as the tool shows it>
  session: <chat or session id>
based_on:                              # the approved specs this was built from (Story 1.1 AC 12)
  - spec: <SPEC-ID>
    version: <version>
    baseline: <tag spec/iln/vN.N, or main commit id>
date: <yyyy-mm-dd>
environment: <test environment / region>    # never production
---

# Tests: <scope>

## 1. Test selection (for a story)

| Change touches | Smoke | Integration | Regression | Applied? | Why |
|---|---|---|---|---|---|
| <row from the selection table in agents/testing.md> | <yes> | <components> | <cases> | <yes / no> | <reason> |

## 2. Test cases

### TC-<PROGRAM>-001: <what it proves>

- **Verifies:** <BR-PROGRAM-001 / FLOW-module-01 / error condition>
- **Type:** <smoke / integration / regression> (<unit / boundary / restart>)
- **Preconditions and test data:** <described, made-up data only>
- **Steps:**
  1. <step>
- **Expected result (from the spec):** <outcome>
- **Story:** <JIRA-KEY, if added or changed for a story>

<!-- Copy the block for each case. Expected results come from the spec, never from reading the code. -->

## 3. Results

| Case | Type | Run on | Result | Evidence |
|---|---|---|---|---|
| TC-<PROGRAM>-001 | <type> | <yyyy-mm-dd> | <passed / failed / blocked> | <job output, return code, screen capture, before/after data reference> |

**Summary:** <n> run, <n> passed, <n> failed, <n> blocked.

## 4. Failures

| Case | What happened | Expected (spec) | Spec item | Sent to |
|---|---|---|---|---|
| TC-<PROGRAM>-00n | <actual result> | <expected> | <BR-...> | <coding / specification agent> |
