# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Matrix Terminal Chat — a greenfield single-file landing page (`index.html`) providing a Matrix-style terminal chat UI that connects to an n8n chatbot via webhook.

**Stack:** Plain HTML/CSS/JS, no build step, no dependencies beyond a Google Font CDN.

## Running the Project

Open `index.html` directly in a browser — no build step, no server required.

## Configuration Before Deploying

Set the `WEBHOOK_URL` constant near the top of the `<script>` block in `index.html` to your actual n8n webhook URL:

```js
const WEBHOOK_URL = 'https://your-n8n-instance.com/webhook/YOUR-WEBHOOK-ID';
```

Also ensure CORS is enabled on the webhook node in n8n (Allow all origins).

## Architecture

All code lives in a single `index.html` file:

- `<head>` — meta tags, Google Font (Share Tech Mono), `<style>` block with CSS variables
- `<body>` — `<canvas id="matrix-canvas">` for rain effect, `#terminal-container` for chat UI, `<script>` block

## Key Design Decisions

- Matrix rain uses `setInterval` at 50 ms (not `requestAnimationFrame`) for a consistent "digital" feel
- Terminal background is `rgba(0,0,0,0.82)` so the rain shows through
- n8n response parser handles `{output}`, `{text}`, `{message}`, array wrappers, and falls back to `JSON.stringify`
