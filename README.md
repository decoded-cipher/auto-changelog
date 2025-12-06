# AI Changelog Generator (GitHub Models)

**AI-powered changelog generator for GitHub (via GitHub Models, no external API key required)**

`auto-changelog` is a GitHub Action that automatically builds a clean, human-friendly — or optionally machine-friendly — changelog from your commit history. It uses GitHub’s built-in Models to analyze commit history, group related changes, and output a structured Markdown changelog — no need for external APIs, tokens, or billing.

## ✨ Why it exists

- Commit messages are often terse, confusing or noisy; raw `git log` makes poor release notes.
- Writing a changelog manually is tedious, error-prone, and often neglected.
- With `auto-changelog`, you get meaningful, organized changelogs — consistently — without extra work.

## 🛠 What it does

- Scans your git history (from last tag or custom range).
- Uses a GitHub-hosted LLM (`openai/gpt-4o-mini` by default) to understand changes, categorize them (Features, Fixes, Improvements, Docs, etc.), and create a timeline.
- Generates a clean Markdown file (e.g. `CHANGELOG.md`) with a clear timeline of meaningful changes in a structured table.
- Formats the output in a parseable table — perfect for rendering changelogs automatically in a webpage or docs site.

## 🚀 How to use

1. Add this to your workflow (e.g. in `.github/workflows/changelog.yml`):

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

         - name: Generate chagelog
           uses: <your-username>/auto-changelog@v1
           with:
             output_path: CHANGELOG.md  # or your custom file
   ```

2. On push or manual run, the action will produce (or update) `CHANGELOG.md`.

3. You can then commit it back to the repo (optionally via the same workflow) so it stays versioned.

## ✅ Key benefits

- **Zero configuration**: no API keys, no secrets, no third-party dependencies — just your existing GitHub environment.
- **Readable & maintainable logs**: changes are merged logically, not just dumped commit-by-commit.
- **Structured output**: machine-parseable tables — ideal for static sites, release pages or documentation.
- **Automatic**: runs on push/tag — keeps your changelog up-to-date without manual work.