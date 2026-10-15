# Deliveroo UK Restaurant Scraper

Extract structured data from [deliveroo.co.uk](https://deliveroo.co.uk) — deliveroo.co.uk — every restaurant delivering to a UK area in a single request, with rating, review count, delivery time, distance and current offers. Filter by name or offer, track changes between runs, export clean JSON.

**[Deliveroo UK Restaurant Scraper on Apify →](https://apify.com/blackfalcondata/deliveroo-co-uk-scraper?fpr=1h3gvi)**

---

## 🚀 How to use this actor

> ### 💚 $5 free Apify credits — every month
> No credit card required. No commitment. Cancel anytime.

### 👉 [Sign up free on Apify →](https://console.apify.com/sign-up?fpr=1h3gvi)

1. **Click sign up** — pick GitHub, Google, or email; takes ~30 seconds
2. **Open this actor** — input is pre-filled with a working example
3. **Click Start** — export results as JSON, CSV, or Excel

Your **$5 monthly platform credit** is enough to run this actor right away — and again every month — scraping typically several hundred to several thousand results per run, depending on your input.

## Key features

**Search with filters** — Search by keyword and location. Filter by 📝 description format, and more.

**Detail enrichment** — Fetch structured metadata for each record.

**Incremental mode** — Only get new or changed records since your last run. Content hash per record — no duplicates, no re-processing.

**Change classification** — Track unchanged, expired, cross-run duplicate detection across runs. Build audit trails of how records evolve over time.

**Compact output** — Emit core fields only (AI-agent / MCP-friendly). Keeps response size small for LLM workflows.

**Result cap** — Stop after N records (up to 5.000). Set to 0 for the full catalog.

**Export anywhere** — Download as JSON, CSV, or Excel. Stream via Apify API, webhooks, or integrations with Make, Zapier, Airbyte, Keboola.

**Structured data** — Every record returns the same schema with consistent field naming. All fields always present — `null` when unavailable, never omitted.

---

## Use cases

**Data pipeline automation**
Integrate with your ETL pipeline to collect structured records from deliveroo.co.uk on a schedule. Export to CSV, JSON, or directly to your database. Use compact mode to control output size.

**Market research**
Monitor records, track trends, and analyze market dynamics with structured, deduplicated data from deliveroo.co.uk.

**Change monitoring**
Run daily or hourly in incremental mode to capture only new, updated, or expired ${NOUNS}. Perfect for price-tracking, churn analysis, and alerting pipelines.

**AI / LLM training data**
Structured JSON per record is ready for RAG pipelines, embeddings, and agent workflows. Compact mode trims tokens for LLM context windows.

---

## Quick start

```json
{
  "location": "london/soho",
  "maxResults": 100,
  "compact": false,
  "excludeEmptyFields": false,
  "incrementalMode": false,
  "skipReposts": false,
  "notifyOnlyChanges": false,
  "notificationLimit": 5,
  "descriptionFormat": "all",
  "includeDetails": false
}
```

---

## Input parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `location` | string | — | The area to list restaurants for, as it appears in a deliveroo.co.uk address: "london/soho". You can also paste the full page URL (https://deliveroo.co.uk/restaurants/london/soho). Use a JSON array for several areas, e.g. ["london/soho","manchester/city-centre"]. |
| `query` | string | — | Optional keyword — keeps only restaurants whose name or current offer matches (e.g. "pizza"). Leave empty to get every restaurant delivering to the area. Use a JSON array for several keywords. |
| `maxResults` | integer | `100` | Maximum number of restaurants to return (0 = every restaurant in the area; a central London area has ~2,000). |
| `compact` | boolean | `false` | Core fields only — name, area, rating, delivery time, distance, offers, URL (for AI-agent/MCP workflows). Extras you enable (menu, address, reviews) and incremental repost fields are kept. |
| `excludeEmptyFields` | boolean | `false` | Drop null, empty-string, and empty-array fields from each record before push. Smaller payloads for AI agents and dashboards. |
| `incrementalMode` | boolean | `false` | Compare against previous run state and return only new or changed restaurants (a changed rating, delivery estimate, offer or open state counts as changed). stateKey is optional — defaults to a value derived from your search inputs (areas, keywords) so different searches never share state. |
| `stateKey` | string | — | Optional. Stable identifier for the tracked search universe. Leave empty to auto-generate from search inputs. |
| `skipReposts` | boolean | `false` | When incremental, skip restaurants whose content matches one that disappeared in a prior run (cross-run duplicate detection). |
| `notifyOnlyChanges` | boolean | `false` | When Incremental Mode is on, only send notifications for NEW and UPDATED restaurants. Has no effect outside incremental mode. |
| `notificationLimit` | integer | `5` | Maximum number of restaurants included in each notification message (1–20). |
| `telegramToken` | string | — | Telegram bot token (from @BotFather). Required for Telegram notifications. |
| `telegramChatId` | string | — | Telegram chat or channel ID (e.g. "-100123456789"). Required when telegramToken is set. |
| `discordWebhookUrl` | string | — | Discord incoming webhook URL. Server Settings → Integrations → Webhooks → New Webhook. |
| `slackWebhookUrl` | string | — | Slack incoming webhook URL. api.slack.com/messaging/webhooks. |
| `whatsappPhoneNumberId` | string | — | Your WhatsApp Business phone-number ID (numeric, from Meta dashboard). Required when whatsappAccessToken is set. |
| `whatsappAccessToken` | string | — | WhatsApp Cloud API permanent access token (System User token from Meta Business). Recipient must have messaged the business number within the last 24h (service-conversation window — free since Nov 2024). |
| `whatsappTo` | string | — | Recipient phone in E.164 format without + (e.g. "436641234567"). Recipient must have messaged your business number within last 24h. |
| `webhookUrl` | string | — | Receives a JSON POST with {metadata, items} after each run. Universal escape hatch for n8n / Make / Zapier / custom backends. |
| `webhookHeaders` | object | — | Optional JSON object of custom headers (e.g. {"Authorization":"Bearer ..."}). |
| `appConnector` | string | — | Optional. Pick a connected app under Settings → API & Integrations to receive your results. Best-effort across MCP connectors as Apify expands its catalog. |
| `mcpIssueTeam` | string | — | Only when the connected app is an issue tracker: the team (name or ID) the summary issue is created under, if that app requires one. |
| `descriptionFormat` | enum | `"all"` | Choose which representation of the listing description to include. `all` keeps every variant; the others keep only the selected one. |
| `startUrls` | array | — | One or more direct search or listing detail URLs to process. When set, other search filters are ignored. |
| `includeDetails` | boolean | `false` | Reserved — currently has no effect on deliveroo.co.uk. For menu, address and reviews use the Include Menu, Include Street Address (includeExtra) and Include Reviews options instead. |
| `includeExtra` | boolean | `false` | Open each delivered restaurant's menu page and add its street address, postcode, neighbourhood and currency. One extra request per restaurant. |
| `includeMenu` | boolean | `false` | Add the restaurant's full menu: every item with category, description, price, discounted price, availability, popular flag and photo, plus the item count. One extra request per restaurant. |
| `maxMenuItems` | integer | `0` | Keep at most this many menu items per restaurant (menuItemCount still reports the full menu size). 0 = the whole menu. |
| `includeReviews` | boolean | `false` | Add customer reviews for each delivered restaurant: reviewer, star rating, comment, tags and date, newest first. Adds a request per 50 reviews. |
| `maxReviews` | integer | `50` | Collect at most this many reviews per restaurant. 0 = every review Deliveroo shows. |
| `emitUnchanged` | boolean | `false` | When incremental mode is on, also emit listings whose content has not changed since the last run. |
| `emitExpired` | boolean | `false` | When incremental mode is on, scan the whole area so restaurants that are no longer listed are detected and recorded in the incremental state as expired. They are not added to the dataset. |
| `radiusKm` | integer | — | Only keep restaurants within this many kilometres of the search area's delivery point (based on the distance Deliveroo shows on each restaurant). Leave empty for no limit. |
| `lat` | number | — | Reserved; currently not used for filtering. Use radiusKm to limit by distance. |
| `lon` | number | — | Reserved; currently not used for filtering. Use radiusKm to limit by distance. |

---

## Output fields

Every record returns the same 40-field schema. Missing values are `null` — never omitted.

- `listingId`
- `restaurantId`
- `partnerDrnId`
- `uname`
- `title`
- `entityName`
- `location`
- `city`
- `branchArea`
- `url`
- `portalUrl`
- `imageUrl`
- `rating`
- `ratingCount`
- `ratingCountText`
- `distance`
- `distanceMiles`
- `deliveryTimeMinutes`
- `deliveryTimeText`
- `isFast`
- `isDeliverooChoice`
- `isSponsored`
- `isOpen`
- `openingAt`
- `offers`
- `fulfillmentMethod`
- `branchType`
- `searchRank`
- `searchArea`
- `searchCity`
- `searchGeohash`
- `searchLat`
- `searchLon`
- `searchQuery`
- `contentQuality`
- `detailFetched`
- `scrapedAt`
- `source`
- `contentHash`
- `changeType`

---

## Sample output

One object per record. Here is a real example from a production run:

```json
{
  "listingId": "e1c8f184ecea6159554d4b6286906c2a61755177c0c6b3fd4286662299edb8aa",
  "restaurantId": "60490",
  "partnerDrnId": "a686660d-9b85-41b8-b55f-f0fa8eaf70f6",
  "uname": "five-guys-piccadilly",
  "title": "Five Guys - Burgers, Fries & Shakes",
  "entityName": "Five Guys - Burgers, Fries & Shakes",
  "location": "leicester square",
  "city": "London",
  "branchArea": "leicester-square",
  "url": "https://deliveroo.co.uk/menu/London/leicester-square/five-guys-piccadilly",
  "portalUrl": "https://deliveroo.co.uk/menu/London/leicester-square/five-guys-piccadilly",
  "imageUrl": "https://rs-menus-api.roocdn.com/images/386c4679-3a15-49ec-ad59-59182b2a11fa/image.jpeg?width=600&height=400&auto=webp&format=jpg&fit=crop"
}
```

*Truncated — full records contain 40 fields. See Output fields for the complete schema.*

**[Try Deliveroo UK Restaurant Scraper now — $5 free credit, no credit card →](https://apify.com/blackfalcondata/deliveroo-co-uk-scraper?fpr=1h3gvi)**

---

## Pricing

Pay only for what you extract. No subscription required — Apify's free $5 credit covers thousands of results.

| Event | Price (USD) |
| --- | --- |
| Actor Start | $0.0045 |
| Result | $0.00108 |

See the [actor on Apify](https://apify.com/blackfalcondata/deliveroo-co-uk-scraper?fpr=1h3gvi) for current pricing.

---

## FAQ

**How do I scrape deliveroo.co.uk?**
Use this actor on Apify to extract structured data from deliveroo.co.uk. Configure your search query and filters in the input, then click Start — no coding required.

**How do I get deliveroo.co.uk data as JSON, CSV, or Excel?**
The actor writes each record to Apify's dataset. Download as JSON, CSV, or Excel from the Console, stream via the API, or push to Make, Zapier, Airbyte, or Keboola.

**Is it legal to scrape deliveroo.co.uk?**
Web scraping of publicly available data is generally legal. This actor only accesses publicly visible information. Always check deliveroo.co.uk's terms of service for your specific use case.

**How much does it cost?**
Pay-per-event pricing — you only pay for ${NOUNS} extracted. Apify's free $5 credit is enough to run thousands of results before you pay anything.

**How does incremental mode work?**
Each record gets a content hash. On subsequent runs, only new or changed records are emitted — saving time, compute, and storage. Expired records can be tracked separately.

**Do I need an API key or credentials?**
No. Just sign up for Apify, paste your input, and click Start. No credit card required.

---

## Related products by Black Falcon Data

[Browse all Black Falcon Data actors →](https://apify.com/blackfalcondata?fpr=1h3gvi)

---

## Getting started with Apify

New to Apify? [Create a free account with $5 credit](https://console.apify.com/sign-up?fpr=1h3gvi) — no credit card required.

1. Sign up — $5 platform credit included
2. Open [Deliveroo UK Restaurant Scraper](https://apify.com/blackfalcondata/deliveroo-co-uk-scraper?fpr=1h3gvi) and configure your input
3. Click **Start** — export results as JSON, CSV, or Excel

Need more later? [See Apify pricing](https://apify.com/pricing?fpr=1h3gvi).

---

## About Black Falcon Data

Black Falcon Data builds production-grade web scrapers for job boards and marketplace data. Browse our full actor catalog at [www.blackfalcondata.com](https://www.blackfalcondata.com).

---

*Last updated: 2026 10*
