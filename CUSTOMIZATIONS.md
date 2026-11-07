# MN JADE theme customizations

This theme is Horizon 4.4.0 with the Tinker preset, styled to match the MN JADE
design canvas. Shopify ignores this file.

Most of the look comes from settings and templates. Custom code is limited to one
stylesheet, two small blocks and one line in `layout/theme.liquid`.

## Added files

| File | Purpose |
| --- | --- |
| `assets/mnj-custom.css` | Everything settings can't do, grouped by page and component |
| `blocks/mnj-sticker.liquid` | Round tilted sticker on the home hero. Text, size, rotation, offset and colors are block settings |
| `blocks/mnj-empty-collection.liquid` | Message shown when a collection has no products. The text is a block setting |

## The one edit to a Horizon file

In `layout/theme.liquid`, directly after `{{ content_for_header }}`:

```liquid
{{ 'mnj-custom.css' | asset_url | stylesheet_tag }}
```

It has to stay after `content_for_header` so it loads after Horizon's component styles.

## Theme settings changed (`config/settings_data.json`)

- **Palette:** background `#ffffff`, foreground `#111111`, color1 `#555555`,
  color2 `#888888`, color3 `#F2F2F2`, color4 `#DDDDDD`.
- **Fonts:** body Nunito 400; subheading, heading and accent Quicksand 600.
- **Type sizes:** paragraph 16 (the stylesheet overrides this to 15), h1 32, h2 32,
  h3 24, h4 18, h5 16, h6 12 in the body font.
- **Buttons:** primary black fill with white text; secondary white fill with a 1px black
  border; both use the accent font.
- **Radii:** inputs 32, popovers 16, predictive search products 32, cards 16,
  cart thumbnails 100.
- **Product cards:** quick add, hover image and card carousel off; hover effect none.
- **Cart:** cart page instead of drawer; discount code, installments and accelerated
  checkout buttons off; currency codes off on cart items and totals.
- **Popover border:** color4. **Sale badge background:** color3.

## Templates and section groups changed

- `sections/header-group.json`: announcement bar removed; logo centered with the menu on
  the row below; country and language selectors off; sticky header off; transparent home
  header off; 1px color4 bottom border; text-style menu.
- `sections/footer-group.json`: newsletter section, policy list and old columns removed;
  one centered group with a divider, brand text and the `footer` menu; copyright with
  "Powered by Shopify" off.
- `templates/index.json`: hero with the sticker, `✿ favorites ✿` product list with a
  `shop all` button, `follow along` panel with four image blocks and an `instagram` button.
- `templates/collection.json`: `✿ shop ✿` heading, `collection-links` tabs, empty message, product grid with filtering,
  sorting and grid density off.
- `templates/product.json`: carousel gallery with 72px thumbnails, bordered details card,
  `more little friends` panel using the recommendations block. Quantity and accelerated
  checkout are disabled static blocks. Sticky add to cart is off.
- `templates/cart.json`: `your cart` title, gray summary panel, `keep shopping` link.

## Redirects to the shop page

Shopify won't let `/collections` or `/collections/all` be removed, and every collection URL
must start with `/collections/`. So the shop page is `/collections/shop`, and a short script in
a `custom-liquid` section sends those two pages there:

- `templates/list-collections.json`: section `shop_redirect`, with a plain link as a fallback
- `templates/collection.json`: section `all_redirect`, which only acts on the built-in `all` page

Neither runs in the theme editor. If the `shop` handle changes, update both.

## Set in the Shopify admin, not in this repo

- **Store name:** `MN JADE` in Settings. The logo and copyright line read it.
- **`main-menu`:** shop, about.
- **`footer` menu:** `faqs ✿`, `shipping`. The ✿ is part of the first link's title.
- **Collections:** handles `shop`, `necklaces`, `bracelets`, `earrings`. The tab label is the
  collection's title. The shop tabs show "Collection title" placeholders if these are missing.
- **Interface wording:** Horizon's locale strings can be changed in Online Store > Themes >
  Edit default theme content. CSS lowercases the labels but can't reword them. For the search
  overlay, set the placeholder to `search for something little ✿` and "View all" to
  `see everything`.

## Empty settings to fill in the theme editor

- Home hero image
- The four `follow along` images
- `instagram` button link

## Deviations from the design

- **Heading sizes:** 32px everywhere, because the type setting has no 30 or 36.
- **Cart summary:** no "summary" heading, and no subtotal row unless a discount applies.
  Horizon shows an estimated total and a tax note instead of separate rows.
- **Cart remove link:** beside the quantity stepper, not under the product name.
- **Product name case:** taken from the product title as typed.
- **Copyright line:** includes the store name, because Horizon always adds it.
- **Search overlay:** no "see everything" link until something is typed, because Horizon only
  shows that button with results. Its label and the search placeholder are locale strings.
- **Shop tabs:** links to separate collections, chosen because `collection-links` already
  marks the active tab. Native filtering renders as dropdowns and has no "all" option.
- **Favorites row:** cards fill the row at about 443px wide rather than the design's 400px.
- **Muted text:** `#888888` on white is about 3.5:1, below the 4.5:1 guideline for small text.
- **Phones:** the design doesn't cover them. The logo drops to 24px and the search flower
  is 44px.

## Values hardcoded in the stylesheet

Settings can't reach these, so change them in `assets/mnj-custom.css`:

- `#e6e6e6`: the flower behind the header and search icons, inside the `--mnj-flower` SVG
- `#f2f2f2`: the cart summary fill, needed because Horizon drops it on phones
- `#555555`: prices in the search overlay
- `17 17 17` at 0.35: the dim behind the search overlay

Two theme settings no longer do anything because the stylesheet overrides them: paragraph
size, and the menu's font size and weight.

## After merging a Horizon update

1. Confirm the stylesheet line is still in `layout/theme.liquid`.
2. For conflicts in `templates/*.json`, `sections/*-group.json` or
   `config/settings_data.json`, keep this theme's values and add any new Horizon settings.
3. Run `shopify theme check`. Seven warnings in Horizon's own files were already there.
4. Check these in `shopify theme dev`, because the stylesheet depends on Horizon's markup:
   - **Header:** `.header-actions__action`, `.cart-bubble`, `.menu-list__link`, and the
     `--menu-top-level-*` and `--padding-block-*` variables
   - **Cards and gallery:** circular images on `.card-gallery` and `media-gallery`
   - **Product page:** `.product-information__grid` columns, recommendations layout
   - **Shop page:** `.collection-links__link` tabs, `.main-collection-grid__empty`
   - **Cart:** `.cart-items__*` rows, `.cart-summary__inner`, the text "remove" link
   - **Search overlay:** `.search-modal__content`, `.predictive-search-*`, `.resource-card__*`.
     Check it open, with results, with no results, and at phone width
5. The "0" cart badge and the "remove" link override `.hidden` and `.visually-hidden` from
   `assets/base.css`. If either disappears, check those two classes first.
