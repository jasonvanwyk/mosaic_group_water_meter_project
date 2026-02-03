# CLTM ERP System Research

**Project:** Mosaic Group Water Meter Monitoring (MGW)
**Date:** 30 January 2026
**Author:** Jason van Wyk, Precept Systems
**Status:** Resolved (2026-02-02) — CLTM is custom in-house application
**Updated:** 3 February 2026

---

## 1. Background

During the site visit on 29 January 2026, Tanya Dowley and Sumir (IT Administrator) referenced the building's existing tenant management / ERP system as "CLTM". Integration with this system is a high-priority requirement — billing data from the water monitoring platform must flow automatically into the ERP to avoid manual data entry and inter-system friction.

The term "CLTM" was captured verbally during the site visit. It may be the product name, an abbreviation, or an informal/internal name.

---

## 2. Web Research Findings

**Result: "CLTM" is not a recognised product name in the South African property management software market.**

Extensive web searches for "CLTM ERP", "CLTM property management", "CLTM tenant management South Africa", and variations returned no matching product. This means "CLTM" is likely:

1. An **abbreviation** or internal shorthand (e.g., "City Life Tenant Management")
2. A **verbal approximation** or mishearing of the actual product name
3. A **niche or bespoke system** without significant web presence
4. An **informal name** used internally by Mosaic Group staff

---

## 3. South African Property Management Software Landscape

The following are the established property/tenant management platforms in South Africa. One of these may be what Mosaic Group refers to as "CLTM":

| System | Vendor | Focus | API | Website |
|--------|--------|-------|-----|---------|
| **MRI Software** | MRI Software (global, SA office) | Full property management ERP | REST API | [mrisoftware.com/za](https://www.mrisoftware.com/za/) |
| **PayProp** | PayProp (SA) | Rental payment processing | API available | [payprop.com/za](https://www.payprop.com/za) |
| **WeconnectU** | WeconnectU (SA) | Body corporate / HOA management | REST API | [weconnectu.co.za](https://www.weconnectu.co.za/) |
| **reOS** | reOS (SA) | Rental management automation | API | [reos.co.za](https://www.reos.co.za/) |
| **PropWorx** | PropWorx (SA) | Estate agent property management | Unknown | [propworx.co.za](https://propworx.co.za/) |
| **Lexpro** | Lexpro (SA) | Sectional title / estates | Cloud-based | [lexpro.co.za](https://lexpro.co.za/property-management/) |
| **MRI Rentbook** | MRI (SA) | Simple rental management | Limited | [rentbook.co.za](https://www.rentbook.co.za/) |
| **Sage 200 Evolution** | Sage (SA) | Accounting + ERP | REST API | [sage.com/za](https://www.sage.com/za/) |

**Note:** Mosaic Group manages 2,500+ residential units and 7,000+ tenants across multiple brands (City Life, Luna, Ole). An operation of this scale likely uses a more capable ERP system (MRI Software, Sage, or a bespoke solution) rather than a lightweight rental tool.

---

## 4. Integration Architecture (Vendor-Agnostic)

Regardless of which ERP system "CLTM" turns out to be, the water monitoring platform should be designed to support multiple integration patterns:

| Method | Complexity | Reliability | Best For |
|--------|-----------|-------------|----------|
| **REST API** (preferred) | Medium | High | Real-time push of billing data |
| **CSV/XML file export** | Low | Medium | Periodic batch import |
| **SFTP file drop** | Low | Medium | Automated file transfer for legacy systems |
| **Webhooks** | Medium | High | Event-driven data push |
| **Direct database** | High | High | Only with vendor approval |
| **Email/PDF report** | Very low | Low | Fallback for manual capture |

**Recommended approach for the proposal:** Design the water monitoring platform to expose a billing API and generate data in multiple formats (JSON, CSV, PDF). The actual integration connector will be built once the ERP's API capabilities are confirmed.

### Data Points Required for Integration

| Water Monitoring System | ERP System | Notes |
|------------------------|------------|-------|
| Unit / apartment ID | Tenant account / lease ID | Mapping table required |
| Floor number | Property section | Hierarchy mapping |
| Consumption (kL) | Billing line item | Usage for the period |
| Billing amount (ZAR) | Invoice line / charge | Consumption x tariff |
| Period start/end | Billing cycle | Must align with ERP cycles |
| Meter serial number | Asset reference | Audit trail |
| Reading date/time | Transaction date | Timestamp |

---

## 5. Resolution — Sumir's Reply (2 February 2026)

Sumir Singh (Mosaic Group IT) replied via email on 2 February 2026 (ref: MGW-COR-202602-003):

### Confirmed Facts

| Item | Detail |
|------|--------|
| **CLTM identity** | Custom-built, in-house application |
| **Developer** | Remote developer (name unknown, coordinated via Sumir) |
| **Vendor** | None — not a commercial product |
| **"CLTM" meaning** | Likely "City Life Tenant Management" (hypothesis 1 from section 2 was correct) |

### Integration Model

- Sumir's developer advised: **Jason should build the water platform with API calls**
- Their developer will integrate those API calls into CLTM on their side
- **Precedent:** Ekhwesi (electricity/prepaid system) already integrated into CLTM via API + Postman collections
- Same pattern expected for water monitoring

### What This Means for Solution Design

1. Water monitoring platform must expose a **clean REST API** with documented endpoints
2. **Postman collection** should be provided (following the Ekhwesi precedent they're familiar with)
3. **We do not need to write the CLTM integration** — their developer handles that side
4. API contract/schema should be shared with Sumir's developer for review
5. Data points from section 4 (unit ID, consumption, billing amount, etc.) remain the required payload

### Previous Contingency Planning (Superseded)

The fallback options (CSV export, PDF reports, etc.) from the original research are no longer the primary concern since API integration is confirmed as the approach. However, CSV/PDF export remains a sensible inclusion in the platform for reporting and audit purposes.

---

## 7. Sources

- [MRI Software South Africa](https://www.mrisoftware.com/za/)
- [PayProp South Africa](https://www.payprop.com/za)
- [WeconnectU](https://www.weconnectu.co.za/)
- [reOS](https://www.reos.co.za/)
- [PropWorx](https://propworx.co.za/)
- [Lexpro](https://lexpro.co.za/property-management/)
- [MRI Rentbook](https://www.rentbook.co.za/)
- [Best Property Management Software South Africa](https://egis-software.com/best-property-management-software-south-africa/)
- [Capterra SA Property Management](https://www.capterra.co.za/directory/20050/real-estate-property-management/software)
