# monetarium-ai-review

The shared Claude review of pull requests in the Monetarium repositories.

## claude-review

A Claude review of a PR, run when the `ai-review` label is added. The review is
posted as a comment review, never as approve or request-changes.

Each repository calls it from its own `.github/workflows/claude-review.yml`:

```yaml
name: claude-review

on:
  pull_request:
    types: [labeled]

jobs:
  review:
    if: github.event.label.name == 'ai-review'
    uses: monetarium/monetarium-ai-review/.github/workflows/claude-review.yml@main
    permissions:
      contents: read
      pull-requests: write
    secrets:
      CLAUDE_CODE_OAUTH_TOKEN: ${{ secrets.CLAUDE_CODE_OAUTH_TOKEN }}
      AI_REVIEW_DEPLOY_KEY: ${{ secrets.AI_REVIEW_DEPLOY_KEY }}
    with:
      requirements: docs/requirements.md   # optional
      trust_boundary: internal/p2p, internal/store   # optional
```

The calling repository needs the `ai-review` label and two secrets:
`CLAUDE_CODE_OAUTH_TOKEN` (`claude setup-token`) and `AI_REVIEW_DEPLOY_KEY`, the
private half of this repository's read-only deploy key.

This repository is private, so it is shared in Settings → Actions → General →
Access → "Accessible from repositories in the 'monetarium' organization".

`.claude/` holds the skills the review runs; the workflow installs them on the
runner. The upstream ones are pinned copies, so a review never runs code that
changed upstream since it was last copied here.

| Skill | Source |
|---|---|
| `rev-ci` | ours |
| `ponytail-review` | ponytail 4.10.0 |
| `receiving-code-review` | obra/superpowers v6.4.2 |
