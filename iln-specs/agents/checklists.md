# Checklists

These checklists are the specification standard's checks. Agents go through them before handing over, the Verification agent
checks them again, GitLab Duo Code Review applies them to merge requests (`.gitlab/duo/mr-review-instructions.yaml`),
and the Mainframe SME and Product Owner use them when they approve (`.gitlab/merge_request_templates/`).

A check you didn't actually perform is a failed check. Write down which ones you did.

## A. Every output

- [ ] It is in your own folder (see `AGENTS.md`, *Where outputs go*), with the file name the agent file gives.
- [ ] It follows its template: every section, in order, with the template's headings unchanged.
- [ ] The header is complete: no field left as `<...>`, a `MAJOR.MINOR.PATCH` version where the template has one,
      and `generated_by` filled in (agent, tool, mode, model, session).
- [ ] No placeholders are left anywhere: nothing in angle brackets, such as `<PROGRAM>`, `<yyyy-mm-dd>` or `<name>`.
- [ ] No section is empty. A section with nothing to say reads `None found`, then where you looked (folders and searches).
- [ ] Evidence is written `path:line` or `path:from-to`, with the path as it is in `iln` (for example `cobol/<MEMBER>:120-134`).
- [ ] No passwords, user ids, account or card numbers, customer data or production data. Dataset names only as far as the spec needs them.
- [ ] You propose a commit message that starts with the Jira key and your agent name: `<JIRA-KEY> <agent>: <what changed>`.

## B. Specifications (program, data, module, application)

**Evidence and grounding**
- [ ] Every business rule, flow step, state change, error, data item, input, output and connection has evidence.
- [ ] You opened every cited line, and it says what the spec claims. None is in a comment line (`*` or `/` in column 7).
- [ ] No evidence range is longer than about 12 lines for a single rule or row: a long range hides what the lines actually do.

**Rules and confidence**
- [ ] Every business rule has **Given**, **When** and **Then** (and **Otherwise** when the code has an else path),
      plus **Evidence**, **Confidence** and **Status**.
- [ ] Every rule and every row with a Confidence column says `confirmed`, `high`, `medium` or `low`.
      Only an SME or the Product Owner makes something `confirmed`.
- [ ] Everything `medium` or `low` has an open question that says why it matters and who should answer.

**Plain language** (the Product Owner reads the spec without the COBOL)
- [ ] Purpose, business rules (Given/When/Then), processing flow, state changes and acceptance criteria use business words.
      No data names (`WS-...`, `CUST-BAL-AMT`), no COBOL verbs (`MOVE`, `PERFORM`, `COMPUTE`, `EVALUATE`), no paragraph names
      except in brackets as evidence. Code names go in the glossary.

**Completeness**
- [ ] Program spec: every paragraph and section of the procedure division is in *Code coverage*, with its lines and what explains it
      (a rule, a flow step, an error row, or "technical" with a reason). Together the rows cover the procedure division without gaps.
- [ ] Program spec: *Restart and recovery* is not empty: it describes commit points and what a rerun does, or says there are none and where you looked.
- [ ] Data spec: field lengths add up to the record length; `REDEFINES`, `OCCURS` and `OCCURS DEPENDING ON` are called out.
- [ ] The program, copybook, transaction or job is in the inventory (`specs/iln/inventory/`). If it isn't, it is an open question.
- [ ] The spec's connections agree with `specs/iln/connections.md` (or its parts). Add what is new there in the same merge request.

**Version and lifecycle**
- [ ] The version is raised for every change: patch for wording or evidence, minor for an added or changed rule, major for a changed purpose or interface.
- [ ] The change log has a line for the current version, with the Jira key.
- [ ] Agents write `draft` or `in-review` only. `reviewed` is set by the Mainframe SME; `approved` needs the Product Owner's approval
      recorded in `approvals` **for this exact version**, and no open question still `open`.
- [ ] `source.commit` names the `iln` commit you read.

## C. Inventory (`specs/iln/inventory/<folder>.md`)

- [ ] *Folder and coverage* adds up: members found = members listed + members excluded (each with a reason).
- [ ] You listed the folder yourself and counted; you didn't rely on search results alone.
- [ ] Every row has evidence (the line that defines it: `PROGRAM-ID`, the `JOB` card, the `DEFINE`).
- [ ] Anything referenced but not found, called through a variable, or in a language you can't read (Easytrieve, Assembler) is under *Unknowns*.
- [ ] `specs/iln/index.md` is updated with the new counts.

## D. Connections and impact analysis

- [ ] Every relationship has evidence and a confidence; dynamic calls and inferred job order are `medium` or lower.
- [ ] Impact analysis: you followed the connections **in both directions** and confirmed each one with a code search
      (`agents/retrieval.md`), and you list the searches you used.
- [ ] Possibly affected components (dynamic calls, components not found, inferred order) are listed for an SME, not dropped.

## E. Outputs built from a spec (design, code change, tests, verification)

- [ ] `based_on` names each spec id, its version and the baseline tag or `main` commit you read it from.
- [ ] Every spec you used is `approved` (for a story) or at least `reviewed` (for the Feature 1 regression suite). If not, stop and say so.
- [ ] Every spec item you were given maps to something in your output, and everything in your output maps back to a spec item.

## F. Before a person commits

- [ ] Read the whole diff. Nothing outside the agent's folders changed.
- [ ] Nothing secret or real: no credentials, no customer or production data, no screenshots holding them.
- [ ] The commit message starts with the Jira key and the agent name.
- [ ] If this repository is `iln` (see `governance/where-it-lives.md`): push with `git push -o ci.skip` unless DevOps has set the
      pipeline to ignore these folders.
