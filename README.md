# micro-shop

A simple, single-product landing page that sells a 3D-printed food-waste caddy
holder and takes card payments through Stripe. No backend, no build step — just
static files.

```
index.html     landing page (the product card lives here)
success.html    post-payment thank-you page
styles.css      design system + page styles
images/         product photos (add your own)
```

## Going live — three steps

### 1. Set up the Stripe Buy Button

1. In the [Stripe Dashboard](https://dashboard.stripe.com/), create a **Product**
   with a price in GBP.
2. Go to **Payment Links → Buy button**, create a button for that price.
3. In the button settings:
   - **Collect customer info:** enable **email** and **name** (for arranging pickup).
   - **Shipping:** leave address collection **off** — this is local pickup only.
   - **After payment → Confirmation page:** set it to your deployed
     `https://your-site/success.html`.
4. Copy the **Buy button ID** and your **publishable key** from the embed snippet.

### 2. Paste the keys into `index.html`

Find the `<stripe-buy-button>` element and replace the placeholders:

```html
<stripe-buy-button
  buy-button-id="buy_btn_xxx"
  publishable-key="pk_live_xxx">
</stripe-buy-button>
```

Both values are safe to commit — they're public client-side keys. Your **secret**
key never appears here. The `£5` price text above the button and the `price` in
the JSON-LD block (in `<head>`) must both match the price set in Stripe.

### 3. Deploy to Netlify

- Connect this repo in Netlify (**Add new site → Import from Git**) and deploy.
  There's no build command — set the publish directory to the repo root.
- Or drag-and-drop the folder onto the Netlify dashboard.
- Netlify gives you HTTPS and a custom domain.

Test with Stripe **test mode** keys first (card `4242 4242 4242 4242`), then swap
to live keys when you're ready.

## Adding more products later

In `index.html`, duplicate the `<article class="product">` block and give it its
own `<stripe-buy-button>` (one per product). The layout is already product-card
shaped, so no restructuring is needed.

## Add product photos

Drop an image into `images/` and, in `index.html`, replace the placeholder
`<div class="photo">…</div>` with:

```html
<img class="photo" src="images/caddy-holder.jpg" alt="3D-printed food-waste caddy holder" />
```

## Local preview

Open `index.html` in a browser, or run a static server from the repo root:

```sh
python3 -m http.server 8000   # then visit http://localhost:8000
```
