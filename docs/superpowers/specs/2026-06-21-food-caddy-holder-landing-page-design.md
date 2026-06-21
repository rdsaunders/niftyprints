# Food Caddy Holder Landing Page — Design

**Date:** 2026-06-21
**Status:** Approved (design phase)

## Goal

A simple, clean landing page to sell a single 3D-printed food caddy holder, with
card payment handled by Stripe. The emphasis is ease of payment. The design must
allow adding a few more products later without re-architecting.

## Decisions

| Area | Decision |
|------|----------|
| Build approach | Static page, no backend, no build step ("mostly no-code") |
| Payment | Stripe embedded `<stripe-buy-button>` web component |
| Stripe config | Created in Stripe Dashboard; only publishable key + buy-button-id in code |
| Fulfillment | Local pickup only — no shipping, no shipping rates |
| Customer data | Stripe collects email + name (to arrange pickup) |
| Hosting | Netlify (git-connected auto-deploy, custom domain + HTTPS) |
| Content | Placeholder image slots and example copy for now |
| Stack | Plain HTML + CSS, no framework |

## Architecture

Single static site, no server-side code.

```
micro-shop/
├── index.html        # the landing page
├── success.html      # post-payment thank-you page
├── styles.css        # shared styles
└── images/           # product photos (placeholders for now)
```

- The buy action is Stripe's embedded `<stripe-buy-button>` custom element,
  loaded via Stripe's hosted `<script>` tag. Clicking it opens Stripe Checkout
  (overlay/redirect) which collects card details and the customer's email + name.
- No secret keys appear in the repo — only the **publishable key** and the
  **buy-button-id**, both safe for client-side / public repos.
- All payment behaviour (price, currency, fields collected, success URL) is
  configured in the **Stripe Dashboard**, not in code.

### Stripe Dashboard configuration (done by the user, documented for reference)

- Create the product + price (GBP) in Stripe.
- Create a Buy Button for that price.
- Enable collection of **email and name**; disable shipping address.
- Set the **success URL** to the deployed `success.html`.
- Copy the generated `buy-button-id` and `publishable-key` into `index.html`.

## Page structure (`index.html`)

Single-column, mobile-first, one primary call-to-action:

1. **Hero** — product name, one-line tagline, large product photo (placeholder).
2. **Details** — short description + 3–4 benefit bullets
   (e.g. "Fits standard kerbside caddies", "Durable PETG", "Wall or freestanding").
3. **Price + Buy** — price shown clearly with the Stripe Buy Button directly
   beneath it as the single primary action.
4. **Pickup note** — short line: local collection, customer will be emailed to arrange it.
5. **Footer** — simple contact line.

The product section is built as a self-contained **product card** so future
products are added by duplicating the card + adding another Buy Button — no
structural rework.

## Design language

- Clean, generous whitespace, one accent colour, mobile-first.
- System font stack (no external font loading).
- Built using the `artifact-design` skill so it reads as intentional, not templated.
- Self-contained: no external CSS/JS beyond the single Stripe script tag.

## Post-payment

`success.html` — small confirmation page: "Payment received — I'll email you to
arrange pickup." Set as the Stripe success URL.

## Deployment

- Connect the repo to Netlify for auto-deploy on push (or drag-drop the folder).
- Netlify provides HTTPS and custom domain support.

## Out of scope (YAGNI)

- No cart, no inventory/stock tracking, no user accounts.
- No CMS, no backend, no database.
- No shipping logic (pickup only).

## Success criteria

- Page loads as a clean single-product landing page on mobile and desktop.
- Buy Button opens Stripe Checkout and completes a (test-mode) payment.
- After payment, the customer lands on the thank-you page.
- Adding a second product later requires only a new card + Buy Button, no rework.
