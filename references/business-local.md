# Business & Ecommerce Visibility

For ecommerce stores and local-service businesses, AI responses lean heavily on Google's first-party business surfaces: Merchant Center for products, Business Profile for local services. A great website with no listing loses to a worse website that has one.

## Google Merchant Center (ecommerce)

If you sell physical products, get a Merchant Center feed set up before optimizing product pages.

**The minimum:**

- A product feed listing every item with title, description, image, price, availability, brand, GTIN/MPN where applicable.
- Feed delivered via direct upload, scheduled fetch, or Content API.
- Account verification: claim the domain in Search Console and link it in Merchant Center.
- Shipping and tax configured so the listing prices match the website prices.

**Why this matters for AI search:** Google's AI features pull product data, prices, and availability from Merchant Center when responding to shopping-related queries. Pages alone aren't enough — the structured product data feeds the AI surfaces.

**Common platform integrations:**

- Shopify: built-in Google channel app.
- WooCommerce: plugins like Google Listings & Ads.
- Custom storefronts: build a feed XML/JSON and submit via scheduled fetch.

**Product page best practices to pair with the feed:**

- Page URL matches the feed's `link` field.
- Visible price matches the feed price (Merchant Center will disable listings for mismatches).
- `Product` JSON-LD on the page agrees with feed data (don't lie in either).
- Real photos, not just the manufacturer's hero shot.

**If you use AI to generate feed content:** AI-generated product data and images carry labeling requirements. Images need IPTC `DigitalSourceType` metadata set to `TrainedAlgorithmicMedia`, and AI-generated titles/descriptions must be labeled as AI-generated. See [structured-data.md](structured-data.md#labeling-ai-generated-content-in-structured-data) for the details.

## Google Business Profile (local / service businesses)

For any business with a physical location or a defined service area, Business Profile is the most underrated lever.

**The minimum:**

- Verify ownership (mail/phone/email verification).
- Accurate name, address, phone (NAP) that matches what's on the website.
- Service area set correctly (cities/regions, not "everywhere").
- Categories chosen carefully — primary category is the most heavily weighted signal.
- Hours of operation, special hours for holidays.
- Photos of the business, vehicles, products, or premises — real ones, regularly added.
- Services or Products section filled in.
- A few well-written posts per month (offers, updates, events).
- Active review collection and response.

**Why this matters for AI search:** Local-intent queries (e.g., "moving company near me," "freight forwarder in Izmir") often surface AI responses that pull directly from Business Profile data. A complete, recently-updated profile with photos and reviews materially improves these surfaces.

**Multi-location businesses:** create one profile per real physical location, not per service or per keyword. Use bulk verification if you have more than ten.

**Surface A — Preferred Sources (2026-05-27):** Google's Preferred Sources feature, which lets users mark sites as sources they want to see in AI Overviews and AI Mode, was extended to cover both surfaces on 2026-05-27 (and to all languages on 2026-04-30). A business can encourage its audience to mark it as a preferred source — a legitimate publisher-discovery path, not a ranking signal or a hack. It doesn't change how Google's systems rank your content; it shifts which sources a specific user's AI features prefer to surface.

**Trade-off (preferred-sources doc, updated 2026-09-18):** to be eligible as a preferred source in AI Mode and AI Overviews, *"you must make sure your site is included in Search generative AI features in Search Console"* — so excluding the site with the Search generative AI control also gives up the preferred-source highlight there (the doc ties this requirement only to those two features). Only domain- and subdomain-level sites qualify, not subdirectories. To prompt readers, Google offers a two-line JavaScript button (`publisher.js` plus `<div google-add-preferred-source-btn>`), a manual/ES-module mode for custom UI, or a plain deeplink (`https://www.google.com/preferences/source?q=example.com`); none of these is *"required … in order to appear as a preferred source"* (developers.google.com/search/docs/appearance/preferred-sources).

## Business Agent (emerging)

Business Agent is a *"conversational experience on Google Search that allows shoppers to chat with your brand."* It is a **retail** feature attached to your **brand profile** and fed by **Merchant Center** — not by Google Business Profile (support.google.com/googleshopping/answer/16410382, undated; current as of 2026-10). Gemini models answer product questions from your Merchant Center data and your website when people search for your brand; it is rolling out in the US first.

To customize it you need a US-based e-commerce store, a verified Merchant Center account, at least 50 approved free listings, and a claimed brand profile. Set it up in Merchant Center (Marketing > Business Agent), from your brand profile card in Search, or on Shopify through the Google & YouTube app. Until you customize it, shoppers see a standard Google-branded version. Google lists training on your own business data, performance insights, and agentic checkout as "coming soon."

Google has also announced Business Agent formats inside ads — **Business Agent for Leads** in Search ads (Google Marketing Live, 2026-05-20) and **Business Agent for YouTube Ads** (business.google.com, 2026-09-02). These are ad products, separate from the organic brand-profile agent.

This is moving fast; the "Get started with Business Agent" article in the Merchant Center or Brand profile help center has current eligibility and features. It is not in the Business Profile help center.

These conversational surfaces are one face of a broader shift: agents increasingly transacting on behalf of users, not just answering questions. See [agentic-experiences.md](agentic-experiences.md) for how that plays out beyond Search.

## Agentic checkout: UCP and ACP

UCP (Universal Commerce Protocol) is an open standard launched 2026-01-11, co-developed by Google and Shopify, with Etsy, Wayfair, Target, and Walmart as additional launch contributors. On Google it powers a **"Buy" checkout directly on AI Mode in Search and in Gemini** (web and mobile app), where you stay the seller of record, and a **cart-transfer** path that sends a Google-built cart to your own site (developers.google.com/merchant/ucp/faq, updated 2026-10-16). Google's separately named "agentic checkout" feature, where an agent buys on your own website, is a different thing. Google's AI optimization guide names UCP as the standard for agentic commerce. Only products flagged with the Merchant Center attribute `native_commerce(checkout_eligibility)` show the "Buy" button, and the program covers products eligible in the US, Canada, and Australia (support.google.com/merchants/answer/16837055, undated; current as of 2026-10). The newest spec is **v2026-08-25**, but **Google's merchant docs still require UCP `2026-04-08`** (developers.google.com/merchant/ucp/guides/overview/cart-overview, updated 2026-10-16). Google states that opting in *"does not influence how we rank your offers in product listings."* UCP lets AI agents discover products, negotiate, check out, and pay without a bespoke per-merchant integration.

The concrete hooks, without over-detailing:

- **Discovery** via a `/.well-known/ucp` profile.
- **Reverse-domain capability names** like `dev.ucp.shopping.checkout`.
- **Payment handlers** (e.g. `com.google.pay`, `dev.shopify.shop_pay`) sitting under a decoupled "Trust Triangle" so the platform never touches raw card numbers.
- **AP2 mandates** (`dev.ucp.common.payment.ap2_mandate` since UCP v2026-08-25; `dev.ucp.shopping.ap2_mandate` in v2026-04-08) — the checkout-and-payment mandate/authorization security layer (Checkout Mandate + Payment Mandate) that plugs into UCP as an extension, not a competing protocol. Google contributed AP2 to the FIDO Alliance on 2026-04-28; FIDO's Payments TWG (chaired by members from Mastercard and Visa) is still developing the specifications. v0.2 (2026-04-28, still the latest release as of 2026-10-17) added "Human Not Present" autonomous payment flows.
- **A `continue_url` fallback** to normal web checkout.

**ACP (Agentic Commerce Protocol)** is a sibling full-stack agentic-commerce protocol co-created by Stripe and OpenAI, launched 2025-09-29. OpenAI's native "Instant Checkout" surface was deprecated and repositioned — transactional integration now happens via ChatGPT apps; ACP also powers product-discovery feeds. Shopify and Stripe merchants get platform coverage without hand-rolling the protocol. UCP and ACP are parallel efforts at the protocol level; the practical stance for both is the same: let platforms absorb the integration.

**Practical stance: watch, don't build.** For most merchants this is not something to hand-roll against a still-changing spec. If you're on a platform like Shopify, UCP support will most likely arrive through the platform. The durable action today is to keep a clean, agent-operable normal web checkout — that's the `continue_url` fallback every agent flow falls back to anyway. See [agentic-experiences.md](agentic-experiences.md) and [agentic-web-standards.md](agentic-web-standards.md).

This is emerging, not a current ranking or visibility requirement. Treat it as something to track, not a deadline.

## Reviews

Reviews are signals across all of these surfaces. The mechanics that matter:

- **Quantity over time** (a steady drip beats a sudden burst, which can look manipulative).
- **Response rate** (respond to most reviews, especially negative ones — calm, specific, helpful).
- **First-party reviews** on Business Profile, not just on third-party sites.
- **Don't incentivize positive reviews** in violation of Google's policies. Asking customers to leave honest feedback is fine; offering discounts in exchange is not.

## Website ↔ business surface consistency

The most common avoidable mistake: NAP inconsistency.

- Same business name everywhere (website footer, schema, Business Profile, Merchant Center, social profiles).
- Same address format.
- Same phone number (including formatting — pick one canonical form).
- Same category/positioning across surfaces.

Inconsistency tells Google's systems "I'm not sure these are the same entity," which weakens the knowledge graph signals that feed AI surfacing.

## Priority order for a new local/ecommerce business

If a small business is starting from zero, the order is roughly:

1. Business Profile claimed, verified, and filled out (services, hours, photos, categories).
2. Website with `Organization` + `LocalBusiness` (or relevant subtype) schema, NAP matching the profile.
3. Merchant Center feed (if ecommerce) with prices/availability synced.
4. Review collection workflow in place.
5. Then on-page content optimization.

Skipping straight to step 5 is the most common mistake and the slowest path to results.
