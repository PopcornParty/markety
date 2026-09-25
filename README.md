# Markety

A global Discord marketplace for community items such as Minecraft bases, stashes, pearl bases, spawners, builds, plots, islands, shops, and mystery shulkers.

Markety does **not** move real money. `$` is the listing’s in-game currency. Trades happen in-game. The bot only records listings, offers, deals, vouches, and reports.

You can set this up on an **iPhone**. You do not install Node or Git.

Host used in this guide: **[JustRunMy.App](https://justrunmy.app/discord-bots)**  
Database: **[Neon](https://console.neon.tech)**  
Code: **[https://github.com/PopcornParty/markety](https://github.com/PopcornParty/markety)**

JustRunMy.App is built for Discord bots. It stays on 24/7 and does not use sleep mode like free Render or Replit.

---

## What you will save in iPhone Notes

Do not post these in Discord or GitHub.

| Name | Where you get it |
| --- | --- |
| `DISCORD_TOKEN` | Discord Developer Portal → Bot → Reset Token |
| `DISCORD_CLIENT_ID` | Discord Developer Portal → OAuth2 → Application ID |
| `MARKETY_OWNER_ID` | Discord app → You → App Settings → Advanced → Developer Mode on, then tap your avatar → Copy User ID |
| `DATABASE_URL` | Neon → Connect → copy the `postgresql://` line |

---

## Step 1 — Create the Discord bot

1. Open Safari: https://discord.com/developers/applications
2. Tap **New Application**. Name it `Markety`.
3. Tap **Bot** → **Reset Token** → **Copy**. Save as `DISCORD_TOKEN`.
4. Leave privileged intents off.
5. Tap **OAuth2**. Copy **Application ID**. Save as `DISCORD_CLIENT_ID`.
6. Tap **OAuth2 → URL Generator**.
7. Tick scopes: `bot` and `applications.commands`.
8. Tick permissions only: View Channels, Send Messages, Embed Links, Attach Files, Read Message History, Use External Emojis, Add Reactions.
9. Do not tick Administrator.
10. Open the invite URL, pick your server, authorize.
11. In the Discord iPhone app: **You** → **App Settings** → **Advanced** → Developer Mode on.
12. Tap your profile picture → **Copy User ID**. Save as `MARKETY_OWNER_ID`.

Only that user ID can use `/admin`.

---

## Step 2 — Create free Postgres on Neon

1. Open Safari: https://console.neon.tech and sign in with GitHub.
2. Create a project named `markety`.
3. Open the project.
4. Tap **Connect** at the top.
5. Leave **Connection pooling** on.
6. Copy the long line that starts with `postgresql://`.
7. Save it as `DATABASE_URL`.

If you ever pasted that URL in a chat, reset the Neon password and copy a new URL.

---

## Step 3 — Download the Markety zip from GitHub

1. Open Safari: https://github.com/PopcornParty/markety
2. Tap the green **Code** button.
3. Tap **Download ZIP**.
4. Open the **Files** app → **Downloads**.
5. You should have `markety-main.zip`.

Direct link: https://github.com/PopcornParty/markety/archive/refs/heads/main.zip

Do not put secrets inside the zip. The unzipped folder needs `package.json`, `src`, and `prisma`.

---

## Step 4 — Put the bot on JustRunMy.App

1. Open Safari: https://justrunmy.app
2. Sign up / log in.
3. Tap **Create Application**.
4. Choose **Zip Upload** and upload `markety-main.zip`.
5. Choose **Node.js**, version **20** or **22**.
6. Add environment variables:

```text
DISCORD_TOKEN
DISCORD_CLIENT_ID
MARKETY_OWNER_ID
DATABASE_URL
NODE_ENV=production
LOG_LEVEL=info
```

7. Set the start command:

```text
npx prisma migrate deploy && npx prisma generate && npm run register:commands && npm start
```

8. If it asks for a port, use `3000`.
9. Tap **Start** / **Deploy** and open **Logs**.

JustRunMy.App does not need cron-job.org.

---

## Step 5 — Check the bot in Discord

Run `/market help`, `/sell`, `/listings`. Run `/admin health` from the owner account.

---

## Where secrets go

Type them only in the JustRunMy.App environment variable screen. Never in the GitHub zip.

---

## License

MIT
