---
name: Check Western Power outages
description: Find current and upcoming electricity outages on the Western Australian SWIS network, for a whole network view, a single incident, or a named suburb.
api: openapi/western-power-outage-openapi.yml
operations:
  - listAllOutages
  - getOutageDetails
  - getSuburbOutageStatus
generated: '2026-07-27'
method: generated
---

# Check Western Power outages

Western Power is the distribution network operator for the South West Interconnected System in Western Australia. These endpoints back its public outage tracker. **They are undocumented.** Western Power publishes no reference, no terms of use, no SLA and no support channel for them, and can change or remove them without notice. Treat every call as best-effort and degrade gracefully.

## Before you start

- **Auth:** none. Anonymous HTTPS GET. Do not send credentials.
- **Base URL:** `https://www.westernpower.com.au/api/corp/outage`
- **Rate limits:** none published and no rate-limit headers are returned. Cloudflare sits in front and may throttle silently. Poll no more than once every few minutes; the underlying data updates on the order of minutes, and the ArcGIS mirror declares a 120-second cache.
- **Coverage:** SWIS only — roughly Kalbarri to Albany and east to Kalgoorlie. Regional WA outside the SWIS is Horizon Power, and this API knows nothing about it.

## 1. Get the whole network picture

Call `listAllOutages` (`GET /all-outages`). It takes no parameters and returns a **bare JSON array** — there is no envelope, no pagination and no filtering. Expect a few hundred records and roughly 60 KB.

Each record carries `outageId`, `outageType`, `startTime`, `restorationTime`, `outageUpdatedTime`, `affectedCustomers`, `areas`, `latitudeCentroid`, `longitudeCentroid` and `onMap`.

Filter client-side. There is no server-side query.

## 2. Read the timestamps correctly

This is the single most common way to get this API wrong. Times use **ISO 8601 basic format**, not the extended format:

```
20260728T190000+08:00     startTime / restorationTime   (AWST, UTC+8)
20260718T160020+00:00     outageUpdatedTime             (UTC)
```

Most date parsers reject these. Parse with an explicit format string (`%Y%m%dT%H%M%S%z`). Do not assume every field shares a timezone — the two outage times are local, the update time is not.

## 3. Classify the outage

`outageType` was observed with two values: `F` (the large majority — planned/future works) and `U` (unplanned). **Western Power publishes no code list**, so treat any other value as unknown rather than mapping it. Do not infer type from the `outageId` suffix: an id ending `-U` was observed carrying `outageType: F`.

## 4. Drill into one outage

Call `getOutageDetails` (`GET /details/{outageId}`) with an `outageId` taken from step 1.

**Handle 204.** An unknown identifier returns `HTTP 204 No Content` with an empty body — not 404, not a JSON error. Check the status before parsing, or you will hand an empty string to your JSON decoder.

A 200 returns the summary fields plus:
- `historyGroup` — dated update history for the incident.
- `mayBeImpactedByBushfire`, `mayBeImpactedByTotalFireBan`, `mayBeImpactedByWeather`, `mayBeImpactedByFireWeatherWarning` and `bushfireCoordinates` — hazard context worth surfacing to a user.
- `moreInformationList` and `calendarModal` — website presentation content. Ignore unless you are rendering a page.

## 5. Answer "is my suburb affected?"

Call `getSuburbOutageStatus` (`GET /status/{suburb}`) with a suburb name, e.g. `BOORAGOON`. Case-insensitive.

**This endpoint never fails.** An unmatched name returns HTTP 200 with all counts zero and your input title-cased back into `suburb`, so you cannot distinguish "no outages here" from "that is not a suburb". Validate the suburb against the `areas` values from step 1 before trusting a zero result.

Useful fields: `inflightOutageCount`, `inflightPlannedOutageCount`, `inflightUnPlannedOutagesCount`, `plannedOutageCount`, `resolvedOutageCount`, `affectedCustomers`, and the pre-composed sentences `restorationTimeFormatted` and `plannedOutageTimeFormatted`.

## Error handling summary

| What you see | What it means |
|---|---|
| `204`, empty body | Outage id not found |
| `200`, all counts zero | No outages **or** unknown suburb — indistinguishable |
| `404`, `text/html` | Path not matched; you got the corporate 404 page, not JSON |
| `302` | You hit a portal endpoint that requires a session |

Always check `Content-Type` before parsing. An error is far more likely to arrive as HTML than as JSON.

## What this API cannot do

No customer, meter, account or premise data — that is available only through a manual business registration and verifiable written customer consent, delivered by email or web portal. No network asset or capacity data — that lives on the WA Government SLIP platform behind a registered account. No outage reporting: outages are reported by phone on 13 13 51, and there is no write operation on this surface.
