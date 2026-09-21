# Session start

Read `<state file>` — and nothing else until it is needed. It holds the state
at the end of the last session: what works, what is broken, what is next.
`<other doc>` is for `<when it is relevant>` only.

Do not read `<source dir>/` end to end: `grep -n` on the name you need is
cheaper than a whole file.

Close the session with `/handoff`. Signs it is time to close without being
asked: more than ~15 files read, more than ~40 tool calls, a warning about
context being compacted. On any of them — write the handoff immediately,
without finishing the edit in progress: an unwritten handoff is lost whole when
context is compacted.

A durable fact about the project goes into `<state file>` the moment it is
learned, not "at the end of the session". The end may never come.

# Rules

- <rule that breaks the project if violated>
- <decision already made, not to be re-litigated>
- <trap someone already fell into once>
- <boundary that must never be crossed without the owner saying so>
