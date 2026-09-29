# TitanBot — Base44 Dev Environment

## What this is
TitanBot is a Discord bot (discord.js v14) with an Express web server on port 3000, PostgreSQL for data persistence, and Lavalink v4 for music playback.

## Boot requirements
- **DISCORD_TOKEN** and **CLIENT_ID** are required — the bot calls `client.login(token)` and will `process.exit(1)` if the token is invalid. These are external credentials provided via the Base44 secrets dashboard.
- **GUILD_ID** is optional (single-server mode). Leave unset for multi-guild.
- PostgreSQL runs as a compose service (`db`). The bot auto-creates tables and bootstraps the schema version on first connect — no separate migration step needed.
- Lavalink runs as a compose service (`lavalink`) for music features.

## How to verify it's running
```bash
curl http://localhost:3000/health   # returns {"status":"healthy",...}
curl http://localhost:3000/         # returns {"message":"TitanBot System Online",...}
curl http://localhost:3000/ready    # returns bot readiness + metrics
```

## Live reload
The bot runs with `node --watch src/app.js` which restarts on file changes.

## Key files
- `src/app.js` — main entry point (TitanBot class, web server, startup sequence)
- `src/config/application.js` — config assembly from env vars
- `src/config/bot.js` — bot config + `validateConfig()` (only exits in production mode)
- `src/utils/database/wrapper.js` — DB facade (PostgreSQL → in-memory fallback)
- `src/utils/postgresDatabase.js` — PostgreSQL driver, auto-creates tables on connect
- `docker-compose.base44.yml` — Base44 dev compose (bot + db + lavalink)
- `.env.base44-defaults` — non-secret dev defaults (overridden by secrets)
