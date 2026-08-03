# Working with the agent

For you, not for Claude. How to drive a session. What Claude does is in
`CLAUDE.md` — this is the other side of the same arrangement.

**The division of labour:** you decide what the project should do and you run
the finished programs yourself. Claude is your hands for everything else —
writing the code, running the checks, reading the output. You never need to type
a git or terminal command.

## Three kinds of request

Say which one you mean, and a session stops feeling unpredictable.

**Tier 1 — irreversible, or it changes what the program produces.**
Deleting data, publishing, pushing to GitHub, changing how output is generated,
anything you cannot simply undo. *Ask for a plan and expect Claude to stop.*
You read the plan and say go. The cost of a wrong tier-1 action is real, so the
pause is worth it every time.

**Tier 2 — reversible, and something would catch a mistake.**
Ordinary feature work and bug fixes in a project under version control.
*Ask for a plan and let it be implemented in one go.* Stopping between plan and
work buys you nothing here — git is the undo button, and the checks are the net.

**Tier 3 — read-only.**
"What does this do?", "is X handled?", "find where Y happens". *Just ask.*
No plan, no confirmation. Nothing changes.

If you do not say, Claude should assume tier 2 for work and tier 1 for anything
that touches the outside world.

## Handing over a batch

When you are going to be away, put several tasks in **one message**, in the
order you want them, and say which are tier 1. Claude works down the list and
stops at the first tier-1 item, with everything before it finished. That is far
better than one task at a time — a session that finishes in ten minutes and then
waits four hours for you is four hours wasted.

Say "do not stop to ask" explicitly if you want the batch finished under stated
assumptions rather than paused. And say what to do with anything found along the
way — the default is that it goes to `docs/BACKLOG.md` rather than getting fixed.

## When to start a second session

Start a separate one when the work is **genuinely independent** — a different
part of the project, or a long investigation you do not want in the way of
ordinary work. Two sessions editing the same files at the same time will
conflict, and neither will know.

Good second session: "read the whole project and tell me what is fragile."
Bad second session: anything touching files the first one is editing.

## If you set up a repeating or self-driving task

Give it a **stop condition that names the check that must pass**, never the
outcome you want to be true.

- Good: *"keep going until the check command passes with no failures."*
- Bad: *"keep going until the app works properly."*

"Works properly" cannot be evaluated, so a loop given that condition either
stops on the first thing that looks fine or never stops at all. A named check
either passes or it does not.

## What to expect back

Claude should tell you what it verified **with the actual numbers**, and say
plainly what it did *not* verify. "It works" is not a report. If you get one,
ask what was measured — the habit is in `CLAUDE.md` and it is fair to hold the
session to it.
