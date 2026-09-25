# Markety

A global Discord marketplace for community items such as Minecraft bases, stashes, pearl bases, spawners, builds, plots, islands, shops, and mystery shulkers.

Markety does **not** move real money. `$` is the listing’s in-game currency. Trades happen in-game. Markety only records listings, offers, deals, vouches, and reports.

---

## If you only have an iPhone

You do **not** need a computer. You do **not** install Node, Git, or any app except Safari, Discord, and GitHub.

Everything below is done in the phone browser.

Free 24/7 hosting is possible, but it is not magic:

- GitHub stores the code. It does **not** keep the bot online.
- Render / Replit free web apps **sleep**. That knocks a Discord bot offline.
- The path below uses **Neon** (free Postgres) + **Koyeb** (free Node host) + **cron-job.org** (free pinger so the host does not sleep).

If a free host later adds a card check or a queue, use the backup host at the bottom. The secrets you type stay the same.

Repo: **https://github.com/PopcornParty/markety**

---

## What you will collect

Write these four values in Notes on your iPhone. Do not post them in Discord or in the repo.

| Name | Where you get it |
| --- | --- |
| `DISCORD_TOKEN` | Discord Developer Portal → Bot → Reset Token |
| `DISCORD_CLIENT_ID` | Discord Developer Portal → OAuth2 → Application ID |
| `MARKETY_OWNER_ID` | Discord → You tab → App Settings → Advanced → Developer Mode on, then tap your avatar → Copy User ID |
| `DATABASE_URL` | Neon dashboard after you create the database |

---

## Step 1 — Make sure the GitHub repo has the bot code

1. On your iPhone open the **GitHub** app or Safari: https://github.com/PopcornParty/markety
2. You should see folders named `src`, `prisma`, and `tests`.
3. If those folders are missing, the host cannot start the bot. The full project has to be pushed to that repo first.

Do not upload a file named `.env`.

---

## Step 2 — Create the Discord bot (Safari)

1. Open https://discord.com/developers/applications and sign in.
2. Tap **New Application**. Name it `Markety`. Create it.
3. Tap **Bot**.
4. Tap **Reset Token** → **Copy**. Paste into Notes as `DISCORD_TOKEN`.
5. Leave privileged intents **off**.
6. Tap **OAuth2**. Copy **Application ID** into Notes as `DISCORD_CLIENT_ID`.
7. Tap **OAuth2 → URL Generator**.
8. Tick scopes: `bot` and `applications.commands`.
9. Tick bot permissions only: View Channels, Send Messages, Embed Links, Attach Files, Read Message History, Use External Emojis, Add Reactions.
10. Do **not** tick Administrator.
11. Copy the invite URL at the bottom, open it, pick your server, authorize.
12. In the Discord iPhone app: **You** tab → **App Settings** → **Advanced** → turn on **Developer Mode**.
13. Tap your profile picture → **Copy User ID**. Paste into Notes as `MARKETY_OWNER_ID`.

Only that user ID can use `/admin`.

---

## Step 3 — Create free PostgreSQL on Neon (Safari)

1. Open https://console.neon.tech and sign up with GitHub.
2. Create a project named `markety`.
3. Open **Connection details** and copy the URI (pooled if shown).
4. Save it in Notes as `DATABASE_URL`.
5. If it has no `sslmode=require`, add `?sslmode=require` or `&sslmode=require`.

---

## Step 4 — Put the bot online on Koyeb (Safari)

1. Open https://app.koyeb.com and sign up with GitHub.
2. Create a service from GitHub repo `PopcornParty/markety`, branch `main`.
3. Use the **Free** instance as a **Web Service**.
4. Add environment variables: `DISCORD_TOKEN`, `DISCORD_CLIENT_ID`, `MARKETY_OWNER_ID`, `DATABASE_URL`, `NODE_ENV=production`, `LOG_LEVEL=info`.
5. Run command if asked:

```text
npx prisma migrate deploy && npx prisma generate && npm run register:commands && node dist/index.js
```

6. Deploy. Open the public Koyeb URL. You should see `{"ok":true,"service":"markety"}`.

---

## Step 5 — Stop the free host from sleeping (Safari)

1. Open https://cron-job.org and create a free account.
2. Create a job that hits `https://YOUR-KOYEB-URL/health` every 10 minutes.
3. Enable it. This is what keeps the bot online overnight.

---

## Step 6 — Check it in Discord

Run `/market help`, `/sell`, `/listings`. Run `/admin health` from the owner account.

---

## Where secrets go

Type them only in the Koyeb environment variables screen. Never in GitHub files. On iPhone you do not create a `.env` file.

---

## Backup hosts if Koyeb refuses the app

Same four secrets, same start command.

- https://justrunmy.app/discord-bots
- HeavenCloud panel after claiming in their Discord

Skip free Render and free Replit. They sleep and the bot goes offline.

---

## License

MIT
