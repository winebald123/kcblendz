# KCBlendz GitHub commit plan

A realistic 125-commit history split across the five-person team. Each member
contributes ~25 commits on their own feature branch, with daily pull requests
merged into `develop`. Final integration merges into `main`.

## Branch strategy

- `main` — the release branch, protected. Only the team lead merges here.
- `develop` — integration branch. All feature branches merge here through PRs.
- `feat/m1-public-ui`, `feat/m2-builder`, `feat/m3-backend`, `feat/m4-account`,
  `feat/m5-admin` — one long-lived branch per member.

## Commit message conventions

`type(scope): short imperative summary`

- `feat` — new feature
- `fix` — bug fix
- `refactor` — internal change, no behaviour change
- `style` — visual / CSS only
- `chore` — tooling, dependencies, configuration
- `test` — adding or updating tests
- `docs` — documentation

Keep summaries under 70 characters. No emojis. No exclamation marks. Reference
the Trello card in the body of the commit when useful.

---

## Member 1 — Public UI (commits 1–25)

```
1.  chore(repo): scaffold flask project, requirements, gitignore
2.  feat(base): add base layout with brand palette and CSS variables
3.  feat(base): add CSS aliases for backwards compatibility
4.  feat(nav): build sticky top navigation with logo and search
5.  feat(nav): add region indicator and mobile hamburger menu
6.  feat(footer): build footer with newsletter strip and payment badges
7.  feat(store): build region picker with SVG flags
8.  feat(home): build hero, featured products and trust strip
9.  feat(home): add testimonials with realistic regional names
10. feat(shop): build filterable product grid with category chips
11. feat(shop): build product card partial with onerror fallback
12. feat(product): build product detail with badges and related items
13. feat(public): add about, contact and FAQ pages
14. feat(public): add privacy, terms, refund, shipping policy pages
15. feat(public): add WhatsApp floating button site-wide
16. style(brand): align all buttons and inputs to brand palette
17. style(nav): unify icon-button style across favorites and cart
18. style(footer): switch address to ALU Kongo, Pamplemousses
19. fix(images): replace broken Choco Gain and Mint Flush photos
20. fix(images): add onerror fallback on every product image
21. style(home): swap stock video for the real KCBlendz video
22. feat(perf): add preconnect hints for Tailwind, fonts, Unsplash
23. feat(perf): cache /static for one week with immutable header
24. style(mobile): tighten spacing and fix overflow on small screens
25. docs(public): comment partials and layout block contracts
```

## Member 2 — Smoothie builder and motion (commits 26–50)

```
26. feat(builder): scaffold two-column builder page layout
27. feat(builder): add cup size step with prices
28. feat(builder): add fruit selector with 1-3 constraint and counter
29. feat(builder): add base, sweetener, add-on, booster steps
30. feat(builder): wire quantity input and submit button enable rule
31. feat(api): add /api/builder/price live JSON pricing endpoint
32. feat(builder): debounce live price recalculation on chip click
33. feat(builder): show order summary lines beneath the preview
34. feat(builder): embed real KCBlendz video in the live preview panel
35. feat(builder): set preload metadata so video does not block paint
36. feat(builder): add Your Blend caption that updates per fruit choice
37. feat(builder): brand gradient overlay so caption stays legible
38. fix(builder): remove sticky-bottom CTA that overlapped form
39. feat(builder): add inline hint beneath disabled submit button
40. feat(blends): map dominant fruit to a real cart image
41. refactor(blends): extract image_for_blend helper and constants
42. feat(blends): use the helper in builder add-to-cart route
43. feat(blends): use the helper when reordering saved smoothies
44. style(chips): brand-coloured chip states (selected, hover, disabled)
45. style(motion): add bubble float animation behind the preview
46. style(motion): add fade-in transition on summary changes
47. fix(builder): clamp fruit selection to 3 with toast on overflow
48. fix(builder): persist state across accidental refresh via session
49. test(builder): unit test image_for_blend for known and unknown fruits
50. docs(builder): document the JSON payload of the price API
```

## Member 3 — Backend, database, payments (commits 51–75)

