# qadar-ai/.github

Central org-level GitHub configuration for reusable workflows and shared defaults.

## Claude PR Review Reusable Workflow

This repository provides `.github/workflows/claude-review.yml` as a reusable workflow.

Use it from any repo with:

```yaml
jobs:
  review:
    uses: qadar-ai/.github/.github/workflows/claude-review.yml@main
    secrets: inherit
```

Requirements:
- `ANTHROPIC_API_KEY` must be available to the calling repo.
- Caller workflow should run on PR events and grant `pull-requests: write` and `issues: write`.
