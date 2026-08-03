# Project Standards

How to write a commit and an ADR. Working rules live in `CLAUDE.md`.

## Commit messages

One clear sentence saying **what changed and why**, then a body when there is
more to say. Prefixes like `feat:` / `fix:` are fine if you want them; they are
a sorting aid, not a safety practice, so nothing here depends on them.

The body is the part that matters. It should say:

- what changed, and why it was worth changing
- **what was verified, with the values** — counts, sizes, hashes, timings, and
  what the run was configured with (`CLAUDE.md`, "write the values")
- **what was deliberately not done**, and why
- anything found along the way that was left alone (and whether it has ever
  actually happened — see `docs/BACKLOG.md`)

A body that says "verified, all good" records nothing. A body that says
"4 files unchanged, 3,304,641 B, zero network calls" can be checked later.

## Design decisions: ADRs

Write an Architecture Decision Record in `docs/adr/` when a decision is:

- **Significant** — it affects structure, behaviour, or how the owner works
- **Debatable** — more than one reasonable approach existed
- **Likely to be questioned later** — by you, or by a future session

Skip them for routine fixes and obvious choices. Write as many as the project
actually generates: a busy month can produce a dozen, a quiet one none. Do not
ration them against a quota.

### How

1. Copy `docs/adr/template.md` to `docs/adr/NNNN-short-title.md`
2. Fill it in — including the alternatives that were genuinely weighed, and the
   reasoning that was actually persuasive, not just the conclusion
3. Add a row to the index in `docs/adr/README.md`
4. Commit it

### Status: Proposed until the work has shipped

An ADR has two statuses: **Proposed** and **Accepted**.

**An ADR covering work still in flight stays Proposed until that work has
shipped and been verified.** Write it early if it helps you think — that is
often its best use — but flip it to Accepted only in the last commit of the
span, once the checks pass and the result has been looked at.

This is the rule that matters most here. A record accepted at the *start* of a
span describes a plan, and the plan then moves while the record does not. In the
project this template is drawn from, exactly that happened: an ADR was marked
Accepted in the first commit of its span, and ten commits later five of its
sentences were untrue — three of them in the Decision.

### Changing an ADR

Prefer a **new ADR that supersedes the old one**, linked both ways, with the
index row updated. That keeps the reasoning of both.

When a record is simply *wrong* — it states something that was never true — fix
it in place and add a dated note in its Status section saying what was corrected
and why. An inaccurate permanent record is worth less than an edited one.
