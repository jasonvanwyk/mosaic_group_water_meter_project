# Mosaic Group Water Meter Monitoring

A custom water meter telemetry and monitoring solution for Mosaic Group, a real estate development company with extensive residential and commercial properties in Durban, South Africa.

## Overview

This project provides automated water consumption tracking across Mosaic Group's property portfolio using smart meters from Precision Meters, IoT telemetry, and a web-based monitoring platform.

**Key Features:**
- Real-time water consumption monitoring
- Multi-property dashboard
- Usage analytics and reporting
- Leak detection and alerts
- Tiered billing calculations
- Historical data and trends

## Client

**Mosaic Group** - https://mosaicgroup.co.za/

Real estate development and brand incubation company focused on urban regeneration. Portfolio includes:
- **Olé** - Student housing (2,000+ students)
- **City Life** - Residential units (2,500+ units, 7,000+ tenants)
- **Luna** - Furnished apartment complexes
- **Oasis** - Co-working spaces
- **Pure Fitness** - Fitness facilities
- **Adapt Media** - Out-of-home advertising

**Location:** 2 Heleza Boulevard, Sibaya Drive, eMdloti, Durban

## Solution Architecture

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

## Technology Stack

| Component | Technology |
|-----------|------------|
| Backend | Node.js + Express |
| Database | SQLite |
| Frontend | SPA (vanilla JS) |
| IoT | LoRa / Ethernet hybrid |
| Meters | Precision Meters smart meters |
| Hosting | LXC on Proxmox, Cloudflare Tunnel |

## Project Status

**Phase:** Initial Contact / Pre-Proposal
**Last Updated:** 2026-01-28

| File | Purpose |
|------|---------|
| [STATUS.md](STATUS.md) | Current status and tracking |
| [RESUME.md](RESUME.md) | Session context for continuity |
| [DIRECTORY.md](DIRECTORY.md) | Project structure index |
| [CLAUDE.md](CLAUDE.md) | AI assistant guidance |

## Documentation

### Project Structure

```
docs/planning/
├── client/              # Client-facing deliverables
├── internal/            # Technical documentation (numbered)
├── correspondence/      # Meeting notes and communications
└── site-visits/         # Field visit reports
```

## Related Projects

This solution is based on the **Fairfield Water** project - a similar water meter monitoring system deployed for Fairfield Dairy.

## Team

| Role | Person/Company | Responsibility |
|------|----------------|----------------|
| Client | Mosaic Group (Tanya Dowley) | Property portfolio, requirements |
| Contractor | Precept Systems (Jason van Wyk) | Platform, telemetry, integration |
| Supplier | Precision Meters (Bradley Cassani, Garth Le Roux) | Smart water meters |

## License

Private / Proprietary

---

*Project started: January 2026*
