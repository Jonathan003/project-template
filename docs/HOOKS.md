# HOOKS — rules worth enforcing instead of writing down

A rule in `CLAUDE.md` is a sentence the agent reads and applies. A hook is a
command the harness runs whether or not anyone remembered. They cost different
things and they fail differently.

## Try a permission rule first

Most of what a hook gets reached for is a plain command or path match, and for
that a **permission rule** in `.claude/settings.json` is shorter and stronger:

- It is evaluated ahead of hook output — a matching `deny` or `ask` rule holds
  whatever a hook returns.
- It applies in every permission mode, including the ones that skip prompts.
- It is JSON with no shell in it, so quoting cannot break it.
- It is live from the first session — **once the workspace is trusted.** That
  condition is not an advantage over a hook: until the trust dialog is accepted,
  nothing under `.claude/` is loaded at all, rules and hooks alike, and nothing
  says so. See `docs/PITFALLS.md` entry 1, which is what this bullet used to get
  wrong.

Use `ask` for what you want to be asked about, `deny` for what must never
happen. A hook earns its place only when the condition is more than a match —
running a command, reading a file, judging from context.

## What is shipped

`.claude/settings.json` carries one rule, in the two forms this machine needs:

    "ask": ["Bash(git push *)", "PowerShell(git push *)"]

Pushing is the owner's decision, so it **asks** rather than denies: a deny rule
cannot be answered, and the owner would have to have the settings edited before
any push could happen at all. It is written twice because a `Bash(...)` rule
does not cover the PowerShell tool, and on Windows both tools exist. The
trailing ` *` enforces a word boundary — it matches `git push` alone and
`git push origin main`, and not a command that merely starts with those letters.

**Verified — the rule fires.** On 2026-08-03, Claude Code 2.1.220, with both
rules listed under `/permissions` → Ask, `git push origin main` was attempted
three times from this repository through the Bash tool. Every attempt prompted.
The dialog, verbatim:

    Bash command
      cd "C:/Claude Code/project-template" && git push origin main
      Push main to origin
    Ask rule Bash(git push *) overrides auto mode for this command.
    /permissions to let auto mode decide
    Do you want to proceed?
    ❯ 1. Yes
      2. No
    Esc to cancel · Tab to amend · ctrl+e to explain

Three things in that text are the actual result, beyond the prompt appearing.
It **names the rule that stopped it** — `Bash(git push *)` — so a prompt can be
traced to the line that caused it rather than guessed at. It says the rule
**overrides auto mode**, which is the guarantee worth having: the modes that
skip prompts do not skip this one. And not proceeding is a real outcome — the
first attempt was intercepted and never executed, leaving the commit local
(`origin/main` at `e3cddbe`, local `HEAD` at `3ca7e86`, ahead 1). Which answer
ended it is not recorded, because it cannot be told apart from here: `No` and
`Esc to cancel` are separate options in the dialog and reach the agent as the
same rejection. The second attempt was answered `1. Yes` and completed:
`e3cddbe..3ca7e86 main -> main`, both refs at `3ca7e86` afterwards.

The third attempt is the one that settles the shape of the guard. It was a
**no-op** — everything was already pushed — and it prompted anyway, and it
prompted **again** after the second attempt had been approved. So the rule
matches the command string, not the outcome, and an approval is not remembered:
it asks every time. That is the behaviour to want here. A guard that stops
asking after the first yes is a guard that is loudest when the risk is lowest.

**The negative half, same day and version:** `git status` on this repository ran
straight through with no prompt, returning `On branch main`, `ahead of
'origin/main' by 1 commit`, `working tree clean`. So the rule is narrow — it
catches pushes without catching git in general. Run it after any change to the
rule, and run it in the same session as the positive test: a `git status` from
before the workspace was trusted proves nothing, since at that point no rule
could have fired for any command.

If the prompt does not appear at all, the first thing to check is not the file.
See `docs/PITFALLS.md` entry 1: an untrusted workspace loads nothing under
`.claude/` and says nothing about it, which is exactly how this rule spent its
first days looking correct and doing nothing. A settings file created inside a
running session may also not be read until the session restarts.

### Both entries are load-bearing — do not dedupe them

`Bash(git push *)` and `PowerShell(git push *)` are not two spellings of one
rule. Each tool is its own permission surface, and a `Bash(...)` rule does not
match a call made through the PowerShell tool.

**That second half stands untested — and why it does is worth more than the
result would have been.** The test *was* performed. On 2026-08-06, Claude Code
2.1.223, the `PowerShell(git push *)` entry was deleted from
`.claude/settings.json`, the session restarted so the file would be re-read
(`docs/PITFALLS.md` entry 1), and `git push origin main` run through the
PowerShell tool with only the Bash entry in place. Whether a dialog appeared
*is* the entire result — and nobody read it. The guard was restored the same day
(`"ask": ["Bash(git push *)", "PowerShell(git push *)"]`, tree clean, local and
remote both at `ad4be50`), so the run is over and its result exists nowhere.

**It cannot be recovered afterwards, because command output does not carry it.**
An approved prompt and no prompt at all reach the agent identically: the command
runs and returns its normal output. Two further pushes from the restored state
that same day, run specifically to re-observe the dialog, both returned
`Everything up-to-date` with both refs at `ad4be50` — and went unread as well.
The reading has to be taken by the person at the keyboard while the dialog is on
screen. A run whose dialog is not read produces nothing, however carefully it was
set up. Redoing it therefore costs what it always did: removing the entry again
and restarting leaves the PowerShell surface genuinely unguarded between the
edit and the restore, and that hole is the silent kind described below — the
Bash rule keeps firing for its own tool, so the guard still looks alive.

