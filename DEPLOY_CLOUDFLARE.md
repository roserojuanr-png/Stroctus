# Deploy Stroctus page to `stroctus.signalonly.net` (Cloudflare)

This repo is a static site (`index.html`), so **Cloudflare Pages** is the easiest option.

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

### 1) Add a simple build script
Create a build command that copies site files to `dist`:

```bash
rm -rf dist && mkdir -p dist/images && cp index.html dist/ && cp -R images/* dist/images/
```

### 2) Pages build settings
- **Framework preset:** None
- **Build command:** (the command above)
- **Build output directory:** `dist`

This removes any ambiguity for Cloudflare about where static files are located.

---

## Option C: Direct Upload (no Git integration)

1. In **Workers & Pages**, create a **Pages** project using **Direct Upload**.
2. Upload `index.html` and the `images/` folder.
3. After deploy, add the custom domain `stroctus.signalonly.net` as above.

---

## Required files checklist
Before deploying, ensure these files exist in the repo root:

- `index.html`
- `images/main-image.jpg`
- `images/the-rebel-faith.jpg`
- `images/gnx75.jpg`
- `images/juan-raul-rosero.jpg`

If image files are missing, the page will show fallback placeholders.

---

## Troubleshooting

- **Error: could not detect static files directory**
  - Set output dir to `.` (dot) for no-build static deploy.
  - Or use the fallback build command and set output dir to `dist`.
- **Domain not active yet:** wait a few minutes for DNS/SSL provisioning.
- **Old content shown:** use Cloudflare dashboard → Pages project → **Retry deployment**.
- **Images not loading:** confirm exact file names and paths under `images/`.

