# Project Status

## Current Status
**Phase:** Post-Site Visit / Proposal Development
**Health:** Green
**Last Updated:** 2026-02-04

## Completed
- [x] Project directory created
- [x] Documentation files initialized
- [x] Folder structure established
- [x] GitHub repository linked
- [x] Contact details captured (Tanya, Bradley, Garth, Ryan, Sumir)
- [x] Mosaic Group research completed
- [x] Reference project identified (Fairfield Water)
- [x] Initial phone call with Tanya Dowley
- [x] Site visit scheduled and confirmed
- [x] Email sent to Tanya confirming meeting
- [x] Email sent to Bradley & Garth updating on progress
- [x] **Site visit conducted (29 Jan 2026)** - met Tanya, Ryan (plumber), Sumir (IT)
- [x] Site photography (main riser)
- [x] Precision Meters vendor spec sheets uploaded (4 products)
- [x] Initial research: Smart metering architecture for high-rise
- [x] **Site visit notes formalized** into full report

## In Progress
- [x] **Internal research** — CLTM ERP, hosting options, solution design groundwork (completed 2026-01-30)
- [x] **Confirm CLTM ERP identity** — resolved 2026-02-02: CLTM is custom-built in-house (not commercial). API integration model confirmed.
- [x] **Confirm Precision Meters LoRaWAN** — confirmed by Garth (2026-02-03): LoRaWAN standard on all ultrasonic sizes
- [x] **Meter pricing received** — 15mm: R2,100, 50mm: R9,360 (ex VAT)
- [x] **Supply chain agreed** — Precision invoices Mosaic Group directly for meters; Precept handles platform
- [ ] **Develop ChirpStack payload codec** — no codec supplied; APP & DEV keys provided per meter

## Site Visit Summary (29 Jan 2026)

| Item | Details |
|------|---------|
| **Date** | Wednesday, 29 January 2026, 12:00 PM |
| **Location** | 477 Anton Lembede Street, Durban (City Life) |
| **Attendees** | Tanya Dowley (part owner), Ryan (plumber), Sumir (IT technician) |
| **Status** | Completed |

### Piping Infrastructure Observed
| Section | Size | Notes |
|---------|------|-------|
| Main riser | 75mm | Single riser through all floors |
| Floor branch | 50mm | Branches off riser at each floor |
| Unit feed | 16mm | From floor branch to each unit |

## Building Profile (confirmed from site visit)

| Item | Details |
|------|---------|
| Building | City Life |
| Total floors | 27 |
| Floors in scope | 12 (first 12 — upper floors have a different tenant type) |
| Units per floor | 48 |
| Total units in scope | 576 |
| Building age | ~60 years |
| Location | 477 Anton Lembede Street, Durban CBD |

## Vendor Specs Collected (Precision Meters)

| Product | Pipe Size | Type |
|---------|-----------|------|
| Ultrasonic Water Meters | 15mm - 25mm | Ultrasonic |
| Ultrasonic 32 Water Meters | 32mm - 50mm | Ultrasonic |
| ASM Polymer Volumetric Rotary Piston | - | Mechanical |
| Infinity Evo Class C Woltmann | - | Woltmann |

## Confirmed Pricing (2026-02-03, from Garth Le Roux)

| Meter | Unit Price (ex VAT) | Qty Needed | Total (ex VAT) |
|-------|---------------------|------------|----------------|
| 15mm ultrasonic (LoRaWAN) — unit meters | R2,100 | 576 | R1,209,600 |
| 50mm ultrasonic (LoRaWAN) — bulk meters | R9,360 | 12 | R112,320 |
| **Total** | | **588** | **R1,321,920** |

### Pilot Phase

| Meter | Unit Price (ex VAT) | Qty | Total (ex VAT) |
|-------|---------------------|-----|----------------|
| 15mm ultrasonic (LoRaWAN) | R2,100 | 6 | R12,600 |
| 50mm ultrasonic (LoRaWAN) | R9,360 | 1 | R9,360 |
| **Pilot total** | | **7** | **R21,960** |

