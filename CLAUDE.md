# CLAUDE.md — Working rules for this project

Rules that always apply. **Keep this file under 60 lines, blanks included.** A
rule nobody reads is not a rule; anything longer belongs in `docs/`.

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
- **Write the values, not the verdict.** "It works" is a claim; counts, timings,
  sizes, hashes, status codes, what the screen showed, and how the run was
  configured are evidence. Repeat a baseline's values rather than pointing at
  them — a pointer cannot be checked, and rots when its input is deleted.
- **Verify through the real entry point.** Calling a function proves it works,
  never that it is reached. For anything triggered by an event the program does
  not raise itself — a keystroke, a click, a route change, a failed request, a
  timeout, a crash — produce the real event, through how it is really started.
- **Say what you did not verify.** Explicitly, in the same breath as what you did.
- **One command runs every check, and this project names it here:**

  > **Check command:** _(none yet — `README.md` says when it gets filled in)_

  Until it exists, verification is per-task and still required. What is deferred
  is the command, not the verifying. Once it exists, run it before every commit
  and say so — and it must fail when the list and the repo disagree, either way.

## Design

- **Show what an action is about to use** — the full path, the account, the
  environment, the record, and the date where it matters — at the moment the
  user confirms it. Reserve a typed confirmation for what cannot be undone: a
  prompt on every step trains the user to click through the one that matters.
- **Never commit a secret.** A token, key or password goes into `.gitignore`
  *before* the file holding it is created, never after. See `docs/HOOKS.md` —
  this is a fact rather than a judgement, so it is a hook candidate.
- **Name a thing once.** When a literal reaches a second file and its drift
  would be silent, pin it with a check, never a comment. See `docs/HOOKS.md`.

## Where things go

| File | Holds |
|---|---|
| `STANDARDS.md` | How to write a commit and an ADR |
| `docs/PITFALLS.md` | Failure modes that have already bitten — **read before diagnosing anything surprising** |
| `docs/HOOKS.md` | Which rules deserve enforcement instead of prose |
| `docs/adr/` | Decisions, with the reasoning that was actually persuasive |
