# Bernillo's Beachside Pizza

Single-page website for Bernillo's Beachside Pizza, an old-school pizza parlor in downtown Fort Bragg, CA. Static HTML and CSS, no build step. The only JavaScript is a short inline script that adds keyboard support to the photo lightboxes.

## Status

The site is content-complete. Everything below is live in `index.html`:

- **Hero** — heading, intro paragraphs, an eight-item checklist (dine in, takeout, etc.), and three pizza photos with name badges plus a hint that they open a gallery. Text sits left and photos right on wide screens; photos drop below the text on phones. The hero photos are served as WebP with JPEG fallback at 400, 800, and 1600px via `srcset`.
- **Photo lightboxes** — clicking a hero photo opens a full-screen gallery of all pizza photos; clicking a pizza name in the menu table opens a gallery of just that pizza. Swipe or use the arrow buttons to move between photos, tap outside or the × to close. Built with `:target`, `:has()`, and CSS scroll-snap; a `@supports not selector(:has(*))` block in `styles.css` gives browsers without `:has()` a one-photo-at-a-time overlay so the galleries still open there. An inline script at the end of `<body>` adds keyboard support: focus moves into the open lightbox, Tab stays inside it, Escape closes, the arrow keys step between photos, and focus returns to the opening link.
- **Pizzas** — price table by size (pizza names link to photo galleries, except Meatzilla which has no photo yet), crust and sauce options, extra toppings list, ranch dressing, beer note, and an Ordering line with a tap-to-call number.
- **Hours & Location** — hours table, address, phone, a Get directions button, and an embedded Google Map.
- **About** — short history and a payment note. Facebook and Instagram links with inline SVG icons sit in the nav (icons only) and the footer.
- **SEO** — title, meta description, canonical link, Open Graph tags with a preview image, `Restaurant` JSON-LD (address, hours, phone, price range, coordinates, social profiles), pizza-emoji favicon, `robots.txt`, and `sitemap.xml`.
- **Copyright year** — a GitHub Action updates the footer year every January 1st.

### Still to fill in

- **Meatzilla photo** — the only pizza in the table without a gallery link. Add a `meatzilla-1.jpg`, a lightbox like the others in `#menu`, and wrap the table cell in a `menu-photo-link`.
- **Site URL** — the site lives at `https://bernillosbeachsidepizza.com/`. That URL appears in `robots.txt`, `sitemap.xml`, and the `<head>` (`<link rel="canonical">`, `og:url`, `og:image`, and the JSON-LD `url`/`image`). Update all of them if the domain ever changes.

## Files

| Path                                                | Purpose                                                                                                                                                                                                                                            |
| --------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `index.html`                                        | All page content, meta tags, JSON-LD schema, lightbox markup, and the inline lightbox keyboard script                                                                                                                                              |
| `styles.css`                                        | Mobile-first styles; checkered bands and placeholders are CSS gradients                                                                                                                                                                            |
| `images/*.jpg`, `images/*.webp`                     | Web-sized photos named by pizza (1600px long side, ~0.4–0.8MB JPEG), each with a WebP copy that the lightboxes serve first. The three hero photos also have `-400`/`-800` JPEG and WebP variants for `srcset`. `og-image.jpg` is the 1200×630 link preview. Full-resolution originals are not kept in the repo |
| `images/favicon.png`, `images/apple-touch-icon.png` | Pizza emoji icons; an inline SVG version is also in the `<head>`                                                                                                                                                                                   |
| `robots.txt`, `sitemap.xml`                         | Crawler files; contain the site URL                                                                                                                                                                                                                |
| `.github/workflows/update-copyright-year.yml`       | Yearly footer-year update                                                                                                                                                                                                                          |
| `vercel.json`                                       | Hosting config: clean URLs, `nosniff` and `Referrer-Policy` headers, long cache lifetime for `images/`                                                                                                                                             |

## Dependencies

