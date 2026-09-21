---
description: Close the session: run the checks and overwrite <state file>
---

Close the current session:

1. If code changed in this session — run `<check command>` and record the
   result as-is, including failures.
2. Overwrite `<state file>` using the template at the end of that file: where
   we stopped, what is next (concretely, with paths to files), open ends,
   checks. Keep the template at the end of the file.
3. Sort durable facts into their topic files. A new documentation file means a
   new line in the index (create `docs/INDEX.md` once there are more than
   three or four documents).
4. Commit if there are uncommitted changes, then push — the repository is the
   backup. Do not commit `<secrets / local config to keep out>`.
5. Answer in one phrase: what to open the next session with.
