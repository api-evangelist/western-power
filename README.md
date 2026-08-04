# Western Power (western-power)

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

Western Power is the Western Australian state-owned statutory corporation that owns and operates the electricity transmission and distribution network — the poles, wires, substations and streetlights — across the South West Interconnected System (SWIS), from Kalbarri in the north to Albany in the south and east to Kalgoorlie, across more than 103,000 km of powerlines, 825,788 poles and towers, 276,000 streetlights and 154 transmission substations. It is a regulated network distributor (DNO/DSO), not a retailer and not a generator; Synergy is the SWIS retailer for residential and small-business customers and AEMO operates the WA Wholesale Electricity Market. Its API posture is honestly minimal — there is no developer portal, no published API program and no published OpenAPI anywhere on westernpower.com.au. It is not, however, API-less: a second enrichment pass on 2026-07-27 found an undocumented first-party JSON API at `westernpower.com.au/api/corp/outage/*` behind the public outage tracker, alongside anonymous search, news and vacancy endpoints and the Esri ArcGIS feature service that draws the outage map. All are internal endpoints of Western Power's web applications — `robots.txt` disallows `/api/` — with no reference, no terms of use, no versioning, no SLA and no support channel. Consumer energy data is real but not programmatic. Australia's Consumer Data Right, the mandate that forced identical banking APIs and was then transplanted into energy, does not reach this organisation: CDR energy covers National Electricity Market retailers, and Western Australia sits outside the NEM while distributors were never designated data holders in any state.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/western-power/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/western-power/refs/heads/main/apis.yml)

## Tags

- Energy
- Australia
- Utilities
- Electricity
- Grid
- Network Distribution
- Smart Metering
- Open Data
- GIS
- Outages

## Timestamps

- **Created:** 2026-07-27
- **Modified:** 2026-07-27

## APIs

### Western Power Outage Areas Feature Service

Live outage data for the South West Interconnected System, served as an Esri ArcGIS Online hosted feature service (layer 0, `Outage_Areas`) that backs the public Western Power outage tracker. Western Power does not document or advertise this as an API, but the service answers anonymous unauthenticated requests (verified HTTP 200 on 2026-07-27), is self-describing via the standard ArcGIS REST `?f=json` metadata convention, and supports the Query capability with a 2,000-record page size. Each outage polygon carries `OUTAGETYPE`, `INCIDENTREF`, `ENARNUMBER`, `OUTAGESTARTTIME`, `ESTIMATEDRESTORATIONTIME`, `PLANNEDOUTAGE`, `NOCUSTOMERSIMPACTED`, `TIMEADDED`, `AFFECTED_AREA` and `AFFECTED_AREA_NOCUSTOMERS`. No API key, token or registration is required; equally, no support, SLA, terms of use or deprecation policy is published for it.