None to install. The page loads one third-party resource: the Google Maps iframe in `#hours-location`. It is lazy-loaded and the rest of the page works if it fails. Everything else (fonts, icons, patterns) is system or inline.

## Local preview

Open `index.html` directly in a browser, or serve it locally:

```
python3 -m http.server
```

The lightboxes use URL fragments (`#photo-1`, `#cheese-1`, etc.), so they work from a plain file URL too.

## Deployment

The site is hosted on Vercel at `https://bernillosbeachsidepizza.com/`, deployed from this repo with no build step. `vercel.json` turns on `cleanUrls` (so `/index.html` redirects to `/`), adds `X-Content-Type-Options` and `Referrer-Policy` headers to every response, and sets a one-year immutable cache header on everything under `images/`; HTML and CSS keep Vercel's default revalidate-every-time policy so content edits show up immediately. Because images are cached that long, replace a photo by adding a file with a new name and pointing the page at it, never by overwriting a file in place. When content changes, update `lastmod` in `sitemap.xml`.

Any other static host would also work: upload everything except `.git`, `.github`, and `.vscode`, and make sure the host compresses HTML and CSS.

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

Each hero photo is a `<figure class="photo-tile">` containing a link, a `<picture>` with WebP and JPEG `srcset`, and a `figcaption` badge. The first photo takes the large landscape slot on wide screens, so it should be the widest shot. The main lightbox at the end of `#hero` holds every photo; its first three slides mirror the hero tiles, so update both when swapping a tile. Per-pizza lightboxes live after the menu table in `#menu` and repeat the relevant slides, so a caption or alt change for a pizza is made in both places.

To add a photo: give it a filename that is not already in use (cached copies of an old name live for a year), resize it to 1600px on the long side (`sips -Z 1600`), make a WebP copy (`cwebp -q 80 name.jpg -o name.webp`) for its lightbox `<picture>`, and for a hero tile also make 400 and 800px JPEGs plus WebP versions (`cwebp -q 80 -resize 400 0`). Keep the `width`/`height` attributes equal to the file's real pixel size.

### SEO tags

All SEO tags live in the `<head>`:

- **Title and meta description** — keep "Fort Bragg" and "Pizza" in the title; keep the description under about 160 characters.
- **Open Graph** — `og:title` is the short name shown in text message and social previews; `og:description` should match the meta description. `og:image` points at `images/og-image.jpg`, a 1200×630 crop of the hero photo, via an absolute URL.
- **JSON-LD** — the `Restaurant` object holds name, address, phone, and hours. Update it whenever those change. It also carries `alternateName` (the Google listing's shorter name), `priceRange`, `geo`, `hasMap`, `sameAs`, payment details, and a `hasMenu` block that repeats the menu table's prices, so update `hasMenu` whenever a price changes. Validate with [Google's Rich Results Test](https://search.google.com/test/rich-results).

Off-page, the biggest lever is a claimed Google Business Profile whose name, address, and phone match the site exactly.

## Reference content

Source notes the site was built from. The page is the canonical version; this is kept for context.

### Hours

- Monday–Wednesday: closed
- Thursday: 6–8pm
- Friday–Sunday: 3:30–8pm

### Menu

"Just the Classics." Sizes 12", 14", 16". Crust: thin or regular. Sauce: red or pesto.

| Pizza          | 12" | 14" | 16" |
| -------------- | --- | --- | --- |
| Cheese         | $25 | $31 | $36 |
| Classic Combo  | $32 | $39 | $43 |
| Hawaiian       | $27 | $33 | $38 |
| Meatzilla      | $32 | $39 | $45 |
| Pepperoni      | $27 | $35 | $38 |
| Veggie Delight | $32 | $38 | $45 |

Extra toppings $2 / $3 / $4 by size: anchovies, extra cheese, ham, linguica, pepperoni, sausage, bell pepper, garlic, onion, jalapeños, mushrooms, olives, pepperoncini, pineapple, spinach. Ranch dressing $1.25 for 2oz, $4 for 8oz. Six rotating draft beers.
