# OpenMind-SI shared workflows

## Claude review

`.github/workflows/claude-review.yml` — reusable workflow that has Claude review a PR.

To enable in a repo, add `.github/workflows/claude-review.yml`:

```yaml
name: Claude Review
on:
  pull_request:
    types: [opened, ready_for_review, synchronize, review_requested]
concurrency:
  group: claude-review-${{ github.event.pull_request.number }}
  cancel-in-progress: true
jobs:
  review:
    if: >-
      github.event.pull_request.head.repo.full_name == github.repository &&
      (github.event.action != 'review_requested' ||
       github.event.requested_team.slug == 'claude-review')
    uses: OpenMind-SI/.github/.github/workflows/claude-review.yml@main
    secrets: inherit
    permissions:
      contents: read
      pull-requests: write
      issues: write
      id-token: write
```

and give the `claude-review` team read access to the repo so it appears in the Reviewers list.
Requires the org secret `CLAUDE_CODE_OAUTH_TOKEN` and the Claude GitHub App installed on the repo.
