# Project Status

## Current Status
**Phase:** Post-Site Visit / Solution Design
**Health:** Green
**Last Updated:** 2026-01-30

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
- [ ] **Internal research** — CLTM ERP system, hosting options, motorised ball valve power, solution design

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
| Building | City Life (Delta Towers) |
| Total floors | 27 |
| Floors in scope | 12 (first 12 — upper floors are a different client group) |
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

## Todo

### Now
- [x] ~~Document full site visit findings~~ — Done (see `docs/planning/site-visits/2026-01-29-city-life-site-visit-report.md`)
- [ ] Research CLTM ERP system (provider, API, documentation, contacts)
- [ ] Research hosting options (cloud providers, hybrid, on-prem)
- [ ] Research motorised ball valve power requirements (12V DC, PoE)

### Next
- [ ] Create site assessment report
- [ ] Build business case (benefits analysis, alternatives comparison)
- [ ] Create technical solution design
- [ ] Develop proposal with phased approach, brief, and scope
- [ ] Coordinate with Precision Meters on meter specs/pricing for 576 units + 12 bulk meters

## Blockers
| Blocker | Owner | Status |
|---------|-------|--------|
| None | - | - |

## Upcoming
| Event | Date | Notes |
|-------|------|-------|
| ~~Document site findings~~ | 2026-01-30 | Done — formalized into site visit report |
| Internal research (CLTM, hosting, valves) | Next | Required before proposal |
| Site assessment report | After research | Formal write-up |
| Business case + proposal | After assessment | Benefits, alternatives, pricing |
| Submit to Tanya | Before 1st week Feb | Tanya reviewing pricing then |

## Key Contacts

| Person | Company | Role | Cell |
|--------|---------|------|------|
| Tanya Dowley | Mosaic Group | Part Owner / Client | 072 227 0883 |
| Ryan | Mosaic Group | In-house Plumber | - |
| Sumir | Mosaic Group | In-house IT Technician | - |
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
- 477 Anton Lembede Street is City Life (Delta Towers) — 27 floors total, 12 in scope, 576 units
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

## Documentation

See [DIRECTORY.md](DIRECTORY.md) for full project structure.
See [docs/planning/contacts.md](docs/planning/contacts.md) for full contact details.
