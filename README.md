# AI Changelog (GitHub Models)

Generate a human-friendly changelog from git history using GitHub Models — **no external API keys**.

This action:
- Collects git commits for a given range
- Sends them to a GitHub-hosted LLM (`openai/gpt-4o-mini` by default)
- Produces a grouped, human-readable changelog in Markdown

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