# Session procedure

A session's context is finite. The goal: an interrupt at any point costs no
more than five minutes to recover from.

## Opening

1. Read `INDEX.md` — only that, it is short.
2. Read `<state file>` — where we stopped.
3. Open, **by the index**, exactly the files the task needs. Do not read
   `docs/` end to end, do not grep the whole tree.
4. Say in one line what is being taken on, and start.

What not to do at the start: read `<source dir>/` end to end, recite the
documentation back to the user, re-ask decisions that are already recorded.

## During the session

Context is spent on reads, not on thinking. Cheaper: `grep -n` instead of a
whole file, a line range instead of a whole file, test output instead of a
retelling of the code.

The moment a fact about the project becomes durable, it goes into its file
right then, not "at the end of the session". The end may never come.

## Tracking what is left

Claude has no exact token counter — it cannot see its own window as a number.
Tracking is two-sided:

- **The indicator in Claude Code** is visible to the user. At around **20 %**
  remaining, the user says `/handoff`.
- **Indirect signs on Claude's side** — enough to offer to close without being
  asked: more than ~15 files read or ~2000 lines total; more than ~40 tool
  calls; a system warning about context being compacted; answers starting to
  lose details from the beginning of the session.

On any of them — **write the handoff immediately**, without finishing the
current edit. An unwritten handoff is lost whole when context is compacted; a
written one survives everything.

## Closing

1. Run `<check command>` if code changed. The result goes into the state file
   as-is, including failures.
2. Overwrite `<state file>` using the template inside it.
3. Sort durable facts into their topic files; add index lines for new files.
4. Commit if there is anything to commit, then push.
5. Say in one phrase what to open the next session with.

The state file is state, not a diary: overwritten whole, history lives in git.

## Next session

Opens at step 1 again. Check that what the state file says is still true — it
can go stale if something was changed by hand between sessions.
