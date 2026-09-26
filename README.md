# Sri Maruthi Enterprises

Vanilla HTML showcase site. Serve the folder over http (the site fetches its content file, so opening `index.html` directly from disk will show no content):

```bash
python3 -m http.server 3000
```

Then visit `http://localhost:3000`.

## One file controls the whole site: `data/content.json`

You never edit `index.html`. Every text, number, image link, size, category, filter, and product lives in `data/content.json`. Edit a value, save, publish, reload.

| Want to change | Section in `data/content.json` |
| --- | --- |
| WhatsApp number (used by every WhatsApp button, once) | `business.whatsappNumber` |
| Pre-filled WhatsApp messages (each button has its own) | `business.whatsappMessage` (Chat on WhatsApp + floating button), `business.appointmentMessage` (first line of the appointment form message), `business.contactMessage` (Contact page button), `business.enquireMessage` and `business.consultMessage` (product Enquire / Consult buttons; `{product}` becomes "Product Name (Type, Variant)", and `{name}`, `{type}`, `{variant}` are also available) |
| Phone (tel: link), email (mailto:, leave `""` to hide), opening hours + note | `business.phone`, `business.phoneDisplay`, `business.email`, `business.hours`, `business.hoursNote` |
| Business name, caption, tagline, copyright, social links | `business` |
| Footer branch line, branch list (footer popover + Contact page + form dropdown), contact note | `branches` |
| Hero title, description, subline, button labels, badge labels, hero image | `homepage.hero` (badge icons stay in code; only the words change) |
| "Shop by Category" heading | `homepage.shopHeading` |
| About, Manufacturing, Contact, Appointment, Categories page text and images | `pages` |
| Contact form: button label, first line of the WhatsApp message, thank-you text | `pages.contact` (`submit`, `whatsappIntro`), `pages.formThanks` |
| Book an Appointment page: heading, lead, field labels, placeholders, error text, time-of-day options, "Other" category label, side panel points | `pages.appointment` (`labels`, `placeholders`, `errors`, `timeOptions`, `otherCategory`, `aside`); the first line of the message is `business.appointmentMessage` |
| Sizes in pixels: hero text, category cards, product cards, header/footer height | `sizes` |
| Categories → sub-categories → types → optional variants (names, descriptions, images) | `catalog.categories` |
| Price filter buttons and which spec fields become filters | `catalog.filters` |
| Placeholder cards per type, products per page | `catalog.placeholderProductsPerType`, `catalog.productsPerPage` |
| Products | `products` |

### Images from the CRM

Upload the photo in the CRM asset area, copy its link (`https://...`), and paste it into any `image`, `icon`, `visualAsset`, or `images` field. Nothing else is needed.

### Products

`products` is a list. Each product is one object:

```json
{
  "id": "cot-001",
  "category": "furniture",
  "subCategory": "cots",
  "type": "premium-teak-wood-cot",
  "variant": "king-size",
  "name": "Premium Teak Wood Cot",
  "description": "Elegant design. Built to last.",
  "longDescription": "Optional longer text shown on the product page.",
  "price": 32999,
  "image": "https://your-crm-host/images/cot-001.jpg",
  "images": ["https://your-crm-host/images/cot-001.jpg", "https://your-crm-host/images/cot-001-side.jpg"],
  "specs": { "material": "Teak Wood", "size": "Queen" }
}
```

- **Add a product:** copy an existing object, paste it into the list, change the values. Give it a new `id`.
- **Remove a product:** delete its object from the list.
- **Change price:** edit `price` (a number, no ₹ or commas). Remove the line or set `null` for "Price on request".
- **Change description:** edit `description` (card) and `longDescription` (product page).
- `category`, `subCategory`, and `type` must match ids under `catalog.categories`. `variant` is optional and only used when that type has `variants`; a product without one still shows on the type page.
- Any `specs` key listed in `catalog.filters.attributes` becomes a checkbox filter automatically; every key shows in the product's spec list.