```
51. feat(db): define users, addresses, products and categories tables
52. feat(db): define orders, order items and payments tables
53. feat(db): define custom smoothies and builder options tables
54. feat(db): define blog posts and notifications tables
55. feat(db): define audit_logs, contact_messages, newsletter tables
56. feat(db): add favorites and reviews tables with indices
57. feat(db): seed admin user, categories and full product catalogue
58. feat(db): seed builder options for all three regions
59. feat(db): seed five long-form wellness articles
60. feat(db): seed 54 realistic sample reviews
61. feat(auth): build register, login, logout, forgot password routes
62. fix(auth): make confirm-password optional to fix signup regression
63. feat(auth): rotate CSRF token on login for hardening
64. feat(cart): implement session cart with region snapshot
65. feat(checkout): build guest and registered checkout flows
66. feat(payments): add luhn_check, detect_card_brand, validate_card_form
67. feat(payments): require real card details on sandbox card flow
68. feat(payments): require proof upload before bank-transfer order completes
69. feat(payments): store only brand and last 4 digits in payments table
70. feat(boot): run init_db at import for clean gunicorn / Railway boot
71. feat(boot): add before-request self-heal if database is empty
72. feat(sec): add X-Frame, X-Content-Type, Referrer, Permissions headers
73. feat(sec): cache-control immutable for /static assets
74. feat(md): add inline markdown filter for bold / italic / code
75. test(backend): cover card validation, region helpers, seeds
```

## Member 4 — Customer dashboard and account (commits 76–100)

```
76.  feat(auth-ui): build login page with brand palette
77.  feat(auth-ui): build register page with confirm-password field
78.  feat(auth-ui): build forgot-password page
79.  feat(account): scaffold account sidebar with role-aware links
80.  feat(account): build dashboard with overview cards
81.  feat(account): build orders list and order detail pages
82.  feat(account): add reorder button that re-adds every line item
83.  feat(account): build saved smoothies list with region badge
84.  feat(account): build address book with default flag
85.  feat(account): build profile and password-change page
86.  feat(account): build notifications page with auto-clear on visit
87.  feat(fav): build favorites table, route and XHR toggle
88.  feat(fav): add heart button to product detail page
89.  feat(fav): add count badge to navigation
90.  feat(fav): build /account/favorites grid with one-click add to cart
91.  fix(fav): align favorites layout with other account pages
92.  feat(reviews): build reviews table and submit route
93.  feat(reviews): add star picker UI with hover preview
94.  feat(reviews): show verified-buyer badge for paid orders
95.  feat(reviews): expose rating summary on product cards
96.  fix(account): keep customer session region after login
97.  style(account): replace red logout with brand colours
98.  style(account): empty states for orders, favorites and saved
99.  test(account): cover favorites toggle, reviews submission
100. test(account): cover signup regression and 2026 admin password
```

## Member 5 — Admin panel, CMS, reports, deployment (commits 101–125)

```
101. feat(admin): scaffold admin layout with sidebar and top bar
102. feat(admin): add admin_required decorator on every admin route
103. feat(admin): build dashboard with stat cards and 7-day chart
104. fix(admin): wrap dashboard chart in fixed-height container
105. feat(admin): product CRUD with image upload or URL
106. feat(admin): per-region availability and featured / new / bestseller flags
107. feat(admin): order list with status, region and search filters
108. feat(admin): order detail with status workflow and customer notification
109. feat(admin): bank-transfer proof preview and verification
110. feat(admin): customer list with search, filter and CSV export
111. feat(admin): customer detail with suspend / activate / delete / promote
112. feat(admin): wellness blog list and editor
113. feat(admin): builder options CRUD per region
114. feat(admin): contact-message inbox with handled flag
115. feat(admin): notification feed for admin events
116. feat(admin): build reports page with region, growth, daily, monthly
117. fix(admin): wrap every reports chart in fixed-height container
118. style(admin): switch chart palette to brand green / orange / yellow
119. feat(admin): build admin profile page with name / phone / password change
120. feat(admin): add Profile link in admin sidebar and dropdown
121. fix(admin): hide cart and favorites icons from admin in nav
122. fix(admin): admin dropdown shows only admin-relevant links
123. test(admin): cover role separation and sandbox visibility
124. chore(deploy): add gunicorn start command and Railway notes
125. docs(readme): comprehensive README with quick start and tests
```

---

## Daily pull-request rhythm

Each member opens **one PR per working day** against `develop`. The PR template:

```markdown
## What
One-sentence summary of the change.

## Why
The Trello card this resolves.

## Screenshots
At least one for any visual change.

## Tests
- [ ] All existing tests still pass: `python -m unittest tests.py`
- [ ] New tests added if business logic changed
```

At least one teammate approves before merge. After merging, the corresponding
Trello card moves to "Review" then "QA" then "Done".

## Final integration

Once every feature branch is merged into `develop` and `python -m unittest tests.py`
reports `OK`, the team lead opens a single PR merging `develop` into `main` titled
**"release: v1.0 production-ready"**. That merge tags the project for
submission.