### Technical Confirmations

| Item | Detail |
|------|--------|
| LoRaWAN radio | Standard on all ultrasonic sizes |
| Alternative protocol | Wireless M-BUS available |
| ChirpStack integration | APP & DEV keys supplied per meter; codec must be developed |
| Default uplink interval | 8 hours |
| Minimum uplink interval | 4 hours (affects battery life) |
| Uplink configuration | Pre-dispatch only (not field-configurable via NFC) |
| 15mm-to-16mm pipe | Tail pieces supplied with meters |
| 50mm spec sheet | Confirmed: 2nd "40mm" column is actually 50mm |

### Supply Chain Arrangement

- **Meters:** Precision Meters invoices Mosaic Group directly (avoids 15% VAT markup)
- **Platform & integration:** Precept Systems invoices Mosaic Group separately

## Todo

### Now
- [x] ~~Document full site visit findings~~ — Done
- [x] ~~Research CLTM ERP system~~ — Done (not identified; must ask Sumir for exact product name)
- [x] ~~Research hosting options~~ — Done (Hetzner SA VPS recommended for cloud; on-prem Proxmox for pilot)
- [x] ~~Research solution design~~ — Done (LoRaWAN + ChirpStack, 3 gateways, RAK3172 nodes, valve-ready PCB)
- [x] **Email Tanya re CLTM** — sent 2026-01-30 12:07, requesting product name and vendor details (awaiting reply)
- [x] **Email Bradley & Garth** — sent 2026-01-30; **all 7 questions answered by Garth 2026-02-03 11:31**

### Next
- [x] ~~Create site assessment report~~ — Done (MD + HTML versions, sent to Bradley/Garth 2026-01-30)
- [ ] Build business case (benefits analysis, ROI, alternatives comparison)
- [ ] Create technical solution design (formal)
- [ ] Develop proposal with phased approach, brief, scope, and pricing
- [ ] Coordinate with Precision Meters on meter specs/pricing for 576 units + 12 bulk meters

## Blockers
| Blocker | Owner | Status |
|---------|-------|--------|
| ~~CLTM ERP identity~~ — **Resolved 2026-02-02.** Custom in-house app, API integration confirmed. | Jason | Done |
| ~~Precision Meters LoRaWAN confirmation~~ — **Resolved 2026-02-03.** All questions answered. | Jason | Done |
| Revised meter pricing from Garth — Tanya requested volume discount, Garth revising with manager | Garth | Waiting |

## Strategic Notes

- **Supplier exclusivity committed:** Jason has committed to Garth that the platform is strategically partnered with Precision Meters only. Jason will not work with other meter suppliers on this project. This must be communicated to Tanya.
- **Tanya shopping around:** Tanya has told Garth she is comparing other meter suppliers. If she selects a different supplier, Precept's platform offering would not apply.
- **Proposal implication:** The proposal should clearly state the platform is built for Precision Meters integration.

## Upcoming
| Event | Date | Notes |
|-------|------|-------|
| ~~Document site findings~~ | 2026-01-30 | Done — formalized into site visit report |
| ~~Internal research~~ | 2026-01-30 | Done — CLTM, hosting, solution design |
| ~~Site assessment report~~ | 2026-01-30 | Done — sent to Precision Meters |
| Business case + proposal | 5 Feb 2026 | Benefits, alternatives, pricing — target delivery |
| Submit to Tanya | 5 Feb 2026 | Tanya advised proposal coming tomorrow |

## Key Contacts

| Person | Company | Role | Cell |
|--------|---------|------|------|
| Tanya Dowley | Mosaic Group | Part Owner / Client | 072 227 0883 |
| Ryan | Mosaic Group | In-house Plumber | 084 701 2780 |
| Sumir | Mosaic Group | In-house IT Technician | 084 347 5755 |
| Bradley Cassani | Precision Meters | Smart Meter Manager | 061 864 0536 |
| Garth Le Roux | Precision Meters | Sales Rep (Referrer) | 072 469 8133 |

