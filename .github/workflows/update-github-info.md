---
name: update-github-info
on:
  schedule:
    - cron: "0 9 * * *"
  workflow_dispatch:
permissions:
  contents: read
model: auto
engine:
  id: copilot
  args: ["--allow-all-urls"]
tools:
  bash: ["cat", "curl"]
  edit:
network:
  allowed:
    - github.blog
    - github.com
    - awesome-copilot.github.com
safe-outputs:
  create-pull-request:
    title-prefix: "[GitHub Info] "
    base-branch: main
    draft: false
    allowed-files:
      - site/content/github-info.md
---

Keep the GitHub Info content current with useful, verified updates for Mona to review.

1. Read `notes/mona-notes.md` and `site/content/github-info.md`.
2. Use `curl` to fetch https://github.blog/latest/, https://github.blog/changelog/, and https://awesome-copilot.github.com/workflows/. Follow links on those pages and fetch the source when needed to verify details. Network egress is limited to the configured allowlist.
3. Select only recent, materially useful updates that fit Mona's practical editorial angle. Keep summaries concise, preserve existing useful content, and cite each item with its title, publication date when available, and a direct source link.
4. Update `site/content/github-info.md` with a concise `Recent official updates` section containing up to three verified items. Do not add speculative claims or duplicate existing items.
5. If there are no suitable new items or the file would not meaningfully improve, leave it unchanged and use the `noop` safe output with a brief reason.
6. When making a meaningful change, open one pull request for Mona to review using the `create-pull-request` safe output. Summarize the practical updates and link their sources in the pull request description.

Do not write directly to `main`, create commits or push branches yourself, or use any write path other than the `create-pull-request` safe output.
