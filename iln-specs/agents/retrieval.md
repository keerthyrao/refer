# Finding the code: retrieval with Bob and GitLab Duo

This is the retrieval part of the RAG approach. **R**etrieve the right code, **A**ugment your prompt with it and the template,
**G**enerate the spec, then **ground** it: every statement cites lines you opened, and a count shows nothing was missed.
No scripts and no separate index are needed. Bob and Duo search and read the repository themselves.

```
 question or worklist item
        │
        ▼
 1. specs/iln/ first ──── already specified? use it, cite the spec id and rule id
        │ not there, or not enough
        ▼
 2. search the code ──────── exact search for names and keywords; Duo semantic search for meaning
        │ hits = pointers
        ▼
 3. open the files ───────── read the whole paragraph, record or step around each hit
        │
        ▼
 4. write from the template, citing path:line for every statement
        │
        ▼
 5. ground and count ─────── checklists.md: every citation opened, every member or paragraph accounted for
```

## What each tool gives you

| Tool | How you retrieve | Notes |
|---|---|---|
| **IBM Bob** | Workspace search (regular expressions across files), listing folders, reading files. Point it at exact files and folders with context mentions: `@/cobol/<MEMBER>`, `@/job/`, `@git-changes`, `@<commit id>` | Use **Ask** mode or an *iln* mode (`.bob/custom_modes.yaml`). Start a new chat after changing `AGENTS.md`, modes or skills. A direct `@` mention includes a file even if `.bobignore` excludes it, so never mention restricted files |
| **IBM Bob Premium Package for Z** (if you have it) | Workflows such as *Explain code*, *Generate program documentation*, *Extract business rules*, *Generate data dictionary*, and the *Impact analysis* skill | Their output is a **draft and a pointer**. Rewrite it into our templates; every item needs evidence you opened; anything you can't prove becomes an open question |
| **GitLab Duo** (Agentic Chat in the IDE or in GitLab) | Reads and searches files in the project. **Semantic code search** (beta, GitLab 18.7 and later, when the admin has turned it on) finds code by meaning ("where is late-payment interest worked out?") from an index of the default branch | Duo context exclusions keep restricted paths out. Semantic search only sees what is merged to the default branch |
| **GitLab Knowledge Graph** (beta) | Relationships between code elements, through MCP | Its documentation doesn't list COBOL or JCL. Don't rely on it for iln; use the connections specs |

Exact search is the workhorse for mainframe code: names are exact (`PGM=<PROGRAM>`, `COPY <COPYBOOK>`), and an exact search
tells you **everywhere** something appears. Semantic search is for questions in business words. Use both when it helps.

## Search recipes for iln

The `iln` folders: `cobol/` (programs, and copybooks kept with them), `job/` (JCL jobs), `procs/` (PROCs), `ctl/` (control cards:
CICS CSD and IDCAMS definitions, utility input), `bms/` (screens), `ezt/` (Easytrieve), `assembler/`, `test/` (existing tests).
Members have no file extension. Patterns are regular expressions; match them case-insensitively. If one doesn't work in your tool,
search for the plain words instead. In the table, `\|` is how Markdown writes `|` inside a table: type a plain `|` ("or") in the search.

