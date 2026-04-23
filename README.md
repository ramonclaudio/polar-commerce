# Polar Commerce

I wanted to see if I could build e-commerce on Polar despite the missing cart system. Polar handles payments beautifully but has zero cart: no multi-item checkout, no cart management, nothing. So I built a workaround: multiple cart items bundle into a single ephemeral Polar product at checkout, full cart composition stored in checkout metadata, reconstructed server-side after payment confirmation via webhook. Ugly but it works.

Convex's real-time subscriptions turned out to be perfect for cart/wishlist state. Add an item on your phone, see it instantly on desktop. No polling, no refresh. Better Auth with the Convex adapter made guest-to-authenticated cart merging straightforward. And Next.js 16 canary gave me React Compiler, Turbopack dev/build caching, component-level caching, and PPR to push perf and UX further.

Experimental e-commerce platform built with Next.js 16, Convex, Better Auth, and Polar.

## Stack

- Next.js 16.0.1-canary.2 (React Compiler, Turbopack caching, component-level caching, PPR)
- React 19.2 (`useEffectEvent`, View Transitions API)
- Experimental: `cacheLife`, `cacheTag`, typed routes, inline CSS
- Convex (real-time database, type-safe schema)
- Better Auth via Convex adapter
- Polar (payments, subscriptions, one-time purchases)

## Setup

```bash
git clone https://github.com/ramonclaudio/polar-commerce.git
cd polar-commerce
npm install
cp .env.example .env.local
```

Seed products and run:

```bash
npm run polar:seed
npm run dev
```

`npm run polar:seed` reads JSON product definitions, uploads images to Polar's CDN, creates products via Polar API, and syncs everything to Convex.

## What works

- Cart with real-time sync across devices/tabs
- Wishlist
- Full auth flow (email / OAuth / 2FA)
- Guest-to-auth cart merge on sign-in (no lost items, no duplicates)
- Checkout with item bundling
- Order reconstruction from webhooks
- Subscription and one-time payments

## Known issues

- **Account menu positioning**: user menu dropdown renders in the top-left corner when navigating to protected pages

## What's missing

- Cart expiration and cleanup
- Bundle lifecycle management
- Per-item tax calculations
- Partial refund logic

## Credits

Started from [@estebansuarez](https://github.com/estebansuarez)'s [v0 template](https://v0.app/templates/storefront-w-nano-banana-ai-sdk-ai-gateway-XAMOoZPMUO5).

## License

MIT
