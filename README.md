# TempMail YN — Disposable Email App

A mobile-friendly temporary email app powered by the free **[mail.tm](https://mail.tm) API**. No signup, no API key, no backend — everything runs in the browser.

## Features

- ⚡ **One-tap random address** — generate a disposable email instantly
- ✍️ **Custom address** — pick your own prefix
- 📥 **Live inbox** — polls every 15 seconds, new mail appears automatically
- 📖 **Read messages** — plain text and sanitized HTML body
- 📎 **Download attachments**
- 🔑 **Sign in** to an existing mail.tm account
- 💾 **Session persistence** — stays logged in after refresh (localStorage)
- 🗑️ **Delete messages / delete account** from the menu

## How to run

The app is a single self-contained HTML file — no build tools, no dependencies.

1. Open `index.html` in any modern browser (Chrome, Edge, Safari, Firefox).
2. Tap **"Generate random address"** or type a custom prefix and create it.
3. Copy your address, sign up wherever you need an email, and watch the inbox fill up.

> **Note:** If you open the file directly (`file://`), the clipboard copy button falls back to a textarea-based copy. All API calls go straight to `https://api.mail.tm`, which allows browser (CORS) access — the same API powering [mail.tm](https://mail.tm) itself.

## Rate limits (important)

mail.tm allows **10 requests per minute per IP**. On mobile data, carriers NAT thousands of subscribers behind a single IP, so that budget is often already spent by strangers — `/domains` can return `429` even when nothing is wrong with this app.

What the app does about it:

- polls the inbox every **15 s** instead of 5 s, and backs off to 45 s after a failure
- retries `/domains` up to 3 times with increasing delay
- caches the domain list in `localStorage`; if the API is unreachable it falls back to the last known domain (and a built-in default) so the dropdown is never empty
- shows the actual failure reason in a status line under the hero text — tap it to retry

Creating an address costs 3 calls (create → token → me), so if you hit `Too many requests`, wait a minute before retrying.

## API reference used

| Purpose | Endpoint |
|---|---|
| List domains | `GET https://api.mail.tm/domains` |
| Create account | `POST https://api.mail.tm/accounts` |
| Get token | `POST https://api.mail.tm/token` |
| Current account | `GET https://api.mail.tm/me` |
| List messages | `GET https://api.mail.tm/messages` |
| Message detail | `GET https://api.mail.tm/messages/{id}` |
| Mark read | `PATCH https://api.mail.tm/messages/{id}` |
| Delete message | `DELETE https://api.mail.tm/messages/{id}` |
| Delete account | `DELETE https://api.mail.tm/accounts/{id}` |

## Attribution

Powered by [mail.tm](https://mail.tm) — free temporary email API. Per their terms, this app does **not** sell, proxy, or mirror the service; it only wraps it for personal use. Please keep the footer credit visible.
