# HOOKS — rules worth enforcing instead of writing down

A rule in `CLAUDE.md` is a sentence the agent reads and applies. A hook is a
command the harness runs whether or not anyone remembered. They cost different
things and they fail differently.

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

| Rule | Hook? | Why |
|---|---|---|
| Do not push without being asked | **Yes** | Always true, easy to forget, outward-facing and awkward to undo. This is the worked example |
| Never commit a secret | **Yes, eventually** | Always true and expensive — but `.gitignore` already covers the known cases, so add the hook when a real credentials file appears, matching its real name |
| Run the check command before committing | **Yes, once it exists** | Always true, easy to skip — but useless before there is a check command, so it is a placeholder |
| Prove every check can fail | **No** | Requires judgement about what "the behaviour it protects" is. Not mechanically decidable |
| Write the values, not the verdict | **No** | Same reason. A hook cannot tell evidence from assertion |
| One change at a time | **No** | "One change" is a judgement call, and a wrong block here would be constant |

The pattern: hooks are for **facts** (this command, this path, this filename).
Rules that need judgement stay in `CLAUDE.md`, where judgement is available.

## The example file

`hooks/settings.example.json` is **inert** — nothing in `hooks/` is read by
anything. To activate it, copy the `hooks` key into `.claude/settings.json` and
delete every key whose name starts with an underscore.

It contains exactly one complete, working entry: a `PreToolUse` hook on `Bash`
that refuses `git push`. It uses the `if` field (`"Bash(git push*)"`) so the
command only runs on a matching call, and returns a `permissionDecision` of
`deny` with a reason the agent sees.

Everything else lives under `_PLACEHOLDERS_DELETE_THIS_WHOLE_KEY` and is
deliberately non-functional: each contains a literal `FILL_IN_...` that must be
replaced first. **Do not copy a placeholder without filling it in** — a hook
pointing at a command that does not exist fails on every matching tool call.

## What was verified, and what was not

- **Verified:** the file parses as JSON; the deny-command emits valid JSON under
  both bash and PowerShell; and the command, extracted back out of the JSON
  after escaping, still produces `permissionDecision: deny`.
- **Not verified:** that the hook fires. It has never been installed in this
  repository — that would mean creating `.claude/settings.json`, which the
  template deliberately does not ship.

That distinction is the point of the rule about proving a check can fail: a hook
that has been written is not a hook that has been shown to run. When you do
activate it, confirm it by attempting the thing it should block **once**, and by
confirming an unrelated command still passes.

One caveat if it appears not to work: the settings watcher only picks up
`.claude/` if a settings file was present when the session started. Opening
`/hooks` once, or restarting the session, reloads it.
