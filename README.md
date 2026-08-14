# mb-design-engineering-web

Event site for **Muslims In Construction**, served as [Cloudflare Workers
static assets](https://developers.cloudflare.com/workers/static-assets/):
everything in `public/` is the site, there is no build step.

Routes:

- `/` — **Box Hill Trek for READ Foundation** (Saturday 12 September 2026)
- `/gala/` — **MIC Gala 2026** awards dinner (Saturday 28 November 2026), with
  the printed leaflet (PDF and front image) served alongside it

## Structure

```
public/                     everything served
  index.html                the trek page (content as supplied — Rev. A draft)
  404.html                  themed not-found page
  favicon.svg / favicon.ico / apple-touch-icon.png
  robots.txt
  _headers                  security + caching headers
  gala/
    index.html              the gala page (content as supplied)
    mic-gala-2026-leaflet.pdf
    mic-gala-2026-leaflet-front.png
  assets/
    css/main.css            trek page styles (split out of the original file)
    css/gala.css            gala page styles (split out of the original file)
    js/countdown.js         trek page countdown (split out of the original file)
    js/gala.js              gala countdown, scroll reveal, notify form
wrangler.jsonc              assets-only Worker config, no script
package.json                wrangler devDependency + dev/deploy/check scripts
CLAUDE.md                   standing git + release policy and the release ledger
```

The page's content, design, and behaviour are exactly as supplied; the inline
`<style>` and `<script>` blocks were moved to `assets/` unchanged. The `TBC`
tags visible on the page are the owner's review markers, kept deliberately.

## Local development

```bash
npm install
npm run dev        # wrangler dev — serves public/ on localhost
```

For a quick look without wrangler: `python3 -m http.server -d public`.

## Verification before every push

1. `npm run check` (`wrangler deploy --dry-run`) — validates the config.
2. Serve `public/`, render index and 404 with headless Chromium at desktop and
   mobile widths, and inspect the screenshots: styles applied, fonts loaded,
   layout intact.

## Deployment

The repo is meant to be connected to **Cloudflare Workers Builds** (Workers &
Pages → Create → Import a repository): every push to `main` then deploys to
production automatically. Manual deploys are `npm run deploy` with a logged-in
wrangler.

Caching is set in `public/_headers`: `assets/` is cached hard for a year, so
**rename an asset file when its contents change** (and update its references);
HTML always revalidates, so releases are visible immediately.

## External resources

Kept as absolute URLs on purpose — do not vendor them:

- **Google Fonts** (`fonts.googleapis.com`, `fonts.gstatic.com`) — Big
  Shoulders Display, Archivo, JetBrains Mono on the trek page; Archivo,
  Newsreader, IBM Plex Mono on the gala page.
- **Google Maps embed** (`www.google.com/maps`) — the venue map iframe on the
  gala page.
- **Open Graph image** for the gala page — hosted on
  `muslimsinconstruction.uk` (the organisation's WordPress media library).
- **Outbound links** — WhatsApp (`chat.whatsapp.com`, `api.whatsapp.com`),
  the National Trust Box Hill page, LinkedIn, Instagram, TikTok.

The pages' canonical URLs point at `muslimsinconstruction.uk` (the
organisation's own domain); that domain is not reachable from the build
environment used to assemble this repo, which is fine — nothing served here
depends on it.

## Content notes, as supplied

- The trek page carries the owner's `TBC` review markers deliberately
  (meeting point, route, fundraising link and similar) — it is a Rev. A draft
  for review.
- On the gala page the three "Book" buttons are placeholders (`href="#"`)
  until a booking platform is chosen, and the nominations notify form has no
  backend endpoint yet — the script acknowledges locally and is marked for
  replacement with a real endpoint. Both are exactly as in the supplied file.
