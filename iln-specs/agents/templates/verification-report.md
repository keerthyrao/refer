---
story: <JIRA-KEY>
type: verification
generated_by:                          # who produced this version (Story 1.1 AC 12, 14)
  agent: verification
  tool: <bob | gitlab-duo>
  mode: <Bob mode, or Duo chat or flow>
  model: <model name as the tool shows it>
  session: <chat or session id>
date: <yyyy-mm-dd>
result: <PASS | FAIL>
---

# Verification: <JIRA-KEY> <story title or baseline task>

## 1. Result

**<PASS / FAIL>**: <n> of <n> checks passed.

## 2. Checks

| # | Check | Result | Evidence | If failed: who fixes what |
|---|---|---|---|---|
| 1 | Impact analysis committed first, with evidence | <pass / fail> | <commit id, file> | <agent: action> |
| 2 | Spec change applied, versions raised, change logs updated | | | |
| 3 | Product Owner approved the spec change before design and coding | | <merge request / approval record> | |
| 4 | Design names a pattern for every changed component | | | |
| 5 | Every spec item maps to code, and every code change to a spec item | | | |
| 6 | Every changed component is in the impact analysis | | | |
| 7 | Every changed spec item has a test; test types follow the selection table; regression covers affected components | | | |
| 8 | Every test has a result with evidence; failures are open | | | |
| 9 | Trace: `generated_by` and `based_on` in every output; commit messages carry the story key and agent name, in order | | <commit list from the merge request> | |
| 10 | No secrets, user ids, account numbers or production data in any output | | | |
| 11 | Evidence sample: cited lines exist and say what is claimed (<n> checked) | | <list> | |
| 12 | Repeated searches and counts find nothing the outputs missed (<n> repeated) | | <searches and counts> | |

<!-- For the Feature 1 baseline, replace checks 1-9 with: every member of every inventoried folder is accounted for;
     every program in index.md has a spec or an honest status; every spec follows its template (checklists A and B);
     every program spec's code coverage covers its procedure division; connections agree with program specs;
     open questions are listed and owned. -->

## 3. Evidence sample

| Spec item | Cited lines | Opened? | Says what is claimed? |
|---|---|---|---|
| <BR-PROGRAM-001> | <file:from-to> | yes | <yes / no: what it actually says> |

## 4. Searches repeated

| Output | Search or count | Agent found | Verification found | Match? |
|---|---|---|---|---|
| <inventory cobol-a-c / impact.md> | <members in cobol A-C / `PGM=<PROGRAM>`> | <n> | <n> | <yes / no> |

## 5. Not checked

<Anything that could not be checked, and why. Each one is a failed check.>
