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
    with:
      requirements: docs/requirements.md   # optional
      trust_boundary: internal/p2p, internal/store   # optional
```

The calling repository needs the `ai-review` label and the
`CLAUDE_CODE_OAUTH_TOKEN` secret (`claude setup-token`).

This repository is public: a public repository can call a reusable workflow
only from a public one.

`.claude/` holds the skills the review runs; the workflow installs them on the
runner. The upstream ones are pinned copies, so a review never runs code that
changed upstream since it was last copied here.

| Skill | Source |
|---|---|
| `rev-ci` | ours |
| `ponytail-review` | ponytail 4.10.0 |
| `receiving-code-review` | obra/superpowers v6.4.2 |
