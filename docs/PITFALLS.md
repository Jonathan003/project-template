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

_(none yet — the first one will arrive the first time something surprises you
badly enough that you would want to be warned again)_
