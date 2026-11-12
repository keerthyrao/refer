# Where these folders live

Bob and Duo can only retrieve what they can see. They see the code and the specs together best when both are in the **same repository**
and the same workspace. That is a decision for the lead (with DevOps), so here are both options.

| | **A. Inside `iln`** | **B. Separate specification repository** |
|---|---|---|
| Bob reads `AGENTS.md`, the modes and the skills | Automatically (they are at the workspace root) | Only if the spec repository is the workspace (or the first folder of it) |
| Bob searches code and specs together | Yes, one workspace | Only if `iln` is added to the same workspace; check it works in your Bob version |
| Duo reads `AGENTS.md`, chat rules, Code Review instructions | Yes, for `iln` | Yes, for the spec repository only |
| Duo semantic code search covers code and specs | Yes, once merged to `iln`'s default branch | Code and specs are indexed as two projects |
| Evidence links (`cobol/<MEMBER>:120`) | Same repository | Point into the other repository |
| Pipelines | `iln`'s `.gitlab-ci.yml` runs on pushes: needs `-o ci.skip` or a DevOps rule | None |
| Approvals | `iln`'s own CODEOWNERS and branch rules, plus the new sections | Its own CODEOWNERS |
| Effect on the team | `AGENTS.md` and the Duo chat rules apply to everyone using Bob or Duo in `iln` | None |

**Recommendation: A, inside `iln`,** because retrieval works with no extra steps in both tools, and the specs sit next to the code they
describe. The reasons the earlier package kept them apart (its commit hook blocked JCL that names datasets; its tools wrote files)
are gone now that there are no scripts. Choose B if the lead wants the specification governed separately from the code.

## A. Adding the folders to `iln`

On the feature branch `fe/<FEATURE-KEY>` in your `iln` clone:

1. **Check what `iln` already has.** The team already uses Bob and Duo there, so some of these may exist (Bob's `/init` writes an `AGENTS.md`):
   `AGENTS.md`, `.bobignore`, `.bob/custom_modes.yaml`, `.bob/rules/`, `.bob/skills/`, `.agents/skills/`, `.gitlab/duo/chat-rules.md`,
   `.gitlab/duo/mr-review-instructions.yaml`. **Merge** any that exist instead of overwriting them: add our text under the team's in
   `AGENTS.md` and `chat-rules.md`, add our six modes to the `customModes:` list, add our groups to `instructions:`, and add our lines to `.bobignore`.
2. **Copy in** everything else from the package: `.bob/skills/`, `.agents/skills/`, `.gitlab/merge_request_templates/`,
   `agents/`, `specs/`, `architecture/`, `test-cases/`, `delivery/`, `governance/` (and the files from step 1 that `iln` didn't have).
3. The package has no `README.md`, `.gitignore` or `.gitattributes`, so `iln`'s own stay as they are. Add one line to `iln`'s `README.md`:
   *"Specification and agents: see `AGENTS.md` and `governance/overview.md`."* Add `.env`, `restricted/` and `*.dump` to `iln`'s
   `.gitignore` if they aren't there.
   Check `iln`'s `.gitattributes` doesn't give `*.md` or `*.yaml` a mainframe (EBCDIC) encoding: these files must stay UTF-8 text.
4. **CODEOWNERS:** GitLab uses only one CODEOWNERS file (the root one comes first), so add the sections from
   [CODEOWNERS.template](CODEOWNERS.template) to the end of `iln`'s root `CODEOWNERS`. Don't add `.gitlab/CODEOWNERS`: it would be ignored.
5. **Pipelines:** push with `git push -o ci.skip` so `iln`'s pipeline doesn't start. Opening a merge request can still start a
   merge request pipeline: ask DevOps to add a rule to `iln`'s `.gitlab-ci.yml` that skips changes only in these folders.
6. **Tell the team** that `AGENTS.md` and `.gitlab/duo/chat-rules.md` now guide everyone's Bob and Duo sessions in `iln`
   (for example: cite evidence, no secrets, no scripts), and agree it with them.
7. Ask the admin to apply the Duo context exclusions to the `iln` project.

## B. A separate specification repository

1. Use this repository as it is, with [overview.md](overview.md) copied to `README.md`, and `.gitlab/CODEOWNERS` from [CODEOWNERS.template](CODEOWNERS.template).
2. **Bob:** open the spec repository folder, then add `iln` to the same workspace (File > Add Folder to Workspace), spec repository first.
   Test it once: in the *iln Specification* mode, ask Bob to open `@/cobol/<MEMBER>` from `iln` and search `iln` for `PROGRAM-ID`.
   If it can't reach `iln`, use option A.
3. **Duo:** in Agentic Chat, name the `iln` project when asking for code, for example *"search the iln project for PGM=<PROGRAM>"*.
   Semantic code search works per project; ask the admin to make sure `iln` is indexed.
4. Evidence paths are paths in `iln` (`cobol/<MEMBER>:120`), and `source.commit` names the `iln` commit read.
