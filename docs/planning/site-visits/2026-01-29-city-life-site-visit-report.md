# Site Visit Report — City Life, 477 Anton Lembede Street

**Project:** Mosaic Group Water Meter Monitoring (MGW)
**Date:** Wednesday, 29 January 2026
**Time:** 12:00 PM — 13:45 PM
**Location:** 477 Anton Lembede Street, Durban CBD
**Author:** Jason van Wyk, Precept Systems

---

## Attendees

| Name | Role | Company |
|------|------|---------|
| Jason van Wyk | Developer / Systems Integrator | Precept Systems |
| Tanya Dowley | Part Owner / Property Manager | Mosaic Group |
| Ryan | In-house Plumber | Mosaic Group |
| Sumir | In-house IT Administrator / Technician | Mosaic Group |

---

## 1. Executive Summary

Precept Systems conducted a site visit at City Life (477 Anton Lembede Street, Durban CBD), a 27-storey residential high-rise managed by Mosaic Group. The visit focused on the first 12 floors, which fall under Mosaic Group's management and are the scope for a proposed water metering and monitoring solution. The upper floors serve a different set of clients and are excluded.

The building has 48 units per floor, totalling 576 units across the 12 floors in scope. The client requires individual unit metering, bulk floor metering for leak detection, and integration with their existing CLTM ERP tenant management system. Mosaic Group currently charges tenants a flat R400/month for water with no usage visibility — the building's total municipal water bill is approximately R1.2M/month.

Tanya Dowley confirmed that budget has been allocated and that she will be reviewing pricing in the first week of February 2026. A phased rollout is preferred, beginning with a pilot of one floor and five units.

---

## 2. Building Profile

| Item | Details |
|------|---------|
| Building name | City Life (Delta Towers) |
| Address | 477 Anton Lembede Street, Durban CBD |
| Total floors | 27 |
| Floors in scope | 12 (first 12 floors only) |
| Units per floor | 48 |
| Total units in scope | 576 |
| Building age | Approximately 60 years |
| Construction | Reinforced concrete, brick, and steel (no basements) |
| Property manager on-site | Tanya Dowley |
| Upper floors | Excluded — different client group |

---

## 3. Water Infrastructure

### 3.1 Municipal Supply

| Item | Details |
|------|---------|
| Municipal mainline | 110mm pipe |
| Municipal meter | Ground level, on pavement outside building |
| Municipal reading frequency | Monthly (by municipality) |
| Building reading frequency | Fortnightly (by Barry, building staff) |
| Supply entry point | Top of building |
| Feed direction | Gravity-fed from top down the riser |
| Pumping | Lower floors are pump-assisted from the riser |

### 3.2 Building Reticulation

| Section | Pipe Size | Notes |
|---------|-----------|-------|
| Main riser | 75mm | Single riser through entire building, top to bottom |
| Floor branch | 50mm | Branches off riser at each floor |
| Unit feed | 16mm | From floor branch to each unit |

The reticulation system is **not a ring feed**. There is only one riser, which is accessible. Water enters each apartment above the front door in the passage. Pipes and cables run along the passage ceiling, with cables in a cable basket. Each unit has an existing shut-off (isolation) valve in the passage, and there is space for a meter at entry point. Water can be shut off per riser on each floor.

### 3.3 Current Water Operations

| Item | Details |
|------|---------|
| Tenant billing model | Flat rate of R400/month added to rent |
| Monthly municipal water bill | R1.2M/month (entire building, all floors) |
| Municipal water tariff | R65.91/kL |
| Monthly water usage | 10,500 kL/month (entire building, all floors) |
| Leak detection method | Visual observation or tenant reporting only |
| Common leak points | Angle joints |
| Leak frequency | Seldom, but hidden leaks (toilets, taps) cannot be detected |
| Estimated water loss | Not easily estimated due to lack of measurement |

---

## 4. Problems Identified

The following problems were identified during the visit, driving the need for a metering and monitoring solution:

