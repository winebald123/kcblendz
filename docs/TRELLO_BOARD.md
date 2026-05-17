# KCBlendz Trello board plan

This is the sprint board for the 5-person KCBlendz build. Every card has clear
subtasks, an owner, and a definition of done. Cards move left-to-right through
the lists below as they progress.

---

## Board name
**KCBlendz · production sprint board**

## Lists (in left-to-right order)
1. **Backlog** — every card we'll eventually pick up
2. **This sprint** — committed for the current week
3. **In progress** — actively being worked on (max 2 per person)
4. **Review / PR open** — code review on GitHub, awaiting merge
5. **QA / testing** — manual testing on the merged branch
6. **Done** — shipped to `develop` and verified

## Labels (use Trello colour labels)
- 🟧 **Frontend** — public templates and styling
- 🟩 **Backend** — Flask routes, schema, business logic
- 🟦 **Builder** — custom smoothie flow
- 🟪 **Admin** — admin panel work
- 🟨 **QA / tests** — unit tests and manual QA
- 🟥 **Bug** — bug fix
- ⬛ **Docs** — README, deployment guide, etc.
- ⚡ **Urgent** — blocking other work

## Team
| Member | Role | Owns |
|--------|------|------|
| **M1** | Public UI lead | Public templates, brand system, store picker |
| **M2** | Builder & motion | Smoothie builder, animations, video integration |
| **M3** | Backend & DB | Flask routes, SQLite schema, seeds, API |
| **M4** | Customer dashboard | Auth, account pages, favorites, reviews |
| **M5** | Admin & CMS | Admin panel, blog CMS, reports, deployment |

---

# Cards

Each card lists its **owner**, **labels** and **subtasks**. Subtasks are the
actual checklist on the Trello card — keep them short, action-oriented and one
deliverable each. Mark each one off as you complete it.

---

## M1 — Public UI

### Card 1 · Project bootstrap
**Labels:** Frontend, Backend
**Owner:** M1 with M3
- [ ] Create the GitHub repository and add team members
- [ ] Initialise the Flask app skeleton (`app.py`, `templates/`, `static/`)
- [ ] Add `requirements.txt` with Flask + Werkzeug
- [ ] Add Tailwind CDN script to `base.html`
- [ ] Configure `SECRET_KEY` from env var
- [ ] Push the first commit and protect the `main` branch

### Card 2 · Brand system & global layout
**Labels:** Frontend
- [ ] Extract the KCBlendz logo colours (green, orange, yellow) into CSS variables
- [ ] Add `Plus Jakarta Sans` and `Righteous` from Google Fonts
- [ ] Build the global `base.html` with flash messages and a content block
- [ ] Add backwards-compatible CSS aliases for old tokens
- [ ] Document the palette in `docs/`

### Card 3 · Sticky navigation + footer
**Labels:** Frontend
- [ ] Build the sticky top nav with logo, search bar and right-cluster icons
- [ ] Add the announcement strip with free-delivery threshold
- [ ] Add a mobile hamburger menu
- [ ] Build the footer with brand columns, newsletter strip and contact info
- [ ] Add the floating WhatsApp button on every page

### Card 4 · Region picker `/store`
**Labels:** Frontend
- [ ] Build three region cards (Mauritius first, Nigeria, Global)
- [ ] Add SVG flags for each region
- [ ] Use the KCBlendz video as the background
- [ ] POST to `/store/<region>` to set the session
- [ ] Add a trust bar (SSL, fresh, real fruit)

### Card 5 · Homepage
**Labels:** Frontend
- [ ] Hero with the KCBlendz video and "Shop the menu / Build your blend" CTAs
- [ ] Featured-products carousel by category
- [ ] Side panels for Glow & Hydration / Wellness Shots / Build it your way
- [ ] "Trust" row (fast delivery, real fruit, secure pay)
- [ ] Testimonials with realistic Mauritius/Lagos names

### Card 6 · Shop & product detail
**Labels:** Frontend
- [ ] Filterable product grid with category, sort, search
- [ ] Build the reusable `_product_card.html`
- [ ] Image `onerror` fallback to a known-good URL
- [ ] Product detail page with badges, price, description, related items

### Card 7 · Public legal & info pages
**Labels:** Frontend, Docs
- [ ] About (with ALU / Kongo / Pamplemousses story)
- [ ] Contact form with category dropdown
- [ ] FAQ
- [ ] Privacy, Terms, Refund, Shipping policies — full text, Mauritius first
- [ ] Add the contact form POST handler

### Card 8 · Wellness Hub
**Labels:** Frontend
- [ ] Wellness index with category filter
- [ ] Wellness post layout with H2/H3 styling
- [ ] 5 long-form seed articles (~7-10 min reads each)
- [ ] Read time and category badges

---

## M2 — Smoothie builder & motion