### Categories, sub-categories, types, variants

The catalog is five levels deep, and every level except the last is a grid of cards the shopper clicks through:

1. Category (Furniture) → 2. Sub-category (Cots) → 3. Type (Premium Teak Wood Cot) → 4. Variant (Modern) → 5. Products (the real listings, with filters).

Under `catalog.categories`, each category has `subCategories`, each sub-category has `types`, and each type can have `variants`. Add or delete objects the same way as products. Each needs an `id` (lowercase, hyphens), a `title`, and optionally `description` and `image` (a missing image falls back to the parent's image).

```json
{
  "id": "l-shape",
  "title": "L Shape Sofa",
  "image": "https://your-crm-host/images/l-shape.jpg",
  "variants": [
    { "id": "fabric", "title": "Fabric", "description": "Upholstered in fabric.", "image": "https://your-crm-host/images/l-fabric.jpg" },
    { "id": "wooden", "title": "Wooden", "description": "Solid wood frame.", "image": "https://your-crm-host/images/l-wooden.jpg" }
  ]
}
```

Rule of thumb used for variants: when the type is a material (Teak Wood, Metal) the variants are styles (Classic, Modern, Carved); when the type is a shape or model (L Shape, LED TV) the variants are materials or sizes (Fabric, Wooden; 32 inch, 43 inch). If a type genuinely comes one way, leave `variants` out and its card opens the product list directly.

Filters, prices and the Enquire / Consult buttons appear only on level 5. The card text at levels 2–4 under the heading comes from `pages.categories.subLead`, `typeLead`, `variantLead`, or the item's own `description`.

### Sizes

`sizes` values are pixels. For example `categoryCardPx: 210` sets the homepage category card square; `heroTitleMaxPx: 54` caps the hero heading size; `productCardMinPx: 200` sets how wide product cards are before the grid adds a column.

### Contact and appointment forms

There is no server, so the forms hand off to WhatsApp: pressing the button opens a chat with `business.whatsappNumber` with the details already typed in. The customer presses send in WhatsApp and you receive it there.

- Contact form (`#/contact`): name, phone, chosen branch, message.
- Book an Appointment (`#/appointment`, every "Book an Appointment" button): full name, phone, category of interest (the category list comes from `catalog.categories` plus `pages.appointment.otherCategory`), customization yes/no with details, preferred date and time, notes. Optional fields left blank are not included in the message.

### Hosting in a CRM / page builder (one HTML box, uploaded images)

Some hosts give you a single HTML source box per page plus an "Upload" for images, and cannot serve `data/content.json` as a separate file. For those:

1. Upload every file from `assets/` (keep the same file names). Note the web address of one uploaded file, for example `https://cdn.example.com/site123/hero-living.png`. The part before the file name is your `assetBase`.
   - If the host keeps sub-folders, upload the folders as they are and `assetBase` is the address of the `assets` folder itself.
   - If the host flattens everything into one folder, upload the files and also change each `assets/hero/...`, `assets/products/...` path in `content.json` to just the file name after `assetBase` (or ask for the paths to be flattened for you).
2. Set `"assetBase"` at the top of `content.json` to that prefix. Optionally set `business.logo` to the uploaded logo address.
3. Open `index.html`, find `<script type="application/json" id="site-content">` near the top of the script section, and paste the entire `content.json` between that tag and its `</script>`.
4. Paste the whole `index.html` into the host's HTML source box for the home page and publish.

When the inline block is filled the site never downloads `data/content.json`, so future edits are made inside that block in the host's editor. Locally, leave the block empty and keep editing `data/content.json` as before.

### Rules

- Keep the file valid JSON: every item in a list separated by a comma, no comma after the last one, text in double quotes. A JSON checker (search "JSON validator") will point to any mistake.
- Ids appear in page links, so avoid renaming an id once a link has been shared. Titles can change freely.
