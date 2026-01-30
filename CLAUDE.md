# Claude AI Assistant Guide

**Project:** Mosaic Group Water Meter Monitoring
**Code:** MGW
**Last Updated:** 2026-01-30

---

## Quick Context

This is a **water meter telemetry and monitoring solution** for Mosaic Group, a real estate development company in Durban with extensive residential and commercial properties (2,500+ housing units, student accommodation, retail, fitness facilities).

The project is similar to the **Fairfield Dairy water monitoring system** (`/home/jason/Projects/fairfield-water`) - a custom-built solution using smart water meters from Precision Meters with IoT telemetry and a web-based monitoring dashboard.

**Business Relationship:**
- **Precision Meters** (Bradley Cassani, Garth Le Roux) referred Jason to Mosaic Group
- Condition: Must use Precision Meters' smart water meters
- Jason provides the telemetry, monitoring platform, and integration services

---

## Project Status

**Phase:** Post-Site Visit / Solution Design
**Next Milestone:** Site assessment report and technical proposal

### Key Files to Read First

1. **[RESUME.md](RESUME.md)** - Session context, what's complete, what's next
2. **[STATUS.md](STATUS.md)** - Current status, blockers, upcoming events
3. **[docs/planning/contacts.md](docs/planning/contacts.md)** - All contact details

### Reference Project

**Fairfield Water:** `/home/jason/Projects/fairfield-water`
- Similar scope: water meter monitoring with IoT telemetry
- Phase 1 deployed: Manual meter reading + tiered billing
- Phase 2 in progress: Automated smart metering with LoRa/Ethernet
- Production URL: https://fairfield.meter-tracker.com

---

## Team & Contacts

### Client (Mosaic Group)

| Person | Role | Contact |
|--------|------|---------|
| **Tanya Dowley** | Part Owner / Primary Contact | 072 227 0883 / 031 562 8716 / tanya@mosaicgroup.co.za |
| **Ryan** | In-house Plumber | 084 701 2780 / ryan@mosaicgroup.co.za |
| **Sumir** | In-house IT Technician | 084 347 5755 / sumir@city-life.co.za |

### Supplier (Precision Meters)

| Person | Role | Contact |
|--------|------|---------|
| **Bradley Cassani** | Smart Meter Manager | 061 864 0536 / bradley.cassani@precisionmeters.co.za |
| **Garth Le Roux** | Sales Representative | 072 469 8133 / 021 510 4266 |

**Precision Meters Office:** Unit 4B, Panther Park, 11 Berkley Road, Maitland 7405

### Contractor (Precept Systems)

| Person | Role | Contact |
|--------|------|---------|
| **Jason van Wyk** | Developer / Systems Integrator | - |

---

## About Mosaic Group

**Website:** https://mosaicgroup.co.za/
**Location:** 2 Heleza Boulevard, Sibaya Drive, eMdloti, Durban

**Business:** Real estate development and brand incubation focused on urban regeneration. Portfolio includes:
- **Olé** - Student housing (2,000+ students)
- **City Life** - Residential units (2,500+ units, 7,000+ tenants)
- **Luna** - Furnished apartment complexes
- **Oasis** - Co-working spaces
- **Pure Fitness** - Fitness facilities (2,000m²)
- **Adapt Media** - Out-of-home advertising

**Water Monitoring Relevance:** Extensive property portfolio with significant water consumption across residential, commercial, and amenity spaces. Utility monitoring critical for operational efficiency and cost management.

### Target Site: 477 Anton Lembede Street (City Life)

| Item | Details |
|------|---------|
| Building | City Life |
| Total floors | 27 |
| Floors in scope | 12 (first 12 floors — upper floors are a different client group) |
| Units per floor | 48 |
| Total units in scope | 576 |
| Building age | ~60 years |
| Municipal supply | 110mm pipe, enters at top of building |
| Main riser | 75mm (single riser, gravity-fed top to bottom) |
| Floor branch | 50mm (branches at each floor) |
| Unit feed | 16mm (to each unit, entry above front door in passage) |
| Construction | Reinforced concrete, brick, and steel (no basements) |
| Current water bill | R1.2M/month (entire building) |
| Current tenant charge | R400/month flat rate |
| ERP system | CLTM (tenant management — integration required) |

---