### Card 9 · Builder skeleton
**Labels:** Builder, Frontend
- [ ] Two-column layout: live preview on the left, steps on the right
- [ ] Seven steps: cup size, fruits, base, sweeteners, add-ons, boosters, quantity
- [ ] Chip-based selection with selected/disabled states
- [ ] Fruit count constraint (1–3) with live counter
- [ ] Submit button enables only when cup + 1-3 fruits + base are chosen

### Card 10 · Live video preview
**Labels:** Builder
- [ ] Embed the KCBlendz `kcblendz-video.mp4` in the preview panel
- [ ] Autoplay, muted, loop, playsinline so it works on mobile
- [ ] Add a "Your blend" caption that updates with the chosen fruits
- [ ] Add a brand gradient overlay so the caption is legible

### Card 11 · Live pricing
**Labels:** Builder, Backend
- [ ] Build `/api/builder/price` (POST JSON, returns unit + total + currency)
- [ ] Wire it to the chips with a debounced fetch on every change
- [ ] Show "Pick a cup, 1-3 fruits, a base to continue" until valid
- [ ] Display total in a dark callout with brand-yellow label

### Card 12 · Custom-blend cart image
**Labels:** Builder, Backend
- [ ] Add `CUSTOM_BLEND_IMAGE_BY_FRUIT` map in `app.py`
- [ ] Add `image_for_blend()` helper using the dominant fruit
- [ ] Use it in `builder_add_to_cart` and `account_saved_add`
- [ ] Write a unit test for the helper

### Card 13 · Fix builder UX issues
**Labels:** Bug, Builder
- [ ] Remove `sticky bottom-4` from the submit button so it stops overlapping the form
- [ ] Add a "submitHint" line under the button
- [ ] Brand-coloured chips and submit button (no fuchsia)
- [ ] Verify on mobile that the right column scrolls cleanly

---

## M3 — Backend & database

### Card 14 · Schema design
**Labels:** Backend
- [ ] Define every table: users, addresses, categories, products, builder_options, custom_smoothies, orders, order_items, payments, blog_posts, notifications, audit_logs, newsletter_subscribers, contact_messages
- [ ] Add favorites and reviews tables
- [ ] Create indices on FKs and hot paths
- [ ] Document the schema in the README

### Card 15 · Seed data
**Labels:** Backend
- [ ] Seed 12 product categories
- [ ] Seed 57 products from the printed KCBlendz catalog
- [ ] Seed 37 builder options across 6 types with NG/MU/USD pricing
- [ ] Seed 5 long-form articles
- [ ] Seed ~54 realistic reviews so the UI is never empty
- [ ] Seed the admin user (`admin@kcblendz.com` / `KCBlendz@2026`)

### Card 16 · Auth & sessions
**Labels:** Backend
- [ ] Implement `/register`, `/login`, `/logout`, `/forgot-password`
- [ ] Make `confirm` password field optional (regression on the old bug)
- [ ] CSRF tokens issued in `before_request` and rotated on login
- [ ] Security headers on every response

### Card 17 · Cart & checkout
**Labels:** Backend
- [ ] Cart in the session, with region snapshot
- [ ] `/cart/add`, `/cart/update`, `/cart/remove`
- [ ] `/checkout` (GET + POST) with full guest/registered flows
- [ ] Region-aware delivery fee with free threshold

### Card 18 · Payment flow
**Labels:** Backend
- [ ] `luhn_check()`, `detect_card_brand()`, `validate_card_form()`
- [ ] Sandbox card processing with brand + last4 stored only
- [ ] Bank transfer with mandatory proof upload
- [ ] Order status transitions and customer notifications
- [ ] Admin-only "test cards available" panel on the payment page

### Card 19 · Public API endpoints
**Labels:** Backend
- [ ] `/api/builder/price` JSON pricing
- [ ] `/sitemap.xml` and `/robots.txt`
- [ ] `/newsletter/subscribe`

---

## M4 — Customer dashboard

### Card 20 · Account layout
**Labels:** Frontend, Backend
- [ ] Build `account/_sidebar.html` with all customer links
- [ ] Logout button in brand colours (no red)
- [ ] Sticky on desktop, stacks on mobile

### Card 21 · Orders & reorder
**Labels:** Backend, Frontend
- [ ] `/account/orders` with status pills
- [ ] `/account/orders/<id>` with line items and status timeline
- [ ] Reorder POST that re-adds every item to the cart
- [ ] Empty state with a "Start shopping" CTA

### Card 22 · Saved smoothies
**Labels:** Backend, Frontend
- [ ] List saved blends with region badge and price
- [ ] "Add to cart" inside the same region only
- [ ] "Delete" button with CSRF
- [ ] Empty state encouraging the builder

### Card 23 · Favorites / wishlist
**Labels:** Backend, Frontend
- [ ] `favorites` table with UNIQUE (user, product)
- [ ] POST `/favorites/toggle/<pid>` with JSON response for XHR
- [ ] Heart icon on product detail and product cards
- [ ] `/account/favorites` page
- [ ] Count badge in the navigation

