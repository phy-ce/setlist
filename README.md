# SETLIST

**Paste a YouTube playlist URL — see the whole thing.** That's the app.

🔗 **Live:** https://phy-ce.github.io/setlist/

Paste any playlist link on the dark landing screen, press **Enter** (or click **→**), and the playlist opens. That's the whole interaction — no accounts, no sign-in, no setup.

## What you get

Once a playlist loads you land on the **Wall** and can swipe left / right (or use the **‹ ›** arrows / bottom dots) between three panels:

- **← Insights** — total runtime up top, then a **vertical timeline**: one band per video where the band's **height = its runtime**, and its background is that video's **thumbnail, darkened**. Longer videos take up more room; scroll straight down the whole playlist. Click a band to open the video.
- **● Wall** *(home)* — a gap-free, full-bleed wall of thumbnails with hover-zoom. The **size slider** (top-right, drag it) grows or shrinks the tiles.
- **Shuffle →** — a random video in the spotlight; hit **↻** to reroll.

The **←** button (top-left) returns to the landing screen.

## Accepted inputs

Paste any of these — the parser figures it out:

- A watch URL with a list param — `https://www.youtube.com/watch?v=...&list=PL...`
- A playlist page URL — `https://www.youtube.com/playlist?list=PL...`
- A raw playlist ID — `PL...`, `UU...`, `LL...`, `FL...`, `OL...`, `RD...`

Anything else shows a quiet inline note and keeps you on the landing screen.

## How it's built

One self-contained `index.html` — vanilla HTML/CSS/JS, no framework, no build step to run it. The **Sora** font is embedded as base64, so it renders identically anywhere with no external requests. Real playlist data comes from the **YouTube Data API v3**; thumbnail colors/backgrounds are read straight from `i.ytimg.com`.

## Running your own copy

The live site already has an API key wired in (restricted to this domain), so **you don't need to do anything to use it** — just open the link.

If you **fork or self-host**, that key won't work on your domain (it's referrer-locked), so add your own free one:

1. [Google Cloud Console](https://console.cloud.google.com/) → **New Project** (any name).
2. **APIs & Services → Library →** enable **"YouTube Data API v3"**.
3. **Credentials → Create credentials → API key** — copy it.
4. Paste it into the config line near the top of the `<script>` in `index.html`:
   ```js
   const YT_API_KEY = "YOUR_KEY_HERE";
   ```
   *(Leave it `""` and the app falls back to a built-in demo playlist — handy for trying the UI with zero setup.)*

**It's free** — no billing or credit card, and the ~10,000 units/day quota is far more than casual use needs (a playlist load costs only a handful of units).

### ⚠️ Restrict the key before deploying publicly

A client-side key is **visible in the page source**. In the Cloud Console, open the key → **Application restrictions → Websites (HTTP referrers)** and add your site, e.g.:

```
your-name.github.io/*
```

That way it only works from your site and nobody can reuse it. (Do this *before* making a repo public.)

## Deploy

Static site — host it anywhere:

- **GitHub Pages** — push the repo, then **Settings → Pages → Branch: `main` / root**. Add your `*.github.io/*` domain to the API-key referrer restriction.
- **Netlify / Vercel / any static host** — drop the folder in.

## For maintainers: the build step

`index.html` is **generated from `index.html.template`.** The template's `@font-face` blocks hold placeholder tokens (`@@SORA300@@`, `@@SORA400@@`, `@@SORA600@@`, `@@SORA700@@`); a build step injects the base64-encoded **Sora** font data into them to produce the final self-contained file. **Edit the template, not the generated `index.html`,** then re-run the injection.

## License

[MIT](LICENSE) © 2026 phy-ce
