# MN JADE theme customizations

This theme is Horizon 4.4.0 with the Tinker preset, styled to match the MN JADE
design canvas. Shopify ignores this file.

Most of the look comes from settings and templates. Custom code is limited to one
stylesheet, a few small blocks and one line in `layout/theme.liquid`.

## Added files

| File | Purpose |
| --- | --- |
| `assets/mnj-custom.css` | Everything settings can't do, grouped by page and component |
| `blocks/mnj-sticker.liquid` | Round tilted sticker on the home hero. Text, size, rotation, offset and colors are block settings |
| `blocks/mnj-empty-collection.liquid` | Message shown when a collection has no products. The text is a block setting |
| `blocks/mnj-collection-tabs.liquid` | The shop page's underlined tabs. Collections are a block setting; the current one is marked in the HTML. The first tab can have its own label (`all`), and the row can be left-aligned or centred |
| `blocks/mnj-product-accordion.liquid` | The product page's expandable rows under add to cart. Holds `_mnj-accordion-row` blocks and shows nothing when no row applies. When it shows rows, the description's spec bullets and bold-titled Sizing/Care paragraphs are hidden so they don't repeat (matched by the description block's `product_description` id) |
| `blocks/_mnj-accordion-row.liquid` | One row: heading, Horizon icon and text (its own, or the product description's first bullet list), shown only for its product types (matched to the product's Type) and when the description contains, or doesn't contain, a phrase such as `sterling silver`. All settings |
| `blocks/mnj-social-profiles.liquid` | Not visible. Adds the shop's Instagram, TikTok and other profile links to the structured data search engines read. In the footer's group; links are block settings |
| `blocks/mnj-custom-request-form.liquid` | The contact and custom order forms: labels above the fields and up to two dropdowns. Sends through Shopify's contact form; every label and option is a block setting |

## The one edit to a Horizon file

In `layout/theme.liquid`, directly after `{{ content_for_header }}`:

```liquid
{{ 'mnj-custom.css' | asset_url | stylesheet_tag }}
```

It has to stay after `content_for_header` so it loads after Horizon's component styles.

## Theme settings changed (`config/settings_data.json`)

- **Palette:** background `#EFEFEB` (warm off-white), foreground `#1F2320`, color1 `#5C605B`,
  color2 `#7A7E78`, color3 `#F6F6F3` (the panels), color4 `#D9DAD3` (lines and input borders).
  Inputs and secondary buttons are filled `#FBFCFC`. Panels have a 1px `#E3E4DE` outline (group
  border settings).
