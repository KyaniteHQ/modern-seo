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

**Why this matters for AI search:** Local-intent queries (e.g., "moving company near me," "freight forwarder in Izmir") often surface AI responses that pull directly from Business Profile data. A complete, recently-updated profile with photos and reviews dramatically improves these surfaces.

**Multi-location businesses:** create one profile per real physical location, not per service or per keyword. Use bulk verification if you have more than ten.

## Business Agent (emerging)

Google has introduced or is rolling out conversational "Business Agent" experiences where customers can chat with a business directly from Search. Availability and feature set vary by region and category. If your category supports it, configure the agent to handle common questions (hours, availability, quote requests) — it acts as a high-intent intercept right inside Google.

This is moving fast; check the Business Profile help center for current capabilities in your category and region.

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
