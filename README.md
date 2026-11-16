# mainmodestudio.com

Source of the Main Mode Studio website. Static HTML/CSS, no build step: edit, commit, push.

- **Live:** https://mainmodestudio.com (custom domain on GitHub Pages, DNS at Cloudflare)
- **Deploy:** push to `main` → `.github/workflows/pages.yml` → live in a minute or two
- **This repo is the only source of the site.** It is not generated from the game repos.

```text
index.html                  Studio home: hero, one feature block per game, hero roster,
                            stages, principles, studio, FAQ, footer
assets/site.css             Single stylesheet shared by every page (Night theme)
assets/<product>-*.webp     Product imagery (icon, feature graphic, screenshots)
products/<slug>/index.html  One page per released product
press/index.html            Press kit: studio facts, game factsheets, downloadable images
app-ads.txt                 AdMob verification — do not rename or move
main-mode-studio-logo.png   Brand mark used in header, footer and favicon
.nojekyll                   Serve files as-is, no Jekyll processing
```

Legal pages live in a separate repo, `wealthbound-legal`, and are served under
`/wealthbound-legal/`. Keep product privacy links pointing there.

DynamiteArena's legal pages (`privacy-policy.html`, `terms-of-service.html`) live in this repo
under `/dynamitearena-legal/`, served at the same kind of address. Their source is the game repo
(`docs/legal/`); edit there first, then copy here. The game links to both addresses
(`scripts/core/app_links.gd`), so never rename or move them.

## Rules that keep the site from breaking as the catalog grows

- The homepage is a **studio page** with one `.game-feature` block per game: status chip,
  title, tagline, a short paragraph, three or four points and its buttons. Everything else
  (screenshot gallery, at-a-glance facts, legal links) lives only on the product page. Once the
  catalog passes three games, turn the homepage blocks into cards instead of adding a fourth.
- Lists that grow sit in a fixed-size container, so adding an item never changes the page:
  the hero roster is a scrolling rail, and each game appears exactly once in the stage list.
- All pages link the same `/assets/site.css` and the same Google Fonts line (Bricolage
  Grotesque 700/800, Inter 400/600/700). Never add a `<style>` block or a `style=""` attribute;
  add a reusable class to the stylesheet instead.
- Breakpoints live only in `site.css`: 940px (two columns stack), 720px (three columns stack),
  560px (phone).
- Product-specific legal links (privacy policy, terms, store page) belong to that product, both
  on its page and under its name in the footer `Games` column — never in the global footer row.
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
3. **Homepage block** — in `index.html`, add an `<article class="game-feature">` to `#games`.
   Alternate the image side with `game-feature-reverse`. Give it an `id` matching the slug.
4. **Stages** — move the game's token in `.stage-list` (On paper → Prototype → Closed test →
   Live) whenever its stage changes. Each game appears in exactly one stage.
5. **Footer** — add a `.footer-product` block (product link + its store and legal links) to the
   `Games` column, in **every** page's footer (`index.html`, `press/index.html` and each
   `products/*/index.html`). On the product's own page mark its link `aria-current="page"`.
6. **Studio section** — keep the copy at studio level; it must keep working with three games in
   the catalog. Numbers in `.stat-list` must be facts, never estimates.
7. **Press kit** — in `press/index.html`, copy one `.press-game` block inside the `Games`
   section and swap its facts and images. Each screenshot tile links to a `.lightbox` block by
   id (`#shot-<game>-<name>`); copy those too and keep the ids unique. Studio-level material (description, factsheet, logo)
   stays in the section above and must not gain game-specific wording. A release with no press
   kit entry is invisible to anyone writing about it.
8. **Check** — every width in the checklist below, every link, and that the new page has no
   `<style>` block, no `style=""` attribute and no hardcoded color outside `site.css`.

## Adding a DynamiteArena hero

Export the portrait to `assets/dynamitearena-hero-<name>.webp` (480×360, transparent) and add
one `<li class="hero-card tint-…">` to the rail in `index.html`, picking one of the `.tint-*`
classes. The rail scrolls, so nothing else changes.

## Checklist before pushing

- [ ] Every internal link resolves (`/`, `/products/<slug>/`, footer legal links)
- [ ] Each page has a unique `<title>`, `description`, canonical and `og:image`
- [ ] Images use `loading="lazy"` below the fold and have real `width`/`height`
- [ ] Alt text on every meaningful image, empty `alt=""` on decorative ones
- [ ] Layout verified at 1440, 1024, 768 and 375px: nothing wider than the screen, no cut-off text
