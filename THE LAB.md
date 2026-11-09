# THE LAB

THE LAB is the renamed, blue control-room redesign of the original Kingmor Lua protection dashboard. The redesign changes the visible brand, navigation, typography treatment, cards, buttons, backgrounds, terminal preview, bot picker, pricing, login, dashboard, OBF tool, and manifest theme while keeping the existing server/API behavior intact.

## Run locally

Requirements: Node.js 18+ and PostgreSQL.

```bash
npm install
export API_SECRET="replace-with-a-long-random-secret"
export DATABASE_URL="postgresql://user:password@host:5432/database"
export SESSION_SECRET="replace-with-another-long-random-secret"
export DISCORD_CLIENT_ID="your-discord-application-client-id"
export DISCORD_CLIENT_SECRET="your-discord-oauth-client-secret"
export DISCORD_REDIRECT_URI="http://localhost:3000/auth/discord/callback"
export BOT_TOKEN="your-discord-bot-token"
export API_BASE="http://localhost:3000"
npm run start
```

Open `http://localhost:3000`. The web server listens on `PORT` when provided, otherwise port `3000`.

## Deploy on Railway

1. Create a new Railway project and add a PostgreSQL service.
2. Deploy this folder as the Node service. If the repository contains a parent folder, set the service root directory to the folder containing `package.json`.
3. Railway detects Node automatically. The start command can remain `npm run start`; it starts the web server and Discord bot together. For a web-only deployment, use `npm run start:server` and run the bot as a separate Railway service with `npm run start:bot`.
4. Add the following variables to the web service:

   - `API_SECRET`: long random secret used by internal API calls and source protection.
   - `DATABASE_URL`: Railway PostgreSQL connection string. Use the PostgreSQL service reference variable if available.
   - `SESSION_SECRET`: long random session secret.
   - `DISCORD_CLIENT_ID`: Discord OAuth application client ID.
   - `DISCORD_CLIENT_SECRET`: Discord OAuth client secret.
   - `DISCORD_REDIRECT_URI`: `https://YOUR-RAILWAY-DOMAIN/auth/discord/callback`.
   - `BOT_TOKEN`: Discord bot token.
   - `API_BASE`: `https://YOUR-RAILWAY-DOMAIN`.
   - `PORT`: leave unset so Railway can provide it automatically.

   Optional multi-bot variables are `BOT_TOKEN_2`, `BOT_TOKEN_3`, `BOT_TOKEN_4`, `DISCORD_CLIENT_ID_2`, `DISCORD_CLIENT_ID_3`, and `DISCORD_CLIENT_ID_4`.

5. In the Discord Developer Portal, add the exact Railway callback URL under OAuth2 Redirects, enable the required bot intents, and invite the bot using the invite link shown by THE LAB.
6. After the first deploy, open `/health`. A healthy response includes `"status":"ok"`; `databaseReady` should become `true` once PostgreSQL initialization completes.

The app creates its PostgreSQL tables automatically on startup. Never commit the secret values to the repository.
