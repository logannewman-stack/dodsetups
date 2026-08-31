# DOD Setups

The contractor equipment storefront for **Detail on Demand**. One page, zero build step:
contractors pick the turn-key detailing rig that matches their vehicle (Sedan, Truck,
Small Van, Large Van), look up their year/make/model for a recommendation, see every part
itemized with street pricing, and place an order.

Everything lives in **`index.html`** — HTML, CSS, JS and the vehicle database in one file.

## Put it live (GitHub Pages)

1. In this repo: **Settings → Pages**
2. Source: **Deploy from a branch** → pick your branch → folder **/ (root)** → Save
3. Your store is at `https://<user>.github.io/dodsetups/` in about a minute. Send that
   link to contractors. (Any static host works too — Netlify/Vercel/S3 — just serve `index.html`.)

## Take payments

Open `index.html` and find the `CONFIG` block at the top of the `<script>`:

```js
const CONFIG = {
  orderEmail: "logannewman@sudbudsdetailing.com",
  stripeLinks: { sedan:"", truck:"", smallvan:"", largevan:"" },
};
```

- **Out of the box** every Buy button opens an order form and emails you a filled-in,
  itemized work order at `orderEmail`. You invoice the contractor from there. Nothing else
  to set up.
- **Instant card checkout**: create a [Stripe Payment Link](https://dashboard.stripe.com/payment-links)
  for each rig (Sedan $1,299 · Truck $1,849 · Small Van $1,999 · Large Van $2,399) and paste
  the URLs into `stripeLinks`. Any rig with a link goes straight to Stripe checkout instead
  of the email form.

## Change prices, parts, or rigs

All four rigs are defined in the `PACKAGES` array in `index.html` — name, price, parts
street value, capacity chips, the two example vehicles, and the full itemized parts list.
Edit numbers there and every card, finder result, and order email updates automatically.
The "What's inside" section pulls from the `SYSTEMS` array next to it.

## Vehicle finder data

The year/make/model database is the `V` object (33 makes, ~250 models, 1990–2027), each
model tagged with a vehicle class:

`s` sedan/coupe/hatch · `t` truck · `sv` small van · `lv` large van · `mv` minivan ·
`u` SUV/crossover · `ul` full-size SUV

`CLASS_MAP` decides which rig each class gets (minivans and full-size SUVs → Small Van Rig,
crossovers → Sedan Rig, etc.) and the note shown with the recommendation. Add a model by
appending `["Model name","class",firstYear,lastYear]` under its make.

## Preview locally

Open `index.html` in a browser, or:

```sh
npx http-server .
```

## Notes

- Street prices shown on rig cards are recent US retail (Harbor Freight, Sun Joe, WEN,
  fleet-supply tanks) gathered Aug 2026 — recheck them a couple of times a year.
- The footer disclaimer already tells buyers street prices move with the market and the
  package price is what they pay.
