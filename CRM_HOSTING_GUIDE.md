# Sri Maruthi Enterprises — CRM Hosting Guide

How to put this website into the CRM (the builder with the HTML source box and the **Upload** button), and how to edit it afterwards.

The website is three things:

| Part | File(s) | Goes where |
|---|---|---|
| The website itself (design + behaviour) | `index.html` | The HTML source box of the page |
| All words, prices, products, phone numbers | `data/content.json` | Pasted inside `index.html` (step 4) |
| Images | `assets/` folder (28 files needed) | The CRM **Upload** button |

Only one CRM page is needed. Categories, product pages, About, Manufacturing, Contact and Book an Appointment are all inside `index.html` and open as `#/categories`, `#/contact`, `#/appointment`, and so on.

---

## Part A — First-time setup

### Step 1. Get the files

Download the project (GitHub → **Code → Download ZIP**, or from the shared folder). You need:

- `index.html`
- `data/content.json`
- the `assets/` folder

Ignore the `.txt` files and the `ChatGPT Image …png` files in the top folder; they are not part of the site.

### Step 2. Upload the images and collect their links

In the CRM click **Upload**, upload one file, then copy the link the CRM gives you. It looks like
`https://myappz-v2.b-cdn.net/agency/…/uploads/0a1b2c3d-….jpeg`.

Keep a simple list: *file name → link*. Upload these 28 files (skip `assets/hero/hero-home.png`, `hero-home.webp` and `assets/sections/workshop.webp`; they are not used).

| File | Used for |
|---|---|
| `assets/brand/logo.png` | Header logo and browser-tab icon |
| `assets/hero/hero-living.png` | Homepage hero photo |
| `assets/categories/furniture-3d.png` | Furniture icon |
| `assets/categories/electronics-3d.png` | Electronics icon |
| `assets/categories/small-appliances-3d.png` | Small Appliances icon |
| `assets/categories/gift-items-3d.png` | Gift Items icon |
| `assets/categories/baby-items-3d.png` | Baby Items icon |
| `assets/categories/visuals/furniture-visual.png` | Furniture homepage card |
| `assets/categories/visuals/electronics-visual.png` | Electronics homepage card |
| `assets/categories/visuals/small-appliances-visual.png` | Small Appliances homepage card |
| `assets/categories/visuals/gift-items-visual.png` | Gift Items homepage card |
| `assets/categories/visuals/baby-items-visual.png` | Baby Items homepage card |
| `assets/sections/about.webp` | About page |
| `assets/sections/manufacturing.webp` | Manufacturing page |
| `assets/sections/showroom.webp` | Contact page |
| `assets/sections/appointment.webp` | Book an Appointment page |
| `assets/products/cot-01.webp` | Cots |
| `assets/products/cot-detail-headboard.webp` | Cot details |
| `assets/products/cot-detail-footboard.webp` | Cot details |
| `assets/products/sofa-01.webp` | Sofas |
| `assets/products/bed-01.webp` | Beds |
| `assets/products/office-01.webp` | Office tables |
| `assets/products/dining-01.webp` | Dining |
| `assets/products/dressing-01.webp` | Dressing tables |
| `assets/products/diwana-01.webp` | Diwana |
| `assets/products/diwana-detail.webp` | Diwana details |
| `assets/products/kitchen-01.webp` | Kitchen |
| `assets/products/tv-01.webp` | TV / electronics |

### Step 3. Put the links into `content.json`

Open `data/content.json` in a text editor (Notepad, VS Code, TextEdit in plain-text mode). Use **Find and Replace → Replace All** once per image:

- Find: `assets/hero/hero-living.png`
- Replace with: the link you copied for that file
- Replace All

Repeat for every file in the table. Some paths appear many times; Replace All fixes them all at once:

| Find | Times it appears |
|---|---|
| `assets/products/bed-01.webp` | 12 |
| `assets/categories/electronics-3d.png` | 12 |
| `assets/products/office-01.webp` | 10 |
| `assets/products/cot-01.webp` | 9 |
| `assets/products/sofa-01.webp` | 9 |
| `assets/categories/gift-items-3d.png` | 8 |
| `assets/categories/baby-items-3d.png` | 8 |
| `assets/products/cot-detail-headboard.webp` | 8 |
| `assets/products/dining-01.webp` | 6 |
| `assets/products/dressing-01.webp` | 6 |
| `assets/products/kitchen-01.webp` | 6 |
| `assets/products/tv-01.webp` | 6 |
| `assets/categories/small-appliances-3d.png` | 5 |
| `assets/products/diwana-01.webp` | 5 |
| `assets/products/cot-detail-footboard.webp` | 4 |
| `assets/products/diwana-detail.webp` | 2 |
| everything else | 1 |

Then the logo: find the line `"name": "SRI MARUTHI ENTERPRISES",` near the top and add a line under it:

```json
"logo": "https://…your uploaded logo link…",
```

Leave `"assetBase": ""` as it is.

When finished, search the file for `assets/` — there should be no matches left.

### Step 4. You do not have to paste `content.json`

The catalog is already inside `index.html`, between these two lines:

`PASTE data/content.json BELOW THIS LINE`

`PASTE data/content.json ABOVE THIS LINE`

Homepage cards, category cards, sub-category cards, and product cards all come from that block. Leave it as it is. The cards show even when you do not paste anything.

Open that block only when you have changed `data/content.json` on purpose and want those changes in the CRM. Replace everything between the two lines with the new file. Leave the two comment lines and the `<script>` tags.

### Step 5. Paste `index.html` into the CRM

