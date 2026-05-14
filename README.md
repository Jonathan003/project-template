# Project Template

Personal template for new software projects. Provides a starting structure with conventions for commit messages and design-decision documentation.

## How to use this template

When starting a new project on GitHub:

1. Click the green **"Use this template"** button at the top of this page → **"Create a new repository"**
2. Name your new repository
3. Clone it locally and start coding

The new repository will have all the structure below, ready to go.

## What's included

- **`STANDARDS.md`** — commit message format and design-decision practices
- **`docs/adr/`** — Architecture Decision Records directory, with template and index
- **`.gitignore`** — generic ignore patterns for Python and Node.js projects

## Quick reference

### Commits

Use Conventional Commits format:

```
type: short description
```

Common types: `feat`, `fix`, `refactor`, `docs`, `chore`. Full list and examples in `STANDARDS.md`.

### Design decisions

For significant decisions (architectural choices, contested approaches, anything future-you might wonder about), write an ADR:

1. Copy `docs/adr/template.md` to `docs/adr/NNNN-title.md`
2. Fill in the sections
3. Add an entry to the index in `docs/adr/README.md`

See `STANDARDS.md` for full guidance.

## Customizing this template for a new project

After creating a new repo from this template, you'll probably want to:

1. **Replace this README** with your project's actual README (installation, usage, etc.)
2. **Keep `STANDARDS.md`** — or modify it if this project needs different conventions
3. **Start writing ADRs** in `docs/adr/` as design decisions come up

## Real-world examples

For concrete examples of completed ADRs, see the [BibleBookFinder ADR directory](https://github.com/Jonathan003/BibleBookFinder/tree/main/docs/adr).
