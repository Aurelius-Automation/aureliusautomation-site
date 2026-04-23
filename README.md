# aurelius-site

Single-page landing site for [aureliusautomation.io](https://aureliusautomation.io).

Pure static files. No build step, no framework, no package manager. Tailwind loads from its CDN at runtime; Inter loads from Google Fonts.

## Files

- `index.html`. The whole page. Every section, every style, every script inlined.
- `favicon.svg`. 32×32 flow mark.
- `README.md`. This file.

## Preview locally

Open the file directly, or serve it from any static server:

```bash
# any of these work
open index.html
python3 -m http.server 8000
npx --yes serve .
```

## Deploying

Three options, pick whichever fits your workflow. All three serve the repo root as a static site. Nothing to configure beyond pointing the host at this directory.

### Railway

1. Create a new project in Railway, connect this repo.
2. On the service, set the **start command** to a static file server, e.g.
   ```
   npx serve . -l $PORT
   ```
   (or add a `Dockerfile`/`railway.json` if you prefer.)
3. Set a custom domain on the service: `aureliusautomation.io`. In Namecheap DNS, flatten `aureliusautomation.io` to the Railway edge hostname Railway provides.

### Vercel

1. `npm i -g vercel` (once).
2. In this directory, run `vercel --prod`.
3. When prompted, accept the defaults. Vercel auto-detects a static project.
4. In the Vercel dashboard, add `aureliusautomation.io` under Domains and follow the CNAME instructions.

Alternative: drag the folder onto `vercel.com/new` in the browser.

### Cloudflare Pages

1. `npm i -g wrangler` (once), then `wrangler login`.
2. In this directory, run
   ```
   wrangler pages deploy . --project-name aurelius-site
   ```
3. Add `aureliusautomation.io` as a custom domain under Pages → Custom domains. Cloudflare will manage the DNS if your domain is already on Cloudflare.

Alternative: drag the folder onto `dash.cloudflare.com` → Pages → Create → Upload assets.

## Domain setup

Whichever host you pick, the DNS pattern is the same:
- Root `@` → CNAME-flatten to the host's edge hostname (Namecheap calls this "ALIAS" or "CNAME record at the root").
- `www` → CNAME to the same edge hostname, or a 301 redirect to root.

## Wiring up the contact form

The form POSTs to `/api/contact` and shows the success state regardless of response. Until a real endpoint is in place, submissions go nowhere.

Cheapest options in order of least lift:

1. **Formspree / FormSubmit.** Change the form's `action` to `https://formspree.io/f/<id>` (or `https://formsubmit.co/hello@aureliusautomation.io`) and remove the fetch call.
2. **Resend plus serverless function.** On Vercel or Cloudflare Pages, add an `api/contact` function that validates the body and calls Resend. Replace the `TODO` comment in `index.html` with the real endpoint URL.
3. **Railway-hosted Node endpoint.** If the site is on Railway, add a second service with a small Express handler.

## Making changes

Edit `index.html` directly. No build, no deps. Push the change and redeploy.

- Colors live in two places: the `tailwind.config` block in `<head>` (named tokens like `charcoal`, `warm-white`, etc.) and the small `<style>` block below it (for things Tailwind doesn't cover cleanly, like card hover states and section title underlines).
- Copy rule: no em dashes anywhere on the page. Use periods, commas, colons, or sentence breaks.
