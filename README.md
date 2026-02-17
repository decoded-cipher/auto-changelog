# AI Changelog Generator (GitHub Models)

**AI-powered changelog generator for GitHub (via GitHub Models, no external API key required)**

`auto-changelog` is a GitHub Action that automatically builds a clean, human-friendly — or optionally machine-friendly — changelog from your commit history. It uses GitHub's built-in Models to analyze commit history, group related changes, and output a structured Markdown changelog — no need for external APIs, tokens, or billing.

## ✨ Why it exists

- Commit messages are often terse, confusing or noisy; raw `git log` makes poor release notes.
- Writing a changelog manually is tedious, error-prone, and often neglected.
- With `auto-changelog`, you get meaningful, organized changelogs — consistently — without extra work.

## 🛠 What it does

- Scans your git history (from last tag or custom range).
- Uses a GitHub-hosted LLM (`openai/gpt-4o-mini` by default) to understand changes, categorize them (Features, Fixes, Improvements, Docs, etc.), and create a timeline.
- Generates a clean Markdown file (e.g. `CHANGELOG.md`) with a clear timeline of meaningful changes in a structured table.
- Formats the output in a parseable table — perfect for rendering changelogs automatically in a webpage or docs site.
- **Optional PR mode:** use a dedicated branch, pull latest from your base branch, generate changelog, commit and push to that branch, and create or update a PR to the base branch — so you never push directly to main/master.

## 🚀 How to use

### Basic: generate changelog in the current branch

Add this to your workflow (e.g. in `.github/workflows/changelog.yml`):

```yaml
name: AI Changelog

on:
  push:
    tags: [ 'v*' ]
  workflow_dispatch:

permissions:
  contents: write
  models: read

jobs:
  changelog:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0
          fetch-tags: true

      - name: Generate changelog
        uses: decoded-cipher/auto-changelog@v1
        with:
          output_path: CHANGELOG.md
```

On push or manual run, the action will produce (or update) `CHANGELOG.md`. You can then commit it back in the same workflow if you want.

### PR mode: changelog on a branch + PR to base (recommended for main/master)

To avoid pushing directly to your main branch, use **PR mode**. The action will:

1. Check out a dedicated changelog branch (or create it from the base).
2. Pull the latest changes from the base branch (e.g. `master`) into it.
3. Generate the changelog.
4. Commit and push only to the changelog branch.
5. Create a PR from the changelog branch to the base branch (or update the existing PR if one is open).

Example: run on every push to `master`, update branch `auto/changelog`, and open/update a PR to `master`:

```yaml
name: AI Changelog

on:
  push:
    branches: [master]
  workflow_dispatch:

permissions:
  contents: write
  models: read
  pull-requests: write

jobs:
  changelog:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0

      - name: Generate changelog and open PR
        uses: decoded-cipher/auto-changelog@v1
        with:
          output_path: CHANGELOG.md
          create_pr: true
          changelog_branch: auto/changelog
          base_branch: master
```

When using `create_pr: true`, you must grant `pull-requests: write` (and `contents: write`) in the workflow.

### Inputs

| Input | Default | Description |
|-------|---------|-------------|
| `range` | (auto) | Git revision range (e.g. `v1.0.0..HEAD`). If empty, uses last tag..HEAD, or HEAD if no tags. |
| `output_path` | `CHANGELOG.md` | File path to write the changelog markdown to. |
| `model` | `openai/gpt-4o-mini` | GitHub Models ID. |
| `create_pr` | `false` | If `true`, use a dedicated branch, commit, push, and create/update a PR to the base branch. |
| `changelog_branch` | `auto/changelog` | Branch used for changelog commits when `create_pr` is true. |
| `base_branch` | `master` | Base branch to merge into the changelog branch and to open the PR into. |

### Outputs

| Output | Description |
|--------|-------------|
| `changelog` | The generated changelog markdown. |

## ✅ Key benefits

- **Zero configuration**: no API keys, no secrets, no third-party dependencies — just your existing GitHub environment.
- **Readable & maintainable logs**: changes are merged logically, not just dumped commit-by-commit.
- **Structured output**: machine-parseable tables — ideal for static sites, release pages or documentation.
- **Automatic**: runs on push/tag — keeps your changelog up-to-date without manual work.
- **Safe for main**: with PR mode, changelog updates go through a branch and a PR instead of pushing directly to your default branch.
