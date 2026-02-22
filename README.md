# Matrix Terminal Chat Interface

A cyberpunk-themed, browser-based chat UI that connects to an AI assistant via an n8n webhook. Features a live Matrix rain animation background and a retro green terminal aesthetic.

**Live demo:** https://matrix-terminal-chat-lilac.vercel.app

---

## What It Does

- Renders a full-screen Matrix rain canvas (Japanese katakana + alphanumerics)
- Overlays a fixed terminal window ("NEURAL LINK TERMINAL v2.1")
- Accepts user text input, sends it to an n8n workflow via webhook
- Displays the AI response in the terminal in real time
- Parses n8n response formats: `output`, `text`, or `message` fields, and array wrappers

---

## Tech Stack

| Layer | Technology |
|---|---|
| Frontend | Plain HTML / CSS / JavaScript — no framework, no build step |
| Font | [Share Tech Mono](https://fonts.google.com/specimen/Share+Tech+Mono) (Google Fonts CDN) |
| AI workflow | [n8n](https://n8n.io) webhook connected to an OpenAI model |
| Backend proxy | Vercel serverless function (`api/chat.js`) — hides the webhook URL from the browser |
| Hosting | [Vercel](https://vercel.com) |

---

## How to Run Locally

No build step required. Open `index.html` directly in a browser:

```bash
open index.html
```

Or serve it over HTTP to avoid any browser restrictions:

```bash
python3 -m http.server 8080
# then open http://localhost:8080
```

> **Note:** When running locally the `/api/chat` proxy is not available (it only runs on Vercel). For local testing, temporarily set `WEBHOOK_URL` in `index.html` to your n8n webhook URL directly and ensure CORS is enabled on the n8n webhook node.

---

## Deployment (GitHub + Vercel)

The webhook URL is never stored in the code. It lives in a Vercel environment variable and is accessed server-side only.

### 1. Push to GitHub

```bash
git init
git add .
git commit -m "Initial commit"
git remote add origin https://github.com/khanhinadxbai/claude-code-test.git
git branch -M main
git push -u origin main
```

### 2. Deploy to Vercel

Import the GitHub repo at [vercel.com/new](https://vercel.com/new):

- Framework Preset: **Other**
- Build Command: *(leave blank)*
- Output Directory: *(leave blank)*

### 3. Set the Environment Variable

In the Vercel project settings → **Environment Variables**, add:

| Name | Value |
|---|---|
| `WEBHOOK_URL` | your n8n webhook URL |

Redeploy for the variable to take effect.

### How the proxy works

```
Browser → POST /api/chat → Vercel (api/chat.js) → n8n webhook → OpenAI → response
```

The browser only ever sees your Vercel domain. The n8n URL is never exposed in page source or DevTools.

---

## Project Structure

```
├── index.html       # Full app — Matrix rain canvas + terminal chat UI
├── api/
│   └── chat.js      # Vercel serverless proxy (reads WEBHOOK_URL from env)
├── .gitignore
└── CLAUDE.md        # Notes for AI-assisted development
```

---

## Configuration Reference

| Constant | File | Purpose |
|---|---|---|
| `WEBHOOK_URL` | `index.html:246` (local) / Vercel env var (production) | n8n webhook endpoint |
| `FONT_SIZE` | `index.html` | Matrix rain character size (default: 14px) |
| Matrix interval | `index.html` | Rain refresh rate (default: 50ms) |
