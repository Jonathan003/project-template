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
- Rules that only *restrict* apply without the workspace trust dialog, so a rule
  shipped with the project is live in the first session.

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

**Verified:** the file parses as JSON, and the rule is the syntax the permission
documentation gives for this exact command.

**Not verified:** that the prompt appears. No push has been attempted here.
Confirm it once by asking for a push and seeing the prompt, and once by asking
for an unrelated git command and seeing none. A rule that has been written is
not a rule that has been shown to fire — the same distinction as proving a check
can fail. One caveat if it seems not to work: a settings file created inside a
running session may not be read until the session restarts.

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

## Which of this template's rules qualify

| Rule | Mechanism | Why |
|---|---|---|
| Do not push without being asked | **Permission rule** | A plain command match, so a rule beats a hook. Shipped and live |
| Never commit a secret | **Hook, eventually** | Always true and expensive — but `.gitignore` already covers the known cases, so add the hook when a real credentials file appears, matching its real name |
| Run the check command before committing | **Hook, once it exists** | Always true, easy to skip, and more than a match — it runs a command. Useless before that command exists |
| Prove every check can fail | **Prose** | Requires judgement about what "the behaviour it protects" is. Not mechanically decidable |
| Write the values, not the verdict | **Prose** | Same reason. Nothing mechanical can tell evidence from assertion |
| One change at a time | **Prose** | "One change" is a judgement call, and a wrong block here would be constant |

The pattern: **matches become rules, procedures become hooks, judgement stays in
`CLAUDE.md`** — where judgement is available.
