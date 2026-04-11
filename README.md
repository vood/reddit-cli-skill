# ThreadPilot Skill

`threadpilot` is a bundled browser-first community CLI skill for OpenClaw.

Created by [Aleksei Vysotskii](https://linkedin.com/in/avysotski), founder of [writingmate.ai](https://writingmate.ai), [mentioned.to](https://mentioned.to), and [aidictation.com](https://aidictation.com).

## What You Get

- Prebuilt binaries in `bin/` (darwin/linux, amd64/arm64)
- Wrapper launcher scripts
- Human-in-the-loop safety for likes and posting
- Rules pull command for AI drafting context
- Cron template for scheduled execution

## Quick Start

```bash
# Check session
scripts/threadpilot whoami

# Pull subreddit rules for authoring context
scripts/threadpilot rules --subreddit openclaw

# Read/search
scripts/threadpilot read --subreddit openclaw --sort new --limit 10
scripts/threadpilot search --query "ai workflow" --subreddit ChatGPT --limit 10
```

## Human-In-The-Loop Actions

Like preview:

```bash
REDDIT_PERMALINK='<url>' REDDIT_DRY_RUN=1 scripts/threadpilot like-target
```

Like confirm:

```bash
REDDIT_PERMALINK='<url>' REDDIT_CONFIRM_LIKE=1 scripts/threadpilot like-target
```

Comment dry run:

```bash
REDDIT_THING_ID=t3_xxxxx REDDIT_PERMALINK='<url>' REDDIT_TEXT='draft text' REDDIT_DRY_RUN=1 scripts/threadpilot post-comment
```

Comment publish:

```bash
REDDIT_THING_ID=t3_xxxxx REDDIT_PERMALINK='<url>' REDDIT_TEXT='approved text' scripts/threadpilot post-comment
```

## Environment Variables

- `THREADPILOT_BIN` (preferred explicit binary path)
- `REDDITCLI_BIN` (legacy binary path override)
- `REDDIT_BROWSER_PROFILE`
- `REDDIT_HEADLESS`
- `REDDIT_LOGIN_USERNAME`
- `REDDIT_LOGIN_PASSWORD`
- `REDDIT_ACCESS_TOKEN`
- `REDDIT_PROXY`
- `REDDIT_CHROME_PATH`

## Compatibility

Primary command is:
- `scripts/threadpilot`

Legacy alias remains available:
- `scripts/reddit-cli`
