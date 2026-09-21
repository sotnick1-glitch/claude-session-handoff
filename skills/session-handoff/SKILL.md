---
name: session-handoff
description: >
  Set up (or repair) the session ritual in a project: a CLAUDE.md that tells
  Claude what to read at session start, a state file that survives context
  loss, and a /handoff command that closes the session. Use when the user says
  "set up handoff", "настрой хендофф", "ритуал сессии", "session ritual",
  "make Claude remember where we stopped", "I keep losing context between
  sessions", or "/session-handoff". One-time setup per project.
---

# Session handoff — setup

A session's context is finite. The goal of this ritual: an interrupt at any
point costs no more than five minutes to recover from. Not a memory system, not
a plugin doing magic — four plain text files that a project carries in its own
git repo.

## Before writing anything

Read the project first, then size the ritual to it:

- What is this project (README, package manifest, language)?
- How are checks run — `npm test`, `pytest`, `cargo test`, a script, nothing?
- Does a state file already exist under another name (`STATUS.md`,
  `NOTES.md`, `docs/handoff.md`, `PROGRESS.md`)? **Reuse it. Never create a
  second one** — two state files means neither is true.
- How much documentation is there already?

Then pick the size:

| Project | What to create |
|---|---|
| Small (one package, few docs) | `CLAUDE.md` + state file + `/handoff` command. Fold the procedure into `CLAUDE.md`. |
| Large (many docs, many subsystems) | Add `docs/session.md` (procedure) and `docs/INDEX.md` (a "question → file" table). |

Do not create `docs/INDEX.md` for two documents. An index over two files is a
third file to read in order to learn about two.

## What to write

Templates are in `templates/` next to this skill: `CLAUDE.md`, `state.md`,
`session.md`, `handoff-command.md`. They are in English — **write the files in
the language the user is speaking**, and fill every `<...>` placeholder with
real values from this project. A template shipped with placeholders left in is
a failed setup.

1. **`CLAUDE.md`** in the repo root — this is the only file loaded
   automatically at session start. Keep it under ~30 lines: what to read on
   open, when to close, and the 4–5 rules of this project that must never be
   violated. Rules mean things that break the project or undo a decision, not
   style preferences. If `CLAUDE.md` already exists, add the ritual to it
   rather than overwriting what is there.
2. **The state file** — create it with the template only (no invented
   content), or, if one exists, keep its content and append the template at the
   end under "Template (do not delete)".
3. **`.claude/commands/handoff.md`** — from `handoff-command.md`, with this
   project's real check command and state file path.
4. Large projects only: **`docs/session.md`** and **`docs/INDEX.md`**.

## Three decisions to carry over

These are what make it survive past the first month:

- **The index is a table of contents, not a summary.** The moment it contains
  content, it becomes another document to read in full, and the point is lost.
- **The state file is state, not a diary.** It is overwritten whole every time.
  History lives in git and can always be recovered from there. A diary grows
  and after twenty sessions nobody reads it.
- **A durable fact is written the moment it is learned**, not "at the end of
  the session". The end may never come.

## After setup

Tell the user two things, in one line each:

- Open a session by saying so in **the project folder** — `CLAUDE.md` is only
  picked up from the directory the session was opened in. A project sitting in
  a subfolder of the session's directory will not be read.
- Close it with `/handoff`.
