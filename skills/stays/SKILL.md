---
name: stays
description: Live hotel and vacation-rental data via the bundled StayingAPI MCP server — search, availability, real price quotes, cross-OTA price comparison and reviews across Airbnb, Booking.com, Vrbo and Google Hotels in one unified schema. Use when the user wants to find places to stay, check dates, compare what a property costs on different booking sites, or read reviews. Do not use for flights, or for accommodation mentioned only in passing.
license: MIT
metadata:
  homepage: https://stayingapi.com
  publisher: StayingAPI
---

# Hotels & vacation rentals (StayingAPI)

This plugin bundles the hosted **`stayingapi` MCP server** (`https://mcp.stayingapi.com/mcp`). Its seven tools are the only data path — never scrape Airbnb, Booking.com, Vrbo or Google Hotels, and never call the REST API directly from this skill.

All seven tools are **read-only**. Nothing here can book, cancel or change a reservation. Always hand the user the listing `url` so they can complete a booking themselves.

**Authentication is automatic.** The MCP server uses OAuth: the first tool call prompts the user to sign in (or create a free account — 300 credits, no card). There is no API key to configure. If tools fail with an auth error, tell the user to complete the sign-in prompt in their client, or re-enable the `stayingapi` MCP server in settings.

## When to use

**DO use when the user:**

- Wants to find somewhere to stay in a place, optionally for dates and a party size → `search_stays`
- Asks what a specific property costs for their dates → `get_price`
- Asks "is it cheaper on Booking or Airbnb?", or wants the best rate across sites → `compare_prices`
- Asks whether a place is free on given dates, or what its minimum stay is → `check_availability`
- Wants detail on one listing — amenities, photos, host, location, rating → `get_listing`
- Asks what guests said about a property → `get_reviews`

**Do NOT use when:**

- The user wants flights, trains or car hire — out of scope
- A hotel or city appears incidentally, with no request for accommodation data
- The user asks you to book something — you cannot; produce the listing URL instead

## Tools (exact MCP surface)

### `search_stays` — the breadth tool
- `location` (string, required): place name (`"Split, HR"`) or `lat,lng`.
- `checkIn` / `checkOut` (date, `YYYY-MM-DD`): required together, not in the past.
- `adults` / `children` / `childAges[]` / `rooms` — `childAges[]` length must equal `children`.
- `platforms[]`: `airbnb` | `booking` | `vrbo` | `google` — drives fan-out. **Restrict this** when the user only cares about one site; each extra platform costs more.
- `propertyType[]`, `amenities[]`, `minBedrooms`, `priceMin`, `priceMax`, `minGuestRating`.
- `limit` (1–40), `cursor`, `sort` (`recommended` | `price_asc` | `price_desc` | `rating_desc`), `currency`.
- Returns results merged into one schema. Keep `limit` tight — results are the main cost driver. Carry `platform` + `platformListingId` from results into every follow-up tool.

### `get_price` — one real quote
- `platform` (enum, required), `listingId` (string, required — native id from `search_stays.platformListingId`, or pass `url`).
- `checkIn` / `checkOut` (required), `adults` / `children` / `childAges[]` / `currency`.
- Always a real numeric price or a typed error — never a different property's price. Ids are platform-native: numeric on Airbnb and Vrbo, a slug on Booking.com and Google Hotels. Pass them verbatim.

### `compare_prices` — cross-OTA comparison
- `name` | `googleHotelId` | `location` (one required), `checkIn` / `checkOut` (required), occupancy and `currency` optional.
- This is the tool for "where is it cheapest". Don't hand-roll it by calling `get_price` per platform.

### `check_availability`
- `platform` (enum, required — single platform, not a fan-out tool).
- `listingId` | `listingIds[]` | `url` (one required), `startDate` / `endDate` (required, window up to 365 days).
- `onlyAvailable` (boolean) to return just bookable dates.
- Reports per day: availability, minimum-night requirement, and whether check-in, check-out and booking are allowed.

### `get_listing`
- `platform` (enum, required), `id` (string, required — platform-native, verbatim).
- Optional `checkIn` / `checkOut` embed a best-effort live price; occupancy and `currency` only apply with dates.
- One call returns amenities on a canonical taxonomy, photos, host, geo and ratings — don't re-call it per question.

### `get_reviews`
- `platform` (enum, required), `listingId` | `url` (one required).
- `limit` (1–100), `cursor`, `language` (ISO-639-1), `minRating`, `sort` (`recent` | `rating_desc` | `rating_asc`).
- **Native rating scales are preserved**: Airbnb, Vrbo and TripAdvisor use 5; Booking.com, Expedia and Hotels.com use 10. Never compare a 9 with a 4.5 — state the scale when you quote a rating.

### `get_job` — polling, always free
- `jobId` (string, required).
- When a request is projected to take longer than ~8 s it returns a job handle instead of a result. Poll `get_job` until `status` is `completed` or `failed`; the payload arrives in `data.result`. Polling costs 0 credits — the work is billed once, on success.

## Credit hygiene

- Narrow `platforms[]` and `limit` on `search_stays` before anything else — breadth is where credits go.
- `compare_prices` in one call beats N× `get_price`.
- Polling `get_job` is free; don't avoid it, and don't spin faster than the returned `estimatedSeconds`.
- One `get_listing` answers amenities + host + geo + rating together.

## Trademark

StayingAPI is an independent service and is not affiliated with, endorsed by, or sponsored by Airbnb, Inc., Booking.com, Vrbo/Expedia Group, or Google LLC. All trademarks are the property of their respective owners.
