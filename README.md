# KCBlendz E-commerce Platform

> **Nourishing Lives, Inspiring Wellness.**
> Full-stack monolithic e-commerce platform for KCBlendz — premium smoothies, juices, wellness shots, dried fruits, and fruit powders. Multi-region (Mauritius / Nigeria / Global), brand-consistent design built around the real KCBlendz logo (green · orange · yellow), custom smoothie builder with live video preview, customer reviews, favorites/wishlist, secure card-entry payment flow, and a full admin panel.

Built with **Flask + SQLite + Tailwind (CDN) + Chart.js** in a single `app.py`.

---

## Quick start

```bash
pip install -r requirements.txt
python app.py
```

Open **http://127.0.0.1:5000**. On first run, the database (`kcblendz.db`) is created and seeded with the real KCBlendz catalog, 5 long-form Wellness Hub articles, sample reviews, builder options for all 3 regions, and an admin user.

## Admin access

```
URL:      /admin
Email:    admin@kcblendz.com
Password: KCBlendz@2025
```

## Sandbox payment test cards

- **Visa:** `4242 4242 4242 4242` · any future MM/YY · any 3-digit CVV
- **Mastercard:** `5555 5555 5555 4444` · any future MM/YY · any 3-digit CVV
- **Amex:** `3782 822463 10005` · any future MM/YY · 4-digit CVV (e.g. `1234`)

## What's in this overhaul

This release is a complete brand-consistency, feature, and security overhaul. Key changes from the previous version:

### Brand & visual system
- **Logo-derived colour palette** — green `#2E8B57`, orange `#F7941D`, yellow `#FBC02D`, on a warm cream base. The fuchsia/lime/sky palette is gone; CSS variable aliases keep older template tokens working while resolving to the new colours.
- **Real KCBlendz video** is the hero on the home page, the background of the region picker, and the live preview inside the custom smoothie builder. The previous static cup illustration was replaced with the actual product being prepared.
- **Address corrected** to Kitchen 2 Kongo, Pamplemousses, Mauritius — that's the campus kitchen and the brand's true HQ. Mauritius leads the region picker and the shipping policy; Nigeria and Global follow.
- **All emojis removed** — every icon is now inline SVG.
- **Nav icons are consistent** — sub-nav is now uniformly text-only (no mixed icons); the right-cluster icon-buttons (favorites, cart, account) all share one visual treatment.
- **Logout button** is brand-coloured (no more out-of-palette red) in both the account sidebar and the navigation dropdown.

### New customer features
- **Favorites / wishlist** — heart button on every product, dedicated `/account/favorites` page, a global count badge in the nav, and one-click "add to cart" from the favorites grid.
- **Reviews & ratings** — stars on every product, a "What customers say" section with verified-buyer badges, and an inline review-submission form. The database is seeded with realistic sample reviews so the UI is never empty.
- **WhatsApp floating button** — site-wide one-tap WhatsApp chat with the brand's number, important for the Mauritius and Nigeria markets.
- **Newsletter strip in the footer** with a 10%-off-first-order hook.

### Real payment flow
- The payment page now asks for **real card details** — card number, name, MM/YY expiry, and CVV — with live card-brand detection (Visa / Mastercard / Amex / Discover / JCB) as the user types.
- **Luhn checksum** validation on card numbers, MM/YY format check, expiry-in-the-future check, and 3-4 digit CVV check before the order is ever marked paid.
- Only the last 4 digits and brand are stored — never the full PAN.
- Bank transfer now correctly **requires proof upload** before completing the order.

### Bug fixes
- **Signup "passwords don't match" bug** — the route no longer demands a `confirm` field when one isn't provided; when it is provided, mismatches are caught and shown clearly.
- **Admin dashboard chart "loading forever / very big"** — the canvas is now wrapped in a 280px fixed-height container with `maintainAspectRatio: false` so Chart.js stops growing infinitely. Colours updated to the brand palette.
- **Session handling** — logged-in users keep their session across pages; the region is preserved correctly; CSRF is rotated on login as expected.

### Wellness Hub expansion
The 5 articles are now full long-form pieces (each 7-10 minute reads, 8-15 H2 sections), covering turmeric, West African superfoods, post-workout recovery, the 3pm energy slump, and the truth about detox.

## Architecture

