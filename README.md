# AI Changelog (GitHub Models)

Generate a human-friendly changelog from git history using GitHub Models — **no external API keys**.

This action:
- Collects git commits for a given range
- Sends them to a GitHub-hosted LLM (`openai/gpt-4o-mini` by default)
- Produces a grouped, human-readable changelog in Markdown

## Usage

Create a workflow file (e.g. `.github/workflows/changelog.yml`):

```yaml
name: AI Changelog

on:
  workflow_dispatch:
  push:
    tags:
      - 'v*'

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
      
      - name: Generate Changelog
        uses: decoded-cipher/auto-changelog@v1.0.0
        with:
          output_path: CHANGELOG.md
```

## Inputs

- `range` (optional): git revision range, e.g. `v1.0.0..HEAD`.  
  If empty, uses `last-tag..HEAD`, or `HEAD` if there are no tags.
- `output_path` (optional): file path to write markdown to (default: `CHANGELOG_AI.md`).
- `model` (optional): GitHub Models ID (default: `openai/gpt-4o-mini`).

## Outputs

- `changelog`: The generated Markdown changelog text.

## Permissions

Your workflow **must** enable models access:

```yaml
permissions:
  contents: write
  models: read
```