## Business Context

**Referral Chain:**
1. Bradley Cassani (Precision Meters) - existing relationship from Fairfield project
2. Garth Le Roux (Precision Meters) - offered the Mosaic Group opportunity
3. Tanya Dowley (Mosaic Group) - client contact

**Condition:** Solution must use Precision Meters smart water meters.

## Reference Project

**Fairfield Water** (`/home/jason/Projects/fairfield-water`)
- Similar scope: water meter monitoring with IoT telemetry
- Phase 1: Manual meter reading + tiered billing (deployed)
- Phase 2: Automated smart metering with LoRa/Ethernet (in progress)
- Production URL: https://fairfield.meter-tracker.com
- Demo platform shared with Tanya: https://meter-tracker.com

## Notes
- Project initialized: 2026-01-28
- Initial phone call with Tanya: 2026-01-28
- Site visit completed: 2026-01-29
- Mosaic Group is a major property developer in Durban
- 477 Anton Lembede Street is City Life — 27 floors total, 12 in scope, 576 units
- Single 75mm main riser -> 50mm floor branches -> 16mm unit feeds
- Current water bill: R1.2M/month, flat tenant charge: R400/month, tariff: R65.91/kL
- CLTM ERP integration required (tenant management/billing system)
- Tanya is part owner of Mosaic Group (not just contact)
- Orders will be phased (not all at once)
- Demo platform (meter-tracker.com) shared with Tanya - not yet registered
- Research completed: LoRaWAN architecture recommended for high-rise
- Vendor specs collected: ultrasonic (15-50mm), mechanical (piston), Woltmann

## Correspondence

| Date | Reference | Description |
|------|-----------|-------------|
| 2026-01-28 | MGW-COR-202601-001 | [Initial contact emails](docs/planning/correspondence/2026-01-28-initial-contact-emails.md) |
| 2026-01-29 | MGW-COR-202601-002 | [Site visit email thread](docs/planning/correspondence/2026-01-29-site-visit-email-thread.md) |
| 2026-01-30 | MGW-COR-202601-003 | [CLTM ERP inquiry to Tanya](docs/planning/correspondence/2026-01-30-tanya-cltm-inquiry.md) |
| 2026-01-30 | MGW-COR-202601-004 | [Site assessment report & meter questions to Bradley/Garth](docs/planning/correspondence/2026-01-30-bradley-site-assessment-email.md) |
| 2026-02-02 | MGW-COR-202602-003 | [Sumir reply — CLTM is custom in-house, API integration](docs/planning/correspondence/2026-02-02-sumir-cltm-reply.md) |
| 2026-02-03 | MGW-COR-202602-001 | [Phone call with Garth](docs/planning/correspondence/2026-02-03-garth-phone-call.md) |
| 2026-02-03 | MGW-COR-202602-002 | [Garth reply — all 7 questions answered](docs/planning/correspondence/2026-02-03-garth-meter-questions-reply.md) |
| 2026-02-03 | MGW-COR-202602-001 | [Phone call with Garth — reply expected today PM](docs/planning/correspondence/2026-02-03-garth-phone-call.md) |
| 2026-02-04 | MGW-COR-202602-004 | [Proposal update to Tanya](docs/planning/correspondence/2026-02-04-tanya-proposal-update.md) |
| 2026-02-04 | MGW-COR-202602-005 | [Tanya reply — meters direct from Garth](docs/planning/correspondence/2026-02-04-tanya-proposal-update-reply.md) |
| 2026-02-04 | MGW-COR-202602-006 | [Phone call with Garth — pricing revision, supplier loyalty](docs/planning/correspondence/2026-02-04-garth-phone-call.md) |

## Documentation

See [DIRECTORY.md](DIRECTORY.md) for full project structure.
See [docs/planning/contacts.md](docs/planning/contacts.md) for full contact details.
