# Session Resume

**Project:** Mosaic Group Water Meter Monitoring
**Code:** MGW
**Last Updated:** 2026-02-04

---

## Right Now

**Phase:** Post-Site Visit / Proposal Development
**Next Action:** Build business case and proposal — target delivery to Tanya by 5 Feb

### Site Visit Completed
| Item | Details |
|------|---------|
| **Date** | Wednesday, 29 January 2026 |
| **Time** | 12:00 PM |
| **Location** | 477 Anton Lembede Street, Durban (City Life) |
| **Attendees** | Tanya Dowley (part owner), Ryan (plumber), Sumir (IT technician) |
| **Status** | Completed |

### Quick Links
| Need | File |
|------|------|
| Project structure | [DIRECTORY.md](DIRECTORY.md) |
| Current status | [STATUS.md](STATUS.md) |
| AI guidance | [CLAUDE.md](CLAUDE.md) |
| Contacts | [docs/planning/contacts.md](docs/planning/contacts.md) |
| Reference project | `/home/jason/Projects/fairfield-water` |

---

## Current Session (2026-02-04)

### Updates
- Emailed Tanya with proposal status update — finalising proposal, waiting on final pricing, out in field today, proposal by tomorrow (5 Feb)
- **Tanya replied (08:20)** — confirmed she will purchase meters directly from Garth (Precision Meters). Garth CC'd on thread. Supply chain arrangement formally acknowledged by client.
- **Garth phone call** — Garth sent Tanya pricing. Tanya requested volume discount and told Garth she's shopping around. Garth raised concern about Tanya using another supplier's meters with Jason's platform. Jason committed to Garth: platform is strategically partnered with Precision Meters only — will not work with other meter suppliers. Jason to communicate this to Tanya. Garth revising pricing with his manager.

---

## Previous Session (2026-02-03)

### Updates
- **09:43** — Called Garth Le Roux (Precision Meters). Confirmed he and Bradley will sit together today and reply to our email (site assessment report + 7 questions) by this afternoon.
- **11:31** — Garth replied to all 7 questions (inline). All technical and commercial questions resolved:
  - LoRaWAN is **standard** on all ultrasonic meters (no add-on needed)
  - 50mm spec sheet error confirmed (mislabelled as 40mm)
  - **Pricing:** 15mm = R2,100/unit, 50mm = R9,360/unit (ex VAT)
  - No ChirpStack codec — APP & DEV keys supplied, codec to be developed
  - Uplink: 8hr default, 4hr minimum (affects battery), configured pre-dispatch
  - Tail pieces supplied with meters (resolves 15mm-to-16mm issue)
  - **Supply chain:** Precision invoices Mosaic Group directly for meters; Precept handles platform separately
  - **Total meter cost:** ~R1.32M ex VAT (576 x R2,100 + 12 x R9,360)
  - **Pilot cost:** ~R21,960 ex VAT (6 x R2,100 + 1 x R9,360)

---

## Earlier Session (2026-01-30)

### Completed
- Site visit conducted at 477 Anton Lembede Street (City Life)
- Met Tanya Dowley (part owner), Ryan (plumber), Sumir (IT technician)
- Photographed main riser and piping infrastructure
- Uploaded Precision Meters vendor spec sheets (4 product PDFs)
- Conducted initial research on smart metering architecture for high-rise
- **Formalized site visit notes** into full report (`docs/planning/site-visits/2026-01-29-city-life-site-visit-report.md`)
- **Corrected building data** across all project docs (27 floors total, 12 in scope, 576 units)
- Updated project docs with new information (water costs, IT infrastructure, CLTM ERP, feature priorities)
- **Completed internal research** — 3 research documents:
  - `docs/planning/internal/Research/03-cltm-erp-research.md` — ERP not identified; must ask Sumir
  - `docs/planning/internal/Research/03-hosting-options-research.md` — Hetzner SA recommended for cloud; on-prem Proxmox for pilot
  - `docs/planning/internal/Research/03-solution-design-research.md` — LoRaWAN architecture, meter selection, valve research, pilot design

