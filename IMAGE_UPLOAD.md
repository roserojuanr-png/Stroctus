# Where to upload images for Stroctus site

Upload image files into the **`images/` folder in this GitHub repository**.

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
