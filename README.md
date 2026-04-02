> Update: new self-contained skill without the need for installing CLI is here: https://github.com/ropl-btc/agent-skills

# twitterapi-io-cli

Read-only CLI for `twitterapi.io`, built for **OpenClaw**, **Claude Code**, and other AI agents that need a simple, reusable interface for reading Twitter/X data.

This repo gives you two things:
- a small installable CLI: `twitterapi-io`
- a matching OpenClaw skill in `skills/twitterapi-io`

## What this is

- a small **read-only CLI** for `twitterapi.io`
- a matching **OpenClaw skill** under `skills/twitterapi-io`
- opinionated commands for common read workflows
- designed to be easy for humans and AI agents to use repeatedly

## What this is not

- not a posting client
- not a like/reply/delete client
- not a full wrapper for every twitterapi.io endpoint

## Features

- save `TWITTERAPI_IO_KEY` locally with `auth`
- fetch one tweet by id or URL
- fetch one user by username
- fetch recent tweets for a user
- fetch replies to a tweet
- fetch quote tweets for a tweet
- fetch thread context for a tweet
- fetch mentions for a username
- run advanced search with pagination and helper time flags
- return compact JSON by default, with `--raw` when needed

## Commands

- `help`
- `auth`
- `tweet`
- `user`
- `user-tweets`
- `replies`
- `quotes`
- `thread-context`
- `mentions`
- `search`

## Install

### Preferred: pipx

```bash
pipx install git+https://github.com/ropl-btc/twitterapi-io-cli.git
```

Then run:

```bash
twitterapi-io help
```

### Fallback: local venv

```bash
git clone https://github.com/ropl-btc/twitterapi-io-cli.git
cd twitterapi-io-cli
python3 -m venv .venv
. .venv/bin/activate
pip install --upgrade pip
pip install .
```

## Get your API key

At `https://twitterapi.io/dashboard`:
- log in
- copy your API key

Per the official docs, all requests authenticate with the `x-api-key` header.

## Authenticate once

Save the API key locally:

```bash
twitterapi-io auth --api-key YOUR_KEY
```

This writes to:

```text
~/.config/twitterapi-io/config.json
```

You can also use an env var instead:

```bash
export TWITTERAPI_IO_KEY='YOUR_KEY'
```

## Usage examples

### Show help

```bash
twitterapi-io help
```

### Fetch one tweet

```bash
twitterapi-io tweet --url 'https://x.com/jack/status/20'
```

or:

```bash
twitterapi-io tweet --id 20
```

### Fetch one user

```bash
twitterapi-io user --username OpenAI
```

### Fetch recent tweets for a user

```bash
twitterapi-io user-tweets --username OpenAI --limit 10
```

Include replies:

```bash
twitterapi-io user-tweets --username OpenAI --limit 10 --include-replies
```

### Fetch replies

```bash
twitterapi-io replies --url 'https://x.com/jack/status/20' --limit 20
```

### Fetch quote tweets

```bash
twitterapi-io quotes --id 20 --limit 20
```

### Fetch thread context

```bash
twitterapi-io thread-context --id 20 --limit 40
```

### Fetch mentions

```bash
twitterapi-io mentions --username OpenAI --limit 20
```

### Search tweets

```bash
twitterapi-io search --query 'AI agents -filter:replies' --from-user OpenAI --within-time 24h --max-tweets 50
```

Use `Top` ranking:

```bash
twitterapi-io search --query 'AI agents' --queryType Top --max-pages 2
```

Use explicit unix-time filters:

```bash
twitterapi-io search --query '$BTC' --since-time 1741219200 --until-time 1741305600 --max-tweets 50
```

### Return raw API objects

```bash
twitterapi-io tweet --id 20 --raw
```

## OpenClaw / AI-agent usage

This repo includes an OpenClaw skill under:

- `skills/twitterapi-io/SKILL.md`
- `skills/twitterapi-io/scripts/twitterapi_io.py`
- `skills/twitterapi-io/references/links.md`

That subfolder is the one to use for skill-specific packaging and publishing.

The main intended interface for end users and agents is still the installed `twitterapi-io` command.

## Docs basis

This CLI is based on the official twitterapi.io docs, especially:
- `GET /twitter/tweets`
- `GET /twitter/user/info`
- `GET /twitter/user/last_tweets`
- `GET /twitter/tweet/replies`
- `GET /twitter/tweet/quotes`
- `GET /twitter/tweet/thread_context`
- `GET /twitter/user/mentions`
- `GET /twitter/tweet/advanced_search`
- authentication via `x-api-key`

## License

MIT
