# Sri Maruthi Enterprises

Vanilla HTML showcase site. Serve the folder over http (product pages fetch JSON, so opening `index.html` directly from disk will not load products):

```bash
python3 -m http.server 3000
```

Then visit `http://localhost:3000`.

The homepage is a single viewport: header, hero, shop-by-category, and compact footer. Internal views stay in the same tab via hash navigation.

## Adding products

Products live in `data/products-<category>.json` (one file per category). Add one object per product; no HTML changes are needed.

```json
{
  "id": "cot-001",
  "category": "furniture",
  "subCategory": "cots",
  "type": "premium-teak-wood-cot",
  "name": "Premium Teak Wood Cot",
  "description": "Elegant design. Built to last.",
  "price": 32999,
  "image": "https://your-crm-host/images/cot-001.jpg",
  "specs": { "material": "Teak Wood", "size": "Queen" }
}
```

- `category`, `subCategory`, and `type` must match ids in `SUBCATEGORIES` inside `index.html`.
- `image` can be any hosted URL (CRM uploads work as-is). Optional `images` lists gallery photos.
- `price` is a number; omit it or use `null` to show "Price on request".
- `specs.material` powers the Material filter where present.
