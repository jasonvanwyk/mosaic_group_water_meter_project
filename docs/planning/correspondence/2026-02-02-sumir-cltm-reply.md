# Sumir Singh — CLTM ERP Integration Reply

**Date:** 2026-02-02, 09:59
**Reference:** MGW-COR-202602-003
**Type:** Email (inbound reply to MGW-COR-202601-003 / site visit thread)

---

## Email: Sumir Singh → Jason van Wyk

**From:** Sumir Singh <sumir@city-life.co.za>
**To:** Jason van Wyk <jason@precept.co.za>
**CC:** Tanya Dowley <tanya@mosaicgroup.co.za>, Ryan <ryan@mosaicgroup.co.za>
**Date:** 2026-02-02, 09:59
**Subject:** Re: Precept Systems Introductory Meeting and Site Assessment 477 Anton Lembede Street Durban

---

## Key Information

### CLTM System Identity

| Item | Detail |
|------|--------|
| **System** | CLTM |
| **Type** | Custom-built, in-house application |
| **Developer** | Remote developer (name unknown) |
| **Vendor** | None — not a commercial product |

### Integration Approach

Sumir spoke with their developer, who advised:

- Jason should build the water monitoring platform with **API calls**
- Their developer will integrate those API calls into CLTM
- **Pattern:** Same approach used for their electricity system (Ekhwesi) — API + Postman collections for seamless integration

### Precedent: Ekhwesi Electricity Integration

- Mosaic Group uses **Ekhwesi** for their electricity/prepaid system
- Ekhwesi provides API access and Postman collections
- Their remote developer integrated Ekhwesi into CLTM via these APIs
- Water monitoring integration expected to follow the same model

---

## Implications for Solution Design

1. **No vendor documentation to source** — CLTM is custom, not a commercial ERP
2. **API-first design required** — water platform must expose a clean REST API
3. **Their developer consumes our API** — we don't need to write the CLTM integration ourselves
4. **Postman collections recommended** — follow Ekhwesi precedent, provide Postman collection for our API
5. **Coordination via Sumir** — developer is remote, Sumir is the on-site liaison

---

## Jason's Reply

**Date:** 2026-02-02, 10:11
**Subject:** Re: Precept Systems Introductory Meeting and Site Assessment 477 Anton Lembede Street Durban

Brief acknowledgement: "This is great—many thanks for the information. I will be in touch soon regarding the project proposal."

---

## Action Items

- [ ] Design water monitoring API with CLTM integration in mind
- [ ] Provide Postman collection for API endpoints (following Ekhwesi precedent)
- [ ] Coordinate with Sumir's remote developer on API contract/schema
