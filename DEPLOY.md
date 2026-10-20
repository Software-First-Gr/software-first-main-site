# Deploying softwarefirst.gr on Cloudflare Pages

This is a **static site** — no build step. Cloudflare Pages serves the repo root as-is.

## How a change goes live

The Pages project `software-first-main-site` is connected to the GitHub repo
`Software-First-Gr/software-first-main-site`, and its **production branch is `production`**, not `main`.

1. Commit to `main` and push. Cloudflare builds a **preview** of every push to `main` (and of any other
   branch), at `https://<id>.software-first-main-site.pages.dev`. Check the change there.
2. To go live, fast-forward `production` to `main`:

   ```sh
   git push origin main:production
   ```

   Cloudflare deploys it to softwarefirst.gr, www.softwarefirst.gr and software-first-main-site.pages.dev
   in about 30 seconds.

To see what's live, run `npx wrangler pages deployment list --project-name software-first-main-site
--environment production` (needs `npx wrangler login`).

## Cloudflare Pages project settings

| Setting                | Value            |
| ---------------------- | ---------------- |
| Production branch      | `production`     |
| Framework preset       | **None**         |
| Build command          | *(leave empty)*  |
| Build output directory | `/` (repo root)  |
| Root directory         | *(leave empty)*  |

## Domain

`softwarefirst.gr` and `www.softwarefirst.gr` are custom domains on the Pages project, and the zone is on
Cloudflare. The move from GitHub Pages is done: the old `CNAME` file is gone, HTTP redirects to HTTPS,
and HSTS is on.

## Notes

- `_headers` sets conservative security headers. No CSP is set on purpose (the site
  uses inline + vendor JS). Cloudflare Pages handles caching/etags automatically.
- Assets (`js/main.js`, `css/main-LTR.css`, …) are not content-hashed; Pages busts the
  edge cache per deployment, so no manual purge is needed on updates.
