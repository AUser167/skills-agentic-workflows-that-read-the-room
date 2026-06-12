---
name: update-github-info
description: Draft website updates for Mona's GitHub Info site from official GitHub sources.
on:
  workflow_dispatch: {}
  schedule: daily
safe-outputs:
  create-pull-request:
    title-prefix: "[mona] "
    draft: true
    fallback-as-issue: false
tools:
  edit: {}
  web-fetch: {}
network:
  allowed:
    - github.com
    - github.blog
---

# Update Mona's GitHub Info website

Read `notes/mona-notes.md` before making changes.

Use these sources:
- `notes/mona-notes.md`
- GitHub Blog: https://github.blog/latest/  # GitHub Blog
- GitHub Changelog: https://github.blog/changelog/  # GitHub Changelog

Update `site/content/github-info.md` with concise, practical updates for readers. When content is taken
from the GitHub Blog or GitHub Changelog, include a one-line source attribution.

Open a pull request for Mona to review. Use a pull request title that mentions Mona or GitHub Info.
Do not write directly to `main`; rely on `safe-outputs` with `create-pull-request` so the agent drafts
changes and a controlled job opens the PR for Mona to review.

Behavior and steps for the agent:

1. Read `notes/mona-notes.md` to learn Mona's editorial preferences and any notes about the site.
2. Web-fetch the GitHub Blog latest feed at `https://github.blog/latest/` and the GitHub Changelog at
   `https://github.blog/changelog/` to find new items relevant to Mona's site.
3. Summarize any relevant updates and add them under the `## Latest GitHub Updates` section in
   `site/content/github-info.md`. If the section does not exist, create it near the top of the content file.
4. Prepare a branch and patch with the content updates and return the patch as the `create-pull-request` safe output.
5. Provide a short PR body describing source links and why the updates are relevant to Mona.

Notes:
- This workflow runs daily and on-demand via `workflow_dispatch`.
- The agent is allowed `edit` and `web-fetch` tools and network access to `github.blog` and `github.com`.
- The agent must not compile or write directly to `main`.
