# Creaiva Marketplace

A full-stack UAE classifieds marketplace for products, home and living, properties, equipment, motors, fashion and jobs. Built with React, Vinext, Cloudflare D1 and R2, hosted with ChatGPT Sites.

## Working features

- Responsive storefront, category browsing, keyword/location/condition/price filters and sorting.
- Listing detail pages with multi-photo galleries. Sample headphone listing demonstrates matching front/side/rear views.
- Secure platform-owned ChatGPT sign-in and account creation pages. A Creaiva profile is created on first authenticated visit. No separate password database.
- Per-account seller dashboard; create, edit, publish, pause, mark sold and delete listings.
- Real image uploads to R2, server-side ownership validation and atomic total-storage quota reservations.
- Private unused uploads, publicly visible active-listing images, photo library and storage release on deletion.
- Saved listings and durable buyer/seller messages. Job applications use the same private inquiry flow.
- Starter (20 MiB), Plus (200 MiB), Business (1 GiB) total storage. Individual images: 10 MiB maximum; JPG, PNG, WebP only; up to 10 photos/listing.
- Stripe subscription checkout, signed webhook processing and billing portal integration when runtime values below are configured. Until then, paid plans transparently accept upgrade-interest requests without charging or changing quotas.

Sample records are explicitly labeled and stored separately from member listings. They cannot receive messages, applications or payments. Asset source and image-generation prompt records are in `asset-notes.json`.

## Local development

Node >=22.13 is required. Use the package manager specified in `package.json`. Install dependencies, then:

```sh
npm run db:generate   # only when schema changes
npm run build
```

Apply each pending SQL migration to the local D1 using the starter's Wrangler workflow. See the original starter documentation in `docs/platform-starter.md`. `npm run dev` starts the local app. The production hosting manifest is `.openai/hosting.json`; retain its project identity.

On a standalone local environment, the starter simulates sign-in for loopback development. Managed Sites preview uses platform sign-in; there is no production demo-account bypass. Hosted authentication headers must be supplied only by the trusted Sites dispatch layer. Do not expose this Worker directly behind an untrusted proxy without replacing that boundary.

## Validation

```sh
node node_modules/typescript/bin/tsc --noEmit
npm run build
node tests/integration.mjs
```

The integration test runs the compiled production Worker against isolated in-memory D1/R2 fixtures with two synthetic users. It verifies authorization, listing/image ownership, private uploads, CRUD, messages, saved items, concurrent quota enforcement, deletion cleanup and disabled-billing behavior. It never touches production data.

## Activate paid storage subscriptions

Configure production **runtime secrets** in Sites, never in source:

- `STRIPE_SECRET_KEY`
- `STRIPE_WEBHOOK_SECRET`
- `STRIPE_PRICE_PLUS` — recurring Stripe Price ID for the 200 MiB plan
- `STRIPE_PRICE_BUSINESS` — recurring Stripe Price ID for the 1 GiB plan

Choose actual prices/currency and recurring intervals in the merchant's Stripe account first. Configure Stripe's webhook endpoint as `https://<site-origin>/api/billing/webhook` for `checkout.session.completed`, `customer.subscription.created`, `customer.subscription.updated`, and `customer.subscription.deleted`. Enable the Stripe customer billing portal. Deploy a saved Site version after configuring secrets.

Checkout never upgrades a user directly. Verified webhook processing fetches the subscription from Stripe and derives the quota from an allowlisted Price ID and subscription status. Cancellation or inactive subscription returns the account to Starter; existing uploads remain, and new uploads are blocked while over quota. Payment has not been configured or live-tested yet.

## Scope and launch status

This is a classifieds/inquiry marketplace. Buyers contact sellers to arrange a purchase; property inquiries and job applications are messages. There is no product cart/checkout, shipping, escrow, refunds, or purchase protection. Standalone email/password authentication and resume-file attachments are not implemented.

New Sites are owner-private by default. Public audience access must be configured before inviting other marketplace users. Before public commercial launch, add business-specific terms/privacy, moderation and reporting, category policies, support operations, and production payment verification. No real reviews, sales metrics or seller verification badges are fabricated.
