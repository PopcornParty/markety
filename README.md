# Markety

Markety is a global Discord marketplace network for community and in-game assets such as Minecraft bases, stashes, pearl bases, spawners, builds, plots, islands, shops, mystery shulkers, and rare server items.

It is **not** a real-money marketplace. The `$` symbol is the currency written on a listing, such as Minecraft server money. Markety does not take payments, hold funds, or complete in-game trades.

A listing created in Server A can be found from Server B. The originating server is stored for audit only.

---

## What you need to do

Do these steps in order. Check each box as you finish it.

### 1. Get the project onto GitHub

A repo already exists:

**https://github.com/PopcornParty/markety**

1. Install [Git](https://git-scm.com/) if you do not have it.
2. On your computer, put the full Markety project folder in one place. You need these folders/files:
   - `src/`
   - `prisma/`
   - `tests/`
   - `.github/`
   - `package.json`
   - `tsconfig.json`
   - `.env.example`
   - `.gitignore`
   - `README.md`
   - `Dockerfile`
   - `Procfile`
3. Open a terminal in that folder and run:

```bash
git init
git remote add origin https://github.com/PopcornParty/markety.git
git add .
git commit -m "Add complete Markety bot"
git branch -M main
git push -u origin main
```

If GitHub already has files, use:

```bash
git pull origin main --allow-unrelated-histories
git push -u origin main
```

Do **not** commit a `.env` file.

### 2. Install the tools on your computer

1. Install [Node.js 20 or newer](https://nodejs.org/) (22 is fine).
2. Confirm it works:

```bash
node -v
npm -v
```

3. In the Markety folder run:

```bash
npm install
```

### 3. Create the Discord bot

1. Open https://discord.com/developers/applications
2. Click **New Application**, name it `Markety`, create it.
3. Open the **Bot** tab.
4. Click **Reset Token** / **Copy** and save the token somewhere private. This is `DISCORD_TOKEN`.
5. Under Privileged Gateway Intents, leave Message Content, Server Members, and Presence **off**. Markety only needs the default Guilds intent.
6. Open **OAuth2 → General**.
7. Copy the **Application ID**. This is `DISCORD_CLIENT_ID`.
8. Open **OAuth2 → URL Generator**.
9. Scopes to tick:
   - `bot`
   - `applications.commands`
10. Bot permissions to tick (do **not** tick Administrator):
    - View Channels
    - Send Messages
    - Embed Links
    - Attach Files
    - Read Message History
    - Use External Emojis
    - Add Reactions
11. Copy the generated invite URL at the bottom.
12. Open that URL in a browser and invite Markety into your Discord server.
13. In Discord, turn on **Settings → Advanced → Developer Mode**.
14. Right-click your own avatar → **Copy User ID**. This is `MARKETY_OWNER_ID`.
    Only this user can use `/admin`. Server owners do not get Markety admin.

### 4. Create PostgreSQL

The bot will not start without a real PostgreSQL database. Do not use a file database.

Pick one:

- [Neon](https://neon.tech/)
- [Supabase](https://supabase.com/)
- [Railway](https://railway.app/)
- [Render](https://render.com/)
- A Postgres database on a VPS

Then:

1. Create a database named `markety` if the host asks for a name.
2. Copy the connection string. It looks like:

```text
postgresql://USER:PASSWORD@HOST:5432/markety?schema=public
```

3. That string is `DATABASE_URL`.

### 5. Put your secrets in `.env`

1. In the Markety folder run:

```bash
cp .env.example .env
```

2. Open `.env` and fill in **exactly these**:

```env
DISCORD_TOKEN=paste_bot_token_here
DISCORD_CLIENT_ID=paste_application_id_here
DATABASE_URL=postgresql://USER:PASSWORD@HOST:5432/markety?schema=public
MARKETY_OWNER_ID=paste_your_discord_user_id_here
LOG_LEVEL=info
NODE_ENV=development
```

3. Optional, for faster command updates while testing in one server:

```env
DISCORD_DEV_GUILD_ID=paste_that_server_id_here
```

Get a server ID with Developer Mode on, then right-click the server name → Copy Server ID.

4. Save the file.
5. Never paste these values into GitHub, Discord, or the README.

Where each secret belongs later:

| Secret | On your PC | On the host that runs the bot |
| --- | --- | --- |
| `DISCORD_TOKEN` | `.env` | Host environment / secret |
| `DATABASE_URL` | `.env` | Host environment / secret |
| `MARKETY_OWNER_ID` | `.env` | Host environment / secret |
| `DISCORD_CLIENT_ID` | `.env` | Host environment |
| `DISCORD_DEV_GUILD_ID` | `.env` only if testing | Usually leave empty in production |

### 6. Set up the database tables

Still in the Markety folder:

```bash
npx prisma generate
npx prisma migrate deploy
npm run prisma:seed
```

That creates the tables and the default categories/tags.

If this fails, `DATABASE_URL` is wrong or the database is not reachable.

### 7. Register slash commands

```bash
npm run register:commands
```

- If `DISCORD_DEV_GUILD_ID` is set, commands appear in that server almost immediately.
- If it is not set, commands are global and can take up to an hour.

### 8. Start the bot on your computer

```bash
npm run dev
```

You should see a log that Markety is online.

In Discord, try:

- `/market help`
- `/sell`
- `/listings`
- `/profile`

If commands are missing, wait, or run `npm run register:commands` again.

### 9. Put it online for real (production)

GitHub stores code. GitHub does **not** keep the bot online. You need a host that runs Node 24/7 plus the same PostgreSQL database.

1. Choose a host: Railway, Render Background Worker, Fly.io, or a VPS.
2. Connect the GitHub repo `PopcornParty/markety`.
3. Set these environment variables on the host (not in GitHub files):

```text
DISCORD_TOKEN
DATABASE_URL
MARKETY_OWNER_ID
DISCORD_CLIENT_ID
NODE_ENV=production
LOG_LEVEL=info
```

4. Use this start command:

```bash
npx prisma migrate deploy && npm run register:commands && node dist/index.js
```

If the host builds first, the build command is:

```bash
npm install
npx prisma generate
npm run build
```

5. Confirm the process stays running. If it sleeps or stops, Discord will show the bot as offline.
6. Invite the bot to every server that should use Markety. The marketplace is shared across all of them.

Docker option:

```bash
docker build -t markety .
docker run --env-file .env markety
```

### 10. After it is running, this is how you use it

Users:

- `/sell` — create a listing (optional photo attachments `image1`, `image2`, `image3`)
- `/listings` — browse the global marketplace
- `/market search` — search
- `/market my-listings` — your listings
- `/market my-offers` — sent and received offers
- `/market my-deals` — deals to complete
- `/profile` — marketplace profile
- `/market help` — help

You (only if your ID is `MARKETY_OWNER_ID`):

- `/admin stats`
- `/admin reports`
- `/admin report-view`
- `/admin report-resolve`
- `/admin report-dismiss`
- `/admin users`
- `/admin suspend`
- `/admin unsuspend`
- `/admin listings`
- `/admin delete-listing`
- `/admin scam`
- `/admin deals`
- `/admin audit`
- `/admin health`

Markety only records the deal. Complete the real trade in-game, then mark the deal completed and leave a vouch.

---

## If something breaks

- **Commands missing:** run `npm run register:commands`. Confirm `DISCORD_CLIENT_ID` is the Application ID, not the token.
- **Bot exits immediately:** `.env` is missing `DISCORD_TOKEN`, `DATABASE_URL`, or `MARKETY_OWNER_ID`, or Postgres is down.
- **Database error:** check the `DATABASE_URL` user, password, host, and that the database exists.
- **Cannot DM users:** they have DMs closed. Markety will say so and still save the deal.
- **Admin command rejected:** only the Discord user ID in `MARKETY_OWNER_ID` is the owner.
- **Images rejected:** use PNG, JPEG, WebP, or GIF, max 3, max 8MB. No exe or other files.
- **Bot offline after deploy:** the host stopped the process. Use a worker/background process, not a one-shot build.

---

## What Markety will never do

Do not add these, and do not use Markety for them:

- PayPal, Stripe, bank, or crypto payments
- Real-money escrow
- Account selling
- Gambling

A report is not a confirmed scam. Only you can add or remove a confirmed scam record after review.

---

## Tests you can run

```bash
npm test
npm run lint
npx prisma validate
```

GitHub Actions in `.github/workflows/test.yml` runs those on push. Do not put `DISCORD_TOKEN` in workflow files.

---

## Required bot permissions (copy this into the Developer Portal)

Scopes: `bot`, `applications.commands`

Permissions: View Channels, Send Messages, Embed Links, Attach Files, Read Message History, Use External Emojis, Add Reactions

Not required: Administrator, Manage Server, Manage Messages, Message Content Intent.

---

## License

MIT
