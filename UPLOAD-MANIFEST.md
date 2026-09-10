# Holoflex — Self-Adhesive Labels landing page: upload manifest

Deploys to **https://www.holoflex.com/self_adhesive_labels/**
Server folder: `/home/holoflex/public_html/self_adhesive_labels/`

Content swap of the approved Garment Tags build. Same design, layout, CSS,
JS, PHP endpoint and Apps Script integration. Built 2026-09-08. Not deployed.

## 1. Upload these (and nothing else) to `/self_adhesive_labels/`

| Path | Purpose |
|---|---|
| `index.html` | Landing page |
| `thank-you-lp.html` | Post-submit confirmation page (fires the conversion once) |
| `submit-enquiry.php` | Enquiry endpoint: CSV → Apps Script webhook → mail() |
| `css/self-adhesive-labels-lp.css` | Standalone stylesheet (renamed from `garment-tags-lp.css`) |
| `js/self-adhesive-labels-lp.js` | Page script (renamed from `garment-tags-lp.js`) |
| `fonts/lp-poppins-400.woff2` | Self-hosted font |
| `fonts/lp-poppins-500.woff2` | Self-hosted font |
| `fonts/lp-poppins-600.woff2` | Self-hosted font |
| `fonts/lp-poppins-700.woff2` | Self-hosted font |
| `fonts/lp-lora-700.woff2` | Self-hosted font |
| `images/lp-holoflex-logo-64.png` / `.webp` | Header logo 1x |
| `images/lp-holoflex-logo-128.png` / `.webp` | Header logo 2x |
| `images/lp-holoflex-logo-footer-80.jpg` / `.webp` | Footer logo 1x |
| `images/lp-holoflex-logo-footer-160.jpg` / `.webp` | Footer logo 2x |
| `images/lp-label-materials-460.jpg` / `.webp` | Core block product photo 1x (460×259) |
| `images/lp-label-materials-920.jpg` / `.webp` | Core block product photo 2x (920×518) |

Total: 23 files. Keep the folder structure exactly as above — every path in
the HTML, CSS and JS is relative to `/self_adhesive_labels/`.

**Do NOT upload:** `_source/` (Vercel preview copy) or this manifest.

## 2. What changed in `submit-enquiry.php` vs Garment Tags

| Constant | Value |
|---|---|
| `LP_PAGE_ID` | `self-adhesive-labels` |
| `LP_CSV_FILE` | `self-adhesive-labels-enquiries.csv` |
| `LP_PAGE_URL` | `https://www.holoflex.com/self_adhesive_labels/` |
| `LP_SUBJECT` | `New self-adhesive label enquiry` |
| `$allowedInterests` | Five label options matching the two `<select>`s in `index.html` |

Unchanged, exactly as Garment Tags: `LP_DATA_DIR` (`/home/holoflex/lp-data`),
`LP_WEBHOOK_URL`, `LP_WEBHOOK_SECRET`, `LP_RATE_MAX` / `LP_RATE_WINDOW`,
`LP_MIN_ELAPSED_MS`, `LP_TIMEZONE`, recipients and From address.

Same Apps Script deployment, same Google Sheet. The `Page ID` column (B)
separates these leads from the Garment Tags leads; `Code.gs` already lists
`self-adhesive-labels` in `PAGE_NAMES`, so nothing needs redeploying there.
The CSV lands as a **new file** in `/home/holoflex/lp-data/` on the first lead;
the shared `rate-limit.json` and `lp-errors.log` are reused.

## 2a. Why Holoflex — the only wording that differs from Garment Tags

The eight card **titles** are live Google Ads callouts and are verbatim on
every landing page. Three card **descriptions** named tags or apparel and were
reworded, one noun each. The Holograms page should follow this same pattern
(swap the product noun only) rather than introducing new wording.

| Card | Garment Tags description | Self-Adhesive Labels description |
|---|---|---|
| Direct From Manufacturer | Tags, ribbon and security features made in-house — no reseller margin. | Labels, substrates and security features made in-house — no reseller margin. |
| Trusted by Top Brands | Apparel and consumer brands across India. | Pharma, FMCG and consumer brands across India. |
| Free Samples & Quote | Handle the tag before you commit. Pricing within 24 hours. | Handle the label before you commit. Pricing within 24 hours. |

The other five descriptions (35+ Years' Experience, Custom Designs, Bulk
Orders Welcome, Made in India, Fast Turnaround) are unchanged.

## 3. Post-upload checks

1. Open `https://www.holoflex.com/self_adhesive_labels/` — fonts, logos and
   the placeholder tiles render; the header nav anchors (`#materials`,
   `#security`, `#applications`, `#why-holoflex`, `#faq`, `#enquiry-footer`)
   all scroll.
2. Submit a test enquiry from the hero form and one from the footer form.
   - Redirects to `thank-you-lp.html`; the `enquiry_form_submit` dataLayer
     event carries `page_id: "self-adhesive-labels"`.
   - A row appears in the Sheet with Page ID `self-adhesive-labels`.
   - The notification email arrives with subject
     `New self-adhesive label enquiry — <company> (hero form)`.
   - `/home/holoflex/lp-data/self-adhesive-labels-enquiries.csv` exists.
3. Append `?gclid=test&utm_source=google&utm_campaign=sal-test` to the URL,
   submit, and confirm the campaign columns populate.

## 4. Photography still to come (placeholders in place)

The core block product photo is in place (added 2026-09-10, kept at the
photo's native 16:9, 460×259 / 920×518; original in `_source/photos/2nd.jpeg`).
The remaining slots ship with neutral `.lp-ph` placeholder tiles. Each slot has the
final `<picture>` markup commented in beside it, with the file names below.
Add the files to `images/`, delete the placeholder `<div>`, uncomment the
`<picture>` and re-upload `index.html`.

| Slot | Files (JPG + WebP each) | Size |
|---|---|---|
| Security block | `lp-security-600`, `-1200` | 600×450, 1200×900 |
| Gallery: Pharmaceutical | `lp-app-pharma-400`, `-800` | 400×267, 800×534 |
| Gallery: FMCG | `lp-app-fmcg-400`, `-800` | 400×267, 800×534 |
| Gallery: Logistics & Warehousing | `lp-app-logistics-400`, `-800` | 400×267, 800×534 |
| Gallery: Agro & Chemicals | `lp-app-agro-chemicals-400`, `-800` | 400×267, 800×534 |
| Gallery: Lubricants & Automotive | `lp-app-lubricants-automotive-400`, `-800` | 400×267, 800×534 |
| Gallery: Building Materials | `lp-app-building-materials-400`, `-800` | 400×267, 800×534 |
| Hero background (optional) | `lp-hero-labels-1600.jpg` via `.lp-hero__bg` in the CSS | ≤200 KB |

## 5. Vercel preview copy

`_source/vercel-preview/` is the review build, same pattern as Garment Tags:
root-absolute paths, Google Tag Manager removed, `[PREVIEW]` in the titles,
`noindex, nofollow` plus an `X-Robots-Tag` header from `vercel.json`, and the
forms validate but do not submit (they show a notice instead). It contains no
PHP. Push that folder to its own GitHub repo and import it into Vercel as a
static project, as was done for `holoflex-garment-tag-lp`. Not deployed yet.
