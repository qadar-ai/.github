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

For Claude action authentication, ensure caller workflows include:

```yaml
permissions:
  contents: read
  pull-requests: write
  issues: write
  id-token: write
```

## Anthropic Credit Guardrails (QAD-796)

This section covers the org-level billing safeguards for the `ANTHROPIC_API_KEY` used by the reusable Claude PR review workflow.

### Owners and Recipients

- Primary owner: DevOps on-call
- Escalation owner: CTO
- Alert recipients: DevOps alias + CTO alias configured in Anthropic Console budget alerts

### Budget Alert Thresholds

Configure monthly usage budget alerts in Anthropic Console at:

- 50% of monthly budget (early warning)
- 80% of monthly budget (action required)
- 95% of monthly budget (critical, immediate top-up required)

### Top-Up Policy

- Preferred: enable auto top-up on the org billing account backing `ANTHROPIC_API_KEY`.
- If auto top-up is not permitted by billing policy: maintain a recurring manual top-up reminder owned by DevOps on-call and escalated to CTO when usage crosses the 80% threshold.

### Incident Check Path

If Claude PR reviews stop or return low-credit errors:

1. Open Anthropic Console billing usage for the org that owns `ANTHROPIC_API_KEY`.
2. Confirm current spend vs monthly budget thresholds above.
3. Confirm alert recipients are still DevOps alias + CTO alias.
4. Execute top-up action per policy above and re-run a failed PR review.
