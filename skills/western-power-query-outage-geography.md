---
name: Query Western Power outage geography
description: Pull outage area polygons from Western Power's Esri ArcGIS feature service for mapping, spatial filtering and statistics.
api: openapi/western-power-arcgis-outage-openapi.yml
operations:
  - getFeatureServerMetadata
  - getOutageAreasLayerMetadata
  - queryOutageAreas
generated: '2026-07-27'
method: generated
---

# Query Western Power outage geography

The Western Power outage tracker is drawn from an Esri ArcGIS Online hosted feature service. Unlike the JSON web API, this surface gives you **polygons** — the actual affected areas — plus SQL filtering, spatial filtering and server-side statistics. It answers anonymous requests and is fully self-describing.

Western Power does not advertise, document or support this service. It is an Esri-hosted item that could be republished under a new service item id at any time, which would break every caller.

## Before you start

- **Auth:** none. Anonymous HTTPS GET.
- **Base URL:** `https://services2.arcgis.com/tBLxde4cxSlNUxsM/ArcGIS/rest/services/WP_Outage_Prod/FeatureServer`
- **Layer:** `0` — `Outage_Areas`, polygon geometry, spatial reference 102100 / 3857 (Web Mercator).
- **Capability:** `Query` only. There are no edit operations available to you.
- **Cache:** the layer declares `cacheMaxAge` 120 seconds. Do not poll faster.

## 1. Read the service before you query it

Call `getOutageAreasLayerMetadata` (`GET /0?f=json`). It returns the authoritative field list, the indexes, the query capabilities and the limits. Do this once at startup rather than hard-coding assumptions — the schema was last changed in December 2025 and can change again.

Key limits it declares: `maxRecordCount` 2000, `standardMaxRecordCountNoGeometry` 32000, `supportsPagination` true.

## 2. Query the layer

Call `queryOutageAreas` (`GET /0/query`). A minimal all-records call:

```
/0/query?where=1%3D1&outFields=*&returnGeometry=false&resultRecordCount=100&f=json
```

Parameters worth knowing:

- `where` — SQL-92. `useStandardizedQueries` is true, so standard SQL syntax applies. Use `1=1` for everything.
- `outFields` — comma-separated, or `*`. Narrow this; the text fields are wide (`Tags` is 4000 characters).
- `returnGeometry` — set `false` when you only want attributes. This also raises your page ceiling from 2,000 to 32,000.
- `f` — `json`, `geojson` or `pbf`. **Use `f=geojson`** if you are feeding a mapping library; the layer's `supportedQueryFormats` is `"JSON, geoJSON, PBF"`.
- `outSR` — reproject on the server. Pass `4326` if you want WGS84 lat/long instead of Web Mercator.

## 3. Paginate

Use `resultOffset` and `resultRecordCount` together. Check `exceededTransferLimit` in the response — when it is true there are more records than you received, regardless of what you asked for.

## 4. Filter spatially

Pass `geometry` plus `geometryType` and `spatialRel`. The layer advertises ten spatial relationships including `esriSpatialRelIntersects` (the default), `esriSpatialRelWithin` and `esriSpatialRelContains`. This is how you answer "is this address inside an outage area" without pulling the whole layer.

## 5. Aggregate without downloading

The layer supports server-side statistics — `COUNT`, `SUM`, `AVG`, `MIN`, `MAX`, percentiles — plus `orderByFields`, `groupByFieldsForStatistics` and `returnCountOnly`. Prefer `returnCountOnly=true` over fetching records to count them.

## 6. Interpret the fields

Field names differ completely from the JSON web API:

| ArcGIS field | Meaning |
|---|---|
| `OBJECTID` | System primary key. Not stable across republishing. |
| `INCIDENTREF` | Incident reference. Indexed — filter on this. |
| `OUTAGETYPE` | Outage classification. No code list is published. |
| `PLANNEDOUTAGE` | Planned flag, carried as a **string**, not a boolean. |
| `OUTAGESTARTTIME`, `ESTIMATEDRESTORATIONTIME` | Dates carried as **strings**, not date fields. Parse defensively. |
| `TIMEADDED` | A real date field — epoch **milliseconds**, UTC. |
| `NOCUSTOMERSIMPACTED` | Integer. |
| `AFFECTED_AREA`, `AFFECTED_AREA_NOCUSTOMERS` | Wide free-text suburb lists. |

## 7. Handle errors

ArcGIS reports failures **inside HTTP 200**:

```json
{"error":{"code":400,"message":"Unable to complete operation.","details":["..."]}}
```

Check for an `error` key on every response before assuming you have a feature set. A 200 status means nothing here.

## Joining to the JSON web API

Both surfaces describe the same incidents, and `outageId` on the web API looks related to `INCIDENTREF` here — but the formats differ and **no published mapping exists**. Verify a live pair before relying on the join. See `data-model/western-power-data-model.yml`.
