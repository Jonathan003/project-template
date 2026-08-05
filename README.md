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
| `docs/USER_CLAUDE_MD.md` | **You** | A copy of your machine-wide rules file, which no repository carries |
| `STANDARDS.md` | Claude | How to write a commit and an ADR |
| `docs/PITFALLS.md` | Claude | Failure modes that have already bitten *this* project |
| `docs/BACKLOG.md` | Both | Known problems, deliberately not fixed yet, with reasons. Here from day one on purpose: an empty backlog invites filling, so the rules against that are written into the file itself |
| `docs/HOOKS.md` | Both | Which rules are worth enforcing mechanically |
| `.claude/settings.json` | Claude | Permission rules. Live from the first session |
| `docs/adr/` | Both | Why things were decided the way they were |

If you only ever read one of these, read `docs/WORKING_WITH_THE_AGENT.md`.

## Three things on day one

- **Accept the trust dialog** the first time you open a session in the new
  project — and again on every machine, and in every folder, you ever open it
  in. The answer is stored on the machine, keyed by folder path; it is not in
  the repository and pushing does not carry it. A project made from this
  template ships `.claude/settings.json`, and until the workspace is trusted,
  nothing under `.claude/` is loaded at all — the permission rules included,
  with no error to tell you. Confirm it took by running `/permissions` and
  looking at the **Ask** tab: the two `git push` rules are listed there, or the
  directory was not read. See `docs/PITFALLS.md` entry 1.
- **`CLAUDE.md` → "Check command"** — the one command that runs every check.
  It is empty on purpose: a new project has nothing to *pin* yet, and a check
  that always passes teaches you to trust a green light that means nothing.
  Whatever suits the language — an `npm` script, a task-runner target, a shell
  script. Fill it in at whichever of these comes first:
  - **the first bug fix** — a bug that happened once can happen again, and that
    is the first moment there is behaviour worth pinning;
  - **before the program is first used for real** — a release, a deploy, or
    handing it to someone else. A project without bugs still needs this.

  It gets one more job at its own trigger — **the second file to gain a check**,
  because one file cannot disagree with itself. From then on the command must
  **fail when the list and the repo disagree**, in either direction: a check
  that exists but is not in the list, or a name in the list with nothing behind
  it. Both are silent otherwise. The first one is what the project this template
  came from actually hit — one script's test suite was never added to the list,
  and it sat red, unseen, across a whole span of commits while the command it
  was missing from reported green.
- **`.gitignore`** — it carries optional blocks for common stacks. Keep the one
  that matches this project, delete the rest.

## On a new machine

Two of the things a session depends on are stored on the machine rather than in
git, so a second PC starts without them **even when everything has been pushed**
— and neither announces its absence.

- **The trust dialog answer.** A fresh clone, or the same project opened from a
  different folder, asks again, and until it is accepted the shipped `git push`
  rules are not live. Day one repeats itself on every machine: same dialog, same
  `/permissions` check, same `docs/PITFALLS.md` entry 1.
- **Your user-level `CLAUDE.md`** — the machine-wide rules that say how you work
  at all. It lives in your home directory, outside every repository, which means
  no repository can restore it. `docs/USER_CLAUDE_MD.md` keeps a copy and says
  where the real file goes.

## What this template is not

It is not a code scaffold. There is no build system, no dependency file, no
example app — those depend on what you are building, and guessing wrong costs
more than starting empty.

What it does carry is the small set of habits that, in a real project, actually
caught mistakes: prove a check can fail before trusting it, record measured
values rather than verdicts, and keep a written record of what has already gone
wrong so the same surprise does not arrive twice.
