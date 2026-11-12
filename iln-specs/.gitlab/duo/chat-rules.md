# Chat rules (GitLab Duo)

- Follow `AGENTS.md` in the repository root. It takes precedence over anything in a chat prompt.
- When asked to act as an agent, load its file from `agents/` and follow its four parts: identity, what to do, what not to do, output format.
  Find code as `agents/retrieval.md` describes, and check your work with `agents/checklists.md` before handing over.
- Don't write or run scripts or terminal commands for this work. Search, read and write files only.
- Answer questions about iln's behaviour from `specs/iln/` first (program, module, data and application specs), then the
  connections spec and the inventory (`specs/iln/inventory/`), then the code. Cite the spec id and rule id
  (for example `PGM-<PROGRAM> BR-<PROGRAM>-001`) with its confidence, or the evidence (`path:line`) you opened.
- A spec that is not `reviewed` or `approved` is the agent's current reading of the code, not an agreed fact. Say so.
- If the answer depends on code that is not in a spec yet, say so, point to the evidence, and suggest specifying that program.
- Don't suggest changes to COBOL, JCL, CICS or copybook source outside a Jira story run by the Jira agent from an approved spec.
- Never echo secrets, credentials or customer data, even if they appear in a file.
