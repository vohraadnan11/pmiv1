# Farm & Flock Equipment — Temporary Catalog Site

This is a lightweight static site: one HTML file, no backend, no database.
Products are pulled live from a **Google Sheet** (published as CSV), and
product images live in a **GitHub folder**. Editing the sheet updates the
site automatically — no re-upload needed.

---

## 1. Set up the Google Sheet (your product data)

1. Create a new Google Sheet.
2. In row 1, add these **exact** column headers (case-sensitive):

   | Name | Category | Brand | Price | Image | Description |
   |------|----------|-------|-------|-------|--------------|

3. Fill in one row per product. Notes on each column:
   - **Name** — required. Row is skipped if this is blank.
   - **Category** — used to build the category grid and filter buttons (e.g. `Feeders`, `Drinkers`, `Incubators`, `Cages`, `Brooders`).
   - **Brand** — your company name, or the partner company's name.
   - **Price** — plain number, e.g. `450`. Leave blank to show "Price on request".
   - **Image** — just the filename, e.g. `nipple-drinker.jpg` (see step 2 for where this file actually lives).
   - **Description** — currently stored but not displayed on the card; kept for your own reference/future use.

4. Publish the sheet as CSV:
   - **File → Share → Publish to web**
   - Under "Link", choose the specific sheet/tab with your products
   - Under the format dropdown, choose **Comma-separated values (.csv)**
   - Click **Publish**, copy the link it gives you

5. Open `index.html`, find this line near the bottom (`<script>` section):
   ```js
   const SHEET_CSV_URL = "PASTE_YOUR_PUBLISHED_GOOGLE_SHEET_CSV_LINK_HERE";
   ```
   Replace the placeholder with the link you copied. Save.

**That's it for future updates** — anytime you edit the sheet (add a row, change a price), the live site reflects it on next page load. No redeploy needed.

⚠️ Note: "Publish to web" makes that sheet's data viewable by anyone with the
link (read-only, they can't edit it). Don't put anything sensitive in that
particular tab.

---

## 2. Add product photos via GitHub

1. In your GitHub repo, create a folder called `images` (already included here with one placeholder file).
2. Upload your product photos into that folder directly from GitHub's web UI (Add file → Upload files) — no coding needed.
3. In your Google Sheet's **Image** column, just type the filename you uploaded, e.g.:
   ```
   nipple-drinker.jpg
   ```
   The site automatically looks for it inside the `images/` folder next to `index.html`.

**Image tips:**
- Resize/compress photos before uploading (aim under 200 KB each) — free tools: [squoosh.app](https://squoosh.app) or TinyPNG.
- Keep filenames simple: lowercase, no spaces (`pan-feeder.jpg`, not `Pan Feeder 1.jpg`).
- If a row has no Image value, or the file isn't found, the site shows a "Photo Coming Soon" placeholder automatically — nothing breaks.

---

## 3. Set up the inquiry form so you actually receive submissions

Right now the form falls back to opening the visitor's email app (`mailto:`), which is unreliable (many people don't have an email app configured on their device/browser).

**Recommended fix — Web3Forms (free, no backend, no signup fee):**

1. Go to [web3forms.com](https://web3forms.com), enter your email, get a free **Access Key** (no account/password needed).
2. In `index.html`, find:
   ```js
   const WEB3FORMS_KEY = "YOUR_WEB3FORMS_ACCESS_KEY";
   ```
   Replace with your real key.
3. Done — every inquiry (with Name, Mobile, Email, Product, Message) now emails straight to your inbox. Name and Mobile are required fields; the form won't submit without them.

---

## 4. Deploying via GitHub

1. Create a GitHub repo (public is fine for this, or private if your GitHub plan supports Pages on private repos).
2. Upload `index.html`, `products.json` (kept only as an offline fallback if the sheet ever fails to load), and the `images/` folder.
3. Go to **Settings → Pages** in your repo → set source to your main branch → save.
4. GitHub gives you a live URL like:
   ```
   https://YOUR_USERNAME.github.io/YOUR_REPO/
   ```

---

## 5. Embedding in Google Sites

Google Sites can't run your HTML/JS directly as a native page — the reliable way is to **embed the GitHub Pages link**:

1. In Google Sites, add an **Embed** block.
2. Choose "Embed URL" and paste your GitHub Pages link from step 4.
3. Google Sites will show it in an iframe. Resize the embed box as needed.

Alternatively, you can just share the GitHub Pages link directly as your website URL — it will look and behave identically to a normal website, and Google Sites becomes unnecessary at that point.

---

## What's temporary about this build, honestly

This setup is genuinely fine to run on for a while — but a few limitations to know about:
- No admin dashboard — you edit the sheet or GitHub folder directly.
- No true search/pagination if your catalog grows very large (1,000 products still renders fine, just no pagination yet).
- No price history or stock tracking — it just reflects whatever the sheet says right now.
- Google's "Publish to web" CSV updates can take a minute or two to propagate after you edit the sheet — refresh the site after a short wait if you don't see changes instantly.

When/if you move to the full WordPress setup discussed earlier, this sheet's data can be exported and re-imported there directly (Excel/CSV import is standard for WordPress product plugins).
