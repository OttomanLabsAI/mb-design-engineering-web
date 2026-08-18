# CLAUDE.md

Standing policy for this repository. Read it before making any change here.

## What this repo is

A Cloudflare Workers static-assets site for MB Design Solutions, structural
engineers in Birmingham. Everything served lives in `public/` and there is no
build step - the files in that directory are the site. The repo is connected to
Cloudflare Workers Builds, so **every push to `main` deploys to production**.

```
public/            everything served
  index.html       the site (fenced PITCH-CHROME blocks inside)
  offer.html       pitch tab 2 - the offer, served at /offer
  404.html
  assets/css/      main.v3.css (the site — bump the name when it changes,
                   assets cache immutable) + offer.css/pitch.css (pitch only)
  _headers         security + caching headers
  robots.txt
wrangler.jsonc     assets-only config, no Worker script
package.json       wrangler devDependency + dev/deploy scripts
```

The repo is currently in its **pitch phase**: a two-tab sales pitch is wrapped
around the site (tab bar chrome on `/` and `/offer`). The README's "Removing
the pitch" section lists exactly what to delete when the client says yes - keep
it accurate if the pitch files move.

## Local development

```bash
npm install
npm run dev          # wrangler dev
```

## Verification - before every push to main

1. `npx wrangler deploy --dry-run`
2. Serve `public/`, render it with headless Chromium, and inspect the
   screenshots: styles applied, fonts loaded, layout intact - on `/`, `/offer`
   and the 404 page, at phone and desktop widths.

Never leave pushed work unverified or half-finished. Work in small, complete
batches: implement, verify, commit, push.

## Git and release workflow

- Before committing: `git config user.name "Fid" && git config user.email "fid_kk@proton.me"`
- Develop on the working branch and push there first. Release verified work by
  fast-forwarding `main` onto it and pushing `main`.
- Every push to `main` is a release. Versions are an ascending `vMAJOR.MINOR`
  sequence starting at `v1.0`; every push bumps the minor regardless of size. A
  major bump is reserved for a ground-up overhaul.
- With every push to `main`, provide release-tag text in the reply, in exactly
  this shape. The owner creates the GitHub release manually - **never push tags**:

  ```
  Tag: v<next>  —  Title: <five to nine words, plain and evocative>
  Description: <one to three sentences of editorial prose describing what changed
  from the owner's point of view — outcomes, not implementation. No bullet lists,
  no jargon, no file names.>
  ```

- Append the release line to the ledger below as part of the same push.
- Commit messages: descriptive imperative first line (what the change does, not
  "update X"), then a short prose body; dash bullets are fine there. One commit
  per coherent piece of work; several may share a push, but each push gets
  exactly one version entry.
- Never include model names, AI attribution trailers, session links, or other
  tooling identifiers in commit messages, titles, or code.

## The page itself

Content, design, and behaviour are as supplied by the owner. Do not tidy markup,
rename classes, rewrite copy, or modernise CSS unless asked - changes to the
design are their own release, requested deliberately. The offer page reuses the
site's own stylesheet and component vocabulary on purpose; keep it that way.

## Release ledger

| Version | Title | Description |
| --- | --- | --- |
| v1.0 | The site, the pitch, and the price | The MB Design Solutions site stands in full at the root — drawing-sheet design, services, process and reviews — with a second tab that makes the case for having it and puts a price on saying yes. Verified at phone and desktop widths, ready for Cloudflare to deploy on every push. |
| v1.1 | The site always arrives properly dressed | Browsers that saw the event pages briefly occupying this address could keep wearing that stylesheet over the new site for up to a year. The site's own look now arrives under a fresh name that every visitor fetches clean, and the pages render correctly even opened straight from the files. |
| v1.2 | The title block now takes you places | The drawing-sheet footer keeps its look but works like a footer should: every panel is a live link — services, process, about and reviews above; the office on the map, the phone, WhatsApp and a quote email below. Phones also stop underlining the company number as if it were one. |
| v1.3 | The quote button holds its shape | On narrow screens the quote button in the header could split across two lines and spill out of its orange box. It now stays on one line and keeps its shape at every width. |
