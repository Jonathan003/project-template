# PITFALLS — failure modes that have already bitten this project

Subtle ways this project has gone wrong. Not general programming advice: only
things that actually happened *here*, and produced no error message, or a
misleading one, when they did.

**Read this before diagnosing anything surprising.** That is the whole point of
the file — the second occurrence of a problem should be cheap.

## What belongs here

An entry earns its place when the failure was **quiet**: wrong output that looked
right, a check that passed while the thing it protected was broken, a step that
silently did nothing. A loud failure — a crash, a missing file, a visible error —
does not need an entry; it announces itself.

Claude also keeps machine-local notes of its own, outside git, readable with
`/memory`. **Those are not the record.** Anything that earns a number belongs in
this file, in the repository, even when it was first noticed in a note —
otherwise the knowledge accumulates where nobody reads it and this file stays
empty while looking maintained.

## Numbering

Entries are numbered in the order they were recorded, and **a number is
permanent**: never reused, never reassigned, because other files cite it. When
adding one, take the next number above the highest currently in use anywhere in
this file — not the next one after whichever entry you happen to be writing
beside. Sections group by area, so the numbers deliberately run out of order.

## Format

Each entry:

```
### N. Short title saying what goes wrong

**Symptom:** what you would actually see. Include the misleading part.

**Cause:** the real mechanism, not the surface.

**Solution:** what was done, or what to do instead.

**Watch for:** the wider class — where else this shape could appear.
```

The **Watch for** line is what makes an entry worth more than the bug it came
from. Write it even when it feels obvious.

---

## Entries

### 1. An untrusted workspace loads nothing under `.claude/`, and says nothing

**Symptom:** a permission rule in `.claude/settings.json` has no effect. The file
is on disk, it is valid JSON, the syntax matches the documentation, and the
command it names runs straight through with no prompt. Nothing is logged, no
warning appears, and `/doctor` reports no problem. Every check you can think of
to run against the *file* passes, because the file was never the problem.

**Cause:** the workspace was never trusted. Claude Code shows a trust dialog the
first time a session starts in a directory; until it is accepted, **the entire
`.claude/` directory is ignored** — permission rules, hooks, and MCP server
config alike. Not partially applied, not applied with a warning: not read. An
untrusted workspace and a workspace with no `.claude/` at all behave
identically, which is exactly why the file looks innocent.

**Cost here:** a push guard was written, documented in `docs/HOOKS.md`, and
committed (`f20e8de`) — and then a push went through unchallenged with the
guard sitting valid on disk. The guard was believed to be live for the whole
period between writing it and testing it, and the belief was reasonable: every
piece of evidence available without testing said it was.

**Solution:** check `/permissions` → the **Ask** tab. The shipped rules
(`Bash(git push *)` and `PowerShell(git push *)`) are listed there when
`.claude/` has loaded, and absent when it has not — an empty tab is the
diagnosis. Fix it by accepting the trust dialog at session startup; restart the
session if it was dismissed. `/permissions` is also the honest confirmation
afterwards, because it reads what the harness actually loaded rather than what
is on disk.

**Watch for:** anything whose absence is silent. Config that is *skipped*
produces the same evidence as config that is *satisfied* — no output either
way. The general shape: never conclude a rule is live from the file being
correct. Ask the thing that would enforce it what it has loaded, and where that
is possible, make it fire once. This is the permission-rule form of "every check
must be proven able to fail".
