# KCBlendz E-commerce Platform

> **Nourishing Lives, Inspiring Wellness.**
> A full-stack monolithic e-commerce platform for KCBlendz — premium
> smoothies, juices, wellness shots, dried fruits and fruit powders.
> Multi-region (Mauritius / Nigeria / Global), with a brand-consistent
> design built around the real KCBlendz logo, a custom smoothie builder
> with live video preview, customer reviews, a favorites/wishlist, a
> secure card-entry payment flow, and a complete admin panel.

Built with **Flask + SQLite + Tailwind (CDN) + Chart.js** in a single
`app.py`.

---

## Quick start

```bash
pip install -r requirements.txt
python app.py
```

Open **http://127.0.0.1:5000**. On first run the database
(`kcblendz.db`) is created and seeded automatically with the real
KCBlendz catalog, 5 long-form Wellness Hub articles, sample reviews,
builder options for all 3 regions, and an admin user.

### Admin access

```
URL:      /admin
Email:    admin@kcblendz.com
Password: KCBlendz@2026
```

### Sandbox payment test cards

- Visa: `4242 4242 4242 4242` — any future MM/YY — any 3-digit CVV
- Mastercard: `5555 5555 5555 4444` — any future MM/YY — any 3-digit CVV
- Amex: `3782 822463 10005` — any future MM/YY — 4-digit CVV (e.g. `1234`)

---

## Deployment (Railway / Render / Fly.io)

The app is production-ready and self-initialising — the database is
created automatically on first import, so a fresh deploy never returns a
500 due to a missing database.

The repository ships with `Procfile`, `railway.json`, `nixpacks.toml`
and `runtime.txt`. On Railway the service starts with:

```
gunicorn app:app --bind 0.0.0.0:$PORT --workers 2 --threads 4 --timeout 60 --preload
```

Performance is tuned for small instances: SQLite runs in WAL mode with a
generous page cache, and static assets are served with a 30-day
`Cache-Control` header so repeat page loads are fast.

To persist data between deploys, mount a volume and point `DB_PATH` /
`static/uploads` at it.

---

## Architecture

```
kcblendz/
├── app.py                  Routes, schema, seed data, business logic
├── requirements.txt
├── Procfile / railway.json / nixpacks.toml / runtime.txt
├── kcblendz.db             SQLite (auto-created on first run)
├── static/
│   ├── img/                logo, brand video, product photography
│   ├── uploads/            admin uploads + payment proof
│   └── css/ · js/
└── templates/
    ├── base.html           brand palette, WhatsApp button, flash UI
    ├── partials/           nav, footer
    ├── public/             store select, home, shop, product, builder,
    │                       cart, checkout, payment, order thanks,
    │                       wellness, about, contact, faq, policies
    ├── auth/               login, register, forgot
    ├── account/            dashboard, orders, profile, favorites,
    │                       saved smoothies, addresses, notifications
    └── admin/              dashboard, products, orders, users,
                            categories, blogs, builder config,
                            reports, messages, notifications, profile
```

---

## Brand palette

Derived directly from the KCBlendz logo.

```
--kc-green       #2E8B57   primary brand colour / logo text
--kc-green-deep  #1F6E43   hover state
--kc-orange      #F7941D   cup illustration + primary CTAs
--kc-orange-deep #D97706
--kc-yellow      #FBC02D   logo ring / accents
--kc-cream       #FFF8E7   warm callout background
--kc-bg          #F5F9F2   page background
--kc-dark        #1B3A2F   primary text (no plain black)
```

All icons are inline SVG — no emoji, no icon fonts.

---

## Features

### Customer

- Three-region storefront (Mauritius MUR / Nigeria NGN / Global USD)
  with region-locked carts and currency-aware pricing.
- Region-aware catalogue: perishable items hidden from the Global store;
  shelf-stable items shippable worldwide.
- Custom Smoothie Builder — cup size, 1–3 fruits, base, sweeteners,
  add-ons and boosters, with a live video preview of the real product
  and real-time price calculation.
- Favorites / wishlist with one-click add-to-cart and a nav badge.
- Product reviews and star ratings with verified-buyer badges.
- Cart, checkout, secure card-entry payment, bank transfer with proof
  upload, and an order-confirmation page.
- Customer account: dashboard, order history, reorder, saved blends,
  multi-address book, notifications, profile, favorites — all sharing a
  consistent sidebar layout.
- Wellness Hub with five long-form original articles (inline markdown
  for headings, bold, lists and links).

### Admin

- Dashboard with a fixed-height, brand-coloured revenue chart.
- CRUD for products, categories, builder options and blog posts.
- Order workflow (pending → processing → ready → delivered → cancelled)
  with automatic customer notifications.
- User management: search, activate / suspend / delete / promote,
  CSV export, per-user detail.
- Admin profile & settings: update name, email and phone; change
  password with current-password confirmation.
- Reports: 30-day daily revenue, monthly summary, top products,
  customer-growth and region comparison — every chart uses a
  fixed-height container so it renders quickly and never overflows.
- Contact-message inbox and notification feed.

### Security & quality

- CSRF token on every state-changing request, rotated on login.
- Werkzeug password hashing.
- Role decorators: `@login_required`, `@admin_required`,
  `@region_required`.
- Soft deletes and an audit log of every admin mutation.
- Card data never stored beyond brand + last four digits.
- Security response headers (X-Frame-Options, X-Content-Type-Options,
  Referrer-Policy).
- Bank-transfer proofs are verified fast — typically within 10 minutes
  during opening hours.

---

## Payment flow

The card branch uses a sandbox flow: it fully validates the card
(Luhn checksum, MM/YY expiry in the future, CVV length, live brand
detection) but does not charge a real account. To go live, replace the
`card` branch of the payment route:

- Nigeria — Paystack initialize + webhook verification.
- Mauritius / Global — PayPal Smart Buttons or Stripe Elements.
- Bank transfer is already complete: the customer uploads proof and the
  admin verifies it on the order detail page.

---

## Configuration

Environment variables (all optional for local dev):

```
KCB_SECRET     Flask secret key (set in production)
PORT           Port to bind (provided automatically by Railway)
FLASK_DEBUG    "1" to enable debug locally
```

In-code configuration lives at the top of `app.py`: `DB_PATH`,
`UPLOAD_FOLDER`, `MAX_UPLOAD_MB`, and the `REGIONS` table.

---

## Switching to PostgreSQL

Replace `sqlite3.connect()` and the row factory in `get_db()` with
`psycopg2` + `RealDictCursor`. The SQL is standard except:
`datetime('now')` → `now()`, `date(...)` → `::date`, and
`strftime('%Y-%m', ...)` → `to_char(..., 'YYYY-MM')`.

---

## Contact

- HQ: Kitchen 2 Kongo — Pamplemousses, Mauritius
- Email: hello@kcblendz.com
- WhatsApp: +234 802 4655 191