Note what the witnessed run below *does* and does not settle. It shows the
PowerShell entry is the one that fires **when both are present**. It cannot show
what happens when that entry is absent, which is the only question the negative
test answers. Two further limits, had the reading been taken: the Bash route was
not re-checked in that session — the Bash entry was left in place and never
retried — so the run spoke only to the PowerShell surface. And the branch was
already up to date, making those two pushes no-ops; a reading would have re-shown
the prompt matching the command string, which the third attempt above already
establishes, rather than the guard stopping a push with commits behind it. That
second gap is closed by the fourth run below; the first is not.

**Verified — the PowerShell entry is the one that fires on Windows.** On
2026-08-06, Claude Code 2.1.223, `git push origin main` was run through the
**PowerShell** tool from this repository. It prompted, and the dialog named the
PowerShell entry outright:

    Ask rule PowerShell(git push *) overrides auto mode for this command.
    Do you want to proceed?
      1. Yes
      2. No

Answered `1. Yes`, it completed: `a158eb2..8124312  main -> main`, with
`git ls-remote origin refs/heads/main` returning
`81243126697b78722c971c267247cddfd22a6187` afterwards and `git status -sb`
reading `## main...origin/main` with no ahead marker. The owner reports the same
result on 2026-08-03 and 2026-08-05. Note that the run recorded above at
2026-08-03, Claude Code 2.1.220, went through the **Bash** tool and was stopped
by the Bash entry — so the two halves of this file cover two different tools, on
purpose.

**Verified — it prompts on a push that really transfers.** Every witnessed
PowerShell prompt before this one sat in front of a no-op, which left the useful
case open. On 2026-08-06, Claude Code 2.1.223, commit `d1f321e` — this section's
own rewrite — was pushed from this repository through the **PowerShell** tool
with both entries present. It prompted, naming the same entry:

    Ask rule PowerShell(git push *) overrides auto mode for this command.

Answered yes, it completed `ad4be50..d1f321e  main -> main`: a real transfer, not
`Everything up-to-date`. Afterwards `git ls-remote origin refs/heads/main`
returned `d1f321e9fb2f7a22d22505c3aa35ac8061dd7855`, and `git status -sb` read
`## main...origin/main` with no ahead marker.

**How that reading was taken is the reason the three runs before it produced
nothing.** Clicking *inside* the terminal window to focus it also answers the
prompt — the click reaches the dialog, which is gone before the line can be read,
and the command then runs looking exactly like a command that was never guarded.
Clicking the window's **title bar** focuses the window without reaching the
dialog, leaving it on screen to be read. That is the whole difference between a
run that is evidence and a run that is not.

On Windows both tools exist and either may carry a push. Deleting either entry
opens a hole on whichever tool loses its rule, and the hole is **silent**: the
surviving rule keeps firing for its own tool, so the guard still looks alive
from the outside.

**A future pass applying "name a thing once" must leave these two alone.** This
is the case that rule does not cover. The two strings are not one name written
twice — they name two different permission surfaces, and pinning them to each
other would be pinning together two things that are allowed to differ.

## When a rule deserves a hook

All three have to hold:

1. **It must hold every single time.** Not "usually", not "unless the owner said
   otherwise" — a hook cannot weigh context, so a rule with legitimate exceptions
   becomes an obstacle the moment one arrives.
2. **Overlooking it is plausible.** Long sessions, many files, a rule stated once
   forty turns ago.
3. **The violation is expensive or hard to undo.** A hook buys nothing on a
   mistake you would notice immediately and fix in a second.

The source project put it best after a comment meant to keep two copies of a
rule in step failed anyway, and the drift went unnoticed for eleven days:
**a comment is not a mechanism.** Neither is a sentence in a rules file.

That incident is the reason `CLAUDE.md` carries **name a thing once**, and it is
worth saying what the rule does *not* ask for. It is not "pin every duplicate".
The source project deliberately left one name unpinned — a settings filename
repeated in four places — because that failure is **loud**, and a loud failure
announces itself without help. The condition is the same one `docs/PITFALLS.md`
admits entries on: pin it when the drift would be **silent**.

### The pin has a trap of its own

A repo-wide check for a forbidden literal **contains that literal**, in its own
source or in the comment explaining it, so it finds itself. In the source
project this was exposed three times in one session. It fails two ways: a pin
that goes red the moment it is written — annoying but visible — and a pin that
passes for the wrong reason, which is the vacuous check this whole file is
about. Exclude the checking file's own path, then break the thing it protects
and confirm it goes red, or the exclusion becomes the next thing nobody proved.

## Which of this template's rules qualify

| Rule | Mechanism | Why |
|---|---|---|
| Do not push without being asked | **Permission rule** | A plain command match, so a rule beats a hook. Shipped and live |
| Never commit a secret | **Hook, eventually** | Always true and expensive — but `.gitignore` already covers the known cases, so add the hook when a real credentials file appears, matching its real name |
| Run the check command before committing | **Hook, once it exists** | Always true, easy to skip, and more than a match — it runs a command. Useless before that command exists |
| Name a thing once | **A check** | Neither prose nor a hook. The mechanism is a check inside the check command, which is the only form that outlives the session that wrote it |
| Prove every check can fail | **Prose** | Requires judgement about what "the behaviour it protects" is. Not mechanically decidable |
| Write the values, not the verdict | **Prose** | Same reason. Nothing mechanical can tell evidence from assertion |
| One change at a time | **Prose** | "One change" is a judgement call, and a wrong block here would be constant |

The pattern: **matches become rules, procedures become hooks, judgement stays in
`CLAUDE.md`** — where judgement is available.
