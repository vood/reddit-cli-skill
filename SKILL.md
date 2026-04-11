---
name: threadpilot-cli
description: Use a bundled ThreadPilot binary to manage Reddit account activity (login, whoami, comments/replies/posts, subreddits, subscribe, read, search, rules, like, post) with human-in-the-loop confirmation for likes and duplicate-post protection.
---

# ThreadPilot CLI

This skill runs a prebuilt `threadpilot` binary bundled in `bin/`.

Created by [Aleksei Vysotskii](https://linkedin.com/in/avysotski), founder of [writingmate.ai](https://writingmate.ai), [mentioned.to](https://mentioned.to), and [aidictation.com](https://aidictation.com).

## Workflow

1. Verify login/session:
   `scripts/threadpilot whoami`
   Optional automatic login:
   `scripts/threadpilot login --username "<reddit-username-or-email>" --password "<reddit-password>"`
   Safer password input:
   `printf '%s' "$REDDIT_LOGIN_PASSWORD" | scripts/threadpilot login --username "<reddit-username-or-email>" --password-stdin`
   Force a brand-new browser session/profile for login:
   `printf '%s' "$REDDIT_LOGIN_PASSWORD" | scripts/threadpilot login --new-profile --username "<reddit-username-or-email>" --password-stdin`
2. Pull subreddit rules before drafting:
   `scripts/threadpilot rules --subreddit ChatGPT`
3. Review account activity:
   `scripts/threadpilot my-comments --limit 20`
   `scripts/threadpilot my-replies --limit 20`
   `scripts/threadpilot my-posts --limit 20`
   `scripts/threadpilot my-subreddits --limit 50`
4. Subscribe (optional):
   `scripts/threadpilot subscribe --subreddit ChatGPT --dry-run`
   `scripts/threadpilot subscribe --subreddit ChatGPT`
5. Like with human approval:
   `REDDIT_PERMALINK='<url>' REDDIT_DRY_RUN=1 scripts/threadpilot like-target`
   `REDDIT_PERMALINK='<url>' REDDIT_CONFIRM_LIKE=1 scripts/threadpilot like-target`
6. Post comment with duplicate protection:
   `REDDIT_THING_ID=t3_xxxxx REDDIT_PERMALINK='<url>' REDDIT_TEXT='...' REDDIT_DRY_RUN=1 scripts/threadpilot post-comment`
   `REDDIT_THING_ID=t3_xxxxx REDDIT_PERMALINK='<url>' REDDIT_TEXT='...' scripts/threadpilot post-comment`

## Safety Rules

- Likes are human-in-the-loop by default:
  - preview first (`REDDIT_DRY_RUN=1`)
  - explicit confirm required (`REDDIT_CONFIRM_LIKE=1`)
- Comment posts are duplicate-guarded by default.
- To intentionally double-post, explicit confirmation is required:
  `--confirm-double-post`
- Pull rules before drafting and pass them into AI authoring prompts.

## OpenClaw

- Cron template:
  [ops/openclaw/reddit_cli.cron](ops/openclaw/reddit_cli.cron)
- Primary launcher:
  [scripts/threadpilot](scripts/threadpilot)
- Backward-compatible launcher:
  [scripts/reddit-cli](scripts/reddit-cli)