| To find | Search for | Where |
|---|---|---|
| Every program | `PROGRAM-ID` | `cobol/`, `assembler/` |
| Copybooks a program uses | `COPY ` and `EXEC SQL INCLUDE` | the program |
| Programs that use a copybook | `COPY +<COPYBOOK>\b` and `INCLUDE +<COPYBOOK>\b` | `cobol/` |
| Calls a program makes | `CALL +'` and `CALL +"` (static); `CALL +[A-Z]` (through a variable: then find the variable's `VALUE` or `MOVE '...' TO`) | the program |
| Who calls a program | `'<PROGRAM>'` and `"<PROGRAM>"` (static calls, `VALUE` literals), `PROGRAM *\( *'?<PROGRAM>` (CICS `LINK`/`XCTL`) | `cobol/` |
| CICS commands | `EXEC CICS` | the program |
| CICS definitions | `DEFINE +(TRANSACTION\|TRANS\|PROGRAM\|PROG\|FILE\|MAPSET)` | `ctl/` |
| What starts a transaction | `TRANSACTION *\( *<TRAN>` then its `PROGRAM(`; `TRANSID *\( *'?<TRAN>` | `ctl/`, `cobol/` |
| Every job | `^//[A-Z0-9#@$]+ +JOB ` | `job/` |
| Steps that run a program | `PGM=<PROGRAM>\b`; `RUN +PROGRAM *\( *<PROGRAM>` (DB2 batch under IKJEFT01) | `job/`, `procs/` |
| Where a PROC is used | `EXEC +<PROC>\b` and `EXEC +PROC=<PROC>\b` | `job/` |
| VSAM definitions | `DEFINE +(CLUSTER\|AIX\|PATH)`, then `KEYS *\(`, `RECORDSIZE *\(`, `INDEXED` | `ctl/`, `job/` |
| Batch file access | `SELECT `, `ASSIGN `, `ORGANIZATION`, `RECORD +KEY`, `ACCESS +MODE`, `OPEN ` | the program |
| The dataset behind a file | the `ASSIGN` name as a DD name: `^//<DDNAME> +DD`, then `DSN=` | `job/`, `procs/` |
| Who creates, updates and reads a dataset | `DSN=<DATASET>`, with `DISP=` on the same DD | `job/`, `procs/`, `ctl/` |
| CICS file access | `EXEC CICS +(READ\|WRITE\|REWRITE\|DELETE\|STARTBR\|READNEXT\|READPREV\|ENDBR)` with `FILE(` or `DATASET(` | `cobol/` |
| DB2 | `EXEC +SQL` with `SELECT`, `INSERT`, `UPDATE`, `DELETE`, `DECLARE` ... `CURSOR` | `cobol/` |
| MQ | `MQOPEN\|MQPUT\|MQPUT1\|MQGET` | `cobol/` |
| File transfer and other systems | `FTP`, `DMBATCH`, `NDM`, `SUBMIT +PROC`, `SYSOUT=`, `INTRDR` | `job/`, `procs/`, `ctl/` |
| Decisions and calculations | `\bIF\b`, `EVALUATE`, `WHEN`, `COMPUTE`, `ADD `, `SUBTRACT`, `MULTIPLY`, `DIVIDE`, level-88 condition names | the program |
| Errors | `INVALID KEY`, `AT END`, `FILE STATUS`, `SQLCODE`, `RESP *\(`, `HANDLE +(ABEND\|CONDITION)`, `ABEND`, `RETURN-CODE` | the program |
| Restart and recovery | `SYNCPOINT`, `COMMIT`, `ROLLBACK`, `CHKP`, `CHECKPOINT`, `RESTART` | the program, its jobs |
| Screens | `DFHMSD`, `DFHMDI`, `DFHMDF` | `bms/` |
| Paragraphs and sections | names starting in column 8 and ending with a full stop, after `PROCEDURE DIVISION` | the program |
| Existing tests and documentation | the `test/` folder, `README`, `*.md`, `*.txt` | whole repository |

COBOL is fixed format: columns 1 to 6 are sequence numbers, a `*` or `/` in column 7 makes the line a comment, code is in columns 8 to 72.
When you open a hit, **ignore it if it is in a comment**.

## Retrieving everything one program needs

Before writing a program spec, collect these, in this order, and open each one:

1. **Its existing specs and mentions:** search `specs/` for the program name.
2. **The program itself.** Large programs: read paragraph by paragraph, and never summarise a part you haven't read.
3. **Its copybooks** (`COPY` lines), each opened.
4. **What starts it:** JCL steps (`PGM=`), CICS transactions (`DEFINE TRANSACTION`), callers (`CALL`, `LINK`, `XCTL`).
5. **Its files:** `SELECT` / `ASSIGN` → the DD in the JCL step → the dataset → its IDCAMS `DEFINE`; or the CICS `FILE` → the CSD `DEFINE FILE`.
6. **Its tables and queues:** `EXEC SQL`, MQ calls.
7. **What it calls**, and whether those programs are already specified.

The same path in reverse (who uses this copybook, file, transaction, job?) is how the Jira agent does impact analysis.

## Completeness: count, don't trust

Search results can be cut off, and long lists are easy to skip. At iln's size, completeness is something you **show**:

- **Folders.** List the folder and count its members. Every member is in the inventory or excluded with a reason, and the counts
  in the inventory's *Folder and coverage* table add up. Work in parts of at most 50 members; name the part (for example `cobol` A-C).
- **Searches.** If a tool says results were truncated, narrow the search (by folder or name prefix) until they aren't, and say so.
- **Programs.** Every paragraph and section of the procedure division appears in the spec's *Code coverage* table.
- **Repeat to check.** The Verification agent repeats a sample of your searches and counts. If it finds more, your output fails.

## What retrieval must never do

- Never cite a line you haven't opened. Never cite a search snippet, a summary or a tool workflow's output as evidence.
- Never open or mention files excluded by `.bobignore` or the Duo context exclusions, or anything holding real data.
- Never fill a gap with "typical mainframe behaviour". Not found means an open question.
- Never paste large parts of the code into a spec: a short quoted condition is enough when it makes a rule exact.
