# cv.corbet.ch v1 (legacy) — Project Knowledge

> **This repo is frozen as of 2026-05-23.** Active development continues in `cv-corbet-ch` (toward v2). v1 is deployed as a separate Cloudflare Worker `cv-v1` at `v1.cv.corbet.ch` so the v1 site stays directly surfable without remembering tags.

> When the user asks about cv source/work without specifying v1, default to the v2 repo at `~/Documents/GitHub/cv/`. Only edit here for critical fixes that must keep v1 alive.

## Stack at freeze

- Astro 6 + Tailwind 4 + daisyUI 5 + bun (per `beaa618`)
- Static build → `dist/`, served by Cloudflare Worker `cv-v1` via the Static Assets pattern (`[assets] directory = "./dist"`)
- Custom domain: `v1.cv.corbet.ch` (declared in `wrangler.toml` `[[routes]]` with `custom_domain = true`)

## Deploy

```bash
cd ~/Documents/GitHub/cv-v1
bun install --frozen-lockfile
bun run build
CLOUDFLARE_EMAIL="$cf_email" CLOUDFLARE_API_KEY="$cf_key" bunx wrangler deploy
```

Where `cf_email` / `cf_key` come from `~/.agent/secrets/cloudflare.md`. Never print them.

## Banner

`src/layouts/BaseLayout.astro` carries an amber banner pointing visitors at `cv.corbet.ch`. Don't remove it — it's the contract that justifies keeping v1 live.

## What changed vs the v3 careervector v1 pattern

careervector v1 reuses the same Cloudflare Pages project as v2/v3 and the URL split (`old.`, `v1.`) is configured at the Pages level. cv-v1 instead runs as its own Worker (`cv-v1`), since cv is on the modern Workers Static Assets pattern. The `cv` Worker (serving `cv.corbet.ch`) is untouched by this repo.
