# CLAUDE.md — Working rules for this project

Rules that always apply. **Keep this file under 60 lines.** A rule nobody reads
is not a rule; anything longer belongs in `docs/`.

## Working

- **One change at a time.** One focused change per commit.
- **Root cause only.** No masking fixes, no workarounds. Diagnose, then fix.
- **Do what was asked.** Fix the thing requested. Anything else found along the
  way is *reported*, not folded in — one line each, saying what it is, what it
  would cost, and **whether it has ever actually happened or is only possible**.
- **A finding that has never fired on a path the owner walks goes to
  `docs/BACKLOG.md` by default**, not into the round in hand. This orders the
  work; it does not limit what may be found, or what may be fixed when asked.

## Verifying

- **Every check must be proven able to fail.** After writing a check, break the
  behaviour it protects, confirm the check goes red, restore, and confirm the
  restore. A check that passes against broken code is worse than no check: it
  produces confidence at no cost.
- **Write the values, not the verdict.** "It works" is a claim; numbers, sizes,
  hashes, counts, and what the run was configured with are evidence. Repeat a
  baseline's values rather than pointing at where they were recorded — a pointer
  cannot be checked by the reader and rots when its input is deleted.
- **Verify through the real entry point.** Calling a function proves the
  function works, never that it is reached. For anything triggered by an event
  the program does not raise itself — a keystroke, a signal, a timeout, a crash
  — produce the real event, through the way the program is really started.
- **Say what you did not verify.** Explicitly, in the same breath as what you did.
- **One command runs every check, and this project names it here:**

  > **Check command:** _(none yet — fill this in the day it exists)_

  Whatever suits the language: an `npm` script, a task-runner target, a shell
  script. Add it at whichever of these comes first:
  - **At the first bug fix** — a bug that happened once can happen again, and
    that is the first moment there is behaviour worth pinning.
  - **Before the program is first used for real** — a release, a deploy, or
    handing it to someone else. A project without bugs still needs this.

  Before then there is nothing to check and a green run would mean nothing.
  Once it exists, run it before every commit and say so in the commit.

## Working with the owner

- The owner has **no programming background**. He runs the finished programs
  himself; you write and run the code and the checks.
- **Explain choices in plain terms, and always include a recommendation.**
- **Echo, do not ask.** Show what a step is about to use — full path, and date
  where it matters — at the moment he confirms it. Add a typed confirmation only
  where an action cannot be undone. A confirmation at every step trains him to
  press Enter through all of them, including the one that matters.
- **Never print or commit secrets** — tokens, keys, passwords. See `.gitignore`.

## Where things go

| File | Holds |
|---|---|
| `STANDARDS.md` | How to write a commit and an ADR |
| `docs/PITFALLS.md` | Failure modes that have already bitten — **read before diagnosing anything surprising** |
| `docs/BACKLOG.md` | Known, understood, deliberately unfixed |
| `docs/HOOKS.md` | Which rules deserve enforcement instead of prose |
| `docs/WORKING_WITH_THE_AGENT.md` | For the owner, not for you |
| `docs/adr/` | Decisions, with the reasoning that was actually persuasive |
