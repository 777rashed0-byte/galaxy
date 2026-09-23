# Galaxy — Poster Shop (starter project)

Galaxy sells posters with customizable frames. This repo is a working
starting point, not a finished store. It is built to be easy to read
and easy to extend.

## Honesty and disclosure rules baked into this project

- The product copy never says "handmade" or "hand-framed." Posters are
  printed and framed by a print partner (drop-ship or in-house — you
  choose in `docs/API_DESIGN.md`). The product page template includes
  a "How it's made" line so this stays truthful.
- `docs/PRIVACY.md` and `docs/TERMS.md` are real, plain-language
  starter policies — not filler text. Edit the company details before
  launch, and have a lawyer review before you go live.
- Payments go through Stripe Checkout / Stripe Elements. The backend
  never stores card numbers, and never logs full card data,
  `client_secret` values, or webhook payloads containing card data.

## Confirmed stack

| Layer | Choice | Why |
|---|---|---|
| Frontend | React + TypeScript + Vite | fast dev loop, no framework lock-in like Next.js |
| Styling | Plain CSS with CSS variables (no Tailwind) | keeps the starter dependency-light; swap in Tailwind later if you want |
| Backend | Node.js + Express + TypeScript | simple, well understood, easy to deploy anywhere |
| Database | PostgreSQL (schema in `backend/src/db/schema.sql`) | relational fit for orders/products; swap for SQLite in dev if you prefer |
| Payments | Stripe (Checkout Sessions) | PCI-DSS compliance is handled by Stripe, not by us |
| Email | Resend or Nodemailer (SMTP) — code supports either | order confirmation emails |

If you'd rather use Next.js, Supabase, or a different payment
processor, that's a small swap — say the word and I'll adjust the
scaffold.

## Project layout

```
galaxy-posters/
  backend/
    src/
      server.ts          Express app entry point
      routes/             products, cart, checkout endpoints
      services/           stripe.ts, email.ts
      db/schema.sql        Postgres schema
    package.json
  frontend/
    src/
      components/          ProductList, ProductDetail, Cart, Checkout, OrderConfirmation
      api/client.ts         typed fetch wrappers
      App.tsx
      index.css
    package.json
  docs/
    API_DESIGN.md
    PRIVACY.md
    TERMS.md
```

## MVP plan

**Phase 1 — static catalog + client-side cart (this scaffold)**
Product list and detail pages read from the backend's `/api/products`
endpoint (seeded, no real DB required yet). Cart lives in React state
and `localStorage`. No real payment.

**Phase 2 — real payments + email + basic admin**
Wire `POST /api/checkout` to Stripe Checkout Sessions. Add a Stripe
webhook handler that marks orders paid and triggers the confirmation
email. Add a small password-protected admin route for order/product
management.

**Phase 3 — accounts and order history**
Add user auth (email + password or magic link), order history page,
saved addresses, and refine accessibility/responsive polish.

## Running it (once you fill in real dependencies)

```bash
# backend
cd backend && npm install && npm run dev

# frontend (separate terminal)
cd frontend && npm install && npm run dev
```

You'll need a `.env` file in `backend/` (see `backend/.env.example`)
with your Stripe secret key and database URL. Never commit `.env`.
