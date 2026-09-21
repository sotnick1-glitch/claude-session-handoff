---
description: Close the session: run the checks and overwrite the project's state file
---

Close the current session.

If this project has its own closing procedure (`docs/session.md`, a section in
`CLAUDE.md`, or its own `/handoff` command), follow that — it wins over this
file. Otherwise:

1. Find the state file: the one `CLAUDE.md` points at, or `STATUS.md` /
   `docs/handoff.md` / whatever this project already uses. There is exactly
   one. If there is none, say so and offer to set the ritual up with the
   `session-handoff` skill instead of inventing a new file now.
2. If code changed in this session — run the project's checks (the command in
   `CLAUDE.md`, the README, or the package manifest) and record the result
   as-is, including failures. Do not summarize a failure into "some tests
   fail" — paste what it said.
3. Overwrite the state file whole, using the template at the end of it: where
   we stopped, what is next (concretely, with paths to files), open ends,
   checks. Keep the template. Do not turn the file into a diary — history is
   in git.
4. Sort durable facts into their topic files rather than leaving them in the
   state file.
5. Commit if there is anything uncommitted, then push. Do not commit secrets
   or local config the project keeps out of git.
6. Answer in one phrase: what to open the next session with.