## Technical Approach (Based on Fairfield)

### Likely Solution Architecture

```
┌─────────────────────┐         ┌─────────────────────┐
│   Smart Meters      │         │   Monitoring        │
│   (Precision Meters)│  ──────▶│   Platform          │
│                     │ IoT/LoRa│                     │
│  - Pulse output     │         │  - Real-time data   │
│  - Flow monitoring  │         │  - Usage analytics  │
│  - Leak detection   │         │  - Billing calcs    │
└─────────────────────┘         └─────────────────────┘
```

### Technology Stack (Fairfield Reference)

| Component | Technology |
|-----------|------------|
| Backend | Node.js + Express |
| Database | SQLite |
| Frontend | SPA (vanilla JS) |
| IoT | LoRa (RAK3172) / Ethernet hybrid |
| Meters | Precision Meters smart meters |
| Hosting | LXC on Proxmox, Cloudflare Tunnel |

---

## Document Structure

```
docs/planning/
├── client/           # Client-facing (proposals, scope of work)
├── internal/         # Technical docs (numbered 00-NN)
│   ├── Research/     # Technical research and analysis
│   └── Vendors/      # Supplier spec sheets and datasheets
├── correspondence/   # Meeting notes (dated YYYY-MM-DD)
└── site-visits/      # Field visit reports
pics/                 # Site visit photographs
```

### Naming Conventions
- **Numbered docs:** `NN-description.md`
- **Dated docs:** `YYYY-MM-DD-description.md`
- **Reference code:** `MGW-COR-YYYYMM-NNN` (correspondence)

---

## Key Decisions Made

| Decision | Choice | Rationale |
|----------|--------|-----------|
| Meter supplier | Precision Meters | Referral relationship, condition of opportunity |
| Solution approach | Custom (like Fairfield) | Flexibility, proven architecture |
| IoT protocol | LoRaWAN (private) | High-rise concrete penetration, no subscriptions |
| Network server | ChirpStack | Open-source, self-hosted, full API access |

---

## Milestones

| Event | Date | Status |
|-------|------|--------|
| Initial contact with Tanya | 2026-01-28 | Done |
| Site visit at 477 Anton Lembede | 2026-01-29 | Done |
| Site visit notes formalized | 2026-01-30 | Done |
| Internal research (CLTM ERP, hosting, etc.) | TBD | Next |
| Site assessment report | TBD | Upcoming |
| Business case and proposal | TBD | Upcoming |
| Submit proposal to Tanya | TBD | Upcoming |

---

## When Working on This Project

1. **Always read RESUME.md first** - It has the latest context
2. **Reference Fairfield project** for technical approach
3. **Update STATUS.md** when completing milestones
4. **Use numbered prefixes** for new internal documents
5. **Date correspondence files** with YYYY-MM-DD prefix
6. **Keep client/ and internal/ separate** - Different audiences
7. **Commit frequently** with descriptive messages

---

## Billing Reference

| Item | Rate |
|------|------|
| Jason consultation | R550/hr |
| Travel (SARS 2026) | R4.76/km |

---

## Demo Platform

**URL:** https://meter-tracker.com
**Container:** LXC 120 (water-monitor) on Proxmox `pve` (10.0.1.11)
**App Path:** `/opt/water-monitor/`
**Database:** `/opt/water-monitor/water_monitor.db`

### Registered Users (as of 2026-01-28)

| ID | Username | Email | Created | Last Login |
|----|----------|-------|---------|------------|
| 1 | jason | jason@precept.co.za | 2025-12-17 | 2026-01-24 |
| 2 | Quin1983 | *(none)* | 2025-12-17 | *(never)* |

**Note:** Tanya Dowley invited to create account (2026-01-28) - not yet registered.

### Access Commands (via Proxmox)

```bash
ssh root@10.0.1.11
pct exec 120 -- cat /opt/water-monitor/.env
pct pull 120 /opt/water-monitor/water_monitor.db /tmp/water_monitor.db
sqlite3 /tmp/water_monitor.db "SELECT * FROM users;"
```

---

## Related Projects

- **Fairfield Water** (`/home/jason/Projects/fairfield-water`) - Sister project, reference implementation
- **Spray & Scouting App** - Another Precept Systems project
- **Crop Monitoring Camera** - Another Precept Systems project