- **Human URL:** [https://www.westernpower.com.au/outages/](https://www.westernpower.com.au/outages/)
- **Base URL:** `https://services2.arcgis.com/tBLxde4cxSlNUxsM/ArcGIS/rest/services/WP_Outage_Prod/FeatureServer`

#### Tags

- Outages
- Grid
- GeoJSON
- ArcGIS
- Real Time

#### Properties

- [Service Metadata](arcgis/wp-outage-featureserver.json)
- [Service Metadata](arcgis/wp-outage-layer0-outage-areas.json)
- [Sample Response](arcgis/wp-outage-sample-query-response.json)
- [Documentation](https://www.westernpower.com.au/outages/)

### Western Power Public Secure Services (SLIP)

Western Power's network asset and capacity spatial data — distribution and transmission overhead powerlines, underground cables, poles, pillars, pits, transformers, enclosures, substations, streetlights, underground power project areas and the Network Capacity Mapping Tool forecast remaining-capacity layers — published as 36 datasets on the WA Government DataWA catalogue and served from the Landgate SLIP platform as ArcGIS REST MapServer/FeatureServer, OGC WFS and OGC WMS endpoints plus GeoJSON, GeoPackage, File Geodatabase and Shapefile downloads. DataWA labels these "open data subject to registering for access"; access is automatically granted on registering an email address and agreeing to Western Power's data licence terms and conditions. Anonymous calls are rejected — both the MapServer root and layer 10 returned HTTP 401 Unauthorized on 2026-07-27, and the bulk download host redirects to `/Account/Login`. Authenticated access uses Esri token authentication via `token.slip.wa.gov.au`.

- **Human URL:** [https://catalogue.data.wa.gov.au/dataset/distribution-overhead-power-lines-wp-031](https://catalogue.data.wa.gov.au/dataset/distribution-overhead-power-lines-wp-031)
- **Base URL:** `https://services.slip.wa.gov.au/arcgis/rest/services/WP_Public_Secure_Services/WP_Public_Secure_Services/MapServer`

#### Tags

- Open Data
- GIS
- Grid
- Network Capacity
- WFS

#### Properties

- [Catalog](catalog/datawa-western-power-datasets.json)
- [Catalog Entry](catalog/datawa-wp-031-distribution-overhead-powerlines.json)
- [Documentation](https://catalogue.data.wa.gov.au/group/about/western-power-group)
- [License](https://catalogue.data.wa.gov.au/dataset/wp-licence-terms-and-conditions)
- [Application](https://www.westernpower.com.au/resources-education/calculators-tools/network-capacity-mapping-tool/)

## Mandate and Access

- **Mandate regime:** none. CDR energy applies to National Electricity Market retailers with AEMO as gateway; Western Australia is outside the NEM, and distribution network operators were never designated data holders in any state.
- **Mandate status:** not applicable. Western Power makes no CDR claim and no CDR register entry, base URI or Consumer Data Standards energy surface was found.
- **Secondary obligation:** the Electricity Industry (Metering) Code 2012 (WA), administered by the Economic Regulation Authority, obliges metering data provision — discharged with a registration form, a verifiable consent form, and delivery by email or web portal. No interface is specified and none was built.
- **Data standard:** no standard reference found. No Green Button/ESPI, no Consumer Data Standards, no IEEE 2030.5, OpenADR, OCPP/OCPI or IEC CIM. Only geospatial conventions apply — Esri ArcGIS REST, OGC WFS/WMS.
- **Consumer data API:** no. Third parties register a business with Western Power, obtain verifiable customer consent per meter, and receive up to two years of interval and accumulated data by email or web portal.
- **Market data open:** partially. The live outage feature service is anonymous and machine-readable but undocumented; the 36 catalogued network datasets are login-and-licence gated (HTTP 401 anonymously).
- **Access gate:** licence agreement. Register a SLIP account and accept Western Power's Data Licence Terms and Conditions for network data; there is no developer signup because there is no developer program.
- **Auth model:** none for the outage service; Esri token authentication for SLIP services; session login for bulk downloads; manual consent for consumer data; username/password for the restricted retailer and generator portal.

## Common Properties

- [Website](https://www.westernpower.com.au/)
- [About](https://www.westernpower.com.au/about/)
- [Blog](https://www.westernpower.com.au/news/)
- [Open Data](https://catalogue.data.wa.gov.au/group/about/western-power-group)
- [License](https://catalogue.data.wa.gov.au/dataset/wp-licence-terms-and-conditions)
- [Documentation](https://www.westernpower.com.au/resources-education/industry-resources/retailers-and-generators/)
- [Registration](https://www.westernpower.com.au/issues-enquiries/requests-preferences/registration-for-access-to-energy-data/)
- [Consent](https://www.westernpower.com.au/issues-enquiries/requests-preferences/verifiable-consent-for-access-to-energy-data/)
- [Portal](https://services.westernpower.com.au/online/nbu/do/restricted/Home)
- [Portal](https://www.mywpprojects.westernpower.com.au/)

## Maintainers

- Kin Lane — kin@apievangelist.com

## Enrichment round — 2026-07-27

Artifacts added in the second pipeline pass. Every OpenAPI here was **derived by API Evangelist** from live probes or harvested service metadata and is labelled as such in its `info` block; Western Power publishes none of them.

- `openapi/` — three OpenAPI 3.1 descriptions (outage web API, corporate web API, ArcGIS feature service)
- `overlays/` — one Overlay 1.0.0 per description, carrying provenance and operational caveats
- `examples/` — live sample responses captured 2026-07-27
- `authentication/` — anonymous, Esri token, session login; no API key, OAuth or OIDC anywhere
- `conventions/` — pagination, ISO 8601 basic timestamps, caching, rate limiting, versioning
- `errors/` — three inconsistent error idioms; 204-as-not-found; ArcGIS errors inside HTTP 200
- `lifecycle/` — no versioning, no deprecation policy, no SLA, no API status page
- `conformance/` — geospatial and vendor standards only; no energy data standard; no published certifications
- `data-model/` — one Outage entity projected two incompatible ways
- `security/` — TLS/HSTS/DNS probe; no vulnerability disclosure programme, no trust centre
- `well-known/` — every `/.well-known/` path probed; all 404
- `packages/` — probed negative; no client library in any registry
- `skills/` — two generated agent skills grounded in verified operationIds
- `mcp/` — candidate tool list only; Western Power operates no MCP server
- `agentic-access/` — recommended `x-agentic-access` contracts; all nine described operations are reads
- `llms/` — generated llms.txt

The site's client bundle references 74 `/api/` paths. The customer, enquiry, organisation and project endpoints handle personal information and were deliberately **not** probed, catalogued or tooled.