| # | Problem | Impact |
|---|---------|--------|
| 1 | **No usage visibility** — No measurement of individual tenant water consumption. Cannot measure what cannot be seen. | Unable to identify waste, allocate costs fairly, or detect hidden issues. |
| 2 | **No tenant incentive** — Flat R400/month charge regardless of usage. No motivation for tenants to conserve water or report leaks. | Excessive consumption and unreported leaks. |
| 3 | **No leak detection** — Leaks detected only visually or when reported. No automated monitoring. Hidden leaks (running toilets, dripping taps) inside apartments go undetected. | Ongoing water loss and damage accumulation. |
| 4 | **Manual readings impractical** — Plain meters without telemetry would require manual reading of 576 meters. Labour-intensive, prone to human error, unreliable (dependent on staff attendance). | Operational overhead and data quality issues. |
| 5 | **Building bears full cost** — The building pays the municipal bill regardless of what tenants pay or use. No cost recovery mechanism tied to actual consumption. | Financial exposure for Mosaic Group. |
| 6 | **Water restrictions unenforceable** — During municipal mandatory water restrictions, there is no mechanism to enforce tenant compliance. Building gets penalised for tenant non-compliance. | Regulatory risk and financial penalties. |
| 7 | **System silos** — Water metering data would need to be manually captured into the invoicing/ERP system. Creates friction, delays, disputes, and manual labour. | Administrative overhead and tenant disputes. |

---

## 5. Client Requirements

### 5.1 Success Criteria

Tanya defined success as:
- Control over the amount of water each tenant uses
- Leak detection, control, and prevention
- Minimised friction between the water monitoring system and the invoicing/ERP system

### 5.2 Feature Priority Matrix

| Feature | Priority | Notes |
|---------|----------|-------|
| Real-time monitoring | High | |
| Leak detection alerts | High | |
| Per-unit billing | High | |
| Historical reports | High | |
| Billing/ERP integration | High | Integration with CLTM ERP system |
| Automated meter readings | High | Eliminates manual reading |
| Tamper detection | High | |
| Tenant portal | Medium | WhatsApp/email notifications could serve this purpose |
| Pressure monitoring | Low | |
| Mobile app | Low | |

### 5.3 Explicit Exclusions and Rejections

| Item | Status | Reason |
|------|--------|--------|
| **Fire alarm and sprinkler system** | **Out of scope** | Must be completely separated and isolated from any proposed water metering solution. This system is purely for tenant water usage metering, monitoring, and control. |
| **Prepaid meters** | **Rejected** | Either too expensive or leads to unwanted consequences — tenants may stop using water to keep apartments clean, and building management would have no visibility into this behaviour. |

### 5.4 Motorised Ball Valves

Motorised ball valves for remote water shutoff and throttling were discussed. These would allow:
- Throttling water if a tenant does not pay for water or rent
- Remote shutoff per unit or per floor in case of emergency leaks

Tanya indicated this is **not required for phase 1** but may be wanted if costing is acceptable. Note: motorised ball valves require a power source (possibly 12V DC). PCB boards should be designed with connectors to support motorised ball valves in future, even if not deployed initially.

---

## 6. IT Infrastructure

### 6.1 Network

| Item | Details |
|------|---------|
| Network ecosystem | Ubiquiti / UniFi |
| Access points per floor | 13–15 |
| AP frequencies | 2.4 GHz and 5 GHz |
| Internet | 1 Gb fibre from Vox |
| Static IP | Available (not yet provided to Precept) |
| Internal cabling | Cat6 solid copper throughout entire building |
| Switches | Ubiquiti UniFi managed switches per floor |
| Gateway | Ubiquiti UniFi Secure Gateway |
| VLANs | Configured — Sumir can allocate a dedicated VLAN for the water monitoring system |
| Tenant WiFi | Available to all tenants (password-based) |

### 6.2 Comms Room

| Item | Details |
|------|---------|
| Location | Not a dedicated server room — office space where switch sits on shelf |
| Power | Available |
| Ethernet | Available |
| UPS | Available (covers comms room and all managed switches) |
| Security | Secure (restricted access) |
| Rack space | Available for server/gateway |

### 6.3 Rooftop

| Item | Details |
|------|---------|
| Access | Available |
| Power | Available |
| Ethernet | Available |
| Security | Secure |
| Suitability | Suitable for LoRa antenna placement |

### 6.4 IoT/LoRa Feasibility

The building construction is a mixture of concrete, brick, and steel. There are no basements. Rooftop access is available for antenna placement. These factors are relevant to LoRa signal propagation assessment (to be evaluated during solution design).

---

## 7. Hosting and Integration

### 7.1 Hosting Preference

| Option | Preference |
|--------|------------|
| Cloud (AWS, Google Cloud, Azure, etc.) | 51% — slight preference |
| On-premises | 49% — near-equal consideration |

A hybrid approach may be appropriate. To be determined during solution design.

### 7.2 ERP Integration

| Item | Details |
|------|---------|
| System | CLTM ERP (tenant management system) |
| Integration requirement | High priority — automated data flow from water monitoring to billing |
| Research needed | System provider, API capabilities, documentation, contact person, API requirements |

