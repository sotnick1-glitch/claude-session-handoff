# session-handoff

A session ritual for Claude Code: start where you stopped, close without
losing anything.

**[Русский](README.md)**

## Why

A session's context is finite. It always runs out — mid-edit, casually,
without warning. What follows is the expensive part: the next session knows
nothing. Not what is already done, not what is broken, not why a decision was
made that way six months ago. You re-explain it by hand for twenty minutes,
and it gets worse every time, because you are forgetting too.

The goal of the ritual: **an interrupt at any point costs no more than five
minutes.**

There is no magic here. Four text files the project carries in its own git
repository.

## What it is made of

| File | Role |
|---|---|
| `CLAUDE.md` in the root | loads automatically at session start. Says what to read, when to close, and lists the 4–5 hard rules of the project |
| the state file (`STATUS.md`, `docs/handoff.md` — whatever you already use) | where we stopped, what is next, open ends, check results. Overwritten whole |
| `.claude/commands/handoff.md` | the `/handoff` command — closing the session in five steps |
| `docs/session.md` and `docs/INDEX.md` | large projects only: the procedure and a "question → file" index |

## What it looks like

The state file at the end of a session is not a diary and not a report — it is
what the next session starts from in under a minute:

```markdown
# Shop — state

**Updated:** 14 March

## Where we stopped
The cart discounts correctly except for a shipping promo code —
`src/cart/discount.ts:88`, branch `promo-shipping`. The test for that case is
written and failing on purpose.

## What is next
Open `discount.ts:88`, route the shipping cost through the same branch as the
items. The red test is `cart.test.ts:140`.

## Open ends
The owner has not decided whether a shipping promo stacks with the volume
discount. For now we assume it does not.

## Checks
`npm test` — 214 of 215, `cart.test.ts:140` failing on purpose.
```

Three minutes of writing at the end of a session against twenty minutes of
re-explaining at the start of the next one.

## Install

In Claude Code:

```
/plugin marketplace add sotnick1-glitch/claude-session-handoff
/plugin install session-handoff@session-handoff
```

Then, in any project, say "set up the session ritual" (or `/session-handoff`).
The skill looks at what the project is, how its checks are run, whether a state
file already exists under another name, and sizes the setup accordingly: three
files for a small project, five for a large one. It writes them in the language
you are speaking.

The plugin is optional: you can copy `skills/session-handoff/templates/` into
your project and fill the `<...>` placeholders by hand. The plugin only does
that for you, and does not forget to size it.

## Using it

- **Open a session:** say so, in the project folder.
- **Close it:** `/handoff`.

⚠️ `CLAUDE.md` is picked up **only from the folder the session was opened in**.
If the project sits in a subfolder and the session was opened a level above,
the file is never read and the ritual never fires. Open the session in the
project folder itself.

## Three decisions that hold it together

Carry these over with the files, or the system falls apart within a month.

**The index is a table of contents, not a summary.** The moment it holds
content, it becomes another document you have to read in full, and the point
is lost. A new fact goes into its topic file; the index gets a link line.

**The state file is state, not a diary.** Overwritten whole. History lives in
git and can always be recovered from there. A diary grows, and after twenty
sessions nobody reads it — Claude included.

**A fact is written the moment it is learned, not "at the end of the
session".** The end may never come. An unwritten handoff is lost whole when
context is compacted — so it gets written before the current edit is finished.

## What is not here

Automation. No hooks, no daemons, no background writes: Claude writes the
files on command, and you see every change in the git diff. That is deliberate
— a system that writes by itself will one day write something untrue, and you
will find out a month later.
