<!-- mcp-name: com.stayingapi/hotel-vacation-rental-mcp -->

<p align="center">
  <a href="https://stayingapi.com">
    <img src="public/brand/logo-512.png" width="160" height="160" alt="StayingAPI" />
  </a>
</p>

<h1 align="center">Hotel &amp; Vacation Rental MCP - live prices, availability &amp; reviews across Booking.com, Airbnb, Vrbo &amp; Google Hotels</h1>

<p align="center">
  <b>Search stays, compare prices across Booking.com, Airbnb, Vrbo and Google Hotels, and pull reviews by just asking your AI. One hosted MCP, 300 free credits to start, no card.</b><br/>
  Seven read-only tools - search, availability, listing detail, price, cross-OTA price compare, reviews, and job polling - for Claude, ChatGPT, Cursor, Claude Code, and any MCP client.
</p>

<p align="center">
  <a href="https://cursor.com/en/install-mcp?name=stayingapi&config=eyJ1cmwiOiJodHRwczovL21jcC5zdGF5aW5nYXBpLmNvbS9tY3AifQ=="><img alt="Install in Cursor" src="https://img.shields.io/badge/Cursor-Install_MCP-000000?style=for-the-badge&logo=cursor&logoColor=white"/></a>
  <a href="https://stayingapi.com/signup"><img alt="300 free credits, no card" src="https://img.shields.io/badge/300_free_credits-no_card-16A34A?style=for-the-badge"/></a>
</p>

<p align="center">
  <a href="https://stayingapi.com"><img src="https://img.shields.io/badge/Website-stayingapi.com-0061FF?style=for-the-badge" alt="Website"/></a>
  <a href="https://stayingapi.com/docs/mcp"><img src="https://img.shields.io/badge/Docs-MCP_Guide-06B6D4?style=for-the-badge&logo=readthedocs&logoColor=white" alt="Docs"/></a>
  <a href="https://api.stayingapi.com/openapi.json"><img src="https://img.shields.io/badge/OpenAPI-Spec-85EA2D?style=for-the-badge&logo=openapiinitiative&logoColor=white" alt="OpenAPI"/></a>
  <a href="./LICENSE"><img src="https://img.shields.io/badge/License-MIT-4CAF50?style=for-the-badge" alt="MIT License"/></a>
  <a href="https://glama.ai/mcp/servers/stayingapi/hotel-vacation-rental-mcp"><img alt="Glama quality score" src="https://glama.ai/mcp/servers/stayingapi/hotel-vacation-rental-mcp/badges/score.svg"/></a>
</p>

