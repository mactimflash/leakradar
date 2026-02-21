# LeakRadar — DevOps Leak Bot (Telegram + Cloudflare Worker + KV + Shodan + Dashboard)

**What you get**
- Cloudflare Worker: Telegram bot + `/scan` + `/bulk` + Shodan enrichment + KV storage + Dashboard API
- Dashboard (static `index.html`) to view bulk results **by domain**
- CSV export

Defensive auditing only.

---

## 1) Deploy Worker

### Install wrangler
```bash
npm i -g wrangler
wrangler login
```

### Create KV namespace
```bash
cd worker
wrangler kv namespace create "KV"
wrangler kv namespace create "KV" --preview
```
Copy IDs into `worker/wrangler.toml`.

### Set secrets
```bash
wrangler secret put TELEGRAM_BOT_TOKEN
wrangler secret put SHODAN_API_KEY
wrangler secret put DASHBOARD_TOKEN
wrangler secret put RUN_TOKEN
```
Optional:
```bash
wrangler secret put DEFAULT_CHAT_ID
```

### Deploy
```bash
wrangler deploy
```

---

## 2) Set Telegram webhook
```bash
curl -s "https://api.telegram.org/bot<TELEGRAM_BOT_TOKEN>/setWebhook?url=<WORKER_URL>/telegram"
```

---

## 3) Use bot
- `/help`
- `/scan example.com`
- `/bulk domain1.com,domain2.com`
- `/status`
- `/export` (CSV preview)

---

## 4) Dashboard
Host `dashboard/index.html` (GitHub Pages / Cloudflare Pages / any static hosting).

Fill:
- Worker URL
- Chat ID
- Dashboard token (DASHBOARD_TOKEN)

Click Refresh / Export CSV.

---
