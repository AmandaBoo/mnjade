# MN JADE product sheets

`MN JADE products MASTER.csv` is the blank starting sheet for new product listings: the header row
only, with the 24 columns the listing prompt uses (Shopify's older import names), UTF-8, CRLF line
endings. Never fill it in. For each batch, copy it to a dated working file outside this repo, e.g.
`~/Downloads/MN JADE products 2026-11-25.csv`, add the rows there following the Obsidian note
`Prompt for future product insertions.md`, then import that copy in Shopify (Products → Import).

Working sheets hold real prices, stock and internal notes, so they stay out of this repo. Shopify's
GitHub sync ignores this folder; it only reads the theme folders.