### Card 24 · Reviews
**Labels:** Backend, Frontend
- [ ] `reviews` table with verified-buyer flag
- [ ] `/product/<slug>/review` POST handler
- [ ] Star picker UI with hover preview
- [ ] Rating summary on the product detail page
- [ ] Auto-mark `is_verified_buyer` when the user has a paid order containing the product

### Card 25 · Addresses & profile
**Labels:** Backend, Frontend
- [ ] Multi-address book with default
- [ ] Profile page with password change

---

## M5 — Admin panel & content

### Card 26 · Admin layout & guard
**Labels:** Admin
- [ ] `@admin_required` decorator
- [ ] Sidebar with the 10 admin sections
- [ ] Admin-only top bar with notifications icon

### Card 27 · Dashboard with charts
**Labels:** Admin
- [ ] Revenue today / month / orders today / customers stat cards
- [ ] 7-day revenue chart with Chart.js, **fixed 280 px container** so the chart stops growing forever
- [ ] Brand colours on chart datasets (green, orange, yellow)
- [ ] Recent orders, new customers, notifications

### Card 28 · Product CRUD
**Labels:** Admin
- [ ] List with search, category filter, active toggle
- [ ] New / edit form with image upload OR URL
- [ ] Per-region availability + featured / new / bestseller flags
- [ ] Soft delete

### Card 29 · Order workflow
**Labels:** Admin
- [ ] List with filters (status / region / search)
- [ ] Order detail with line items, payment history, proof preview
- [ ] Status update with notification to the customer
- [ ] Bank transfer verification

### Card 30 · Customer management
**Labels:** Admin
- [ ] List with search and filters
- [ ] CSV export
- [ ] Detail view (orders, blends, addresses)
- [ ] Suspend / activate / delete / promote-to-admin actions

### Card 31 · Wellness Hub CMS
**Labels:** Admin
- [ ] List of all posts (draft + published)
- [ ] Editor with markdown-ish content (## headings, - bullets)
- [ ] Cover image upload
- [ ] Publish toggle

### Card 32 · Reports
**Labels:** Admin
- [ ] 30-day daily revenue chart
- [ ] 12-month monthly revenue chart
- [ ] Top 10 products by units sold
- [ ] Customer growth chart
- [ ] Region revenue comparison

### Card 33 · Admin/customer role separation
**Labels:** Admin, Bug
- [ ] Admin dropdown shows only admin-relevant items (no Favorites, Saved, Addresses, My Orders)
- [ ] Cart and Favorites icons hidden for admins
- [ ] Sandbox payment test-card panel admin-only

---

## Shared

### Card 34 · Unit tests
**Labels:** QA / tests
- [ ] `tests.py` with `unittest`
- [ ] CardValidationTests (Luhn, brand detect, expiry/CVV)
- [ ] PublicRouteTests (all public pages return 200)
- [ ] AuthTests (register single pw / matching / mismatched, 2026 password)
- [ ] AuthorizationTests (role guards)
- [ ] FavoritesAndReviewsTests
- [ ] CustomBlendImageTests
- [ ] RegionHelperTests
- [ ] CartTests
- [ ] SandboxVisibilityTests
- [ ] SeedDataTests
- [ ] All tests must pass before any merge

### Card 35 · Manual QA pass
**Labels:** QA / tests
- [ ] Smoke-test every public page in all three regions
- [ ] Place an order as guest, customer and reorder
- [ ] Submit a review, toggle a favorite
- [ ] Switch regions mid-session and verify the cart resets correctly
- [ ] Test on mobile (320 px), tablet (768 px), desktop (1280 px)

### Card 36 · Documentation
**Labels:** Docs
- [ ] Comprehensive README with quick start, features, tech stack, deployment
- [ ] This Trello board plan
- [ ] GitHub commit plan (125 commits across 5 members)
- [ ] Inline comments on tricky business logic

### Card 37 · Deployment
**Labels:** Backend
- [ ] Configure gunicorn
- [ ] Add a Procfile / start command for Render / Railway
- [ ] Set `KCB_SECRET` and other env vars
- [ ] Deploy and verify the live URL
- [ ] Share the demo link in the README

---

# How to use this board

1. **Daily standup** — everyone briefly mentions what they finished yesterday, what they're working on today, and any blockers. Move your card to "In progress" only when you're really starting work.
2. **Pull requests** — open against `develop`. Add a screenshot in the PR description. Link the Trello card in the PR body.
3. **Review** — at least one teammate must approve before merge. Tick the subtask "code reviewed" on the card.
4. **Move to QA** after merge to `develop`. Tester runs through the card's "definition of done" subtasks.
5. **Done** — only after `develop` → `main` and the feature is live on the demo URL.

---

# Definition of done (applies to every card)

- [ ] Subtasks all ticked off
- [ ] Code merged into `develop`
- [ ] Unit tests added or updated where applicable
- [ ] Manual QA completed in at least two regions
- [ ] Screenshot attached to the Trello card
- [ ] No console errors in the browser