```
kcblendz/
├── app.py                  # ~1100 lines: routes, schema, seeds, business logic
├── requirements.txt
├── kcblendz.db             # SQLite (created on first run)
├── static/
│   ├── img/
│   │   ├── logo.png                 # round-glassy KCBlendz logo
│   │   ├── kcblendz-video.mp4       # real brand video (hero, builder preview, store picker)
│   │   ├── kcblendz-catalog.jpeg    # printed menu used to seed pricing
│   │   ├── kcblendz-product.png     # product photography
│   │   ├── kcblendz-products.png    # product photography
│   │   └── custom-cup.svg           # placeholder cup
│   ├── uploads/            # admin uploads + payment proof
│   ├── css/  &  js/        # space for custom assets
└── templates/
    ├── base.html           # brand palette, WhatsApp button, flash UI
    ├── partials/           # nav, footer
    ├── public/             # store_select, home, shop, product, builder, cart, checkout, payment, order_thanks, wellness, about, contact, faq, all policy pages
    ├── auth/               # login, register (with confirm field), forgot
    ├── account/            # dashboard, orders, profile, favorites (new), saved_smoothies, addresses, notifications
    └── admin/              # dashboard (fixed chart), products, orders, users, categories, blogs, builder_config, reports, messages, notifications
```

## Brand palette

```
--kc-green:       #2E8B57   /* primary "KCBlendz" text colour from the logo */
--kc-green-deep:  #1F6E43   /* hover state */
--kc-orange:      #F7941D   /* cup illustration + CTA buttons */
--kc-orange-deep: #D97706
--kc-yellow:      #FBC02D   /* outer ring of logo, accents */
--kc-cream:       #FFF8E7   /* warm background for callouts */
--kc-bg:          #F5F9F2   /* page background */
--kc-dark:        #1B3A2F   /* primary text (no plain black) */
```

CSS aliases `--fuchsia`, `--lime`, `--sky`, `--purple`, `--bg`, `--dark` all map to the brand palette so templates that still use the old tokens render in the correct colours.

## Features at a glance

### Customer-facing
- 3-region store (Mauritius MUR · Nigeria NGN · Global USD) with hand-drawn SVG flags and forced region selection on first visit.
- Region-aware product catalogue — fresh items hidden from Global; shelf-stable items shippable worldwide.
- Custom Smoothie Builder with **video live-preview** of the real KCBlendz product as ingredients update.
- Favorites/wishlist for logged-in users.
- Reviews with verified-buyer badges.
- Cart, checkout, real-card payment flow, bank-transfer with proof, order confirmation.
- Customer accounts: orders, reorder, saved blends, multi-address book, profile, notifications, favorites.
- Wellness Hub: 5 long-form, original articles.

### Admin
- Dashboard with fixed-height Chart.js revenue graph (no more infinite growth).
- CRUD for products, categories, builder options, blog posts.
- Order workflow (pending → processing → ready → delivered → cancelled) with customer notification.
- Customers panel with search, suspend/activate/delete/promote, CSV export.
- Reports: 30-day daily revenue, 12-month monthly, top 10 products, customer growth, region comparison.
- Contact-message inbox and notification feed.

### Security & quality
- CSRF token on every POST form, rotated on login.
- bcrypt password hashing.
- Role decorators (`@login_required`, `@admin_required`, `@region_required`).
- Soft deletes; audit log of every admin mutation.
- Card data never stored beyond brand + last4.
- Security headers: X-Frame-Options, X-Content-Type-Options, Referrer-Policy.

## Going live with real payments

The card branch currently uses a sandbox flow — it validates the card properly (Luhn, expiry, CVV, brand) but doesn't actually charge. To wire live payments, replace the `card` branch of `/payment/<id>/process`:

- **Nigeria** — Paystack init redirect + webhook verification (`paystack_initialize_transaction()` then `paystack_verify()`).
- **Mauritius / Global** — PayPal Smart Buttons (client-side) and capture on success; or Stripe Elements if you'd rather have a single global card processor.
- **Bank transfer** — already complete: customer uploads proof, admin verifies in `/admin/orders/<id>`.

## Configuration

Top of `app.py`:

```python
SECRET_KEY            # set via env var KCB_SECRET in production
DB_PATH               # default: kcblendz.db
UPLOAD_FOLDER         # static/uploads
MAX_UPLOAD_MB         # 8MB default
REGIONS               # currency, symbol, country
```

## Deploy

### Gunicorn
```bash
pip install gunicorn
gunicorn -w 4 -b 0.0.0.0:8000 app:app
```

### Render / Railway / Fly.io
Start command: `gunicorn -w 2 -b 0.0.0.0:$PORT app:app`. Persist `/data` (or wherever `DB_PATH` points) and `static/uploads`.

### Switch to PostgreSQL
Replace `sqlite3.connect()` and the `Row` factory in `get_db()` with `psycopg2` + `RealDictCursor`. The SQL is standard except `datetime('now')` → `now()`, `date(...)` → `::date`, and `strftime('%Y-%m', ...)` → `to_char(..., 'YYYY-MM')`.

## Contact

- **HQ:** Kitchen 2, Kongo · Pamplemousses, Mauritius
- **Email:** hello@kcblendz.com
- **WhatsApp:** +234 802 4655 191
