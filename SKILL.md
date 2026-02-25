---
name: reddit-cli
description: Use a bundled redditcli binary to manage Reddit account activity (whoami, my-comments, my-replies, my-posts, my-subreddits, subscribe, like, post) with human-in-the-loop confirmation for likes and duplicate-post protection for comments.
---

# Reddit CLI

This skill runs a prebuilt `redditcli` binary bundled in `bin/`.

## Workflow

1. Verify login/session:
   `scripts/reddit-cli whoami`
   Optional automatic login:
   `scripts/reddit-cli login --username "<reddit-username-or-email>" --password "<reddit-password>"`
   Safer password input:
   `printf '%s' "$REDDIT_LOGIN_PASSWORD" | scripts/reddit-cli login --username "<reddit-username-or-email>" --password-stdin`
   Force a brand-new browser session/profile for login:
   `printf '%s' "$REDDIT_LOGIN_PASSWORD" | scripts/reddit-cli login --new-profile --username "<reddit-username-or-email>" --password-stdin`
2. Review account activity:
   `scripts/reddit-cli my-comments --limit 20`
   `scripts/reddit-cli my-replies --limit 20`
   `scripts/reddit-cli my-posts --limit 20`
   `scripts/reddit-cli my-subreddits --limit 50`
3. Subscribe (optional):
   `scripts/reddit-cli subscribe --subreddit ChatGPT --dry-run`
   `scripts/reddit-cli subscribe --subreddit ChatGPT`
4. Like with human approval:
   `REDDIT_PERMALINK='<url>' REDDIT_DRY_RUN=1 scripts/reddit-cli like-target`
   `REDDIT_PERMALINK='<url>' REDDIT_CONFIRM_LIKE=1 scripts/reddit-cli like-target`
5. Post comment with duplicate protection:
   `REDDIT_THING_ID=t3_xxxxx REDDIT_PERMALINK='<url>' REDDIT_TEXT='...' REDDIT_DRY_RUN=1 scripts/reddit-cli post-comment`
   `REDDIT_THING_ID=t3_xxxxx REDDIT_PERMALINK='<url>' REDDIT_TEXT='...' scripts/reddit-cli post-comment`

## Safety Rules

- Likes are human-in-the-loop by default:
  - preview first (`REDDIT_DRY_RUN=1`)
  - explicit confirm required (`REDDIT_CONFIRM_LIKE=1`)
- Comment posts are duplicate-guarded by default.
- To intentionally double-post, explicit confirmation is required:
  `--confirm-double-post`

## OpenClaw

- Cron template:
  [ops/openclaw/reddit_cli.cron](ops/openclaw/reddit_cli.cron)
- Primary launcher:
  [scripts/reddit-cli](scripts/reddit-cli)
