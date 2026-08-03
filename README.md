# Project Template

The starting point for a new project. Language-neutral — web, desktop, a script,
anything. Nothing here assumes a particular stack.

## Starting a new project

1. On GitHub, click **"Use this template"** → **"Create a new repository"**
2. Name it
3. Open a Claude Code session in it and say what you want to build

Claude does the rest of the setup: cloning, wiring, and filling in the two
blanks below. You do not need to run git or terminal commands yourself.

## What is here, and who it is for

| File | For | Holds |
|---|---|---|
| `CLAUDE.md` | Claude | The working rules. Read every session, applied every turn |
| `docs/WORKING_WITH_THE_AGENT.md` | **You** | How to drive a session — what to hand over, what to check |
| `STANDARDS.md` | Claude | How to write a commit and an ADR |
| `docs/PITFALLS.md` | Claude | Failure modes that have already bitten *this* project |
| `docs/BACKLOG.md` | Both | Known problems, deliberately not fixed yet, with reasons |
| `docs/HOOKS.md` | Both | Which rules are worth enforcing mechanically |
| `.claude/settings.json` | Claude | Permission rules. Live from the first session |
| `docs/adr/` | Both | Why things were decided the way they were |

If you only ever read one of these, read `docs/WORKING_WITH_THE_AGENT.md`.

## Two blanks to fill in on day one

- **`CLAUDE.md` → "Check command"** — the one command that runs every check.
  It is empty on purpose: a new project has nothing to check yet, and a check
  that always passes teaches you to trust a green light that means nothing.
  It gets filled in at the first bug fix.
- **`.gitignore`** — it carries optional blocks for common stacks. Keep the one
  that matches this project, delete the rest.

## What this template is not

It is not a code scaffold. There is no build system, no dependency file, no
example app — those depend on what you are building, and guessing wrong costs
more than starting empty.

What it does carry is the small set of habits that, in a real project, actually
caught mistakes: prove a check can fail before trusting it, record measured
values rather than verdicts, and keep a written record of what has already gone
wrong so the same surprise does not arrive twice.
