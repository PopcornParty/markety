# Markety

Markety is a global Discord marketplace network for community and in-game assets.

It is designed for communities such as Minecraft servers, where people trade bases, stashes, pearl bases, spawners, custom builds, plots, islands, shops, mystery shulkers, rare server items, and similar assets.

Markety is **not** a real-world money marketplace.

- The `$` symbol is the currency written on a listing, such as Minecraft server money.
- Markety does not take payments, hold funds, or complete in-game trades.
- Real-world money, gambling, and account trading are prohibited.
- A report is not a confirmed scam. Only the Markety owner can add a confirmed scam record after review.

The marketplace is global. A listing created through Server A can be discovered by someone using Markety in Server B. The originating Discord server is stored for audit context only. It is not the marketplace boundary.

Full source lives in this repository after you push the `src/`, `prisma/`, and `tests/` directories from the generated project.

## Quick start

```bash
npm install
cp .env.example .env
# set DISCORD_TOKEN, DATABASE_URL, MARKETY_OWNER_ID, DISCORD_CLIENT_ID
npx prisma generate
npx prisma migrate deploy
npm run prisma:seed
npm run register:commands
npm run dev
```

Secrets never go in source. Put them in `.env` locally and in host secrets in production.
