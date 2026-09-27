# Bernillo's Beachside Pizza

Single-page website for Bernillo's Beachside Pizza, an old-school pizza parlor in downtown Fort Bragg, CA. Static HTML and CSS, no build step, no JavaScript.

## Status

The site is content-complete except for the items under [Still to fill in](#still-to-fill-in). Everything below is live in `index.html`:

- **Hero** — heading, two intro paragraphs, a six-item checklist (dine in, takeout, etc.), and three pizza photos with name badges. Text sits left and photos right on wide screens; photos drop below the text on phones.
- **Photo lightbox** — clicking a hero photo opens a full-screen view. Swipe or use the arrow buttons to move between photos, tap outside or the × to close. Built with `:target`, `:has()`, and CSS scroll-snap; no script.
- **Pizzas** — price table by size, crust and sauce options, extra toppings list, ranch dressing, beer note, and an Ordering line with a tap-to-call number.
- **Hours & Location** — hours table, address, phone, a Get directions button, and an embedded Google Map.
- **About** — history placeholder and Facebook/Instagram links with inline SVG icons.
- **SEO** — title, meta description, Open Graph tags, `Restaurant` JSON-LD, pizza-emoji favicon, `robots.txt`, and `sitemap.xml`.
- **Copyright year** — a GitHub Action updates the footer year every January 1st.

### Still to fill in

- **History** (`#about`) — currently "History coming soon."
- **Social links** (`#about`) — point at facebook.com and instagram.com homepages; replace with the real page URLs and add them to the JSON-LD as `sameAs`.
- **Site URL** — `robots.txt` and `sitemap.xml` assume GitHub Pages at `https://sjproctor.github.io/bernillos/`. Change both if the site moves to a custom domain, and add a `<link rel="canonical">`, `og:url`, and `og:image` to the `<head>` at that point.
- **Pizza names on photos** — the badges say Classic Combo, Veggie Delight, and Meatzilla based on what's visible; correct them in the `figcaption` elements if wrong.

## Files

| Path | Purpose |
|---|---|
| `index.html` | All page content, meta tags, JSON-LD schema, lightbox markup |
| `styles.css` | Mobile-first styles; checkered bands and placeholders are CSS gradients |
| `images/hero-*.jpg` | Web-sized photos used on the page (1200–1600px, ~0.4–0.7MB each) |
| `images/IMG_*.jpeg` | Original uploads (~2.5MB each); not referenced by the page |
| `images/favicon.png`, `images/apple-touch-icon.png` | Pizza emoji icons; an inline SVG version is also in the `<head>` |
| `robots.txt`, `sitemap.xml` | Crawler files; contain the site URL |
| `.github/workflows/update-copyright-year.yml` | Yearly footer-year update |

## Dependencies

None to install. The page loads one third-party resource: the Google Maps iframe in `#hours-location`. It is lazy-loaded and the rest of the page works if it fails. Everything else (fonts, icons, patterns) is system or inline.

## Local preview

Open `index.html` directly in a browser, or serve it locally:

```
python3 -m http.server
```

The lightbox uses URL fragments (`#photo-1`, etc.), so it works from a plain file URL too.

## Deployment

Any static host works. Upload the whole repo except `.git` and `.github`, or point GitHub Pages at the `main` branch root. The `images/` folder, `robots.txt`, and `sitemap.xml` must ship alongside `index.html` and `styles.css`.

### Copyright year workflow

`update-copyright-year.yml` runs at 06:15 UTC on January 1st, rewrites the year after `&copy;` in the footer, and commits as `github-actions[bot]`. It can also be run by hand from the Actions tab. Two caveats:

- GitHub disables scheduled workflows in public repos after 60 days with no pushes. If that happens, re-enable it from the Actions tab or push any commit.
- If branch protection later requires pull requests on `main`, the bot's direct push will fail.

## Editing content

### Hours, address, phone

These appear in three places and must stay in sync: the JSON-LD block in `<head>`, the `#hours-location` section, and the footer. The address is `220 E Redwood Ave` everywhere. The Google Maps embed and Get directions link search for the Google listing name ("Bernillo's Pizza"), so leave those queries alone unless the listing is renamed.

### Menu

Prices are in the `.menu-table` in `#menu`. Crust/sauce and topping prices are the `.menu-note` lines beneath the Pizzas and Extra Toppings headings, separated by `<span class="dot">` bullets. Toppings are a plain `<ul class="toppings-list">`.

### Photos

Each hero photo is a `<figure class="photo-tile">` containing a link, an `<img>`, and a `figcaption` badge. The first photo takes the large landscape slot on wide screens, so it should be the widest shot. The lightbox slides at the end of `#hero` mirror the same three images and captions; update both when swapping a photo. Resize new photos to about 1600px on the long side before adding them.

### SEO tags

All SEO tags live in the `<head>`:

- **Title and meta description** — keep "Fort Bragg" and "Pizza" in the title; keep the description under about 160 characters.
- **Open Graph** — `og:title` and `og:description` should match the title and description. Add `og:image` and `og:url` once the site has a fixed URL.
- **JSON-LD** — the `Restaurant` object holds name, address, phone, and hours. Update it whenever those change. Good next additions: `url`, `image`, `priceRange`, `geo`, `hasMap`, `sameAs`, and a `hasMenu` block. Validate with [Google's Rich Results Test](https://search.google.com/test/rich-results).

Off-page, the biggest lever is a claimed Google Business Profile whose name, address, and phone match the site exactly.

## Reference content

Source notes the site was built from. The page is the canonical version; this is kept for context.

### Hours

- Monday–Wednesday: closed
- Thursday: 6–8pm
- Friday–Sunday: 3:30–8pm

### Menu

"Just the Classics." Sizes 12", 14", 16". Crust: thin or regular. Sauce: red or pesto.

| Pizza | 12" | 14" | 16" |
|---|---|---|---|
| Cheese | $25 | $31 | $36 |
| Classic Combo | $32 | $39 | $43 |
| Hawaiian | $27 | $33 | $38 |
| Meatzilla | $32 | $39 | $45 |
| Pepperoni | $27 | $35 | $38 |
| Veggie Delight | $32 | $38 | $45 |

Extra toppings $2 / $3 / $4 by size: anchovies, extra cheese, ham, linguica, pepperoni, sausage, bell pepper, garlic, onion, jalapeños, mushrooms, olives, pepperoncini, pineapple, spinach. Ranch dressing $1.25 for 2oz, $4 for 8oz. Six rotating draft beers.
