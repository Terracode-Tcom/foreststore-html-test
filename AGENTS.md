# Foreststore HTML site (`html-v1`)

You are helping a Terracode developer build a shop whose pages are written in
plain HTML. The platform fills the pages with shop data on each request and
writes the head tags (title, description, social and structured data) itself.

## Layout

*   `foreststore.json` lists the pages, partials, custom routes and the data each
    page queries. `"output": "site"` names the folder that holds the site.
*   `site/` holds the pages, `partials/` and `assets/`. Every file in `site/` is
    part of the shop, so keep notes and tools outside it.

| Page type | Address |
|---|---|
| `index` | `/` |
| `product` | `/products/<slug>` |
| `collection` | `/collections/<slug>`, `/categories/<slug>` |
| `listing` | `/collections`, `/categories` |
| `search` | `/search` |
| `cart` | `/cart` |
| `page` (legal pages, data key `legal`) | `/pages/<slug>` |
| `404` | anything else |

Custom pages go in `routes` with a `path` such as `/about` and their own `seo`.

## Placeholders

| Attribute | Example |
|---|---|
| `data-fsd-text` | `<h1 data-fsd-text="product.name">Name</h1>` |
| `data-fsd-format` | `money`, `number`, `date`, `richtext`, `legal` |
| `data-fsd-attr` | `data-fsd-attr="src:p.thumbnail.url; alt:p.thumbnail.alt"` |
| `data-fsd-image` | `data-fsd-image="400"` resizes the image on the platform |
| `data-fsd-href` | `data-fsd-href="/products/{p.slug}"` |
| `data-fsd-each`, `data-fsd-limit` | `data-fsd-each="related.data as p"` |
| `data-fsd-if`, `data-fsd-unless` | `data-fsd-if="product.in_stock"` |
| `<fs-include name="header"></fs-include>` | Inserts a partial, inside `<body>` only |

Data names: the page's query keys, `shop`, `legal_links`, `site`, `menus`, `seo`,
`url`, `page`, and loop names. Text inside a filled element is sample text for
when the file is opened directly: the platform replaces it.

## Rules

*   Each page has its own `<head>` and `<body>`.
*   No `<title>`, description, robots, Open Graph, Twitter, canonical or JSON-LD
    tags.
*   No placeholders on `script`, `style`, `textarea`, `title` or inside
    `template`.
*   No `url(/…)` in CSS: use a relative address.
*   The product and collection pages query the item in their address with
    `"slug": "$url.slug"`.
*   Leave checkout and sign-in to the platform: no `<base>`, refresh meta, password
    field, link to `/ca/account` or `/ca/checkout`, or reference to
    `/__foreststore/`. Use `data-fs-account` and `data-fs-checkout`, and
    `data-fs-add-to-cart` and `data-fs-cart-count` for the cart.

`check html` reports every other rule with file and line.

## Loop

After every change run `foreststore-template-kit check html`. Exit 0 means the
site compiles, 1 lists errors to fix, 2 means the check could not run. Explain
each error and fix it. Then run `foreststore-template-kit site` and look at the
pages; edit a file and reload.

The preview uses sample data. It has no merchant settings, sitemaps, Analytics,
checkout or sign-in. A passing check is not a publish: the kit cannot publish
yet.
