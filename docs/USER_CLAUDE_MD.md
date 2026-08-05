# The user-level CLAUDE.md — a copy, because the real one is not in any repo

The rules in `CLAUDE.md` say how work is done *in this project*. There is a
second rules file that says how work is done *with this owner*, on every project
on the machine — and it lives outside every repository, so nothing pushes it
anywhere and nothing restores it.

This page is that file's documented copy. It exists so the content survives one
disk.

## Where the real file belongs

| Platform | Path |
|---|---|
| Windows | `C:\Users\<your-user>\.claude\CLAUDE.md` |
| macOS / Linux | `~/.claude/CLAUDE.md` |

**This page is not that file, and Claude does not read this page as rules.** It
is documentation inside the repository; the real one is a separate file in your
home directory. Copying the block below into this page changes nothing — it has
to go into the path above.

Claude Code loads the user-level file *before* a project's `CLAUDE.md`, every
session. That is why the project file does not repeat any of it: it would only
be read twice.

## On a new machine

Ask Claude to create the file at the path above with the content below. It is
one file, no install step, and it applies from the next session onwards. Then
confirm it took: ask what the working rules are and the answer should include
running the finished programs yourself.

If you have changed how you work since this copy was written, say so instead of
copying blindly — then ask for this page to be updated to match, so the copy
stays worth having.

## The content

```markdown
# How I work with Claude Code

Applies to every project on this machine.

- I have **no programming background** and never type terminal or git commands.
  You are my hands for all code, git and shell work.
- **Explain choices in plain terms, and always include a recommendation.**
- **I run the finished programs myself.** Never run a project's interactive
  tools, menu steps or launchers for me.
- **Never print tokens, secrets or credentials.**

## How much to do before checking with me

- **No undo, or it changes what a program produces** — deleting data,
  publishing, pushing, changing generated output: **plan first and stop** for my
  approval.
- **Reversible, and a check or git can undo it** — ordinary code and fixes:
  **plan and implement in one go**, then report.
- **Read-only investigation** — reading, searching, explaining: **just do it**
  and report.
```

## How this maps onto the project's own files

The three tiers above are the same division `docs/WORKING_WITH_THE_AGENT.md`
calls tier 1, tier 2 and tier 3. That file is the longer version, with the
switches — `ask` rules and plan mode — that make a tier stick without either of
us remembering it. The block above is the part that has to exist on the machine
before any project is opened at all.
