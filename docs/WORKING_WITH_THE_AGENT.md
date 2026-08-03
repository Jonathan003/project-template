# Working with the agent

For you, not for Claude — how to drive a session. What Claude does is in
`CLAUDE.md`; this is the other side of it. **The division of labour:** you
decide what the project should do and you run the finished programs yourself.
Claude is your hands for everything else, and you never need to type a git or
terminal command.

## Three kinds of request

Say which one you mean, and a session stops feeling unpredictable. Two of the
three also have a switch, so you do not have to keep saying it.

**Tier 1 — irreversible, or it changes what the program produces.**
Deleting data, publishing, pushing to GitHub, changing how output is generated,
anything you cannot simply undo. *Ask for a plan and expect Claude to stop.*
You read the plan and say go. For a specific command you always want to be
asked about, an `ask` rule in `.claude/settings.json` makes the pause automatic
and unforgettable — `git push` already has one. See `docs/HOOKS.md`.

**Tier 2 — reversible, and something would catch a mistake.**
Ordinary feature work and bug fixes in a project under version control.
*Ask for a plan and let it be implemented in one go.* Stopping between plan and
work buys you nothing here — git is the undo button, and the checks are the net.

**Tier 3 — read-only.**
"What does this do?", "is X handled?", "find where Y happens". *Just ask.*
Press **Shift+Tab** for **plan mode** and tier 3 stops depending on either of us
remembering: Claude can read, search and plan, but cannot edit anything until
you approve a plan.

If you do not say, Claude should assume tier 2 for work and tier 1 for anything
that touches the outside world.

## Starting something big

Do not write the spec yourself. Say what you want in a sentence, then ask to be
interviewed: *"interview me in detail with the AskUserQuestion tool — the hard
parts I might not have considered, edge cases, tradeoffs, what happens when it
goes wrong. Keep going until we have covered it, then write the spec to
`SPEC.md`."* Then **start a fresh session** and point it at `SPEC.md`: it spends
its whole attention on building rather than on the conversation that produced
the spec, and you have something written to hold the result against.

## Handing over a batch

When you are going to be away, put several tasks in **one message**, in the
order you want them, and say which are tier 1. Claude works down the list and
stops at the first tier-1 item, with everything before it finished.

Say "do not stop to ask" if you want the batch finished under stated assumptions
rather than paused. And say what to do with anything found along the way — the
default is `docs/BACKLOG.md`, not a fix.

## When to start a second session

Start a separate one when the work is **genuinely independent** — a different
part of the project, or a long investigation you do not want in the way of
ordinary work. Two sessions editing the same files at the same time will
conflict, and neither will know.

Good second session: "read the whole project and tell me what is fragile."
Bad second session: anything touching files the first one is editing.

**If the second session is for reviewing rather than building, say so in its
opening prompt — it is read-only, it must not edit or commit.** A reviewing
session that quietly starts fixing what it found is the conflict this section
exists to prevent, and one sentence is cheaper than any other way of avoiding
it. Put that session in **plan mode** (Shift+Tab) as well and the rule is
enforced rather than requested.

## If you set up a repeating or self-driving task

Give it a **stop condition that names the check that must pass**, never the
outcome you want to be true. "Until the check command passes with no failures"
can be evaluated; "until the app works properly" cannot, so a loop given it
either stops on the first thing that looks fine or never stops at all.

**`/goal <condition>`** is that as a feature: a separate evaluator re-checks the
condition after every turn and the session keeps working until it holds.

## Checking what came back

Claude should tell you what it verified **with the actual numbers**, and say
plainly what it did *not* verify. "It works" is not a report.

Before calling a span finished, ask for eyes that have not seen the reasoning:
*"use a subagent to review this diff against the plan"*, or run
**`/code-review`**. The reviewer gets the diff and your criteria, not the
argument that produced them, and reports back into this session.

Say what counts as a finding: **only gaps affecting correctness or the stated
requirements.** A reviewer asked for gaps reports some even when the work is
sound; chasing all of them buys defensive code and tests for cases that cannot
happen. That is the official form of the rule already in `CLAUDE.md`: what is
found along the way is reported, not folded in.
