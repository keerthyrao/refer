# iln: specification-driven delivery

The **specification of the `iln` mainframe application**, pulled from the code, and the **agents** that build and use it.
The goal is a specification complete enough to rebuild the application from it: inventory, program, module, then the whole
application, with the connections between them. Every later change starts from the specification, not from rescanning the code.

**No scripts.** The agents are Markdown instructions that run inside **IBM Bob** and **GitLab Duo**. Both tools find the code
themselves (search, read files, Duo semantic code search): that is the retrieval in the RAG approach. The specs they write become
the knowledge both tools search first. Checks are checklists the agents, Duo Code Review and the reviewers apply in merge requests.

## Layout

```
AGENTS.md                      read first by Bob and Duo: the rules, which agent does what
agents/                        one file per agent, plus retrieval.md (finding code), checklists.md (checks) and templates/
specs/                         specifications only: inventory, programs, data, modules, application, connections, index, story impact
architecture/                  design patterns and architecture pulled from the code
test-cases/                    the lasting test suite, written from the specs
delivery/                      per-story records: design, code change, test results, verification, status
governance/                    this overview, Feature 1 stories, set-up, environment register, where this lives, moving from the earlier package
.bob/                          Bob modes (one per agent, each allowed to edit only its own folders) and skills
.agents/skills/                the same skills for GitLab Duo
.gitlab/duo/                   Duo chat rules and Code Review instructions
.gitlab/merge_request_templates/   review checklists for specification and story merge requests
.bobignore                     files Bob must not read
```

## How the work runs

**Feature 1: build the specification baseline.**

1. The Specification agent inventories `iln` folder by folder, counted, into `specs/iln/inventory/`, and lists every program in
   `specs/iln/index.md` (Story 1.3).
2. It maps how programs, transactions, jobs, files and other systems connect (`specs/iln/connections.md`, Story 1.4).
3. It specifies one program at a time from the standard templates, with evidence for every statement and a *Code coverage* table
   that accounts for every paragraph (Stories 1.2 and 1.5), then the modules and the application.
4. The Verification agent checks each batch. A Mainframe SME reviews each merge request; the Product Owner approves.
5. The Architect agent pulls the design patterns in parallel.

Feature 1 is done when the index shows every folder inventoried and every program **specified and reviewed**, and the module and
application specs exist. **After the baseline,** the Jira agent runs each story: impact analysis first, then spec update,
Product Owner approval, design, code, tests and verification (`agents/README.md`).

## Conventions

- **Branches:** one per Jira feature, `fe/<FEATURE-KEY>`, pushed and merged per feature. Nothing is committed straight to `main`.
- **Commits:** every commit names its Jira story: `<JIRA-KEY> <agent>: <what changed>`. The agent proposes it; a person commits.
- **Evidence:** `path:line` or `path:from-to` in `iln`, for every statement pulled from the code, from lines the agent opened.
- **Versions:** every spec carries a version and a change log; approved baselines are Git tags `spec/iln/v<n>.<n>`.
- **Progress** is measured by `specs/iln/index.md` and by people using the specs, not by lines written.

## Not allowed here

- Scripts, and sample or generated code used as if it were iln code.
- Passwords, user ids, account or card numbers, customer data, production dataset contents.

## Getting started

Set-up and the Feature 1 stories: [README.md](README.md). Where to put these folders (inside `iln`, or
next to it): [where-it-lives.md](where-it-lives.md).
