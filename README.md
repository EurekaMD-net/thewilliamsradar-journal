# thewilliamsradar.com — editorial source of truth

Weekly Williams Radar journal posts published at [thewilliamsradar.com](https://thewilliamsradar.com).

## Layout

```
pages/        one markdown file per weekly edition
uploads/      images and other media referenced by posts
```

Each post is a markdown file with YAML frontmatter:

```markdown
---
title: "The Williams Radar — Weekly Journal WXX · YYYY"
slug: "wXX-YYYY"
draft: false
---

# Body...
```

The filename (without `.md`) is the URL slug. `draft: false` publishes; `draft: true` keeps the post hidden.

## Publish workflow

```
git add pages/wXX-YYYY.md
git commit -m "content: WXX·YYYY <one-line summary>"
git push origin main
```

The CMS engine on the VPS reads from the local clone of this repo. After the push, the operator (or an automated pull on the VPS) refreshes the working tree:

```
ssh vps 'cd /root/claude/thewilliamsradar-journal && git pull'
```

Then the CMS picks up the new file on the next request — no engine restart required for content changes.

## Architecture

This repo is **content only**. The CMS engine that renders these posts lives in a separate repo: [`EurekaMD-net/very-light-cms`](https://github.com/EurekaMD-net/very-light-cms). The signal-generation methodology (the Williams Entry Radar) lives in a third repo: `EurekaMD-net/williams-entry-radar`.

Three repos, three concerns, no mixing:

| Repo                       | Concern                                                     |
| -------------------------- | ----------------------------------------------------------- |
| `very-light-cms`           | CMS engine — reusable, deploys any content repo             |
| `williams-entry-radar`     | Signal methodology — scans tickers, produces weekly signals |
| `thewilliamsradar-journal` | Editorial content — interprets signals, publishes journal   |

## Conventions

- One post per ISO week. Filename: `w<WW>-<YYYY>.md` (e.g. `w17-2026.md`).
- Follow-up section: every edition starts by reporting on the prior week's signals (no edits, no omissions).
- Disclaimer at the bottom of every post: educational information, not financial advice.
