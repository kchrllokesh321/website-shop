# Sri Maruthi Enterprises

Vanilla HTML showcase site. Serve the folder over http (the site fetches JSON, so opening `index.html` directly from disk will not load the catalog):

```bash
python3 -m http.server 3000
```

Then visit `http://localhost:3000`.

The homepage is a single viewport: header, hero, shop-by-category, and compact footer. Internal views stay in the same tab via hash navigation.

## Editing the catalog without touching code

Everything a customer sees in the catalog is read from JSON files in `data/`. You never edit `index.html` to change content.

| What you want to change | File | Field |
| --- | --- | --- |
| Category name, one-line description, homepage card image | `data/catalog.json` → `categories[]` | `title`, `description`, `icon`, `visualAsset` |
| Sub-categories under a category (Cots, Sofa, …) | `data/catalog.json` → `categories[].subCategories[]` | `id`, `title`, `description`, `image` |
| Types under a sub-category (Premium Teak Wood Cot, …) | `data/catalog.json` → `subCategories[].types[]` | `id`, `title`, `description`, `image` |
| Price filter buttons | `data/catalog.json` → `filters.priceRanges[]` | `label`, `min`, `max` (`null` max = no upper limit) |
| Which spec fields become filters (Material, Size, Colour…) | `data/catalog.json` → `filters.attributes[]` | `key` must match a key inside product `specs` |
| WhatsApp number and pre-filled messages | `data/catalog.json` → `settings` | `whatsappNumber`, `whatsappMessage`, `appointmentMessage` |
| How many placeholder cards a type shows until real products exist | `data/catalog.json` → `settings.placeholderProductsPerType` | set to `0` once real listings are in |
| Products | `data/products-<category>.json` | one object per product (see below) |

### Workflow with the CRM

1. Upload the photo in the CRM's asset area and copy its link (`https://...`).
2. Open the matching JSON file and paste the link into the `image` field (or add it to `images` for a gallery photo).
3. Edit any text fields the same way.
4. Save and publish the site. The next page load shows the change; nothing else needs rebuilding.

Keep ids in lowercase with hyphens (`premium-teak-wood-cot`). Ids are used in the page URL, so changing an id changes the link.

### Adding a product

Add one object to `data/products-<category>.json`:

```json
{
  "id": "cot-001",
  "category": "furniture",
  "subCategory": "cots",
  "type": "premium-teak-wood-cot",
  "name": "Premium Teak Wood Cot",
  "description": "Elegant design. Built to last.",
  "longDescription": "Optional longer text shown on the product page.",
  "price": 32999,
  "image": "https://your-crm-host/images/cot-001.jpg",
  "images": ["https://your-crm-host/images/cot-001.jpg", "https://your-crm-host/images/cot-001-side.jpg"],
  "specs": { "material": "Teak Wood", "size": "Queen" }
}
```

- `category`, `subCategory`, and `type` must match ids in `data/catalog.json`.
- `image` can be any hosted URL. `images` is optional and feeds the product gallery.
- `price` is a number; omit it or use `null` to show "Price on request".
- Any key in `specs` that is listed in `filters.attributes` becomes a checkbox filter automatically, and every key shows in the product's specs list.

### Adding a type or sub-category

Add an entry in `data/catalog.json` with an `id`, `title`, optional `description`, and an `image` link. It appears in the sidebar and grids immediately; products point to it through their `subCategory` and `type` fields.

### Removing something

Delete the entry from the JSON. Products that still point to a removed type simply stop appearing in that sub-category.