1. In the CRM, open the route you publish (`/`).
2. Click **Edit** on the HTML source.
3. Select everything in the box and delete it.
4. Open `index.html`, Ctrl+A, Ctrl+C, and paste into the box.
5. **Save**, then **Publish**.

### Step 6. Check

Open the published site and confirm:

- Logo in the header, hero photo, five category cards with images.
- Tap **Furniture** → a sub-category → a product: images show.
- Header **Contact** → Contact page with call button, WhatsApp button, three branches.
- **Book an Appointment** → the form opens; fill Name, Phone, Category and press **Send via WhatsApp**: WhatsApp opens with the details.
- **Chat on WhatsApp** on the hero → WhatsApp opens with "Hi, I'm interested in your products…".

If an image is missing, that link was not replaced or was pasted wrongly. Search the JSON block for the old `assets/…` name.

---

## Part B — Editing the site later

All edits are made in the CRM's HTML source box, inside the block that starts `<script type="application/json" id="site-content">`. Everything between that line and its `</script>` is the same JSON described in `README.md`.

Words, prices, and phone numbers still live in the JSON block. Pictures can be set either way.

### Pictures — two ways

The CRM does not keep an `assets/` folder. Uploading a file does not put it at `assets/hero/hero-living.png`. The file gets a new link like `https://myappz-v2.b-cdn.net/agency/…/uploads/….jpeg`.

**Way 1. Click the picture and press Upload** (logo, hero, category cards, product photos, sub-category photos):

1. Click the picture itself, not the name under it.
2. In the picture panel, press **Upload** (or pick one from the library).
3. Save, then Publish.

Clicking the **name** under a category still opens that category. In the builder, clicking the **picture** does not. On the published site, clicking the picture opens the category as before. Every category card, including the first one, has a picture you can click.

On a product, the **first photo is the main picture**. It shows on the product card and in the large photo. Use the arrows beside that photo to move to photo 2 and photo 3, click each one, and press Upload. You do not set a separate main image. Swipe, or press the arrows, to see the photos you added.

**Way 2. Paste the link in the JSON** (same as before):

1. Upload, then click the file in the Asset Library so the link is copied.
2. Paste it over the matching field: `homepage.hero.image`, a category `visualAsset` or `icon`, or a product `image`.
3. The hero can also take the link in `data-hero-src="https://…"` on the hero image tag.

If the picture on the page is already an `https://` link, that is the one that shows. A pasted `https://` link in the JSON is used when the picture tag still points at `assets/…`.

### Where things live

| Want to change | Edit |
|---|---|
| WhatsApp number (used by every button) | `business.whatsappNumber` — digits only, with country code, e.g. `919154444114` |
| Phone shown on Contact page | `business.phone` (for the call link) and `business.phoneDisplay` (how it looks) |
| Email, opening hours | `business.email`, `business.hours` — leave `email` as `""` to hide it |
| Pre-filled WhatsApp text for each button | `business.whatsappMessage` (Chat on WhatsApp), `business.appointmentMessage` (first line of the appointment message), `business.contactMessage`, `business.productMessage` |
| Business name, caption, tagline, copyright, social links | `business` |
| Branch names, addresses, map links | `branches.list` — `name`, `address`, `mapQuery` (what to search in Google Maps) |
| Hero heading, description, button labels | `homepage.hero` |
| About / Manufacturing / Contact / Appointment page words | `pages.about`, `pages.manufacturing`, `pages.contact`, `pages.appointment` |
| Categories, sub-categories, types, their images | `catalog.categories` |
| Products, prices, descriptions, photos, filters | `products` |
| Text and card sizes | `sizes` |

### Add a product

1. Upload the photo in the CRM, copy the link.
2. In the `products` list, copy an existing product from `{` to `}` and paste it after the last one (add a comma between them).
3. Change `id` (unique, lowercase, hyphens), `title`, `price`, `description`, `image` (the new link), and its `category` / `subCategory` / `type` ids so it appears on the right page.
4. Save, publish.

### Remove a product

Delete its whole `{ … }` block from `products`, and the comma before it if it was last. Save, publish.

### Change a price or a description

Find the product by its `title`, change the value in quotes. Save, publish.

### Change an image

Use either way from **Pictures — two ways** above. Click the picture and press Upload, or paste the copied link into the JSON field (`visualAsset` for a homepage category card, `icon` for the Categories page, `image` for a product).

A normal photo stays a normal photo. It does not become the drawn 3D category icon. The homepage cards use the five files in `assets/categories/visuals/`. Upload those if you want the same cut-out look, then click each card picture and choose the file you just uploaded.

### JSON rules (the only things that break the page)

- Text goes in double quotes: `"title": "Teak Cot"`.
- Items in a list are separated by commas; **no comma after the last item**.
- Every `{` has a `}` and every `[` has a `]`.
- If the page loads blank after an edit, paste the JSON block into a free online "JSON validator"; it points at the line with the mistake.

---

## Troubleshooting

| Symptom | Cause / fix |
|---|---|
| Blank page after publishing | JSON has a syntax error (usually a missing or extra comma). Validate it. |
| One image missing | Its `assets/…` path was not replaced, or the link has a typo. |
| Old version still showing on your phone | The phone cached it. Close the tab and reopen the link, or clear site data. |
| WhatsApp button opens the wrong number | Change `business.whatsappNumber` (digits only, no `+`, no spaces). |
| Category picture click opens the category instead of Upload | Click the picture, not the name. Paste the latest `index.html` into the page so the picture is not inside the link. |
| Visual text edits disappeared | Headings and prices still come from the JSON. Change them there. Pictures can be changed with Upload. |