> **4 booking platforms, one schema · 7 read-only tools · cross-OTA price compare · OAuth 2.1, no key pasted into the agent · 300 free credits to start, no card**
> [StayingAPI](https://stayingapi.com) is an independent service for accommodation data. StayingAPI is not affiliated with, endorsed by, or sponsored by Airbnb, Booking.com, Vrbo, or Google Hotels. These are trademarks of their respective owners, used here descriptively to indicate the data sources StayingAPI's API and MCP server can query.

---

## Why a Hotel & Vacation Rental MCP

You are already comparing stays across four sites by hand: a hotel on Booking.com, an apartment on Airbnb, a villa on Vrbo, and Google Hotels to sanity-check the rate. Every one of them has a different page, a different rating scale, and no API you can just sign up for.

**Connect once, then just ask.** Add this MCP server to Claude, ChatGPT, Cursor, or any MCP client a single time, and your AI assistant can search real stays, quote real prices for your dates, compare one property's rate across booking sites, and read normalized reviews - inside the conversation, with no code and no repeated setup.

Everything comes back in **one unified schema**. A hotel room and a holiday villa return the same object shape, ratings carry their native scale instead of being silently rescaled, and the cross-OTA comparison returns each site's offer plus a computed lowest and median price, so you do not re-derive them yourself.

**Quick taste:**

```txt
Find 2-bed stays in Lisbon for these dates under EUR 150,
compare the top one's price across Airbnb, Booking.com and Vrbo,
and summarize its reviews.
```

That single prompt spans three tools - `search_stays`, `compare_prices`, `get_reviews` - without you writing a line of code.

---

## Quick Install

> **Requirements:**
>
> - A StayingAPI account ([sign up](https://stayingapi.com/signup) - 300 free credits, no card)
> - **Either** OAuth (Claude, ChatGPT - no key handling at all) **or** an API key (`stay_live_...` / `stay_test_...`) from your dashboard

> **Recommended: add a rule so your AI invokes it automatically**
>
> Paste this into your client's custom instructions or rules file:
>
> ```txt
> When I mention a hotel, an Airbnb/Booking.com/Vrbo/Google Hotels listing,
> a place to stay, travel dates, or ask to compare accommodation prices or
> read stay reviews, automatically use the StayingAPI MCP tools to fetch
> real data before answering.
> ```

**Server URL:** `https://mcp.stayingapi.com/mcp` (Streamable HTTP transport)

<details>
<summary><b>Install in Claude (Desktop &amp; Web) - Recommended</b></summary>

Settings → Connectors → **Add custom connector** → paste:

```
https://mcp.stayingapi.com/mcp
```

Claude runs the OAuth 2.1 sign-in for you in a browser popup, which links the connector to your StayingAPI account and its credit balance - **no API key needed.** The seven read-only tools then appear in your tool list.

Prefer the desktop config file? Add to `claude_desktop_config.json`:

```json
{
  "mcpServers": {
    "stayingapi": {
      "url": "https://mcp.stayingapi.com/mcp"
    }
  }
}
```

</details>

<details>
<summary><b>Install in Claude Code (CLI)</b></summary>

OAuth (recommended):

```bash
claude mcp add --transport http stayingapi https://mcp.stayingapi.com/mcp
```

Then run `/mcp` inside Claude Code and complete the OAuth prompt.

Prefer a bearer key, or running headless?

```bash
claude mcp add --transport http stayingapi https://mcp.stayingapi.com/mcp \
  --header "Authorization: Bearer stay_live_YOUR_KEY_HERE"
```

</details>

<details>
<summary><b>Install in ChatGPT</b></summary>

1. Enable **Developer Mode**: Settings → Apps & Connectors → Advanced settings.
2. Go to **Settings → Connectors → Add MCP server** (Create in the Connectors panel).
3. Paste the server URL:

```
https://mcp.stayingapi.com/mcp
```

4. Complete the OAuth sign-in when prompted - **no API key needed.**

</details>

<details>
<summary><b>Install in Cursor (One-Click or Manual)</b></summary>

**One-click:** use the [Install in Cursor](https://cursor.com/en/install-mcp?name=stayingapi&config=eyJ1cmwiOiJodHRwczovL21jcC5zdGF5aW5nYXBpLmNvbS9tY3AifQ==) badge at the top of this README.

**Manual:** add to `~/.cursor/mcp.json` (global) or `.cursor/mcp.json` (per project):

```json
{
  "mcpServers": {
    "stayingapi": {
      "url": "https://mcp.stayingapi.com/mcp"
    }
  }
}
```

To use a bearer key instead of the OAuth flow, add a `headers` object with `"Authorization": "Bearer stay_live_YOUR_KEY_HERE"`.

</details>

<details>
<summary><b>Any other MCP client</b></summary>

Add `https://mcp.stayingapi.com/mcp` as a **Streamable HTTP** MCP server.

- Clients that support OAuth 2.1 with Dynamic Client Registration authenticate automatically - nothing to configure.
- Clients that do not can present a bearer key instead: `Authorization: Bearer stay_live_...`.

Only formats verified against [stayingapi.com/docs/mcp](https://stayingapi.com/docs/mcp) are listed above on purpose. If your client is not here, the generic Streamable HTTP setup is the whole configuration.

</details>

---

## Authentication

Two ways in, both ending at the same account and the same credit balance:

- **OAuth 2.1 + PKCE (S256) with Dynamic Client Registration** - recommended for Claude and ChatGPT. You authorize once in a browser popup and the connector is linked to your StayingAPI account. DCR-aware clients discover everything automatically, so **no key is ever pasted into the agent**.
- **Bearer API key** - for key-based clients and scripts. Keys are `stay_live_...` (real fan-out to live sources) or `stay_test_...` (deterministic sandbox fixtures, always 0 credits). Send as `Authorization: Bearer stay_live_...`.

**One balance, separate rate bucket.** MCP calls draw from the same single credit balance as your REST keys. REST is rate-limited per key; MCP is rate-limited per user - separate buckets, one wallet.

> **Security note:** treat `stay_live_` keys like passwords. A new key's secret is shown once and stored only as a SHA-256 hash, so it is never recoverable. Never commit one to a repo or paste it into a shared config; revoke and re-mint from the dashboard if a key leaks.

---

## Available Tools

All seven are **read-only** (annotated `readOnlyHint: true`, `idempotentHint: true`). There are no write tools, so nothing here can book, cancel, or change anything. Each tool maps 1:1 to a REST endpoint, runs the same validation and cache pipeline, and returns the same unified `{ data, meta }` envelope.

### 1. `search_stays`

Discover properties across platforms by location, dates, occupancy and filters, merged into one schema. This is the breadth endpoint and the clearest demonstration of "one schema, every platform".

| Param | Type | Required | Notes |
| --- | --- | --- | --- |
| `location` | string | yes | Place name (`Split, HR`) or `lat,lng` |
| `checkIn` / `checkOut` | date | no | `YYYY-MM-DD`; required together; not in the past |
| `adults` / `children` / `childAges[]` / `rooms` | integer | no | `childAges[]` length must equal `children` |
| `propertyType[]` | enum[] | no | `hotel` \| `apartment` \| `house` \| `villa` \| `cottage` \| `other` |
| `amenities[]` | enum[] | no | Canonical amenity taxonomy |
| `minBedrooms` / `priceMin` / `priceMax` / `minGuestRating` | number | no | Filters applied post-normalization |
| `platforms[]` | enum[] | no | `airbnb` \| `booking` \| `vrbo` \| `google`; drives fan-out |
| `limit` / `cursor` / `sort` / `currency` | mixed | no | `limit` 1-40; `sort` = `recommended` \| `price_asc` \| `price_desc` \| `rating_desc` |

Response (trimmed):

```jsonc
{
  "data": [
    {
      "id": "stays_booking_abramovic2",
      "platform": "booking",
      "platformListingId": "abramovic2",
      "url": "https://www.booking.com/hotel/hr/abramovic2.html",
      "name": "Apartments Abramović",
      "propertyType": "apartment",
      "location": { "lat": 43.51, "lng": 16.44, "city": "Split", "country": "HR" },
      "starRating": null,
      "guestRating": 9.1, "ratingScale": 10, "reviewCount": 142,
      "maxOccupancy": 4, "bedrooms": 2, "bathrooms": 1,
      "amenities": ["pool", "kitchen", "air_conditioning", "wifi"],
      "host": { "name": "Marko", "isSuperhost": false },
      "price": { "currency": "USD", "nightlyPrice": 303, "totalPrice": 2122, "nights": 7 }
    }
  ],
  "meta": {
    "platforms": ["airbnb", "booking"],
    "cached": false, "partial": false, "currency": "USD",
    "pagination": { "limit": 20, "cursor": null, "nextCursor": "eyJ…", "hasMore": true },
    "platformResults": [
      { "platform": "airbnb", "status": "ok", "cached": false, "count": 18 },
      { "platform": "booking", "status": "ok", "cached": true, "count": 20 }
    ],
    "warnings": []
  }
}
```

### 2. `check_availability`

Day-by-day availability for a known listing (or a batch of listings) on one platform over a date window. Each day reports whether it is available, its minimum-night requirement, and whether check-in, check-out and booking are allowed.

| Param | Type | Required | Notes |
| --- | --- | --- | --- |
| `platform` | enum | yes | Single platform; not a fan-out tool |
| `listingId` / `listingIds[]` / `url` | string | one of | A listing id, a batch of ids, or a full listing URL |
| `startDate` / `endDate` | date | yes | Not in the past; window up to 365 days |
| `onlyAvailable` | boolean | no | If true, only bookable dates are returned |

Response (trimmed):

```jsonc
{
  "data": [
    {
      "platform": "airbnb",
      "listingId": "42307961",
      "dates": [
        { "date": "2026-07-13", "available": true, "minNights": 7, "checkIn": true, "checkOut": false, "bookable": true },
        { "date": "2026-07-14", "available": true, "minNights": 7, "checkIn": false, "checkOut": false, "bookable": true }
      ]
    }
  ],
  "meta": { "platforms": ["airbnb"], "cached": false, "partial": false, "warnings": [] }
}
```

### 3. `get_listing`

Full normalized detail for one listing: amenities on a canonical taxonomy, photos, host, geo, ratings, and - when you pass dates - an embedded live price. Detail is cached 24 h and any embedded price at the 1 h price TTL, composed at read time, so a stale price never rides on fresh detail.

| Param | Type | Required | Notes |
| --- | --- | --- | --- |
| `platform` | enum | yes | `vrbo` \| `booking` \| `airbnb` \| `google` |
| `id` | string | yes | Platform-native listing id, verbatim from the source |
| `checkIn` / `checkOut` | date | no | Presence embeds a best-effort live price |
| `adults` / `children` / `childAges[]` / `currency` | mixed | no | Only used with dates |

Response (trimmed):

```jsonc
{
  "data": {
    "id": "stays_booking_abramovic2",
    "platform": "booking",
    "platformListingId": "abramovic2",
    "name": "Apartments Abramović",
    "propertyType": "apartment",
    "location": { "lat": 43.51, "lng": 16.44, "city": "Split", "country": "HR" },
    "guestRating": 9.1, "ratingScale": 10, "reviewCount": 142,
    "maxOccupancy": 4, "bedrooms": 2, "bathrooms": 1,
    "amenities": ["pool", "kitchen", "air_conditioning", "wifi"],
    "images": ["https://…"],
    "host": { "name": "Marko", "isSuperhost": false },
    "price": { "currency": "USD", "nightlyPrice": 303, "totalPrice": 2122, "nights": 7 }
  },
  "meta": { "platforms": ["booking"], "cached": false, "partial": false, "warnings": [] }
}
```

### 4. `get_price`

A real price quote for one listing, for your dates and occupancy. Pass the platform-native id returned by `search_stays` (numeric on Airbnb and Vrbo, a slug string on Booking.com and Google Hotels) or a full listing URL. The response is always a real numeric price or a typed error - **never a different property's price**.

| Param | Type | Required | Notes |
| --- | --- | --- | --- |
| `platform` | enum | yes | `vrbo` \| `booking` \| `airbnb` \| `google` |
| `listingId` | string | yes | Native id from `search_stays.platformListingId`, or pass `url` |
| `checkIn` / `checkOut` | date | yes | `YYYY-MM-DD`; `checkOut` after `checkIn` |
| `adults` / `children` / `childAges[]` / `currency` | mixed | no | ISO-4217 currency, passed through and echoed |

Response (trimmed):

```jsonc
{
  "data": {
    "platform": "booking",
    "listingId": "abramovic2",
    "currency": "EUR",
    "nightlyPrice": 303,
    "totalPrice": 2122,
    "fees": { "cleaning": null, "service": null, "taxes": null },
    "nights": 7,
    "occupancy": { "adults": 2, "children": 2, "childAges": [8, 13] },
    "source": "booking",
    "url": "https://www.booking.com/hotel/hr/abramovic2.html"
  },
  "meta": { "platforms": ["booking"], "cached": false, "partial": false, "warnings": [] }
}
```

### 5. `compare_prices`

**The flagship tool.** Compare one property's price across booking sites in a single call, resolved through the Google Hotels backbone. The response carries the individual offers plus StayingAPI-computed `min` and `median` as first-class fields, so you read the cheapest and typical cross-site price without re-deriving them.

| Param | Type | Required | Notes |
| --- | --- | --- | --- |
| `name` / `googleHotelId` / `location` | string | one of | Property name to resolve, a precise Google Hotels id, or a disambiguating place |
| `checkIn` / `checkOut` | date | yes | `YYYY-MM-DD`; not in the past |
| `adults` / `children` / `childAges[]` / `currency` | mixed | no | ISO-4217 currency, passed through and echoed |

Response (trimmed):

```jsonc
{
  "data": {
    "property": "Hotel X, Sibenik",
    "checkIn": "2026-07-13",
    "checkOut": "2026-07-20",
    "currency": "EUR",
    "min": 2122,
    "median": 2151,
    "offers": [
      { "ota": "booking.com", "totalPrice": 2122, "currency": "EUR", "url": "https://…" },
      { "ota": "expedia", "totalPrice": 2180, "currency": "EUR", "url": "https://…" }
    ]
  },
  "meta": { "platforms": ["google"], "cached": false, "partial": false, "warnings": [] }
}
```

### 6. `get_reviews`

Normalized, paginated reviews for one listing on one platform. **Native rating scales are preserved and echoed alongside each rating** (Airbnb, Vrbo and TripAdvisor use 5; Booking.com, Expedia and Hotels.com use 10) and never silently rescaled, so a 9 and a 4.5 are not accidentally compared.

| Param | Type | Required | Notes |
| --- | --- | --- | --- |
| `platform` | enum | yes | Platform enum |
| `listingId` / `url` | string | one of | Listing id on that platform, or a full listing URL |
| `limit` / `cursor` | mixed | no | `limit` 1-100; opaque base64 cursor |
| `language` / `minRating` / `sort` | mixed | no | ISO-639-1 filter; `sort` = `recent` \| `rating_desc` \| `rating_asc` |

Response (trimmed):

```jsonc
{
  "data": [
    {
      "platform": "booking",
      "listingId": "abramovic2",
      "reviewId": "r987",
      "rating": 9, "ratingScale": 10,
      "title": "Perfect family stay",
      "text": "Spotless apartment a short walk from the old town…",
      "author": "Jane D.", "date": "2026-05-10", "tripType": "family",
      "language": "en", "ownerResponse": "Thank you!",
      "liked": "Location and cleanliness", "disliked": null
    }
  ],
  "meta": {
    "platforms": ["booking"], "cached": false, "partial": false,
    "pagination": { "limit": 20, "cursor": null, "nextCursor": "eyJ…", "hasMore": true },
    "warnings": []
  }
}
```

### 7. `get_job`

Poll a long-running scrape that was returned as an async job. When a request is projected to run longer than roughly 8 seconds it returns a job handle; the agent calls `get_job` until `status` is `completed` or `failed`. **Polling always costs 0 credits** - the underlying work is billed once, on successful completion.

| Param | Type | Required | Notes |
| --- | --- | --- | --- |
| `jobId` | string | yes | The handle returned by the original call. Results are retained 24 h |

Response while running, then on completion (trimmed):

```jsonc
// While running - polling is free
{
  "data": { "jobId": "job_3kf…", "status": "running", "pollUrl": "/v1/jobs/job_3kf…", "estimatedSeconds": 12 },
  "meta": { "creditsCharged": 0, "platforms": ["vrbo"] }
}

// On success - the payload arrives in data.result, in the same unified schema
{
  "data": { "jobId": "job_3kf…", "status": "completed", "result": [ /* the endpoint's payload */ ] },
  "meta": {
    "platforms": ["vrbo"], "currency": "USD",
    "platformResults": [ { "platform": "vrbo", "status": "ok", "cached": false, "count": 15 } ],
    "warnings": []
  }
}
```

A failed job still returns successfully with `status: "failed"` and the reason nested at `data.error` (a `{ type, code, message, retryable }` object). Failed work is free.

---

## Data you get

Every call returns the same unified `{ data, meta }` envelope, whether the property is a hotel or a holiday home:

| Field group | Examples |
|---|---|
| Identity | `id`, `platform`, `platformListingId`, canonical listing `url` |
| Core facts | property type, bedrooms, bathrooms, max occupancy, geo (`lat`/`lng`/city/country) |
| Ratings | `guestRating` with an explicit `ratingScale` (5 or 10, never silently rescaled), `reviewCount`, `starRating` |
| Amenities | Canonical taxonomy across all four platforms (`pool`, `kitchen`, `air_conditioning`, `wifi`, ...) |
| Price | `nightlyPrice`, `totalPrice`, `nights`, `currency`, and a `fees` breakdown (cleaning, service, taxes) |
| Cross-platform compare | Per-site `offers[]` plus StayingAPI-computed `min` and `median` |
| Availability | Day-by-day `available`, `minNights`, `checkIn`, `checkOut`, `bookable` |
| Reviews | Rating on its native scale, title, text, author, date, trip type, language, owner response, liked/disliked |
| Host & media | Host name, superhost flag, full-resolution photo URLs |
| Call metadata | `requestId`, per-platform status, cache flags, pagination cursor, warnings |

**Coverage:** Booking.com, Airbnb, Vrbo and Google Hotels - hotels and short-term rentals in the same schema, so one integration covers both halves of the accommodation map.

---

## Use Cases & Prompts

| Use case | Example prompt |
| --- | --- |
| **Trip planning** | "Find 2-bed stays in Lisbon 12-19 May under EUR 150 a night with a kitchen and air conditioning, and rank them by guest rating." |
| **Price arbitrage across sites** | "This hotel in Split, 13-20 July, 2 adults: what does it cost on Booking.com versus the other sites, and how far below the median is the cheapest?" |
| **Review summarization** | "Pull the last 50 reviews for this listing and tell me the three complaints that keep coming up, and whether the host replies." |
| **Availability check** | "Is this Vrbo villa free for any 7-night window in August, and what is the minimum stay?" |
| **Family fit** | "Same dates, 2 adults and 2 kids aged 8 and 13 - which of these three places actually sleeps us all, and what is the total with fees?" |
| **Market scan** | "Scan apartments in Split for the first week of July - what is the typical nightly rate and how does my property compare?" |
| **Rate parity check** | "For these five hotels, compare each one's rate across booking sites and flag any where one site is more than 10 percent off the median." |

---

## Pricing

| | |
| --- | --- |
| **Starter credits** | **300 free credits at signup, no card** |
| **Sandbox** | `stay_test_` keys return deterministic fixtures at **0 credits**, forever |
| **Failed calls** | Failed, empty, blocked and not-found calls are **never billed** - `creditsCharged` is 0 |
| **Async polling** | `get_job` polling is **always 0 credits**; the work is billed once, on success |
| **Paid plans** | Credit-based. Current per-call costs and plan prices: [stayingapi.com/pricing](https://stayingapi.com/pricing) |

This README stays number-free on paid pricing by design, so it never drifts from the live [pricing page](https://stayingapi.com/pricing). MCP and REST draw from the **same single credit balance** - there is no separate MCP wallet or MCP pricing.

[Sign up](https://stayingapi.com/signup) · [View pricing](https://stayingapi.com/pricing)

---

## Troubleshooting

Branch on `error.type` (the class, mapped 1:1 to the HTTP status) and then on `error.code` (a stable, finer-grained reason). Every error also carries a `requestId`, a `retryable` flag, and a `docUrl`.

<details>
<summary><b>401 <code>authentication_error</code> - missing or bad credentials</b></summary>

Codes: `missing_api_key`, `invalid_api_key`, `revoked_api_key`.

- Check the key starts with `stay_live_` or `stay_test_`, and that no stray whitespace was copied.
- Confirm the key has not been revoked in your dashboard. A revoked key returns `revoked_api_key` immediately.
- On OAuth clients, remove and re-add the connector to re-authorize.
- Auth is checked before validation, billing or any upstream work, so a 401 is **never billed**.
</details>

<details>
<summary><b>403 <code>permission_denied</code> - usually email verification</b></summary>

Codes: `email_unverified`, `scope_insufficient`, `subscription_required`.

- `email_unverified` is the common one: live credit is locked until you confirm your account email. Your `stay_test_` sandbox key is unaffected and keeps returning full fixtures at zero cost, so you can build end-to-end before verifying.
- `subscription_required` means a top-up was attempted without an active paid subscription - top-ups are a subscriber add-on.
</details>

<details>
<summary><b>402 <code>insufficient_credits</code></b></summary>

Code: `credit_balance_too_low`. The balance is below what the call would cost. Check it programmatically instead of guessing - the account endpoint returns `credits.balance`, `plan`, key `env` and your `rateLimit.requestsPerMinute`. Top up or upgrade at [stayingapi.com/pricing](https://stayingapi.com/pricing).
</details>

<details>
<summary><b>400 <code>invalid_request</code> - the request itself</b></summary>

Most common codes: `missing_parameter`, `invalid_date_range` (`checkOut` must be strictly after `checkIn`), `date_in_past` (evaluated in UTC), `child_ages_mismatch` (`childAges[]` length must equal `children`), `window_too_long` (availability windows cap at 365 days), `invalid_cursor`, `limit_out_of_range`, `mutually_exclusive_params` (you passed both `listingId` and `url`, or neither), `needs_country`.

`needs_country` deserves a note: a bare Booking.com slug passed to `get_listing` is ambiguous, because Booking.com slugs are **not globally unique** - the same slug exists per country. Pass the country, or pass the full listing URL.
</details>

<details>
<summary><b>404 <code>not_found</code> - including the deliberate one</b></summary>

Codes: `listing_not_found`, `job_not_found`, `identity_mismatch`.

`identity_mismatch` is intentional: the resolved listing's canonical identity did not match what was requested, so StayingAPI **refuses to return a different property** rather than serve plausible-looking wrong data. `job_not_found` also fires for an expired job (results are retained 24 h) or a job belonging to another account - a job id is not a capability.
</details>

<details>
<summary><b>429 <code>rate_limited</code></b></summary>

Code: `rate_limit_exceeded`. Retryable - honour the `Retry-After` header rather than looping tightly. MCP is rate-limited **per user** and REST **per key**: separate buckets, one shared credit balance.
</details>

<details>
<summary><b>503 / 504 upstream problems</b></summary>

`upstream_unavailable` (`all_actors_failed`, `actor_blocked`) means every primary and fallback source failed or was blocked; `upstream_timeout` means synchronous upstream work exceeded the request deadline. Both are retryable with backoff, and both cost **0 credits**. For long scrapes, expect the async path instead: a job handle you poll with `get_job`.
</details>

<details>
<summary><b>OAuth will not connect</b></summary>

- Make sure a popup blocker is not stopping the sign-in window, then clear cookies and retry the connector authorization.
- OAuth discovery starts from the server's 401 response. If a client cannot connect at all, confirm it supports MCP OAuth 2.1 with Dynamic Client Registration - otherwise fall back to a bearer key.
</details>

<details>
<summary><b>A job is stuck, or "completed" but empty</b></summary>

`estimatedSeconds` is a projection, not a guarantee - a job usually finishes in tens of seconds but can exceed 240 seconds on a slow platform, so budget in minutes. A **failed** job still returns HTTP 200 with `status: "failed"` and the reason at `data.error`, not in a top-level error envelope, so branch on `data.status === "failed"`. A completed job carries `meta.pagination: null` - cursor pagination only applies to synchronously returned list responses.
</details>

---

## Also available as a REST API

Building an app or a backend instead of an agent? The same data ships as a plain JSON REST service.

| | MCP | REST API |
| --- | --- | --- |
| **Best for** | AI assistants and agents | Apps, backends, pipelines |
| **Setup** | Add a URL, authorize once | Bearer key, code integration |
| **Auth** | OAuth 2.1 + PKCE (no key in the agent) | `Authorization: Bearer stay_live_...` |
| **Rate limit** | Per user | Per key |
| **Credits** | **Same single balance** | **Same single balance** |
| **Schema** | Identical unified schema | Identical unified schema |
| **Get started** | This README | [Quickstart](https://stayingapi.com/docs/quickstart) · [API reference](https://stayingapi.com/docs) · [OpenAPI](https://api.stayingapi.com/openapi.json) |

Base URL: `https://api.stayingapi.com/v1`. The seven MCP tools map 1:1 to `/v1/search`, `/v1/availability`, `/v1/listing/{platform}/{id}`, `/v1/price`, `/v1/price-compare`, `/v1/reviews` and `/v1/jobs/{jobId}`.

---

## Connect

- **Website:** [stayingapi.com](https://stayingapi.com)
- **MCP guide:** [stayingapi.com/docs/mcp](https://stayingapi.com/docs/mcp)
- **Docs:** [stayingapi.com/docs](https://stayingapi.com/docs) · **OpenAPI:** [api.stayingapi.com/openapi.json](https://api.stayingapi.com/openapi.json)
- **Workflows & recipes:** [stayingapi.com/workflows](https://stayingapi.com/workflows)
- **Status:** [status.stayingapi.com](https://status.stayingapi.com)
- **Contact:** [hello@stayingapi.com](mailto:hello@stayingapi.com)

**Related repositories** - part of the StayingAPI open resource set: [hotel-api](https://github.com/stayingapi/hotel-api) · [airbnb-api](https://github.com/stayingapi/airbnb-api) · [booking-com-api](https://github.com/stayingapi/booking-com-api) · [vrbo-api](https://github.com/stayingapi/vrbo-api) · [google-hotels-api](https://github.com/stayingapi/google-hotels-api) · [travel-api](https://github.com/stayingapi/travel-api) · [travel-workflows](https://github.com/stayingapi/travel-workflows) · [travel-skills](https://github.com/stayingapi/travel-skills)

---

## MCP Registry

Published to the official [Model Context Protocol Registry](https://registry.modelcontextprotocol.io/) as:

```
com.stayingapi/hotel-vacation-rental-mcp
```

Registry manifests in this repo: [`server.json`](server.json) (MCP registry) · [`smithery.yaml`](smithery.yaml) (Smithery) · [`glama.json`](glama.json) (Glama).

---

## FAQ

**Is there an Airbnb or Booking.com API in 2026?**
Not one you can sign up for and start using today. Airbnb has no open public API - access runs through its partner programs, which are aimed at approved software partners and property-management systems, not general developers. Booking.com's Demand API is similar: it requires a partner or affiliate agreement and an approval process before you get credentials. Vrbo sits inside Expedia Group's partner API program, and Google Hotels data flows through hotel partner feeds rather than a self-serve developer API. That gap is why "airbnb api" is one of the most-searched, least-served queries in travel tech. StayingAPI is the self-serve alternative: sign up, get a key with 300 free credits and no card, and query accommodation data across all four sources through one REST API or this MCP server, with every response normalized to the same schema.

**What is a hotel or accommodation MCP?**
A hotel or accommodation MCP is a Model Context Protocol server that exposes accommodation data as tools an AI agent can call directly: search stays, check day-by-day availability, fetch full listing detail, quote a real price for specific dates, compare that price across booking sites, and read normalized reviews. Instead of you copying results into a chat, the assistant fetches live data mid-conversation and reasons over it, which means it can chain steps on its own: search, then price the best candidate, then summarize its reviews. This one is hosted at `https://mcp.stayingapi.com/mcp`, speaks the Streamable HTTP transport, and exposes seven read-only tools covering Booking.com, Airbnb, Vrbo and Google Hotels in a single unified schema. It works with Claude, ChatGPT, Cursor, Claude Code and any other MCP client, and authenticates over OAuth 2.1, so no API key is ever pasted into the agent and nothing here can book, cancel or change a reservation.

**What is the difference between the StayingAPI REST API and this MCP server?**
They are two front doors to the same service, not two products. The REST API is for apps, backends and data pipelines: you send a bearer key, you get JSON, you control the loop. The MCP server is for AI assistants and agents: you add one URL, authorize once over OAuth 2.1, and the seven tools become things the model can call on its own. Underneath they are identical - the same validation, the same adapter and cache pipeline, the same unified schema, the same error taxonomy, and the **same single credit balance**. There is no separate MCP wallet or MCP pricing. The only real difference is rate limiting: REST is metered per key, MCP per user. Pick REST when you are writing the code, MCP when the model is.

**How is this different from a single-platform API or a scraper?**
A single-platform API gives you one source's view, so cross-platform questions ("is this villa cheaper on Booking.com or Vrbo?") are unanswerable by construction. A SERP scraper hands you a raw page snapshot that you parse and normalize yourself, with no rating scale, no canonical amenity taxonomy, and no guarantee the row you parsed is the property you asked about. This server returns four sources in **one schema**, keeps native rating scales explicit instead of silently rescaling them, and ships `compare_prices`, which returns each site's offer plus a computed lowest and median. It also refuses to guess: if a listing's canonical identity does not match what you asked for, you get an `identity_mismatch` error rather than a plausible-looking wrong property. Failed, empty and blocked calls are never billed.

**How do I compare hotel prices across Booking.com, Airbnb and Vrbo with AI?**
Connect this MCP server to your assistant, then ask in plain language: "compare this hotel's price for 13-20 July across booking sites". The model calls `compare_prices` with the property name and your dates. It comes back with each site's total in your chosen currency plus StayingAPI-computed `min` and `median` fields, so the assistant can tell you not just the cheapest offer but how far below typical it sits. For short-term rentals where the same physical property is listed on several sites under different names, the usual flow is `search_stays` to find candidates, then `get_price` per platform for an exact quote at your dates and occupancy. Add the auto-invoke rule from [Quick Install](#quick-install) and your assistant will reach for these tools on its own whenever you mention a stay.

**Can an AI agent search vacation rentals and short-term rentals, not just hotels?**
Yes, and that is deliberate - it is why this is a hotel **and** vacation rental MCP. Hotels arrive via Google Hotels and Booking.com; short-term rentals via Airbnb and Vrbo. Crucially they come back in the **same object shape**: a city-centre hotel room and a rural holiday villa both return property type, bedrooms, bathrooms, max occupancy, a canonical amenities list, a guest rating with its explicit scale, and a price with a fee breakdown. So an agent can answer "what sleeps 4 with a pool near Split, hotel or apartment, whichever is cheaper" in one pass, instead of you running two integrations and reconciling them. `search_stays` accepts a `platforms[]` filter and a `propertyType[]` filter, so you can narrow to just rentals, just hotels, or let both compete.

**What data does it return?**
Every tool returns the same `{ data, meta }` envelope. Property records carry identity (platform, native listing id, canonical URL), core facts (property type, bedrooms, bathrooms, max occupancy, geo), a guest rating with an explicit `ratingScale` so a 9-out-of-10 is never confused with a 4.5-out-of-5, a canonical amenities list unified across all four platforms, host details, full-resolution photo URLs, and pricing with nightly, total, nights and a fees breakdown. Availability returns per-day `available`, `minNights`, `checkIn`, `checkOut` and `bookable`. Reviews return rating on its native scale, title, text, author, date, trip type, language, owner response, and liked/disliked. The `meta` block always reports the request id, per-platform status and cache flags, pagination cursor, and any warnings, so you can tell a partial result from a complete one.

**How do I add it to Claude, ChatGPT or Cursor?**
Claude Desktop and Claude Web: Settings, Connectors, Add custom connector, then paste `https://mcp.stayingapi.com/mcp` and complete the OAuth popup. Claude Code: `claude mcp add --transport http stayingapi https://mcp.stayingapi.com/mcp`, then run `/mcp` and authorize. ChatGPT: enable Developer Mode under Settings, Apps & Connectors, Advanced settings, then Settings, Connectors, Add MCP server, paste the same URL and complete the OAuth prompt. Cursor: use the one-click badge at the top of this README, or add `{"mcpServers":{"stayingapi":{"url":"https://mcp.stayingapi.com/mcp"}}}` to `~/.cursor/mcp.json` for every project, or to `.cursor/mcp.json` for just one. Any other MCP client works too - add the same URL as a Streamable HTTP server, and clients that support Dynamic Client Registration will negotiate OAuth without further configuration. Setup takes under a minute in every case, because the server is hosted: there is nothing to install, no runtime to manage, and no package to keep updated. Full per-client detail is in [Quick Install](#quick-install), and only configurations verified against the official docs are listed there, since a wrong config block costs you the one install attempt you get.

**How much does it cost?**
Signup gives you **300 free credits with no card**, and `stay_test_` sandbox keys return deterministic fixtures at zero cost forever, so you can build and test the whole integration before spending anything. After that it is credit-based, with current per-call costs and plan pricing on the [pricing page](https://stayingapi.com/pricing) - this README deliberately carries no paid numbers so they can never go stale here. Three things are always free regardless of plan: failed, empty and blocked calls (`creditsCharged` is 0), `get_job` polling, and every sandbox call. MCP and REST draw on the **same single balance**, so adding the MCP server does not create a second bill. Live credit unlocks after you verify your account email; the sandbox works fully before that.

**Is StayingAPI affiliated with Airbnb, Booking.com, Vrbo or Google Hotels?**
No. StayingAPI is an independent service. **StayingAPI is not affiliated with, endorsed by, or sponsored by Airbnb, Booking.com, Vrbo, or Google Hotels. These are trademarks of their respective owners, used here descriptively to indicate the data sources StayingAPI's API and MCP server can query.** We are not a partner, a reseller, or an authorized integration of any booking platform, we hold no agreement with any of them, and none of them endorse, review or approve this project. Platform names appear throughout this README, in the `platforms[]` enum, and in tool parameters purely as descriptive labels for which data source a given call targets, in the same way a price-comparison tool names the shops it checks. StayingAPI does not book, cancel or modify reservations: all seven tools are strictly read-only, annotated `readOnlyHint: true`, and there are no write tools at all. If you need to actually reserve a stay, follow the canonical listing `url` returned with every property and book on that platform directly.

---

<p align="center">
  <b><a href="https://stayingapi.com/signup">Get your key — 300 free credits, no card</a></b> · <a href="https://stayingapi.com/docs/mcp">MCP docs</a> · <a href="https://stayingapi.com/pricing">Pricing</a> · <a href="https://status.stayingapi.com">Status</a>
</p>

<p align="center">
  <sub>Released under the <a href="./LICENSE">MIT License</a>. StayingAPI is an independent service and is not affiliated with or endorsed by any booking platform. Platform names are trademarks of their respective owners.</sub>
</p>
