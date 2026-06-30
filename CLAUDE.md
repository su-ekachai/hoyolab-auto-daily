# CLAUDE.md

## What this is
A single-file, zero-dependency Node script (`index.js`, ES module, native `fetch`)
that does HoYoLAB daily check-in for ZZZ/GI/HSR/HI3/ToT. It runs on a GitHub
Actions cron — there is no local server, no build, no test suite. Keep it that way
unless a feature genuinely needs more.

## Run it
- CI: `.github/workflows/login.yml` runs `node index.js` daily (06:00 UTC+8).
- Local: `COOKIE=... GAMES=... node index.js` (Node 20+, no `npm install` needed).
- `version.yml` just compares `package.json` version against upstream `sglkc/hoyolab-auto-daily`.

## Inputs (env)
- `COOKIE` (secret) — one account per line: `ltuid_v2=...; ltoken_v2=...`
- `GAMES` (var) — one space-separated line per account, e.g. `gi hsr zzz`
- `DISCORD_WEBHOOK`, `DISCORD_USER` (optional) — Discord notifications.
- `TELEGRAM_TOKEN`, `TELEGRAM_CHAT_ID` (optional) — Telegram notifications. Independent of Discord.

## Conventions
- Zero runtime dependencies. Use native `fetch`/`URL`. **Do not add npm packages**
  for anything a few lines of stdlib can do.
- Stay single-file. No `src/`, no build step, no TypeScript — `// @ts-check` + JSDoc
  if you want type hints.
- Game endpoints live in the `endpoints` map in `index.js`; add a game by adding a row.
- Secrets only via GitHub Secrets — never log the cookie, never push it to the
  `messages` array (which is sent to Discord). `debug` logs are console-only by design.

## When editing
- Pin any third-party GitHub Action to a commit SHA, not a tag.
- Keep workflow `permissions:` least-privilege.
- Non-trivial logic change → leave one runnable check (a tiny `node -e` assert is enough).

## Relevant skills
- `ponytail` — default posture for this repo; question every addition (YAGNI).
- `superpowers:systematic-debugging` — before fixing any check-in failure.
- `superpowers:verification-before-completion` — run it before claiming a fix works.
- `secrets-management` / `security-review` — when touching cookie or webhook handling.
- `update-config` — for changes to `.github/workflows/` or settings.
