# Business Case

**Project:** Mosaic Group Water Meter Monitoring — City Life, 477 Anton Lembede Street
**Reference:** MGW-PRO-202602-002
**Version:** 1.1
**Date:** 2026-02-05
**Author:** Jason van Wyk, Precept Systems

---

## Revision History

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2026-02-05 | Initial draft |
| 1.1 | 2026-02-05 | Restructured investment breakdown; added node hardware, deployment costs, operating manual; updated hourly rate; added system resilience and IP terms; sewer tariff clarification |

---

## Related Documents

| Ref | Document | Description |
|-----|----------|-------------|
| MGW-PRO-202602-001 | Site Assessment Report | Site visit findings and building profile |
| MGW-PRO-202602-003 | Scope of Work | Detailed deliverables (TBD) |
| MGW-PRO-202602-004 | Proposal | Commercial terms and pricing (TBD) |

---

## Important Notice — Assumptions

This business case is based on a number of assumptions documented in the [Assumptions](#assumptions) section at the end of this document. These include estimates for water consumption proportionality, conservation impact, municipal bill composition, and occupancy. All financial projections should be treated as indicative and will be refined as confirmed data becomes available. Key assumptions that should be verified before proceeding are flagged throughout the document.

---

## 1. Executive Summary

**What gets measured gets managed.**

Mosaic Group's City Life building at 477 Anton Lembede Street currently spends approximately **R1.2 million per month** on water for 27 floors. The first 12 floors under Mosaic Group's management account for an estimated **R533,000/month** of that bill — yet tenant water charges recover only **R230,400/month** (43% cost recovery). That leaves a shortfall of approximately **R302,600 every month** — money that Mosaic Group absorbs with no visibility into where the water is going.

This business case proposes installing smart water meters on all 576 units and 12 floor bulk meters across the first 12 floors, with a monitoring platform that provides real-time visibility, automated billing data, and leak detection.

**Three pillars drive the value:**

1. **Conservation through awareness** — When tenants know their usage is measured, consumption typically drops 10–25%. At 15%, that saves an estimated ~R80,000/month.
2. **Fair billing and cost recovery** — Usage-based billing replaces the flat R400/month charge, lifting revenue from R230K to R373K–R627K/month depending on the rate applied.
3. **Leak detection** — Bulk-vs-unit meter reconciliation per floor identifies leaks immediately. A single running toilet costs ~R4,900/month. A concealed pipe leak costs ~R14,800/month.

**Investment overview:**

| Component | Est. Cost (ex VAT) | Procured From |
|-----------|---------------------|---------------|
| Smart meters (588 units) | R1,321,920 | Precision Meters (direct to Mosaic) |
| IoT telemetry hardware (588 nodes) | R235,000–R325,000 | Precept Systems |
| Platform development and integration | R70,200–R105,600 | Precept Systems |
| Deployment, installation and training | R31,800–R48,000 | Precept Systems |
| Infrastructure (gateways, server, NAS) | R47,000–R65,000 | Precept Systems |
| **Total capital** | **~R1,706,000–R1,866,000** | |

**The system pays for itself within 6–10 months** under conservative assumptions. Every month without metering is another R302,600 in unrecovered water costs.

---

## 2. Current State Analysis

### 2.1 Water Costs

| Metric | Value | Source |
|--------|-------|--------|
| Total building water bill | R1,200,000/month | Tanya Dowley (site visit, 29 Jan) |
| Municipal water tariff | R65.91/kL | Mosaic Group records |
| Reported total consumption | ~10,500 kL/month | Mosaic Group records (all 27 floors) |
| Effective all-in rate | ~R114/kL | R1,200,000 ÷ 10,500 kL |

**Note on bill composition:** The municipal water tariff is R65.91/kL, yet the effective rate from the total bill is ~R114/kL. The difference (~R48/kL) likely includes sewage/sanitation surcharges and/or fixed municipal charges. **Whether the sewage/sanitation component is charged on a per-kL basis or as a fixed charge needs to be confirmed** with eThekwini Municipality or Mosaic Group's accounts department. If sewage is consumption-based, then conservation savings apply to the full ~R114/kL effective rate. If a significant portion of the bill is fixed charges, per-kL savings would be closer to R65.91/kL. Financial calculations in this document use the effective all-in rate (~R114/kL) as the base case, with sensitivity shown at the water tariff only (R65.91/kL) where applicable.

### 2.2 Twelve-Floor Estimate

The first 12 of 27 floors represent 44.4% of the building's occupied area. Assuming proportional water consumption:

| Metric | Calculation | Value |
|--------|-------------|-------|
| Share of building | 12 ÷ 27 | 44.4% |
| Estimated consumption (12 floors) | 10,500 × 44.4% | ~4,667 kL/month |
| Estimated water cost (12 floors) | R1,200,000 × 44.4% | ~R533,000/month |
| Annual water cost (12 floors) | R533,000 × 12 | ~R6,400,000/year |

### 2.3 Revenue Model

| Metric | Value |
|--------|-------|
| Total tenants (12 floors) | 576 |
| Flat monthly water charge | R400/tenant |
| Monthly tenant revenue | R230,400 |
| Monthly water cost (12 floors) | ~R533,000 |
| **Monthly shortfall** | **~R302,600** |
| **Annual shortfall** | **~R3,631,200** |
| **Cost recovery rate** | **43%** |

### 2.4 Per-Tenant Economics

| Metric | Value |
|--------|-------|
| Average consumption per tenant | ~8.1 kL/month (4,667 ÷ 576) |
| Average cost per tenant (all-in) | ~R923/month (8.1 × R114) |
| Amount charged to tenant | R400/month |
| **Subsidy per tenant** | **~R523/month** |

### 2.5 Pain Points

| Problem | Impact | Business Risk |
|---------|--------|---------------|
| **No usage visibility** | Cannot identify high consumers, waste, or anomalies | Operating blind on a R6.4M/year expense |
| **No tenant incentive** | Flat R400 charge regardless of usage | No motivation to conserve or report leaks |
| **No leak detection** | Leaks detected visually or when reported by tenants | Hidden leaks (running toilets, dripping taps) accumulate undetected |
| **Manual reading impractical** | 576 meters would require daily manual reading — labour-intensive, error-prone, dependent on staff attendance | Not a viable alternative to automated monitoring |
| **Cost gap widening** | Municipal tariffs increase annually; R400 flat charge has not kept pace | Shortfall grows every year |
| **No data for disputes** | No evidence to resolve tenant billing queries | Administrative friction and tenant dissatisfaction |
| **ERP disconnect** | Water data must be manually entered into CLTM tenant management system | Administrative overhead, delays, errors |

---

## 3. Proposed Solution Benefits

### 3.1 Pillar 1 — Conservation Through Awareness

Published residential studies consistently show that metering and feedback reduce water consumption by **10–25%**, with a commonly cited median of **15%**. The mechanism is simple: when tenants can see their usage (or know it is being measured), behaviour changes.

| Conservation Scenario | Reduction | kL Saved/Month | Monthly Saving (at R114/kL) | Annual Saving |
|-----------------------|-----------|----------------|-----------------------------|---------------|
| Conservative | 10% | 467 kL | R53,200 | R638,700 |
| Base case | 15% | 700 kL | R79,800 | R958,000 |
| Optimistic | 25% | 1,167 kL | R133,000 | R1,596,000 |

*Savings calculated at the effective all-in rate of ~R114/kL. If the sewage component of the municipal bill is not consumption-based (see Section 2.1), conservation savings at the water tariff only (R65.91/kL) would be approximately 58% of the figures above — e.g., base case ~R554,000/year instead of ~R958,000/year.*

### 3.2 Pillar 2 — Fair Billing and Cost Recovery

Replacing the flat R400/month with usage-based billing shifts cost recovery dramatically. The table below shows monthly revenue at different per-kL rates applied to the estimated 4,667 kL consumed across 12 floors:

| Billing Rate | Revenue/Month | vs Current R230K | Gap to R533K Cost | Cost Recovery |
|--------------|---------------|-------------------|---------------------|---------------|
| R400 flat (current) | R230,400 | — | -R302,600 | 43% |
| R80/kL | R373,360 | +R143,000 | -R159,640 | 70% |
| R100/kL | R466,700 | +R236,300 | -R66,300 | 88% |
| R114/kL (at cost) | R532,138 | +R301,700 | -R862 | ~100% |
| R130/kL | R606,710 | +R376,300 | +R73,710 | 114% |

**Key observation:** Even at the conservative R80/kL rate, monthly revenue increases by R143,000 — nearly R1.72M/year in additional cost recovery. At R114/kL (passing on the actual cost), the shortfall is eliminated entirely.

The appropriate billing rate is a business decision for Mosaic Group. The monitoring system provides the consumption data needed to implement any rate structure.

### 3.3 Pillar 3 — Leak Detection

Installing bulk meters on each floor's 50mm branch pipe enables **bulk-vs-unit reconciliation**: the floor bulk meter reading should approximately equal the sum of the 48 unit meter readings on that floor. Any significant discrepancy indicates water loss between the branch and the units — a leak.

| Leak Type | Typical Flow | Monthly kL | Monthly Cost (at R114/kL) | Detection Method |
|-----------|-------------|------------|---------------------------|------------------|
| Running toilet (1 unit) | 1 L/min | ~43 kL | ~R4,900 | Unit meter shows continuous flow with no occupancy pattern |
| Dripping tap | 0.25 L/min | ~11 kL | ~R1,250 | Unit meter shows baseline consumption above normal |
| Pipe leak (concealed) | 3 L/min | ~130 kL | ~R14,820 | Bulk meter exceeds sum of unit meters on that floor |
| Major pipe burst | 10+ L/min | ~432 kL | ~R49,250 | Bulk meter alarm; immediate alert |

**Without metering, these leaks can run undetected for months.** A single running toilet across the 576 units costs ~R4,900/month. If 5% of units have an undetected running toilet (29 units), that alone costs ~R142,000/month — nearly half the current shortfall.

### 3.4 Operational Benefits

| Benefit | Description |
|---------|-------------|
| **Automated meter reading** | Eliminates manual reading of 576 meters — readings delivered automatically every 5–15 minutes |
| **CLTM ERP integration** | Consumption data fed directly into the tenant management system via API — no manual data entry |
| **Management reporting** | Automated daily, weekly, monthly consumption reports per floor, per unit |
| **Anomaly detection** | Alerts for unusual consumption patterns, zero-flow (vacant units still consuming), continuous flow (leaks) |
| **Dispute resolution** | Timestamped, tamper-evident consumption data for tenant queries |
| **Municipal bill verification** | Compare building total (sum of floors) against municipal meter reading |
| **Foundation for future phases** | Upper 15 floors, motorised valves for remote shutoff/throttling, other Mosaic properties |

### 3.5 Motorised Ball Valves

This solution **does not include motorised ball valves** for remote water shutoff or throttling. However, all system hardware is designed to be **valve-ready** — the IoT node circuit board includes connectors and driver circuitry for future motorised valve integration. If Mosaic Group wishes to add remote shutoff capability in a future phase, the valves and control hardware can be added to the existing installed system without replacing any components. Motorised ball valves will be quoted separately if and when requested.

---

## 4. Investment Required

### 4.1 Smart Meters (Precision Meters — Direct to Mosaic Group)

Precision Meters will invoice Mosaic Group directly for all meters. This avoids unnecessary markup and allows Mosaic Group to claim VAT on the meter purchase.

| Item | Qty | Unit Price (ex VAT) | Total (ex VAT) |
|------|-----|---------------------|----------------|
| 15mm ultrasonic meter (LoRaWAN) — unit meters | 576 | R2,100 | R1,209,600 |
| 50mm ultrasonic meter (LoRaWAN) — bulk floor meters | 12 | R9,360 | R112,320 |
| **Total meters** | **588** | | **R1,321,920** |

*Pricing as quoted by Garth Le Roux, Precision Meters, 3 February 2026 (576 × R2,100 = R1,209,600 + 12 × R9,360 = R112,320). Volume discount has been requested and is pending revision by Precision Meters management. Final meter cost may be lower.*

### 4.2 IoT Telemetry Hardware — Nodes (Precept Systems)

Each meter requires a custom-built IoT telemetry node that reads the meter's pulse output and transmits consumption data wirelessly to the monitoring platform. Nodes are designed and assembled by Precept Systems. Hardware costs are separate from development and installation services.

| Item | Qty | Est. Unit Cost (ex VAT) | Est. Total (ex VAT) |
|------|-----|-------------------------|---------------------|
| Precept IoT node (LoRaWAN, battery-powered, valve-ready) | 588 | R400–R550 | R235,200–R323,400 |

*Estimated cost per node includes LoRa radio module, custom PCB, battery, enclosure, antenna, and assembly/testing. Firm pricing in the formal Proposal.*

### 4.3 Platform Development and Integration (Precept Systems)

Design, development, and integration services. This covers software and firmware — not hardware or on-site installation.

| Activity | Estimated Hours | Est. Cost (ex VAT) |
|----------|-----------------|---------------------|
| Platform adaptation and configuration | 40–60 hrs | R24,000–R36,000 |
| Custom node firmware development | 20–30 hrs | R12,000–R18,000 |
| ChirpStack codec and device profile | 10–15 hrs | R6,000–R9,000 |
| CLTM ERP integration (API development, Postman collection) | 25–40 hrs | R15,000–R24,000 |
| Operating manual (system operations, troubleshooting, backup procedures) | 10–15 hrs | R6,000–R9,000 |
| Project management | 12–16 hrs | R7,200–R9,600 |
| **Total platform development** | **117–176 hrs** | **R70,200–R105,600** |

*Hourly rate: R600/hr (ex VAT). Precept Systems is not VAT registered. Firm pricing in the formal Proposal.*

### 4.4 Deployment, Installation and Training (Precept Systems)

On-site services for physical installation, commissioning, and knowledge transfer.

| Activity | Estimated Hours | Est. Cost (ex VAT) |
|----------|-----------------|---------------------|
| Gateway configuration and network deployment | 15–20 hrs | R9,000–R12,000 |
| On-site installation support and commissioning | 30–50 hrs | R18,000–R30,000 |
| Training (Sumir, building management) | 8–10 hrs | R4,800–R6,000 |
| **Total deployment and training** | **53–80 hrs** | **R31,800–R48,000** |

*Note: Plumbing work (physical meter installation) is performed by Ryan (Mosaic Group in-house plumber). Precept provides the IoT nodes, gateway configuration, and system commissioning. Each unit has an existing isolation valve — water shutoff during installation is per unit only, so neighbouring tenants on the same floor are not disrupted.*

### 4.5 Infrastructure (Procured by Precept, Invoiced to Mosaic)

| Item | Est. Cost (ex VAT) |
|------|---------------------|
| LoRaWAN gateways (4 units, indoor, PoE) | R28,000–R36,000 |
| Server (production-grade, on-premises) | R12,000–R18,000 |
| NAS backup device (2-bay, mirrored drives) | R7,000–R11,000 |
| **Total infrastructure** | **R47,000–R65,000** |

*Note: If cloud hosting is preferred, server and NAS hardware costs are replaced by a monthly hosting fee (see Section 4.7). The gateways are always on-premises regardless of hosting choice.*

### 4.6 System Resilience and Backup

The system is designed with no single point of failure for data. The backup strategy follows a three-tier approach:

| Tier | Method | Purpose |
|------|--------|---------|
| **Tier 1 — Local** | Automated daily backup to on-site NAS (mirrored drives) | Fast recovery from software failure or corruption |
| **Tier 2 — Cloud** | Automated daily off-site backup to SA-hosted cloud storage | Recovery from hardware failure, theft, or physical damage |
| **Tier 3 — Meter storage** | Smart meters store readings internally | No consumption data is lost during any system outage — readings sync when restored |

**Disaster recovery:** The entire platform runs in containerised deployment (Docker). If the primary server fails, the system can be rebuilt from backup on replacement hardware or migrated to cloud hosting. Target recovery: system operational within 4–8 hours of hardware replacement.

**Recovery options in the event of server failure:**

| Option | Approach | Cost Implication |
|--------|----------|-----------------|
| Replacement server | Source equivalent hardware, restore from NAS or cloud backup | R12,000–R18,000 + labour |
| Temporary cloud migration | Deploy to SA-hosted cloud VPS from backup, redirect gateways | Hosting fee until on-prem restored |
| Pre-configured spare server | Maintain a cold-standby server on-site, ready for immediate swap | ~R12,000–R18,000 additional CAPEX |

**Responsibilities:** Mosaic Group is responsible for ensuring the NAS and server remain powered and operational, and that automated backups are completing successfully (dashboard indicators provided). Precept Systems provides the backup configuration, monitoring, and recovery support.

### 4.7 Operating Costs (Monthly)

| Item | Est. Monthly Cost (ex VAT) |
|------|----------------------------|
| Platform monitoring (remote health checks, alerts, basic troubleshooting) | R500 |
| Cloud backup storage (off-site, SA-hosted) | R100–R200 |
| Maintenance reserve | R400–R800 |
| **Total monthly operating (on-premises hosting)** | **R1,000–R1,500** |
| **Annual equivalent** | **R12,000–R18,000** |

If cloud hosting is preferred instead of on-premises:

| Item | Est. Monthly Cost (ex VAT) |
|------|----------------------------|
| SA-hosted cloud VPS (production spec) | R800–R1,500 |
| Platform monitoring | R500 |
| Cloud backup storage | R100–R200 |
| Maintenance reserve | R400–R800 |
| **Total monthly operating (cloud-hosted)** | **R1,800–R3,000** |
| **Annual equivalent** | **R21,600–R36,000** |

*Cloud hosting costs are indicative. Exact pricing depends on provider selection (e.g., xneelo, Afrihost, AWS af-south-1) and will be confirmed in the Scope of Work. SA-based hosting is required for POPIA compliance.*

### 4.8 Investment Summary

| Category | Est. Amount (ex VAT) | Procured From |
|----------|----------------------|---------------|
| Smart meters (588) | R1,321,920 | Precision Meters → Mosaic Group |
| IoT telemetry nodes (588) | R235,000–R325,000 | Precept Systems → Mosaic Group |
| Platform development and integration | R70,200–R105,600 | Precept Systems → Mosaic Group |
| Deployment, installation and training | R31,800–R48,000 | Precept Systems → Mosaic Group |
| Infrastructure (gateways, server, NAS) | R47,000–R65,000 | Precept Systems → Mosaic Group |
| **Total capital investment** | **~R1,706,000–R1,866,000** | |
| Monthly operating (on-premises) | R1,000–R1,500/month | |

*All Precept Systems estimates are indicative. Firm pricing will be provided in the formal Proposal document (MGW-PRO-202602-004).*

---

## 5. Return on Investment

### 5.1 Annual Benefit Scenarios

The total annual benefit combines billing improvement (Pillar 2) with conservation savings (Pillar 1). Leak detection savings (Pillar 3) are excluded from the base case as they are event-dependent — but can be substantial.

| Scenario | Billing Rate | Conservation | Billing Gain/Year | Conservation Saving/Year | **Total Annual Benefit** |
|----------|-------------|-------------|-------------------|-------------------------|--------------------------|
| **Conservative** | R80/kL | 10% reduction | R1,716,000 | R639,000 | **R2,355,000** |
| **Base case** | R100/kL | 15% reduction | R2,836,000 | R958,000 | **R3,794,000** |
| **Optimistic** | R130/kL | 20% reduction | R4,516,000 | R1,277,000 | **R5,793,000** |

*Billing gain = (consumption × billing rate × 12) − current annual revenue (R230,400 × 12 = R2,764,800). Conservation savings calculated at the effective all-in rate of ~R114/kL (see note on sewage in Section 2.1).*

### 5.2 Payback Period

| Scenario | Total Investment | Annual Net Benefit | Payback Period |
|----------|-----------------|-------------------|----------------|
| Conservative | R1,866,000 | R2,337,000 | **9.6 months** |
| Base case | R1,866,000 | R3,776,000 | **5.9 months** |
| Optimistic | R1,866,000 | R5,775,000 | **3.9 months** |

*Net benefit = gross benefit minus ~R18,000/year operating costs (on-prem). Investment uses the upper-range estimate to be conservative.*

### 5.3 Sensitivity Analysis — What If Only One Pillar Delivers?

| Scenario | Annual Benefit | Payback |
|----------|---------------|---------|
| **Billing only** (R100/kL, no conservation) | R2,836,000 | 7.9 months |
| **Conservation only** (keep R400 flat, 15% reduction at R114/kL) | R958,000 | 23 months |
| **Conservation only** (keep R400 flat, 15% reduction at R65.91/kL) | R554,000 | 40 months |
| **Leak detection only** (detect and fix R156K/month in leaks) | R1,866,000 | 12 months |

**The billing improvement alone justifies the investment** with payback under 8 months. Conservation and leak detection provide additional returns but are not required for the financial case to hold.

### 5.4 Break-Even Analysis

The system reaches break-even within 12 months if **any one** of the following occurs:

| Condition | Threshold |
|-----------|-----------|
| Billing rate increase only | Raise from R400/month flat to ~R670/month flat (unjustifiable without consumption data) |
| Conservation only (at R114/kL) | 29% reduction in consumption across 12 floors |
| Leak detection only | Identify and fix R156,000/month in leaks (~1,368 kL/month) |

In practice, all three pillars contribute simultaneously. The combined effect means break-even is reached well within the first year under any reasonable scenario.

### 5.5 Five-Year Projection (Base Case)

| Year | Cumulative Investment | Cumulative Benefit | Cumulative Net | ROI |
|------|----------------------|-------------------|----------------|-----|
| 1 | R1,884,000 | R3,794,000 | R1,910,000 | 101% |
| 2 | R1,902,000 | R7,588,000 | R5,686,000 | 299% |
| 3 | R1,920,000 | R11,382,000 | R9,462,000 | 493% |
| 4 | R1,938,000 | R15,176,000 | R13,238,000 | 683% |
| 5 | R1,956,000 | R18,970,000 | R17,014,000 | 870% |

*Assumes base case benefits remain constant (no tariff escalation). Operating costs of ~R18,000/year included. Real returns will be higher as municipal tariffs increase annually.*

---

## 6. Phased Approach

A phased rollout is recommended. This minimises risk, validates the solution before full commitment, and aligns with Mosaic Group's preference for phased payments.

### 6.1 Pilot Phase

| Item | Detail |
|------|--------|
| **Scope** | 1 floor: 6 unit meters (15mm) + 1 bulk floor meter (50mm) + 7 IoT nodes + 1 gateway |
| **Duration** | 6–8 weeks (including installation and monitoring period) |
| **Purpose** | Validate RF coverage, meter accuracy, ERP integration, platform functionality |

**Pilot Investment (estimated):**

| Item | Est. Cost (ex VAT) | From |
|------|---------------------|------|
| 6× 15mm unit meters + 1× 50mm bulk meter | R21,960 | Precision Meters |
| 7× IoT telemetry nodes | R2,800–R3,850 | Precept Systems |
| 1× LoRaWAN gateway | R7,000–R9,000 | Precept Systems |
| Platform setup, firmware, codec, commissioning | R40,000–R55,000 | Precept Systems |
| **Pilot total** | **~R72,000–R90,000** | |

**What the pilot validates:**

- LoRaWAN signal propagation through reinforced concrete floors
- Meter accuracy and pulse output reliability
- ChirpStack network server and codec functionality
- CLTM ERP integration (automated billing data flow)
- Dashboard and reporting (management acceptance)
- Installation process and per-unit time (refined rollout planning)

**Important:** All pilot hardware becomes part of the full system — nothing is throwaway. The pilot floor meters, nodes, gateway, and platform are reused in Phase 1.

### 6.2 Phase 1 — Remaining 11 Floors

| Item | Detail |
|------|--------|
| **Scope** | 11 floors: 570 unit meters + 11 bulk meters + 581 IoT nodes + 3 gateways + server + NAS |
| **Duration** | 8–12 weeks (floor-by-floor, ~1 floor per week) |
| **Prerequisites** | Successful pilot, client approval to proceed |

**Phase 1 Investment (estimated, remaining after pilot):**

| Item | Est. Cost (ex VAT) | From |
|------|---------------------|------|
| 570× 15mm unit meters + 11× 50mm bulk meters | R1,299,960 | Precision Meters |
| 581× IoT telemetry nodes | R232,400–R319,550 | Precept Systems |
| 3× LoRaWAN gateways | R21,000–R27,000 | Precept Systems |
| Server + NAS | R19,000–R29,000 | Precept Systems |
| Remaining development, ERP integration, deployment, training, operating manual | R62,000–R99,000 | Precept Systems |
| **Phase 1 total** | **~R1,634,000–R1,774,000** | |

### 6.3 Future Phases (Not Priced)

| Phase | Scope | Trigger |
|-------|-------|---------|
| Upper 15 floors | 720 additional unit meters + 15 bulk meters + additional gateways | Business decision by Mosaic Group and upper-floor tenant agreement |
| Motorised ball valves | Remote shutoff and throttling per unit (system is valve-ready) | Client request, acceptable cost, power infrastructure |
| Other Mosaic properties | Extend platform to additional buildings in portfolio | Proven success at City Life |

### 6.4 Why Phased?

| Reason | Benefit |
|--------|---------|
| **Risk management** | Pilot validates the technology before committing R1.3M+ in meters |
| **Cash flow** | Payments spread across phases, not a single upfront commitment |
| **Learning** | Installation process refined during pilot, applied to Phase 1 |
| **Confidence** | Building management sees real data before full rollout — easier board approval |
| **No waste** | Pilot hardware is reused — not a sunk cost |

---

## 7. Risk Summary

| Risk | Likelihood | Impact | Mitigation |
|------|------------|--------|------------|
| **Tenant resistance to usage-based billing** | Medium | Medium | Gradual transition; set initial rate conservatively (R80/kL); communicate transparently; provide usage visibility before changing billing |
| **RF coverage gaps in concrete building** | Low–Medium | Medium | 4 gateways provide redundant coverage; pilot validates before full deployment; additional gateways can be added if needed |
| **ERP integration complexity** | Medium | Medium | CLTM developer confirmed API integration model; Postman collection provided; pilot validates data flow end-to-end |
| **Installation disruption to tenants** | Low | Low | Each unit has an existing isolation valve — water shutoff is per unit only, so neighbouring tenants are not disrupted; estimated 1 hour per unit; work scheduled during 10:00–14:00; Mosaic notifies tenants in advance |
| **Meter pricing changes** | Low | Medium | Volume discount pending from Precision Meters; firm pricing to be locked before Phase 1 order |
| **Load shedding** | Medium | Low | Smart meters are battery-powered (unaffected); gateways on UPS-backed PoE switches; server on building UPS; data buffered during outages, no readings lost |
| **Server hardware failure** | Low | Medium | NAS + cloud backup (three-tier strategy); containerised deployment allows rebuild within 4–8 hours; see Section 4.6 |
| **Meter supplier change** | Low | High | Platform is designed for Precision Meters integration (see Section 9); selecting a different supplier would require a compatibility assessment |

---

## 8. Alternatives Considered

| Option | Capital Cost | Annual Operating | 5-Year TCO | Recommendation |
|--------|-------------|-----------------|------------|----------------|
| **1. Status quo** | R0 | R0 | R0 (but R18.2M in unrecovered costs) | Not recommended |
| **2. Manual meter reading** | ~R1.32M (meters only) | R150,000+ (labour) | ~R2.1M | Not recommended |
| **3. Prepaid meters** | ~R1.5–R2M | R50,000+ | ~R1.8–R2.3M | Rejected by client |
| **4. SaaS platform (third-party)** | ~R1.32M (meters) + R50–100K (setup) | R60,000–R180,000/year | ~R1.7–R2.3M | Not recommended |
| **5. Custom platform (Precept)** | ~R1.71–R1.87M | R12,000–R18,000/year | ~R1.77–R1.96M | **Recommended** |

### Option 1: Status Quo — Do Nothing

| Aspect | Assessment |
|--------|------------|
| Capital cost | R0 |
| Ongoing cost | R0 |
| Water cost exposure | R533,000/month (12 floors), R6.4M/year |
| Revenue recovery | R230,400/month (43%) |
| Monthly shortfall | R302,600 |
| **Verdict** | **Not recommended** — every month of inaction costs R302,600 in unrecovered water |

### Option 2: Meters Only — Manual Reading

Install meters without telemetry. Read manually.

| Aspect | Assessment |
|--------|------------|
| Capital cost | ~R1,321,920 (meters only) |
| Labour cost | 576 meters × 10 min/read × R28.79/hr = ~R2,764/read. Monthly: ~R33,000. Annual: ~R150,000+ |
| Pros | Cheaper than automated; provides some consumption data |
| Cons | Labour-intensive; error-prone; no real-time visibility; no automated leak detection; no ERP integration; reading staff must access every floor every month; data quality degrades when staff are absent |
| **Verdict** | **Not recommended** — saves on platform but creates permanent labour cost and forfeits the most valuable benefits (real-time monitoring, leak detection, automated ERP integration) |

### Option 3: Prepaid Meters

| Aspect | Assessment |
|--------|------------|
| Capital cost | Higher per unit (prepaid mechanism + tamper protection) |
| Pros | No billing disputes; tenants prepay for water |
| Cons | Tenants may stop using water to save money — apartments not cleaned, hygiene issues; building management loses visibility into consumption patterns; no leak detection if meters are "off" |
| **Verdict** | **Rejected by client** — Tanya Dowley explicitly excluded prepaid meters during the site visit due to unacceptable behavioural consequences |

### Option 4: Third-Party SaaS Platform

Use a commercial water monitoring platform with monthly subscription.

| Aspect | Assessment |
|--------|------------|
| Capital cost | ~R1,321,920 (meters) + R50,000–R100,000 (setup/hardware) |
| Monthly subscription | R5,000–R15,000/month |
| 5-year platform cost | R300,000–R900,000 |
| Pros | Turnkey; vendor support; faster initial deployment |
| Cons | High recurring fees erode ROI; vendor lock-in; data leaves site; limited customisation; CLTM ERP integration may not be supported; dependent on vendor's continued operation |
| **Verdict** | **Not recommended** — subscription fees of R60K–R180K/year significantly reduce the financial case; limited flexibility for CLTM integration and future expansion |

### Option 5: Custom Platform (Precept Systems) — Recommended

| Aspect | Assessment |
|--------|------------|
| Capital cost | ~R1,706,000–R1,866,000 (meters + nodes + platform + infrastructure) |
| Annual operating | ~R12,000–R18,000/year (on-premises) |
| 5-year TCO | ~R1,770,000–R1,956,000 |
| Pros | Full control; CLTM ERP integration built to spec; no recurring subscription; data stays on-premises or SA-hosted cloud; expandable to other buildings; proven architecture (Fairfield Dairy reference); **Mosaic Group receives full intellectual property** — all designs, schematics, firmware, application code, and documentation are handed over (see Section 10) |
| Cons | Requires engagement with Precept Systems for development and initial support |
| **Verdict** | **Recommended** — lowest total cost of ownership; maximum flexibility; purpose-built for Mosaic Group's requirements; full IP ownership protects Mosaic Group's investment regardless of future circumstances |

---

## 9. Strategic Partnership — Precision Meters

Precept Systems operates as a technology integration partner aligned with Precision Meters, a specialist manufacturer of SANS 1529-certified ultrasonic water meters. The monitoring platform has been specifically designed, developed, and tested for seamless integration with Precision Meters' product range. This alignment ensures:

| Factor | Benefit |
|--------|---------|
| **Deep technical integration** | Platform firmware, codec, and dashboard purpose-built for Precision Meters' pulse output and LoRaWAN characteristics |
| **Single-vendor consistency** | One meter supplier across the entire building — uniform data format, consistent accuracy, shared spare parts |
| **Coordinated support** | Precept Systems provides field support in KwaZulu-Natal; Precision Meters provides product support from Cape Town |
| **Quality assurance** | Precision Meters supplies certified ultrasonic meters — industrial quality, not consumer-grade |

As a result of this strategic alignment, the monitoring platform is developed exclusively for Precision Meters products. This ensures the highest level of integration quality, consistent support, and a single point of accountability for the complete metering solution.

Should Mosaic Group wish to evaluate meters from an alternative supplier, a technical compatibility assessment would be required, and additional development may be necessary to achieve equivalent integration quality. Precept Systems would be happy to discuss any such requirements openly.

---

## 10. Intellectual Property and System Ownership

### IP Handover

Full handover of all project deliverables occurs at the completion of the project, or upon completion of the final delivered phase if the project is discontinued after any phase. Deliverables include:

| Deliverable | Format |
|-------------|--------|
| Hardware schematics and PCB designs | Source files (KiCad or equivalent) |
| Node firmware source code | Source repository |
| Application and API source code | Source repository |
| ChirpStack codec and device profiles | JavaScript source |
| Operating manual | PDF and/or Markdown |
| System architecture and configuration documentation | Technical documentation |

### Ownership Terms

- **During active development,** Precept Systems retains working access to all project materials to continue development and provide support.
- **Upon handover,** Mosaic Group owns all project-specific deliverables. Should anything happen to Precept Systems, Mosaic Group's investment is protected — the complete system documentation, source code, and schematics can be provided to any competent service provider to continue maintenance or development.
- **Mosaic Group may not** share the delivered intellectual property with a business that competes directly with Precept Systems, provided that Precept Systems remains operational and available to service the project.
- **Precept Systems reserves the right** to reuse underlying intellectual property (design patterns, code patterns, architectural approaches) in other projects. This is standard practice and does not diminish Mosaic Group's ownership of their specific implementation.

---

## 11. Recommendation

### The Financial Case

| Metric | Value |
|--------|-------|
| Total investment | ~R1.87M (upper estimate) |
| Annual benefit (base case) | ~R3.79M |
| Payback period | 5.9 months (base) / 9.6 months (conservative) |
| 5-year net return | ~R17.0M |
| 5-year ROI | 870% |

### The Operational Case

- **Eliminates the blind spot** on a R6.4M/year water expense
- **Automated data flow** from meters to dashboard to CLTM ERP — no manual intervention
- **Real-time leak detection** prevents silent water loss that currently goes unnoticed
- **Operating manual and training** ensure Mosaic Group can operate the system independently
- **Foundation for portfolio-wide water management** across Mosaic Group properties

### The Strategic Case

- **Every month without metering is another R302,600** in unrecovered water costs
- Municipal tariffs increase annually — the shortfall grows without intervention
- Usage-based billing is the industry standard for multi-tenant properties
- Metering data provides evidence for tenant communication, municipal disputes, and board reporting
- **Full IP ownership** — Mosaic Group's investment is protected regardless of future circumstances

### Recommendation

**Proceed with the phased deployment.** Begin with the pilot phase (1 floor) to validate the solution, then roll out to the remaining 11 floors. The investment is justified under any reasonable scenario — even under the most conservative assumptions, the billing improvement alone pays back the investment within 8 months.

---

## 12. Next Steps

| # | Action | Owner |
|---|--------|-------|
| 1 | Review and approve this Business Case | Tanya Dowley / Board |
| 2 | Confirm meter pricing (volume discount pending from Precision Meters) | Garth Le Roux |
| 3 | **Confirm municipal bill composition** (sewage/sanitation tariff structure — see Section 2.1) | Tanya Dowley / Accounts |
| 4 | Review Scope of Work and Proposal (to follow) | Tanya Dowley |
| 5 | Select pilot floor | Tanya Dowley / Ryan |
| 6 | Approve project commencement (pilot phase) | Board |
| 7 | Precept Systems initiates platform setup and pilot deployment | Jason van Wyk |

---

## Approval

| Role | Name | Decision | Signature | Date |
|------|------|----------|-----------|------|
| Property Management | Tanya Dowley | Approve / Reject | | |
| Board of Directors | | Approve / Reject | | |

---

## Assumptions

This business case is based on the following assumptions. If any prove incorrect, the financial projections should be revised.

| # | Assumption | Status |
|---|------------|--------|
| 1 | Water consumption on the first 12 floors is proportional to their share of the building (44.4%). Actual consumption may differ if floor usage patterns vary. | Assumed |
| 2 | The effective all-in rate of ~R114/kL is derived from R1,200,000 ÷ 10,500 kL. This includes all municipal water-related charges. | Assumed |
| 3 | **Whether the municipal bill includes consumption-based sewage/sanitation charges needs to be confirmed.** If a significant portion of the bill is fixed charges not linked to consumption, conservation savings per kL would be lower than presented. | **To be confirmed** |
| 4 | Conservation savings of 10–25% are based on published residential metering studies. Actual results depend on tenant behaviour, communication, and enforcement. | Assumed |
| 5 | All 576 units are occupied and currently pay R400/month for water. Vacancy rates would reduce both costs and revenue proportionally. | Assumed |
| 6 | Meter pricing of R2,100 (15mm) and R9,360 (50mm) ex VAT is as quoted on 3 February 2026. Volume discount is pending and may reduce the total. | Quoted, pending revision |
| 7 | IoT node and infrastructure costs are estimated. Firm pricing will be provided in the formal Proposal document. | Estimated |
| 8 | Platform service hours are estimates at R600/hr. Firm pricing will be provided in the formal Proposal document. | Estimated |
| 9 | The CLTM ERP system supports API integration as confirmed by Sumir (2 February 2026). Integration complexity is estimated, not scoped in detail. | Confirmed in principle |
| 10 | Cloud hosting costs are indicative and depend on provider selection. Exact pricing to be confirmed. | Estimated |

---

## Sources

- **Water costs and tariff:** Tanya Dowley, site visit 29 January 2026; Mosaic Group records
- **Building profile and infrastructure:** Site visit report, 29 January 2026 (MGW-PRO-202602-001)
- **Meter pricing:** Garth Le Roux, Precision Meters, email 3 February 2026
- **Conservation studies:** General residential metering literature (10–25% reduction commonly cited)
- **Platform estimates:** Precept Systems, based on Fairfield Dairy reference project
- **ERP integration:** Sumir (Mosaic Group IT), email 2 February 2026

---

*Document Ref: MGW-PRO-202602-002 | Version 1.1 | Status: Draft for client review*