### 7.3 Dashboard Requirements

| Item | Details |
|------|---------|
| Type | Central, non-tenant-facing dashboard |
| Access control | RBAC (role-based access control) for admin and users |
| Tenant access | Not via dashboard — WhatsApp/email notifications considered |

---

## 8. Installation Constraints

| Constraint | Details |
|------------|---------|
| Tenant disruption | Must be minimised — least interruption to tenants |
| Labour costs | Must be minimised — least labour installation costs |
| Infrastructure setup | Ethernet cabling is too expensive — not an option |
| Tenant notification | Mosaic will inform tenants of water supply disruption during installation |
| Estimated install time | 1 hour per unit |
| Preferred shutoff window | 10:00 AM — 14:00 PM (tenants typically out of building) |
| Water shutoff scope | Per riser on each floor |

### Installation Roles

| Task | Responsible Party |
|------|------------------|
| Plumbing / meter installation | Ryan (Mosaic Group in-house plumber) |
| System build, configuration, nodes, gateway | Jason van Wyk (Precept Systems) |
| Node installation assistance, learning | Sumir (Mosaic Group IT) |
| Meter supply | Precision Meters (obligatory supplier) |
| Motorised ball valves (if needed) | Jason van Wyk (Precept Systems) |
| All other technology (nodes, gateways, server, DB) | Jason van Wyk (Precept Systems) |

---

## 9. Decision Process and Timeline

### 9.1 Decision Process

| Item | Details |
|------|---------|
| Final decision maker | Tanya Dowley |
| Approval required from | Board of directors |
| Procurement process | Through Tanya |

### 9.2 Budget

| Item | Details |
|------|---------|
| Budget allocated | Yes |
| Amount | Not disclosed |
| Payment preference | Phased payments |
| Price sensitivity | High — Tanya is very price-sensitive |

### 9.3 Timeline

| Item | Details |
|------|---------|
| Pricing review | First week of February 2026 |
| Target start | Q1 2026 |
| Hard deadline | None |

---

## 10. Phased Rollout

Tanya is keen on a phased rollout. Jason concurs — this minimises risk to all parties.

| Phase | Scope | Notes |
|-------|-------|-------|
| Pilot | 1 floor + 5 units | Validates solution before wider deployment |
| Subsequent phases | Remaining floors and units | Batch size and cadence to be determined |
| Motorised ball valves | Deferred | Not phase 1, but design should accommodate future addition |

Jason explained that the system is highly modular and customisable. Any combination of architecture, customised dashboards, phased rollout options, and multi-architecture deployment is possible.

The proposed system topology comprises:
- Water meters from Precision Meters (obligatory)
- Nodes from Precept Systems
- Gateway from Precept Systems
- Server and database from Precept Systems
- Motorised ball valves from Precept Systems (if needed — PCBs must be designed valve-ready)

Exact architecture, technology stack, and communications protocol to be confirmed after proper site and area assessment.

---

## 11. Additional Notes

- At this stage, only this building (City Life, 477 Anton Lembede) requires the solution from Mosaic Group.
- All correspondence to Mosaic Group must be directed to Tanya Dowley only at this stage.
- Tanya will email Jason the contact details for Ryan and Sumir (see email correspondence).
- The cost of water triggered the need for this solution.
- Precision Meters has no KZN presence — Jason is positioned as the local field contact.

---

## 12. Photographic Evidence

| Photo | Description | Path |
|-------|-------------|------|
| City Life riser and floor main pipe/valve | Main riser and floor branch piping infrastructure | `pics/city-life-29012026.jpeg` |

---

## 13. Action Items

| # | Action | Owner | Status |
|---|--------|-------|--------|
| 1 | Research CLTM ERP system — provider, API capabilities, documentation, contacts | Jason | Pending |
| 2 | Request static IP details from Sumir | Jason | Pending |
| 3 | Request Ryan and Sumir contact details from Tanya | Jason | Pending |
| 4 | Research hosting options (cloud providers, hybrid approaches) | Jason | Pending |
| 5 | Research motorised ball valve power requirements (12V DC, PoE feasibility) | Jason | Pending |
| 6 | Coordinate with Precision Meters on meter specifications and pricing for 576 units + 12 bulk meters | Jason | Pending |
| 7 | Complete site assessment report | Jason | Pending |
| 8 | Develop technical proposal with phased approach and business case | Jason | Pending |
| 9 | Submit proposal to Tanya (before pricing review, 1st week Feb) | Jason | Pending |
