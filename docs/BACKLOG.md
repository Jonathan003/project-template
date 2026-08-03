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

_(none yet)_
