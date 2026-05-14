# Project Standards

Conventions used in this project for commit messages and design decisions.

## Commit messages

Use Conventional Commits format:

```
type: short imperative description
```

### Types

- **`feat:`** new feature for users
- **`fix:`** bug fix
- **`refactor:`** code restructure without behavior change
- **`docs:`** documentation only
- **`chore:`** maintenance, dependencies, build config
- **`style:`** formatting, whitespace (no logic change)
- **`perf:`** performance optimization
- **`test:`** adding or fixing tests

### Format rules

- First line: max ~70 characters
- Imperative mood ("add" not "added", "fix" not "fixed")
- Lowercase after the colon, no period at end
- For breaking changes, add `!` before the colon: `feat!: change API shape`

### Examples

```
feat: add user authentication
fix: prevent crash when input is empty
refactor: extract validation logic into helper
docs: update README installation steps
chore: bump dependencies
```

For longer commits, add a blank line and a body:

```
fix: prevent crash when input is empty

The InputField component was calling .trim() on a possibly-null
value. Added defensive null check before processing.
```

## Design decisions: ADRs

For significant design decisions, write an Architecture Decision Record (ADR) in `docs/adr/`.

### When to write one

Write an ADR when a decision is:

- **Significant** — affects architecture, user interaction, or major code structure
- **Debatable** — multiple reasonable approaches exist
- **Likely to be questioned later** — by future-you or by someone else reading the code

Skip ADRs for routine bug fixes, cleanup, or obvious choices. Use commit messages for those.

Realistic frequency: 1-2 ADRs per month for an actively maintained project. Not every commit, not every refactor.

### How to write one

1. Copy `docs/adr/template.md` to `docs/adr/NNNN-title.md` (NNNN = next sequential number, zero-padded to 4 digits)
2. Fill in: Status, Context, Decision, Alternatives Considered, Consequences, Review Trigger
3. Add an entry to the index in `docs/adr/README.md`
4. Commit with a `docs:` prefix: `docs: add ADR NNNN for [topic]`

### Status meanings

- **Proposed** — under discussion, not yet decided
- **Accepted** — currently in effect
- **Deprecated** — no longer relevant but not replaced
- **Superseded by ADR-NNNN** — replaced by a newer ADR

ADRs are **never edited** after Acceptance. If a decision changes, write a new ADR that supersedes the old one (link both ways).

### Real-world examples

See [BibleBookFinder's ADRs](https://github.com/Jonathan003/BibleBookFinder/tree/main/docs/adr) for concrete examples of completed ADRs.
