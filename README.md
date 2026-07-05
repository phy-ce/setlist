# SETLIST

Paste a YouTube playlist URL, see it as a full-bleed wall of thumbnails. That's the whole app.

Type or paste a playlist link on the dark landing screen, press **Enter** (or click the **→**), and the playlist opens as a gap-free gallery wall with hover-zoom and a *NOW VIEWING* header. The **←** button takes you back. No accounts, no lists, no ratings.

It's a single self-contained HTML file — vanilla HTML/CSS/JS, no framework, no build step needed to run it.

## Run it

**Demo mode works instantly with zero setup.** Just open `index.html` in a browser and you'll see a built-in sample playlist ("Late Night Synthwork") rendered exactly like the real thing.

For **live** API calls, some browsers restrict `fetch()` from `file://` pages, so serve the folder over http:

```bash
npx serve .
# or use the VS Code "Live Server" extension
```

Then open the URL it prints (e.g. `http://localhost:3000`).

## Enable real playlists

By default SETLIST runs in demo mode. To load real playlists you need a free YouTube Data API v3 key:

1. Go to the [Google Cloud Console](https://console.cloud.google.com/).
2. Create a **New Project** (any name).
3. **APIs & Services → Library →** search **"YouTube Data API v3" → Enable**.
4. **APIs & Services → Credentials → Create credentials → API key.** Copy it.
5. Open `index.html` and paste the key into the config line near the top of the `<script>`:

   ```js
   const YT_API_KEY = "YOUR_KEY_HERE";
   ```

That's it — reload and paste any playlist URL.

**It's genuinely free:** no billing account or credit card required, and the default quota is ~10,000 units/day, which is far more than enough for casual use (each playlist load costs only a handful of units).

## Security note

A client-side API key is **visible in the page source** to anyone who views it. Before deploying publicly, restrict the key in the Cloud Console:

- **Credentials → your key → Application restrictions → HTTP referrers**, and add your site's domain (e.g. `https://yourname.github.io/*`).

This stops others from reusing your key on their own sites. For a purely local/demo use you can leave the key empty and stay in demo mode.

## Deploy

SETLIST is a static site — host it anywhere:

- **GitHub Pages:** push the folder to a repo, enable Pages on the branch.
- **Netlify:** drag-and-drop the folder onto the Netlify dashboard.

Remember to add your deployed domain as an HTTP-referrer restriction on the API key.

## Accepted playlist inputs

The URL parser accepts any of:

- A full watch URL with a list param — `https://www.youtube.com/watch?v=...&list=PL...`
- A playlist page URL — `https://www.youtube.com/playlist?list=PL...`
- A raw playlist ID pasted directly — `PL...`, `UU...`, `LL...`, `FL...`, `OL...`, `RD...`

Anything else shows a quiet inline message and keeps you on the landing screen.

## For maintainers: the build step

`index.html` is **generated from `index.html.template`.** The template contains `@font-face` blocks whose `src` holds placeholder tokens (`@@SORA300@@`, `@@SORA400@@`, `@@SORA600@@`, `@@SORA700@@`). A separate build step injects the base64-encoded **Sora** font data into those tokens to produce the final, fully self-contained `index.html`. If you edit the design, edit the **template** and re-run that step — don't hand-edit the generated file.
