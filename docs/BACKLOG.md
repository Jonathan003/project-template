# BACKLOG — known, understood, deliberately not fixed

The record of things we know about and have **chosen** not to fix yet. It exists
so that deferred is a decision with a reason attached, rather than something that
quietly got forgotten — and so a later reader can tell "nobody noticed this"
apart from "we looked at this and left it".

## What belongs here

A real finding, understood, with a reason it was not fixed now.

By default this is where a finding goes when it **has never fired on a path the
owner actually walks** — a defect that is real but only reachable in a situation
that has not arisen (see `CLAUDE.md`). Recording it costs a minute; fixing it
costs a round that was meant for something else.

## What does not

- Anything that produces **wrong output**, loses data, or leaks a secret. Those
  are fixed, never deferred.
- Speculative cleanups nobody has evidence for.
- Warnings. A standing "never do X" is a pitfall, not a debt — it belongs in
  `docs/PITFALLS.md`. Parked here it reads as an outstanding task and invites
  exactly the change it argues against.

## Rules

Every entry states **what it is**, **what it would cost if it bit**, **why it was
deferred**, and **what reopens it**.

Closing an entry means **deleting** it — in the commit that fixes it, or in a
commit that says it will not be done and why. An entry that turns out to be wrong
is deleted, not kept "for the record".

Re-read this file when it grows past a handful of entries. A backlog nobody will
act on makes every future reader re-decide the same items, which is the failure
this file exists to prevent. Closing several at once as won't-do is a normal,
healthy outcome.

## Format

```
- **N — one-line summary of the finding.**
  What it is, concretely.

  **What it would cost if it bit:** the real consequence.

  **Why it is deferred:** including whether it has ever actually happened.

  **Reopens if:** a condition, not a feeling.
```

---

## Open

- **1 — "Write the values" has no worked example when the output is not a file.**
  This template's verification practices came from a project whose program
  emitted files, where the evidence was a byte comparison of new output against
  a frozen baseline. That practice was dropped rather than adapted, on the
  reasoning that most web projects have no byte-comparable artifact at all, and
  that snapshot testing — the nearest equivalent — fails the same way a vacuous
  check does: snapshots get re-blessed without being read. Nothing was put in
  its place. `CLAUDE.md` now lists status codes and what the screen showed among
  its examples, but a list of examples is not a worked example. There is still
  no demonstration of what a recorded value *is* when the artifact is a rendered
  view or an API response — what it is compared against, and what makes it able
  to fail.

  **What it would cost if it bit:** the verification rules are the part of this
  template that gives the owner evidence instead of assurance. A project that
  reaches "write the values", finds nothing it can measure, and writes down
  whichever numbers happen to be available produces something that reads like
  evidence and is not. This failure is silent by construction: no check goes red
  when the values recorded are the wrong ones, so it would be discovered only by
  someone re-deriving the result by hand.

  **Why it is deferred:** it has never fired. No project has been built from
  this template yet, so nothing has met the gap. It is also not honestly fixable
  in the abstract — the analysis that identified it said plainly that it did not
  know what should replace byte comparison for a UI project, and inventing an
  answer with no real artifact in front of us would produce exactly the ceremony
  this template exists to avoid.

  **Reopens if:** the first real project whose output is not a file is started
  from this template. Close it by writing the worked example against that
  project's actual output — in `CLAUDE.md` if it fits the line limit, in `docs/`
  if it does not.
