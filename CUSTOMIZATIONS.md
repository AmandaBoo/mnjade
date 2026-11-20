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
| `blocks/mnj-collection-tabs.liquid` | The shop page's underlined tabs. Collections are a block setting; the current one is marked in the HTML. The first tab can have its own label (`all`) |
| `blocks/mnj-social-profiles.liquid` | Not visible. Adds the shop's Instagram, TikTok and other profile links to the structured data search engines read. In the footer's group; links are block settings |
| `blocks/mnj-custom-request-form.liquid` | The contact and custom order forms: labels above the fields and up to two dropdowns. Sends through Shopify's contact form; every label and option is a block setting |

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
  header off; 1px color4 bottom border; text-style menu; phone navigation bar on, with the
  hamburger hidden in the stylesheet.
- `sections/footer-group.json`: newsletter section, policy list and old columns removed;
  one centered group with a divider, the flower mark image and the `footer` menu; copyright
  with "Powered by Shopify" off.
- `templates/index.json`: hero with the sticker, `✿ favorites ✿` product list with a
  `shop all` button, `follow along` panel with four image blocks and `instagram` and `tiktok` buttons side by side.
- `templates/collection.json`: `✿ collection title ✿` heading (`✿ shop ✿` on the all tab), `mnj-collection-tabs` tabs, empty message, product grid with filtering
  on (sorting and grid density off), and a green circle with the logo and a `custom order form` label, linking to `/pages/custom`.
- `templates/product.json`: carousel gallery with 72px thumbnails, bordered details card,
  `more little friends` panel using the recommendations block. Quantity and accelerated
  checkout are disabled static blocks. Sticky add to cart is off.
- `templates/cart.json`: `your cart` title, gray summary panel, `keep shopping` link.
- `templates/page.json` (the default page template, used by faqs and any new page): `✿ page title ✿`,
  the page's text in a gray panel, and a `say hi` link to the contact page. This replaced an `info` template.
- `templates/page.about.json` (new): about. Title, a 520px banner image, a heading over the
  page's text, three circular photos and a `shop all` button.
- `templates/page.custom.json` (new): `✿ page title ✿`, the `mnj-custom-request-form` in a gray panel, and a
  `how it works` column with the page's text and a circle photo.
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
- **Collections:** handles `shop`, `necklaces`, `bracelets`, `earrings`, `charms`. The `shop` collection is
  automated (every product not tagged `hidden`) and titled `shop`, which is its page heading; its tab says
  `all` from the tabs block's "First tab label". The tab label is the
  collection's title. Tabs are picked in the `mnj-collection-tabs` block's settings.
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

- **Heading sizes:** 32px everywhere, because the type setting has no 30 or 36.
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
- **Shop tabs:** links to separate collections in a small `mnj-collection-tabs` block. Horizon's
  `collection-links` section cleared the underline on hover, so it was replaced. Native filtering
  renders as dropdowns and has no "all" option.
- **Custom request form:** no inspiration photo upload, because Shopify's forms can't accept files.
- **Shop custom card:** sits centered on its own row under the products, because Horizon's product grid
  only holds products.
- **Shop heading:** shows the collection's name (`✿ necklaces ✿`) instead of always `✿ shop ✿`, so each
  collection page has its own main heading for search engines.
- **Shop filters:** not in the design. They sit centred under the tabs, without Horizon's product
  count; on phones they open from a `filter` button.
- **Sold out:** Horizon's badge, lowercased and moved onto the photo's circle, and the card's price is
  struck through.
- **Favorites row:** cards fill the row at about 443px wide rather than the design's 400px.
- **Muted text:** `#888888` on white is about 3.5:1, below the 4.5:1 guideline for small text.
- **Phones:** the design doesn't cover them. The logo drops to 24px, and `shop` and `about`
  sit in a row under it instead of a hamburger. A menu with many links would need the
  hamburger back, or a scrolling row.

## Values hardcoded in the stylesheet

Settings can't reach these, so change them in `assets/mnj-custom.css`:

- `#e6e6e6`: the flower behind the header and search icons, inside the `--mnj-flower` SVG
- `#f2f2f2`: the cart summary fill, needed because Horizon drops it on phones, and the policy
  page panel, because policy pages don't get the theme's colour settings
- `#555555`: prices in the search overlay
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
   - **Phone header:** `.header-drawer`, `.menu-list--mobile`, and the `leftA` grid area
   - **Cards and gallery:** circular images on `.card-gallery` and `media-gallery`
   - **Product page:** `.product-information__grid` columns, recommendations layout, and the details
     card's side padding on phones (`.product-details > .group-block`, which Horizon clears to 0)
   - **Shop page:** `.main-collection-grid__empty` (the tabs are our own block); filters by
     `.facets__form`, `.facets__filters-wrapper`, `.products-count-wrapper`, `.facets-horizontal-remove`
     and `.facets-toggle`; sold-out cards by `.color-custom-badge-sold-out`, `.product-badges` and `.price`
   - **Cart:** `.cart-items__*` rows, `.cart-summary__inner`, the text "remove" link
   - **Policy pages:** `.shopify-policy__container`, `__title` and `__body`, which come from
     Shopify rather than Horizon
   - **Pages:** sections are styled by their template key (`__say_hi`, `__about_banner`,
     `__about_circles`, `__contact_main`, `__custom_main`, and `__custom_card` on the shop page, whose
     label link is stretched over the card), and the phone field by `#ContactForm-phone` if Horizon's contact form is added back
   - **Search overlay:** `.search-modal__content`, `.predictive-search-*`, `.resource-card__*`.
     Check it open, with results, with no results, and at phone width
5. The "0" cart badge and the "remove" link override `.hidden` and `.visually-hidden` from
   `assets/base.css`. If either disappears, check those two classes first.
