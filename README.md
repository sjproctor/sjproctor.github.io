# Bernillos

- Single page
- Mobile first
- Info:
  - location: 220 E Redwood, Fort Bragg, CA 95437
  - hours: Closed M, T, W, Th 6-8pm, F 3:30-8pm, Sat 3:30-8pm, Sun 3:30-8pm
  - contact: (707) 964-9314
  - menu: “Just the Classics”
  - history: placeholder
  - bio of current owner: placeholder
  - photos of pizzas: placeholder
- Vibe: old school “hole in the wall” pizza parlor with red and white checked curtains
- Technical
  - Super simple tech stack: HTML + CSS, very limited dependencies
  - SEO for Fort Bragg, pizza

## Site

Static single-page site, no build step, no JS, no external fonts/CDNs — just open `index.html`.

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

- Menu items (currently just says "Just the Classics")
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
