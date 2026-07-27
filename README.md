# Western Power (western-power)

Western Power is the Western Australian state-owned statutory corporation that owns and operates the electricity transmission and distribution network — the poles, wires, substations and streetlights — across the South West Interconnected System (SWIS), from Kalbarri in the north to Albany in the south and east to Kalgoorlie, across more than 103,000 km of powerlines, 825,788 poles and towers, 276,000 streetlights and 154 transmission substations. It is a regulated network distributor (DNO/DSO), not a retailer and not a generator; Synergy is the SWIS retailer for residential and small-business customers and AEMO operates the WA Wholesale Electricity Market. Its API posture is honestly minimal — there is no developer portal, no published API program and no OpenAPI anywhere on westernpower.com.au. Consumer energy data is real but not programmatic. Australia's Consumer Data Right, the mandate that forced identical banking APIs and was then transplanted into energy, does not reach this organisation: CDR energy covers National Electricity Market retailers, and Western Australia sits outside the NEM while distributors were never designated data holders in any state.

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
