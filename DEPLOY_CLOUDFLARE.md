# Deploy Stroctus page to `stroctus.signalonly.net` (Cloudflare)

This repo is a static site (`index.html`), so **Cloudflare Pages** is the easiest option.

---

## Your current error and the fix

If Cloudflare shows:

- `Failed: error occurred while running deploy command`
- Wrangler logs under `/opt/buildhome/.config/.wrangler/logs/...`

then your Pages project likely has a **Deploy command** configured (often a Wrangler command) that is failing.

### Fix this immediately in Cloudflare Pages settings

In **Build & deployments** for your Pages project:

- **Framework preset:** `None`
- **Build command:** *(empty)*
- **Deploy command:** *(empty)*
- **Build output directory:** `.`

> For this static site, you do **not** need `wrangler pages deploy` inside Cloudflare Pages Git builds.

Then click **Save** and **Retry deployment**.

---

## Option A (Recommended): Cloudflare Pages + GitHub (no build)

### 1) Push this repo to GitHub
If not already on GitHub, create a GitHub repo and push this code.

### 2) Create a Pages project
1. Open Cloudflare Dashboard.
2. Go to **Workers & Pages** → **Create application** → **Pages** → **Connect to Git**.
3. Select your GitHub repo.
4. Build settings:
   - **Framework preset:** None
   - **Build command:** *(leave empty)*
   - **Deploy command:** *(leave empty)*
   - **Build output directory:** `.` (dot = repository root)
5. Click **Save and Deploy**.

> Important: use `.` (dot), **not** `/`. Using `/` can trigger:  
> `Could not detect a directory containing static files...`

Because this is static HTML, no build is required.

### 3) Add custom domain
1. In the new Pages project, open **Custom domains**.
2. Click **Set up a custom domain**.
3. Enter `stroctus.signalonly.net`.
4. Cloudflare will add/verify the required DNS record automatically if `signalonly.net` is in the same account.

### 4) SSL and verification
- Cloudflare will issue SSL certs automatically.
- Once status is Active, visit:
  - `https://stroctus.signalonly.net`

---

## Option B (Fallback): Deploy with an explicit `dist/` output

Use this if Cloudflare keeps complaining about static directory detection.

### 1) Use this build command

```bash
rm -rf dist && mkdir -p dist/images && cp index.html dist/ && cp -R images/* dist/images/
```

### 2) Pages build settings
- **Framework preset:** None
- **Build command:** (the command above)
- **Deploy command:** *(empty)*
- **Build output directory:** `dist`

This removes ambiguity about where static files are located.

---

## Option C: Direct Upload (no Git integration)

1. In **Workers & Pages**, create a **Pages** project using **Direct Upload**.
2. Upload `index.html` and the `images/` folder.
3. Add custom domain `stroctus.signalonly.net`.

---

## Required files checklist
Before deploying, ensure these files exist in the repo root:

- `index.html`
- `images/stroctus-records.jpg`
- `images/rebel-faith.jpg`
- `images/gnx75.jpg`
- `images/juan-raul-rosero.jpg`

If image files are missing, the page will show fallback placeholders.


## If the same Wrangler assets error still appears

Your log snippet indicates Cloudflare is still running a Wrangler deploy command in CI.

This repository now includes `wrangler.jsonc` with:

```json
{
  "assets": { "directory": "./dist" }
}
```

So if a deploy command runs `wrangler ...` without `--assets`, Wrangler can still find your static files.

### What to do in Cloudflare now

1. **Preferred:** remove Deploy command entirely (leave it empty).
2. If you cannot remove it, keep the command but ensure the repo contains `wrangler.jsonc` (already added) and redeploy.
3. Optional explicit command form: `npx wrangler versions upload --assets=.`

---


## Image upload location (most important)

Upload your real photos to the repository path **`images/`** in GitHub.

Recommended file names:

- `images/stroctus-records.jpg`
- `images/rebel-faith.jpg`
- `images/gnx75.jpg`
- `images/juan-raul-rosero.jpg`

See `IMAGE_UPLOAD.md` for click-by-click upload steps.

---

## Troubleshooting

- **Error: failed while running deploy command**
  - Remove any Deploy command (leave it blank).
  - Do not run Wrangler deploy commands in Pages Git builds for this project.
- **Error: could not detect static files directory**
  - Set output dir to `.` for no-build static deploy.
  - Or use fallback build command and output dir `dist` (only if you also copy images into `dist/images`).
- **Domain not active yet:** wait a few minutes for DNS/SSL provisioning.
- **Old content shown:** Pages project → **Retry deployment**.
- **Images not loading:** confirm exact file names and paths under `images/`, and make sure image files are committed to GitHub (not only present locally).
- **Images still not loading after upload:**
  - Confirm Cloudflare Pages is deploying the same branch where you uploaded files.
  - Retry deployment and hard refresh browser cache.
  - Filenames are case-sensitive on deploy (`THE-REBEL-FAITH.JPG` != `the-rebel-faith.jpg`).



## Hard fix for your exact current command

Your log shows Cloudflare is executing:

`npx wrangler versions upload`

This repo now includes both `wrangler.toml` and `wrangler.jsonc` pointing assets to `.` (repo root), so files in `images/` are deployable directly.
That means this exact command can succeed without extra flags.

If your project still fails, update Deploy command to:

`npx wrangler versions upload --assets=.`

Then retry deploy.



If you upload images to `images/` in GitHub, they will be deployed with this root-assets setup.
