# Bernillos

## Content

Old school “hole in the wall” pizza parlor with red and white checked curtains.

### Hours

- Monday: closed
- Tuesday: closed
- Wednesday: closed
- Thursday: 6-8pm
- Friday: 3:30-8pm
- Saturday: 3:30-8pm
- Sunday: 3:30-8pm

### Location & Contact

- 220 E Redwood, Fort Bragg, CA 95437
- (707) 964-9314

### Menu

“Just the Classics”

Sizes: 12", 14", 16"
Crust: thin, regular
Sauce: red, pesto
- Cheese: $25, $31, $36
- Classic combo: $32, $39, $43
- Hawaiian: $27, $33, $38
- Meatzilla: $32, $39, $45
- Pepperoni: $27, $35, $38
- Veggie delight: $32, $38, $45

Extra toppings: $2 for 12", $3 for 14", $4 for 16"
Extra toppings: anchovies, extra cheese, ham, linguica, pepperoni, salami, sausage, bell pepper, garlic, onion, jalapenos, mushrooms, olives, pepperoncini, pineapple, spinach, sundried tomato
Ranch dressings: $1.25 for 2oz, $4 for 8oz
Beers: 6 rotating drafts

### Additional Content

- History of the restaurant
- Bio of current owner
- Photos of pizzas

## Site

Static single-page site, HTML + CSS, no dependencies, no build step, no JS, no external fonts/CDNs — just open `index.html`.

- `index.html` — page content, meta tags, and JSON-LD `Restaurant` schema for SEO
- `styles.css` — mobile-first styles; red/white checkered look is done with CSS gradients (no images)

### Local preview

Open `index.html` directly in a browser, or serve it locally:

```
python3 -m http.server
```

### Deployment

Any static host works (GitHub Pages, Netlify, S3, etc.) since there's no build process — just upload `index.html` and `styles.css`.

### Placeholder content to fill in

- Beer table (`#menu` section lists six taps with placeholder names and prices)
- History section
- Owner bio
- Photos of pizzas (`#photos` section has empty placeholder tiles)

### Updating SEO tags

All SEO-related tags live in the `<head>` of `index.html`:

- **Title** — update the `<title>` element. Keep it descriptive and include "Fort Bragg" and "Pizza".
- **Meta description** — update the `content` of `<meta name="description" ...>`. Keep it under ~160 characters.
- **Open Graph tags** — `og:title`, `og:description`, `og:type`, `og:locale` control how the page looks when shared on social media. Update `og:title`/`og:description` alongside the title/description above. Add `og:image` (with a real photo) once one is available.
- **Structured data (JSON-LD)** — the `<script type="application/ld+json">` block defines a `Restaurant` schema (address, phone, hours) that helps search engines understand the business. Update it whenever the hours, address, or phone number change. Validate changes with [Google's Rich Results Test](https://search.google.com/test/rich-results).

To add a new tag, add another `<meta>` element in the `<head>` (e.g. `<meta name="keywords" content="...">`) or extend the JSON-LD object with additional [schema.org Restaurant](https://schema.org/Restaurant) properties (e.g. `menu`, `image`, `priceRange`, `sameAs` for social profiles).
