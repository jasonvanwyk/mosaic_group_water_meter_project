# CLTM ERP System Research

**Project:** Mosaic Group Water Meter Monitoring (MGW)
**Date:** 30 January 2026
**Author:** Jason van Wyk, Precept Systems
**Status:** Incomplete — requires client clarification

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

## 5. Action Required — HIGHEST PRIORITY

The fastest path to resolving this is to contact **Sumir** (Mosaic Group IT Administrator) directly.

### Draft Email to Sumir

> **Subject:** Water Monitoring Project — ERP System Information Request
>
> Hi Sumir,
>
> Following our site visit on Wednesday, I am working on the technical solution design for the water monitoring system. One of the key requirements is integration with your tenant management / ERP system.
>
> During the visit, I captured the system name as "CLTM" — could you confirm:
>
> 1. The full product name and version
> 2. The software vendor / provider company name
> 3. The vendor's website
> 4. Your login URL for the system
> 5. Whether the system has an API (for programmatic data exchange)
> 6. Any technical documentation or developer guides available
> 7. Your account manager or support contact at the vendor
> 8. How utility billing data is currently captured into the system (manual entry, import, etc.)
>
> This information will allow me to design the integration between the water monitoring platform and your billing system, ensuring automated and frictionless data flow as Tanya requested.
>
> Kind regards,
> Jason van Wyk
> Precept Systems

**Note:** Sumir's contact details have been requested from Tanya (pending).

---

## 6. Contingency Planning

If the ERP system has **no API** (common with older/bespoke systems):

| Fallback | Implementation |
|----------|---------------|
| CSV export | Generate monthly billing CSV for manual import into ERP |
| PDF reports | Generate per-unit billing reports for manual capture |
| Screen scraping | Automate data entry via browser automation (last resort) |
| Middleware | Build a lightweight API wrapper if the ERP has a database accessible via ODBC/SQL |

The proposal should present API integration as the recommended approach, with CSV/PDF export as an included fallback at no additional cost. This manages the risk of the ERP not supporting modern integration.

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
