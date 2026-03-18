# Where to upload images for Stroctus site

Upload image files into the **`images/` folder in this GitHub repository**.

No, you do not need to upload images in this chat.

## Required filenames (recommended)

- `images/stroctus-records.jpg`
- `images/rebel-faith.jpg`
- `images/gnx75.jpg`
- `images/juan-raul-rosero.jpg`

The page also tries common variants (`.jpeg`, `.png`, `.webp`), but the names above are best.

## GitHub web UI steps

1. Open your repo on GitHub.
2. Open the `images/` folder.
3. Click **Add file** → **Upload files**.
4. Upload your 4 image files.
5. Make sure names match the list above.
6. Click **Commit changes** to `main` (or your deploy branch).

## After upload

1. Go to Cloudflare Pages.
2. Trigger **Retry deployment** (or wait for auto-deploy).
3. Hard refresh the site (`Ctrl+Shift+R` / `Cmd+Shift+R`).

If placeholders still show, the image files are either not committed or names don't match.


## If you already uploaded different names

Your current uploaded files (from your screenshot) are:

- `images/THE-REBEL-FAITH.JPG`
- `images/rebel_faith_3000x3000.jpeg`
- `images/the-rebel-faith.jpg`
- `images/JuanRaulRosero.jpg`

The page now tries these exact names too.

Missing files still needed:
- `images/gnx75.jpg` (or `.jpeg/.png`)
- `images/stroctus-records.jpg` (hero image)



Your Cloudflare deploy is configured to publish repo-root assets, so `images/` is included.


## If you are still seeing placeholders with your pasted HTML

Your pasted HTML uses fixed single paths like:

- `images/main-image.jpg`
- `images/the-rebel-faith.jpg`

That older version does not try alternate filenames/case variants.
This repository's current `index.html` already includes `data-src-candidates` + JS fallback cycling.

So make sure Cloudflare is deploying the latest commit from this repo branch, not an older cached build.
