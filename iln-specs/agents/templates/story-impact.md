---
story: <JIRA-KEY>
type: impact-analysis
generated_by:                          # who produced this version (Story 1.1 AC 12, 14)
  agent: jira
  tool: <bob | gitlab-duo>
  mode: <Bob mode, or Duo chat or flow>
  model: <model name as the tool shows it>
  session: <chat or session id>
date: <yyyy-mm-dd>
source:
  repository: iln
  commit: <commit id analysed>
risk: <low | medium | high>
---

# Impact analysis: <JIRA-KEY> <story title>

## 1. The story in one or two sentences

<What business behaviour changes, and how we will know it's done.>

## 2. Starting points

| Component | Kind | Why it's a starting point | Spec | Evidence |
|---|---|---|---|---|
| <NAME> | <program / transaction / job / file / table / screen> | <the story names it / it implements the behaviour> | <link> | <file:line> |

## 3. Affected (must be changed or re-tested)

| Component | Kind | How it is affected | Path from the starting point | Change or re-test | Evidence |
|---|---|---|---|---|---|
| <NAME> | <kind> | <calls a changed program / uses a changed copybook / reads a changed file> | <A → B → C> | <change / re-test> | <file:line> |

## 4. Possibly affected (an SME must confirm)

| Component | Why it might be affected | Question for the SME |
|---|---|---|
| <NAME or "unknown target"> | <dynamic call / not found in the source / inferred job order> | <question> |

## 5. Relied on (test scope, not changed)

| Component | Kind | Relationship | Evidence |
|---|---|---|---|
| <NAME> | <kind> | <called by / read by / runs before> | <file:line> |

## 6. Specs affected

| Spec | Current version | Expected change |
|---|---|---|
| [<PROGRAM>](../../programs/<PROGRAM>.md) | <1.2.0> | <rule BR-...-004 changed / new rule> |

## 7. How it was found

| Search or spec used | Where | Hits | Used for |
|---|---|---|---|
| `PGM=<PROGRAM>` | `job/`, `procs/` | <n> | <affected: the jobs that run it> |
| `COPY +<COPYBOOK>` | `cobol/` | <n> | <affected: every program that uses it> |
| <spec and section, for example connections.md section 3> | `specs/iln/` | <n> | <starting list, then confirmed by the searches above> |

<!-- Every connection in sections 3 to 5 is confirmed by a code search here, not only by a spec. A reviewer can repeat these. -->

## 8. Risk

**<low / medium / high>**: <reason: number of components, shared copybooks or files touched, online and batch both affected, restart behaviour affected.>

## 9. Open questions

| ID | Question | Owner | Blocks the story? | Status |
|---|---|---|---|---|
| Q-<JIRA-KEY>-01 | <question> | <owner> | <yes / no> | open |
