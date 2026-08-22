# mainmodestudio.com

Source of the Main Mode Studio website. Static HTML/CSS, no build step: edit, commit, push.

- **Live:** https://mainmodestudio.com (custom domain on GitHub Pages, DNS at Cloudflare)
- **Deploy:** push to `main` → `.github/workflows/pages.yml` → live in a minute or two
- **This repo is the only source of the site.** It is not generated from the game repos.

```text
index.html                  Studio home: hero, product catalog, about, footer
assets/site.css             Single stylesheet shared by every page
assets/<product>-*.webp     Product imagery (icon, feature graphic, screenshots)
products/<slug>/index.html  One page per released product
press/index.html            Press kit: studio facts, game factsheets, downloadable images
app-ads.txt                 AdMob verification — do not rename or move
main-mode-studio-logo.png   Brand mark used in header, footer and favicon
.nojekyll                   Serve files as-is, no Jekyll processing
```

Legal pages live in a separate repo, `wealthbound-legal`, and are served under
`/wealthbound-legal/`. Keep product privacy links pointing there.

## Rules that keep the site from breaking as the catalog grows

- The homepage stays a **studio page**: hero, catalog of cards, about, footer. Product detail
  (screenshots, feature lists, store links, legal links) lives only on the product page.
- All pages link the same `/assets/site.css`. Never add a `<style>` block to a page; add a
  reusable class to the stylesheet instead.
- Product-specific legal links (privacy policy, store page) belong to that product, both on its
  page and inside the footer `Products` column — never in the global footer row.
- Absolute, root-relative paths (`/assets/...`, `/products/<slug>/`) everywhere, so pages work
  at any depth.
- Anything that names a single game — hero art, the featured card, the homepage `og:image` —
  belongs to the flagship of the moment, not to the site. Revisit those three when a new
  flagship ships.

## Adding a new product

1. **Assets** — export to `assets/` as WebP, named `<slug>-*.webp`:
   `<slug>-icon.webp` (512²), `<slug>-feature.webp` (1024×500), plus screenshots (576×1024).
2. **Product page** — copy `products/wealthbound/index.html` to `products/<slug>/index.html` and
   update: `<title>`, description, `og:*`, canonical URL, `theme-color`, hero copy, tags, store
   buttons, screenshots, and the three feature points.
3. **Homepage card** — in `index.html`, add an `<article class="product-card">` inside
   `.portfolio-grid`, at the placeholder comment. Keep `product-card-featured` on the newest or
   flagship release only; the other cards use the plain `product-card` class and sit two per row.
4. **Footer** — add a `.footer-product` block (product link + its store and privacy links) to the
   `Products` column, in **every** page's footer (`index.html` and each `products/*/index.html`).
   On the product's own page mark its link `aria-current="page"`.
5. **About section** — update the `Latest release` / `In progress` lines in `.studio-facts` on the
   homepage. The about copy itself is written at studio level on purpose: it must keep working
   with three games in the catalog, so keep game-specific stories on the product pages.
6. **Press kit** — in `press/index.html`, copy one `.press-game` block inside the `Games`
   section and swap its facts and images. Each screenshot tile links to a `.lightbox` block by
   id (`#shot-<game>-<name>`); copy those too and keep the ids unique. Studio-level material (description, factsheet, logo)
   stays in the section above and must not gain game-specific wording. A release with no press
   kit entry is invisible to anyone writing about it.
7. **Check** — desktop and mobile widths, every link, and that the new page has no
   `<style>` block and no hardcoded color outside `site.css`.

## Checklist before pushing

- [ ] Every internal link resolves (`/`, `/products/<slug>/`, footer legal links)
- [ ] Each page has a unique `<title>`, `description`, canonical and `og:image`
- [ ] Images use `loading="lazy"` below the fold and have real `width`/`height`
- [ ] Alt text on every meaningful image, empty `alt=""` on decorative ones
- [ ] Layout verified at ~1440px, ~960px and ~390px