### Earlier (2026-01-28)
- Initial phone call with Tanya Dowley
- Site visit scheduled
- Email sent to Tanya confirming meeting
- Email sent to Bradley & Garth updating on progress
- Correspondence saved to `docs/planning/correspondence/`
- Demo platform shared with Tanya: https://meter-tracker.com

### Piping Infrastructure (from site visit)
| Section | Size | Notes |
|---------|------|-------|
| Main riser | 75mm | Single riser through all floors |
| Floor branch | 50mm | Branches off riser at each floor |
| Unit feed | 16mm | From floor branch to each unit |

**Building:** City Life — 27-storey high-rise, 12 floors in scope, 48 units per floor (576 units)
**Note:** Meters to be ordered in phases. Must source from Precision Meters. Pilot: 1 floor + 5 units.

---

## Project Context

**Water meter telemetry and monitoring solution** for Mosaic Group's block of flats in Durban CBD.

### Business Relationship
- **Precision Meters** (Bradley Cassani, Garth Le Roux) referred Jason to Mosaic Group
- **Condition:** Must use Precision Meters' smart water meters
- Jason provides: telemetry, monitoring platform, integration services

### Site: 477 Anton Lembede Street, Durban (City Life)
- 27-storey high-rise, 12 floors in scope (upper floors have a different tenant type; entire building owned by Mosaic Group)
- 48 units per floor, 576 units total in scope
- Building age: ~60 years, construction: concrete/brick/steel
- Single 75mm main riser (gravity-fed from top), 50mm branch per floor, 16mm feed to each unit
- Requires bulk meters (50mm) per floor for leak detection (12 meters)
- Requires unit meters (16mm) for individual flat metering (576 meters)
- Phased rollout planned — pilot: 1 floor + 5 units
- Current water bill: R1.2M/month, flat tenant charge: R400/month
- CLTM ERP integration required for billing

### Reference Project
**Fairfield Water** (`/home/jason/Projects/fairfield-water`)
- Same approach: smart meters + IoT + web dashboard
- Phase 1 deployed, Phase 2 in progress
- Production: https://fairfield.meter-tracker.com

---

## Key Contacts

| Person | Company | Role | Cell |
|--------|---------|------|------|
| **Tanya Dowley** | Mosaic Group | Part Owner / Client | 072 227 0883 |
| **Ryan** | Mosaic Group | In-house Plumber | 084 701 2780 |
| **Sumir** | Mosaic Group | In-house IT Technician | 084 347 5755 |
| **Bradley Cassani** | Precision Meters | Smart Meter Manager | 061 864 0536 |
| **Garth Le Roux** | Precision Meters | Sales Rep | 072 469 8133 |

---

## Site Visit Checklist (29 Jan) - COMPLETED & DOCUMENTED

### Assessed
- [x] Building layout and infrastructure (27 floors, 12 in scope, 576 units)
- [x] Existing water infrastructure and piping (110mm municipal -> 75mm riser -> 50mm floor -> 16mm unit)
- [x] Bulk meter locations (12 floor branches, 50mm each)
- [x] Unit meter installation requirements (576 units, 16mm, entry above front door in passage)
- [x] Connectivity options (Ubiquiti/UniFi, Cat6, 1Gb fiber, VLAN available)
- [x] Gateway placement options (comms room + rooftop, both with power/ethernet/security)
- [x] Power availability (UPS to comms room and switches, rooftop has power)

### Discussed
- [x] Timeline and priorities (Q1 2026 start, pricing review 1st week Feb)
- [x] Phased rollout preferences (pilot: 1 floor + 5 units)
- [x] Budget considerations (allocated, undisclosed, price-sensitive, phased payment)
- [x] Decision-making process (Tanya + board of directors)