- **Fonts:** body Nunito 400; subheading, heading and accent Quicksand 600.
- **Type sizes:** paragraph 16, h1 32, h2 32, h3 24, h4 16 (card names), h5 16, h6 12 in the body font,
  matched by eye to Maison Third: Nunito text uses their sizes (16 body, 18 menu, 14 filter row), and
  Quicksand, whose letters are about a fifth bigger than their Cardo, runs smaller (card names 16 for
  their 20; the collection and product titles 36px, 28px on phones, set in the stylesheet, for their 44
  and 33). The product page price is 16px (its block's custom size).
- **Buttons:** primary black fill with white text; secondary white fill with a 1px black
  border; both use the accent font.
- **Radii:** inputs 32, popovers 16, predictive search products 32, cards 16,
  cart thumbnails 100.
- **Product cards:** quick add, hover image and card carousel off; hover effect none.
- **Cart:** cart page instead of drawer; discount code, installments and accelerated
  checkout buttons off; currency codes off on cart items and totals.
- **Popover border:** color4. **Sale badge background:** color3.

## Templates and section groups changed

- `sections/header-group.json`: announcement bar on top (`#2C332F` dark green, white 14px text, edited in
  the theme editor); one header row with the menu on the left, logo centred and icons on the right;
  country and language selectors off; sticky header always on (the announcement bar scrolls away);
  transparent home header off; 1px color4 bottom border; text-style menu (dropdowns come from nested links in
  the main menu); phone navigation bar off, so phones use the menu drawer.
- `sections/footer-group.json`: newsletter section, policy list and old columns removed;
  one centered group with a divider, the flower mark image and the `footer` menu; copyright
  with "Powered by Shopify" off.
- `templates/index.json`: hero with the sticker, `✿ favorites ✿` product list (the manual
  `favorites` collection, first 3 in its manual order) with a
  `shop all` button, `follow along` panel with four image blocks and `instagram` and `tiktok` buttons side by side.
- `templates/collection.json`: large left-aligned `✿ collection title ✿` heading (custom size 3.5rem in the
  heading font), the `mnj-collection-tabs` section switched off (the header's shop dropdown replaces it),
  empty message, product grid with no side padding so it lines up with the heading, filters on with
  sorting, the item count on the right (grid density off), and a green circle with the logo and a `custom order form` label, linking to `/pages/custom`.
- `templates/product.json`: carousel gallery with 72px thumbnails, arrows (shown on phones too) and zoom off, bordered details card (title, price, add to cart, then the description, like Maison Third), the
  `mnj-product-accordion` under the description (`details`, the description's spec
  bullets, for every product; sizing for bracelets; care for bracelets, plain necklaces, silver necklaces and earrings, in the product sheet's wording),
  `more little friends` panel using the recommendations block. Quantity and accelerated
  checkout are disabled static blocks. Sticky add to cart is off.
- `templates/cart.json`: `your cart` title, gray summary panel, `keep shopping` link.
- `templates/page.json` (the default page template, used by faqs and any new page): `✿ page title ✿`,
  the page's text in a gray panel, and a `say hi` link to the contact page. This replaced an `info` template.
- `templates/page.about.json` (new): about. Title, a 520px banner image, a heading over the
  page's text, three blob-shaped photos and a `shop all` button.
- `templates/page.custom.json` (new): `✿ page title ✿`, the `mnj-custom-request-form` in a gray panel, and a
  `how it works` column with the page's text and a blob-shaped photo, then `✿ things I can help you find ✿`:
  blob-shaped photos, each with a name, all editable in the theme editor.
- `templates/page.contact.json`: `✿ say hi ✿`, the `mnj-custom-request-form` in a gray panel with a
  `what's it about?` dropdown, and an instagram, tiktok and photo column.

## Redirects to the shop page

Shopify won't let `/collections` or `/collections/all` be removed, and every collection URL
must start with `/collections/`. So the shop page is `/collections/shop`, and a short script in
a `custom-liquid` section sends those two pages there:

- `templates/list-collections.json`: section `shop_redirect`, with a plain link as a fallback
- `templates/collection.json`: section `all_redirect`, which only acts on the built-in `all` page

Neither runs in the theme editor. If the `shop` handle changes, update both.

## Set in the Shopify admin, not in this repo

- **Store name:** `MN JADE` in Settings. The copyright line and the logo's alt text read it.
- **Logo files** in Content > Files: `mn-jade-logo.png` (header), `mn-jade-mark.png` (footer)
  and `mn-jade-favicon-green.png` (the flower in #7EBD7C, matching the shop page's custom order
  circle, on a see-through background). The theme points at these names, so replace a file by uploading
  one with the same name, or pick a new one in the theme editor. They're cropped from the
  design canvas; the wordmark font, Fredoka, is drawn into the logo image, so the site's text
  fonts don't change.
- **`main-menu`:** shop, about.
- **`footer` menu:** `shipping ✿`, `refunds ✿`, `faqs ✿`, `contact ✿`, `custom`. Each ✿ is part of the link
  title before it.
- **Pages** (Online Store > Pages), each with its template: FAQs (handle `faq`) uses the default; `about` uses
  `about`; `contact` uses `contact`; `something custom` (handle `custom`) uses `custom`. Shipping and refunds are
  Shopify policies instead. The page's own text fills the panel on default pages, the story on about, and
  `how it works` on custom.
- **Form messages** from the contact and custom pages arrive in the store's contact email, marked
  `contact` or `custom request`.
- **Collections:** handles `shop`, `necklaces`, `bracelets`, `earrings`, `charms`, plus `favorites`
  (manual, not in the menu) for the homepage. The `shop` collection is
  automated (every product not tagged `hidden`) and titled `shop`, which is its page heading; its tab says
  `all` from the tabs block's "First tab label". The tab label is the
  collection's title. Tabs are picked in the `mnj-collection-tabs` block's settings;
  `charms` is left out of the tabs for now (the collection still exists).
- **Filters** come from the Search & Discovery app (Apps > Search & Discovery > Filters). The length
  filter reads the products' `Length` variant option.
- **Interface wording:** Horizon's locale strings can be changed in Online Store > Themes >
  Edit default theme content. CSS lowercases the labels but can't reword them. For the search
  overlay, set the placeholder to `search for something little ✿` and "View all" to
  `see everything`.

## Empty settings to fill in the theme editor

- About page banner and its three circle photos, and the contact and custom page photos
- Contact page tiktok text, which starts as `@[handle]`, and its link

- Home hero image
- The four `follow along` images
- `instagram` button link

## Deviations from the design

- **Heading sizes:** section headings 32px, because the type setting has no 30 or 36; collection and
  product titles 36px (28px on phones) to look like Maison Third's 44px.
- **Cart summary:** no "summary" heading, and no subtotal row unless a discount applies.
  Horizon shows an estimated total and a tax note instead of separate rows.
- **Cart remove link:** beside the quantity stepper, not under the product name.
- **Product name case:** taken from the product title as typed.
- **Copyright line:** includes the store name, because Horizon always adds it.
- **Shipping and refunds:** the footer links to Shopify's policy pages, so the text lives only in
  Settings > Policies. The stylesheet gives them the info pages' look, but Shopify sets their
  layout and titles, so they read "shipping policy" and "refund policy" and have no "say hi" link.
- **Search overlay:** no "see everything" link until something is typed, because Horizon only
  shows that button with results. Its label and the search placeholder are locale strings.
- **Shop tabs:** switched off for now (the header's shop dropdown links the collections). When on, they're
  links to separate collections in a small `mnj-collection-tabs` block. Horizon's
  `collection-links` section cleared the underline on hover, so it was replaced. Native filtering
  renders as dropdowns and has no "all" option.
- **Custom request form:** no inspiration photo upload, because Shopify's forms can't accept files.
- **Shop custom card:** sits centered on its own row under the products, because Horizon's product grid
  only holds products.
- **Shop heading:** shows the collection's name (`✿ necklaces ✿`) instead of always `✿ shop ✿`, so each
  collection page has its own main heading for search engines.
- **Shop filters:** laid out like Maison Third's: filters on the left, the item count and sort on the right;
  on phones they open from a `filter` button. The sort menu hides relevance, best selling, both
  alphabetical orders and oldest first. Horizon has no "Filter:" label to switch on.
- **Pinned shop bar:** not in the design; added on request. The filters (the `filter` button on phones)
  stay under the sticky header while scrolling, and the tabs too if their section is switched back on;
  the heading scrolls away. Phone tabs stay on one row.
- **Sold out:** Horizon's badge, lowercased and moved onto the photo's edge, and the card's price is
  struck through.
- **Favorites row:** cards fill the row at about 443px wide rather than the design's 400px.
- **Announcement bar and pinned header:** the bar keeps its own colour and wording, not the design's.
- **Blob photos:** the design's five still shapes; product cards also morph on hover (phones: a slow
  drift), which the design can't show. The product page's own photo stays still.
- **Muted text:** `#888888` on white is about 3.5:1, below the 4.5:1 guideline for small text.
- **Phones:** the design doesn't cover them. The logo drops to 24px, and `shop` and `about`
  sit in a row under it instead of a hamburger. A menu with many links would need the
  hamburger back, or a scrolling row.

## Values hardcoded in the stylesheet

Settings can't reach these, so change them in `assets/mnj-custom.css`:

- `#e2e3dc`: the flower behind the header, menu and search icons, inside the `--mnj-flower` SVG
- `#f6f6f3` with a `#e3e4de` outline: the cart summary fill, needed because Horizon drops it on phones, and the policy
  page panel, because policy pages don't get the theme's colour settings
- `#5c605b`: prices in the search overlay
- `#b9bbb4`: the outline of unselected product thumbnails
- `--mnj-blob-1` to `-5` and `--mnj-blob-hover`: the blob shapes
- `17 17 17` at 0.35: the dim behind the search overlay
- White: the logo in the shop page's custom order circle, made white by a CSS filter, so any
  image picked there shows as a white silhouette

The shop page's custom order card copies the product grid's column maths so it matches a product
card at every width: 250px minimum columns (the grid's "medium" card size), 32px gaps and the grid
section's 24px side padding. If you change those grid settings, update the card's widths too.

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
   - **Phone header:** the menu button `.menu-drawer-container .header__icon--menu` on its flower, with no
     padding; search moved to the `rightA` grid area and `.account-button` hidden (also whenever
     `data-menu-style='drawer'`)
   - **Header menu:** 18px links from the menu block setting. From 750 to 999px `.menu-list__list-item` is at
     least 51% wide so Horizon's `overflow-list` reaches its `minimum-items` (2) and the header swaps to
     the menu drawer instead of a "More" dropdown; in that drawer layout from 750px the row gets 12px top
     and bottom padding and the icon flowers their 40px size
   - **Product options:** single-value options (`.variant-option[data-testid='variant-option-single']`) and an
     empty `variant-picker` are hidden on product pages
   - **Sort menu:** `.sorting-filter__option` radios and `.sorting-filter__select` options by `value`
   - **Cards and gallery:** blob shapes (`--mnj-blob`, cycling by `:nth-child` of the card or image block)
     on `.card-gallery` images, the gallery's `slideshow-slides` images and `.slideshow-controls__thumbnail`
     (outlined, `aria-selected`), the shop's custom card, search results and `.cart-items__media-image`;
     the hover morph needs `(hover: hover)`, the phone drift `(hover: none)`; the product page's main photo
     stays still; on phones the
     gallery's `--page-margin` inset on `.product-information__media` and the arrows' `.mobile\:hidden` override
   - **Product page:** `.product-information__grid` columns (stacked and 560px wide from 750 to 999px), recommendations layout, and the details
     card's side padding on phones (`.product-details > .group-block`, which Horizon clears to 0)
   - **Shop page:** `.main-collection-grid__empty` (the tabs are our own block); filters by
     `.facets__form`, `.facets__filters-wrapper`, `.products-count-wrapper`, `.facets-horizontal-remove`
     and `.facets-toggle` (both pinned with `--header-height`, which Horizon's sticky header sets); sold-out cards by `.color-custom-badge-sold-out`, `.product-badges` and `.price`
   - **Cart:** `.cart-items__*` rows, `.cart-summary__inner`, the text "remove" link
   - **Policy pages:** `.shopify-policy__container`, `__title` and `__body`, which come from
     Shopify rather than Horizon
   - **Pages:** sections are styled by their template key (`__say_hi`, `__about_banner`,
     `__about_circles`, `__contact_main`, `__custom_main`, `__custom_lookbook` (its rows of circles), and `__custom_card` on the shop page, whose
     label link is stretched over the card), and the phone field by `#ContactForm-phone` if Horizon's contact form is added back
   - **Search overlay:** `.search-modal__content`, `.predictive-search-*`, `.resource-card__*`. Blank "recently
     viewed" cards (deleted products) are hidden by `.resource-card__link[href='']`.
     Check it open, with results, with no results, and at phone width
5. The "0" cart badge and the "remove" link override `.hidden` and `.visually-hidden` from
   `assets/base.css`. If either disappears, check those two classes first.
