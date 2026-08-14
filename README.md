# mb-design-engineering-web

Event site for **Muslims In Construction** — currently the **Box Hill Trek for
READ Foundation** page (Saturday 12 September 2026). Served as [Cloudflare
Workers static assets](https://developers.cloudflare.com/workers/static-assets/):
everything in `public/` is the site, there is no build step.

## Structure

```
public/                     everything served
  index.html                the trek page (content as supplied — Rev. A draft)
  404.html                  themed not-found page
  favicon.svg / favicon.ico / apple-touch-icon.png
  robots.txt
  _headers                  security + caching headers
  assets/
    css/main.css            trek page styles (split out of the original file)
    js/countdown.js         trek page countdown (split out of the original file)
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
  Shoulders Display, Archivo, JetBrains Mono.
- **WhatsApp group invite** (`chat.whatsapp.com`) and the National Trust Box
  Hill page (`nationaltrust.org.uk`) — outbound links on the page.

The page's canonical URL points at `muslimsinconstruction.uk` (the
organisation's own domain); that domain is not reachable from the build
environment used to assemble this repo, which is fine — nothing served here
depends on it.
