# MB Design Solutions — website

The site for MB Design Solutions, structural engineers in Birmingham, served by
Cloudflare Workers as static assets. There is no build step: the files in
`public/` are the site.

While the concept is being sold, the repo also carries a **two-tab pitch**
wrapped around the site — see [The pitch](#the-pitch) below.

## Structure

```
public/
  index.html          the site (pitch tab bar fenced inside — see below)
  offer.html          pitch tab 2: the case and the price (served at /offer)
  404.html            themed "sheet not found" page
  favicon.svg         MB monogram on a graphite sheet
  robots.txt
  _headers            security + caching headers
  assets/css/
    main.css          the site's stylesheet
    offer.css         offer-page components        (pitch only)
    pitch.css         tab-bar chrome + nav offsets (pitch only)
wrangler.jsonc        assets-only Cloudflare config, no Worker script
package.json          wrangler devDependency + dev/deploy/check scripts
```

## Local development

```bash
npm install
npm run dev     # wrangler dev — Cloudflare's real routing (clean /offer URL, 404 page)
npm run check   # wrangler deploy --dry-run — validate the config
```

## Deployment

The repo connects to **Cloudflare Workers Builds**: every push to `main`
deploys to production. To connect it: Cloudflare dashboard → Workers & Pages →
Create → Import a repository. A failed build from before the first release is
expected and clears on the next push to `main`.

## The pitch

MB Design Solutions has no existing website, so the pitch sells the concept
rather than comparing against an old site:

- `/` — the new site itself (tab 1)
- `/offer` — the case for having one, and the price (tab 2)

An identical numbered tab bar sits above both pages as chrome around the demo.
`/offer` is kept out of search results (meta `noindex`, an `X-Robots-Tag`
header, and a `robots.txt` disallow).

**Prices quoted on `/offer`:** £500 one-off for the site, £50 per round of
changes afterwards. To change them, edit `public/offer.html` — they appear in
the two price cards, the closing paragraph, and the footer title block.

## Removing the pitch (the day the client says yes)

1. Delete `public/offer.html`, `public/assets/css/offer.css`, and
   `public/assets/css/pitch.css`.
2. In `public/index.html`, delete both fenced blocks marked `PITCH-CHROME`
   (one `<link>` in `<head>`, one `<nav>` straight after `<body>`).
3. In `public/_headers`, delete the `/offer` block at the bottom (marked with
   a pitch comment).
4. In `public/robots.txt`, delete the `Disallow: /offer` line and its comment.

Nothing else references the pitch; the site is left alone at the root.

## External resources

Fonts load from Google Fonts (`fonts.googleapis.com` / `fonts.gstatic.com`):
Saira Condensed, Saira, and Space Mono. They are referenced by absolute URL and
not vendored into the repo.

## Content placeholders (as supplied)

- WhatsApp links point at `https://wa.me/44XXXXXXXXXX` (hero and contact card)
- Email shows `info@PLACEHOLDER.co.uk` (contact row and the quote mailto button)
- The third review card is an intentional empty slot for the next real review

Update these in `public/index.html` when the real details are confirmed.
