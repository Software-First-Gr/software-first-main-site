# Deploying softwarefirst.gr on Cloudflare Pages

This is a **static site** — no build step. Cloudflare Pages serves the repo root as-is.

## Cloudflare Pages project settings

When creating the project (Cloudflare dashboard → **Workers & Pages → Create → Pages → Connect to Git**),
connect the GitHub repo `Software-First-Gr/software-first-main-site` and use:

| Setting                | Value            |
| ---------------------- | ---------------- |
| Production branch      | `main`           |
| Framework preset       | **None**         |
| Build command          | *(leave empty)*  |
| Build output directory | `/` (repo root)  |
| Root directory         | *(leave empty)*  |

That's it — every push to `main` redeploys; other branches/PRs get preview URLs.

## Custom domain + DNS cutover

1. In the Pages project → **Custom domains → Set up a domain** → `softwarefirst.gr`
   (add `www.softwarefirst.gr` too if you use it).
2. Point DNS at Cloudflare. Either move the domain's nameservers to Cloudflare
   (full zone — recommended), or add the CNAME/records Cloudflare tells you to.
3. After HTTPS is confirmed working on the live domain, enable **Always Use HTTPS**
   and **HSTS** in the Cloudflare zone (SSL/TLS settings), and delete the `CNAME`
   file below.

## Notes

- `CNAME` (containing `softwarefirst.gr`) is a **GitHub Pages** artifact. It's harmless
  on Cloudflare Pages (served as an inert `/CNAME` file). Keep it until the DNS cutover
  is done so GitHub Pages stays a working fallback, then remove it.
- `_headers` sets conservative security headers. No CSP is set on purpose (the site
  uses inline + vendor JS). Cloudflare Pages handles caching/etags automatically.
- Assets (`js/main.js`, `css/main-LTR.css`, …) are not content-hashed; Pages busts the
  edge cache per deployment, so no manual purge is needed on updates.