### Captured
- [x] Photographs of key areas (`pics/city-life-29012026.jpeg`)
- [ ] Building floor plans — not available
- [x] Existing infrastructure details (isolation valves per unit, no existing meters)
- [x] Notes on challenges/constraints (see formal report)

**Full report:** `docs/planning/site-visits/2026-01-29-city-life-site-visit-report.md`

---

## Next Steps

1. ~~**Document full site visit findings**~~ — Done
2. ~~**Internal research**~~ — Done (CLTM ERP, hosting, solution design)
3. ~~**Email Tanya re CLTM**~~ — Sent 2026-01-30; **answered by Sumir 2026-02-02** — CLTM is custom in-house app, API integration model confirmed
4. ~~**Email Bradley & Garth**~~ — Sent 2026-01-30; **all 7 questions answered by Garth 2026-02-03**
4. ~~**Site assessment report**~~ — Done (MD + HTML, sent to Bradley/Garth 2026-01-30)
5. **Next:** Build business case (benefits analysis, ROI using confirmed pricing)
6. **Then:** Technical solution design with phased rollout
7. **Then:** Develop ChirpStack payload codec for Precision ultrasonic meters
8. **Proposal:** Develop proposal with brief, scope, pricing (meters direct from Precision, platform from Precept)
9. **Present:** Submit proposal to Tanya (before 1st week Feb pricing review)

---

## Correspondence Log

| Date | Reference | Description |
|------|-----------|-------------|
| 2026-01-28 | MGW-COR-202601-001 | Initial contact emails |
| 2026-01-29 | MGW-COR-202601-002 | Site visit email thread (contact details, water tariff data) |
| 2026-01-30 | MGW-COR-202601-003 | CLTM ERP inquiry to Tanya (answered by Sumir 2 Feb) |
| 2026-01-30 | MGW-COR-202601-004 | Site assessment report & meter questions to Bradley/Garth (awaiting reply) |
| 2026-02-02 | MGW-COR-202602-003 | Sumir reply — CLTM is custom in-house, API integration confirmed |
| 2026-02-03 | MGW-COR-202602-001 | Phone call with Garth — reply expected today PM |
| 2026-02-03 | MGW-COR-202602-002 | Garth reply — all 7 questions answered (pricing, LoRaWAN, supply chain) |
| 2026-02-04 | MGW-COR-202602-004 | Email to Tanya — proposal update, delivery by 5 Feb |
| 2026-02-04 | MGW-COR-202602-005 | Tanya reply — will purchase meters directly from Garth |
| 2026-02-04 | MGW-COR-202602-006 | Phone call with Garth — Tanya shopping around, pricing revision, supplier loyalty commitment |

---

## Notes

- Precision Meters has no KZN presence (Cape Town based)
- Jason positioned as N3 corridor contact for field support
- ~720 meters is a significant project - phased approach wise
- Leak detection (bulk meters) likely higher priority
- Demo platform shared: https://meter-tracker.com
- **Tanya has NOT registered on demo platform yet** (as of 2026-01-30)
- Building confirmed as City Life — 27 floors, 12 in scope, 48 units/floor, 576 units
- Building is ~60 years old, concrete/brick/steel construction, no basements
- Single 75mm riser, gravity-fed from top, pumped to lower floors
- Municipal supply: 110mm, R1.2M/month bill, R65.91/kL, 10,500 kL/month (whole building)
- Current tenant charge: flat R400/month — no usage-based billing
- CLTM ERP integration required — CLTM is custom in-house (not commercial). Their remote developer will consume our API (same pattern as Ekhwesi electricity integration). Postman collection recommended.
- Ubiquiti/UniFi network, Cat6 copper, 1Gb Vox fiber, VLANs, UPS
- Prepaid meters rejected, fire/sprinkler system out of scope
- Research completed on LoRaWAN architecture for high-rise deployment
- Vendor spec sheets uploaded from Precision Meters (4 product lines)
- Site visit notes formalized into report (2026-01-30)
