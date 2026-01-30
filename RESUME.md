# Session Resume

**Project:** Mosaic Group Water Meter Monitoring
**Code:** MGW
**Last Updated:** 2026-01-30

---

## Right Now

**Phase:** Post-Site Visit / Solution Design
**Next Action:** Document full site visit findings, then develop technical proposal

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

## Last Session (2026-01-30)

### Completed
- Site visit conducted at 477 Anton Lembede Street (City Life)
- Met Tanya Dowley (part owner), Ryan (plumber), Sumir (IT technician)
- Photographed main riser and piping infrastructure
- Uploaded Precision Meters vendor spec sheets (4 product PDFs)
- Conducted initial research on smart metering architecture for high-rise

### Previous Session (2026-01-28)
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

**Building:** City Life / Delta Towers - 15-story high-rise, 48 units per floor (~720 units)
**Note:** Meters to be ordered in phases. Must source from Precision Meters.

---

## Project Context

**Water meter telemetry and monitoring solution** for Mosaic Group's block of flats in Durban CBD.

### Business Relationship
- **Precision Meters** (Bradley Cassani, Garth Le Roux) referred Jason to Mosaic Group
- **Condition:** Must use Precision Meters' smart water meters
- Jason provides: telemetry, monitoring platform, integration services

### Site: 477 Anton Lembede Street, Durban (City Life / Delta Towers)
- 15-story high-rise, 48 units per floor (~720 units total)
- Single 75mm main riser through all floors
- 50mm branch per floor, 16mm feed to each unit
- Requires bulk meters (50mm) for leak detection
- Requires unit meters for individual flat metering
- Phased rollout planned

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
| **Ryan** | Mosaic Group | In-house Plumber | - |
| **Sumir** | Mosaic Group | In-house IT Technician | - |
| **Bradley Cassani** | Precision Meters | Smart Meter Manager | 061 864 0536 |
| **Garth Le Roux** | Precision Meters | Sales Rep | 072 469 8133 |

---

## Site Visit Checklist (29 Jan) - COMPLETED

### Assessed
- [x] Building layout and infrastructure
- [x] Existing water infrastructure and piping (75mm riser -> 50mm floor -> 16mm unit)
- [ ] Bulk meter locations (11 points for leak detection) - *awaiting full notes*
- [ ] Unit meter installation requirements - *awaiting full notes*
- [ ] Connectivity options (LoRa coverage, Ethernet availability) - *awaiting full notes*
- [ ] Gateway placement options - *awaiting full notes*
- [ ] Power availability at meter locations - *awaiting full notes*

### Discussed
- [ ] Timeline and priorities - *awaiting full notes*
- [ ] Phased rollout preferences - *awaiting full notes*
- [ ] Budget considerations - *awaiting full notes*
- [ ] Decision-making process - *awaiting full notes*

### Captured
- [x] Photographs of key areas (main riser photo)
- [ ] Building floor plans (if available) - *awaiting full notes*
- [ ] Existing meter details - *awaiting full notes*
- [ ] Notes on challenges/constraints - *awaiting full notes*

**Note:** Full site visit findings pending - Jason to provide detailed notes.

---

## Next Steps

1. **Now:** Document full site visit findings (awaiting Jason's detailed notes)
2. **Next:** Create site assessment report
3. **Design:** Create technical solution for phased rollout
4. **Proposal:** Develop proposal with Precision Meters pricing
5. **Present:** Submit proposal to Tanya

---

## Correspondence Log

| Date | Reference | Description |
|------|-----------|-------------|
| 2026-01-28 | MGW-COR-202601-001 | Initial contact emails |

---

## Notes

- Precision Meters has no KZN presence (Cape Town based)
- Jason positioned as N3 corridor contact for field support
- ~720 meters is a significant project - phased approach wise
- Leak detection (bulk meters) likely higher priority
- Demo platform shared: https://meter-tracker.com
- **Tanya has NOT registered on demo platform yet** (as of 2026-01-30)
- Building identified as City Life / Delta Towers - 15 floors, 48 units/floor
- Single 75mm riser serves entire building
- Research completed on LoRaWAN architecture for high-rise deployment
- Vendor spec sheets uploaded from Precision Meters (4 product lines)
