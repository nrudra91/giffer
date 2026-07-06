# 🎬 Giffer — GIF Caption Studio

A dead-simple, free, **100% local** web app for adding captions to GIFs. Pick a GIF, add one or many captions, style them, control exactly when each appears and disappears, and save the result — without blowing up the file size.

No accounts, no uploads, no paid tiers. Everything runs in your browser.

## Features

- **Load any GIF** — drag & drop or browse.
- **Multiple captions**, each fully independent — and **duplicate** any caption (⧉) to reuse its styling.
- **Full styling** — font (Impact, Anton, Bangers & more), size, fill color, outline color/width, background shape (rectangle / rounded / pill) with opacity, alignment, and UPPERCASE.
- **Drag to position** captions directly on the preview.
- **Timing control** — set each caption's start and end with sliders, plus optional fade or slide-up entrance/exit animations. A timeline shows every caption's window with a live playhead.
- **Keep file size down** — re-encodes with color quantization; adjust colors, scale, and speed, and see the new size vs. the original before downloading.

## Running it

The app loads two small encoding libraries as ES modules, so it needs to be served over `http://` (a double-clicked `file://` won't load the modules).

**macOS:** double-click **`Start Giffer.command`** — it starts a tiny local server and opens the app in your browser. Leave that window open while you work.

**Any platform (manual):**

```bash
node serve.js        # then open http://localhost:4599
# or:  python3 -m http.server 4599
```

> First launch needs internet once to fetch the two libraries ([gifuct-js](https://github.com/matt-way/gifuct-js) + [gifenc](https://github.com/mattdesl/gifenc)); the browser caches them afterward.

## Deploying (Cloudflare + GitHub Actions)

The app is fully static (everything in `public/`), so it deploys as a Cloudflare Worker with static assets. Every push to `master` triggers [.github/workflows/deploy.yml](.github/workflows/deploy.yml), which runs `wrangler deploy`.

One-time setup:

1. In [wrangler.jsonc](wrangler.jsonc), replace `giffer.example.com` with your domain (its zone must already be on Cloudflare).
2. In the GitHub repo settings → **Secrets and variables → Actions**, add:
   - `CLOUDFLARE_API_TOKEN` — create at dash.cloudflare.com → My Profile → API Tokens, using the **Edit Cloudflare Workers** template.
   - `CLOUDFLARE_ACCOUNT_ID` — shown on the right side of any zone's Overview page (or Workers & Pages → Overview).
3. Push. Wrangler creates the DNS record for the custom domain automatically on first deploy.

## Tech

Plain HTML/CSS/JS — no build step. GIF decoding via `gifuct-js`, re-encoding via `gifenc`, compositing on `<canvas>`.
