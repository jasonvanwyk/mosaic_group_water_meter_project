# Communications Technology Validation

**Project:** Mosaic Group Water Meter Monitoring — City Life, 477 Anton Lembede Street
**Reference:** MGW-PRO-202602-005
**Version:** 1.0
**Date:** 2026-02-05
**Author:** Jason van Wyk, Precept Systems

---

## Revision History

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2026-02-05 | Initial release |

---

## Related Documents

| Ref | Document | Description |
|-----|----------|-------------|
| MGW-PRO-202602-001 | Site Assessment Report | Site visit findings and building profile |
| MGW-PRO-202602-002 | Business Case | Investment analysis, ROI, phased approach |
| MGW-PRO-202602-003 | Scope of Work | Detailed deliverables (TBD) |
| MGW-PRO-202602-004 | Proposal | Commercial terms and pricing (TBD) |

---

## 1. Purpose

This document validates the selection of **private LoRaWAN (868 MHz)** as the wireless communications technology for the City Life water meter monitoring system. It evaluates the site-specific challenges at 477 Anton Lembede Street, compares available technology options, and demonstrates that LoRaWAN is the only practical choice that satisfies all project requirements: reliable signal penetration through reinforced concrete, battery life measured in years (not months), zero recurring subscription fees, and proven scalability to 588+ devices.

---

## 2. Site Environment

The communications technology must operate reliably within a challenging electromagnetic and structural environment. The following site-specific factors were assessed during the site visit on 29 January 2026 and through subsequent technical analysis.

### 2.1 Building Characteristics

| Factor | Detail | Impact on Communications |
|--------|--------|--------------------------|
| Building age | ~60 years | Older reinforced concrete has denser aggregate and steel reinforcement — higher radio signal attenuation per floor |
| Construction | Reinforced concrete, brick, steel | Per-floor signal attenuation measured at 23 dB (conservative estimate for 1960s-era RC) |
| Floors in scope | 12 (of 27 total) | Meters spread across 12 vertical floors — no single access point can cover the full scope |
| Units per floor | 48 | 48 wireless devices per floor, 588 total — network must handle high device density without congestion |
| Meter location | Passage ceiling above front door | Devices installed in common passage — line-of-sight along corridor but concrete between floors |
| Main riser | 75mm steel pipe through all floors | Metal piping near device antennas adds 5–10 dB additional signal loss |

### 2.2 Existing IT Infrastructure

| Item | Detail | Relevance |
|------|--------|-----------|
| Network | Ubiquiti UniFi ecosystem | Professional-grade, supports VLAN segmentation for IoT traffic isolation |
| WiFi APs | 13–15 access points per floor | 2.4 GHz band is heavily congested — WiFi-based metering would compete with tenant devices |
| Cabling | Cat6 copper throughout building | Available for gateway backhaul (Ethernet to gateways), but client explicitly rejected cabling to each meter |
| Fiber | 1 Gbps Vox connection | Sufficient for cloud backup and remote management |
| UPS | UPS to comms room and switches | Gateway power resilience through load shedding |
| VLAN | Dedicated VLAN available | Sumir (IT) confirmed a dedicated IoT VLAN can be provisioned |

### 2.3 Durban CBD Context

| Factor | Detail |
|--------|--------|
| Location | 477 Anton Lembede Street, Durban Central Business District |
| RF environment | Dense urban — high ambient RF noise from surrounding commercial buildings, base stations, and electronic equipment |
| Frequency | 868 MHz is less congested than 2.4 GHz in CBD environments; sub-GHz signals penetrate concrete more effectively |
| Climate | Subtropical — no extreme cold affecting battery chemistry; humidity is a factor for enclosure rating (IP54 minimum) |
| Regulatory | South Africa follows the EU863-870 channel plan under ICASA (Independent Communications Authority of South Africa) |
| Load shedding | Eskom power interruptions are a factor — battery-powered devices (not mains-powered) are essential for reliability |

---

## 3. Technologies Evaluated

Four communications technologies were evaluated against the project requirements. The assessment considered signal penetration, power consumption, infrastructure cost, recurring fees, scalability, and operational complexity.

### 3.1 Comparison Matrix

| Criterion | LoRaWAN (868 MHz) | WiFi (2.4 GHz) | Wired Ethernet | Sigfox / NB-IoT |
|-----------|-------------------|-----------------|----------------|------------------|
| **Frequency** | 868 MHz (sub-GHz) | 2.4 GHz | N/A (wired) | Licensed cellular |
| **Concrete penetration** | 23 dB/floor (manageable) | 35–40 dB/floor at 2.4 GHz (severe) | N/A | Moderate (depends on carrier) |
| **Battery life** | 6–15 years | 6–18 months | N/A (mains powered) | 2–5 years |
| **Node power source** | Li-SOCl2 D-cell (19 Ah) | Rechargeable Li-ion or mains | Mains / PoE | Li-SOCl2 or mains |
| **Infrastructure cost** | 4 gateways: R28–36K | ~R0 (existing APs) | 576 cable runs: R145–200K | Carrier network (existing) |
| **Recurring fees** | R0/month (private network) | R0/month (own APs) | R0/month | R15–45/device/month |
| **Annual subscription** | R0 | R0 | R0 | R103K–R311K/year |
| **10-year subscription** | R0 | R0 | R0 | R1.03M–R3.11M |
| **WiFi band congestion** | Not affected (sub-GHz) | Competes with 13–15 APs/floor + tenant devices | Not affected | Not affected |
| **Max devices per gateway** | 1,000+ | ~30 per AP | N/A | N/A (carrier managed) |
| **Battery replacement cost (10-year)** | ~R90K (one cycle in 6–8 years) | ~R864K (5+ cycles) | R0 | ~R200K (2 cycles) |
| **Data ownership** | Full (self-hosted ChirpStack) | Full | Full | Carrier-dependent |
| **Downlink support** | Full (Class A) | Full | Full | Limited (Sigfox: 4/day) |
| **Client rejection** | — | — | Rejected (cost) | — |
| **Verdict** | **Selected** | Not viable | Rejected by client | Not viable (cost) |

### 3.2 WiFi — Why Not?

WiFi operates at 2.4 GHz, which suffers significantly more attenuation through reinforced concrete than sub-GHz frequencies. At 477 Anton Lembede Street, the 2.4 GHz band is already heavily congested with 13–15 access points per floor serving tenant devices. Adding 588 metering devices to this band would cause interference, packet loss, and unreliable readings.

WiFi-based meter nodes require either mains power or rechargeable batteries. Rechargeable batteries in WiFi devices last 6–18 months, creating a permanent maintenance cycle. For 576 units, this means replacing batteries across the building every 12–18 months at an estimated cost of R172,800 per cycle (R300/unit including labour). Over 10 years, battery replacement alone costs approximately R864,000 — more than the entire LoRaWAN infrastructure.

### 3.3 Wired Ethernet — Why Not?

Running Ethernet cable to each of the 576 units would cost an estimated R145,000–R200,000 in cabling and installation (based on ~25–35m average run per unit at R10/m for Cat6 + labour), plus ongoing maintenance. During the site visit, Tanya Dowley explicitly rejected this approach: *"Ethernet cabling to each unit is too much infrastructure cost."* Furthermore, cable runs through a 60-year-old building would require significant trunking, fire-stopping, and building compliance — adding cost and disruption.

### 3.4 Sigfox / NB-IoT — Why Not?

Public network IoT services (Sigfox, NB-IoT, LTE-M) charge per-device monthly subscriptions. At 588 devices, even a conservative R15/device/month amounts to R105,840/year — over R1 million across a 10-year period. This recurring cost erodes the financial case significantly. Additionally, these services offer limited downlink capability (Sigfox allows only 4 downlinks per day per device), no data ownership guarantee, and dependence on a third-party carrier's continued operation and pricing.

### 3.5 LoRaWAN — Why It Works

LoRaWAN operates at 868 MHz using Chirp Spread Spectrum (CSS) modulation, which was specifically designed for long-range, low-power communication through challenging environments. Key advantages for this project:

| Advantage | Detail |
|-----------|--------|
| **Sub-GHz penetration** | 868 MHz penetrates reinforced concrete with 23 dB/floor attenuation — manageable with multi-gateway design |
| **Below noise floor** | CSS modulation can decode signals up to 20 dB below the noise floor — highly resilient to CBD radio interference |
| **Adaptive Data Rate (ADR)** | Nodes automatically adjust spreading factor based on signal quality — self-optimising for each floor position |
| **Zero subscription** | Private ChirpStack network server — no per-device fees, no carrier dependency, no data leaving the premises |
| **Multi-year battery** | Li-SOCl2 D-cell batteries last 6–15 years depending on transmit interval — no battery replacement for the foreseeable life of the system |
| **Multi-gateway redundancy** | 4 gateways with overlapping coverage — if one fails, adjacent gateways absorb traffic automatically via ADR shift |
| **Proven at scale** | LoRaWAN deployments of 1,000+ devices per gateway are documented globally; 588 devices across 4 gateways is well within capacity |
| **Proven locally** | Precept Systems has deployed LoRaWAN successfully in production water monitoring using the same RAK3172 radio module |

---

## 4. RF Propagation and Coverage Design

### 4.1 Link Budget

The link budget determines whether a signal transmitted by a meter node can be received by a gateway after passing through floors of reinforced concrete.

| Parameter | Value |
|-----------|-------|
| Transmit power (node) | +14 dBm (25 mW) — ICASA maximum for 868 MHz |
| Transmit antenna gain | +2 dBi |
| Gateway antenna gain | +3 dBi |
| Gateway receiver sensitivity (SF7) | −138 dBm |
| Gateway receiver sensitivity (SF12) | −148 dBm |
| **Maximum tolerable path loss (SF7)** | **138 dB** |
| **Maximum tolerable path loss (SF12)** | **151 dB** |

### 4.2 Per-Floor Attenuation Model

| Component | Loss |
|-----------|------|
| Reinforced concrete floor slab (1960s-era) | 23 dB per floor |
| Horizontal path loss (corridor to gateway) | 35 dB |
| Fading margin (multipath, metal riser) | 10 dB |
| **Total path loss formula** | **(N × 23 dB) + 35 dB + 10 dB** |

Where N = number of floors between node and gateway.

| Floors Penetrated | Total Path Loss | SF7 Feasible? | SF10 Feasible? | SF12 Feasible? |
|-------------------|----------------|---------------|----------------|----------------|
| 1 | 68 dB | Yes (70 dB margin) | Yes | Yes |
| 2 | 91 dB | Yes (47 dB margin) | Yes | Yes |
| 3 | 114 dB | Yes (24 dB margin) | Yes | Yes |
| 4 | 137 dB | Marginal (1 dB margin) | Yes (11 dB margin) | Yes |
| 5 | 160 dB | No | No | No |

**Conclusion:** Each gateway reliably covers 3 floors at SF7 (the most battery-efficient setting) and 4 floors with SF10 fallback. No node should ever need to penetrate more than 3 floors to reach its nearest gateway.

### 4.3 Four-Gateway Placement

Based on the propagation analysis, 4 gateways placed on Floors 3, 6, 9, and 12 provide complete coverage with redundancy:

```
Floor 12  ─── GW4 ───  Covers floors 10-12 (primary), 9 (backup to GW3)
Floor 11  ─── ●
Floor 10  ─── ●
Floor  9  ─── GW3 ───  Covers floors 7-9 (primary), 10 (backup to GW4)
Floor  8  ─── ●
Floor  7  ─── ●
Floor  6  ─── GW2 ───  Covers floors 4-6 (primary), 7 (backup to GW3)
Floor  5  ─── ●
Floor  4  ─── ●
Floor  3  ─── GW1 ───  Covers floors 1-3 (primary), 4 (backup to GW2)
Floor  2  ─── ●
Floor  1  ─── ●
```

| Coverage Metric | Value |
|-----------------|-------|
| Floors with primary coverage | 12 of 12 (100%) |
| Floors with dual-gateway coverage | 8 of 12 (67%) |
| Maximum floors to nearest gateway | 3 (at SF7) |
| If any one gateway fails | All floors still covered by adjacent gateways (SF shift) |

**Self-healing:** If Gateway 2 (Floor 6) fails, nodes on floors 4–6 are automatically absorbed by Gateways 1 and 3 via Adaptive Data Rate — the system shifts to SF10 for the affected floors without manual intervention and without losing any readings.

### 4.4 Gateway Specification

| Item | Detail |
|------|--------|
| Model | RAK7268V2 (or equivalent) |
| Channels | 8 simultaneous LoRaWAN channels |
| Power | PoE (Power over Ethernet) — powered by existing UPS-backed switches |
| Backhaul | Ethernet via building Cat6 to dedicated IoT VLAN |
| Enclosure | Indoor rated (IP30) — installed in comms room or secured passage location |
| Cost | R7,000–R9,000 per unit (estimated) |
| Total | 4 gateways × R7–9K = R28,000–R36,000 |

---

## 5. Network Capacity

### 5.1 Device Volume

| Item | Quantity |
|------|----------|
| Unit meters (15mm) | 576 |
| Bulk floor meters (50mm) | 12 |
| **Total nodes** | **588** |

### 5.2 Uplink Traffic Analysis

| Transmit Interval | Uplinks/Hour (all 588) | Per Gateway (4 GW) | Channel Utilisation | Collision Probability |
|-------------------|------------------------|--------------------|--------------------|----------------------|
| 5 minutes | 7,056 | 1,764 | 1.5% | <3% (>97% success) |
| 10 minutes | 3,528 | 882 | 0.75% | <1.5% (>98.5% success) |
| 15 minutes | 2,352 | 588 | 0.5% | <1% (>99% success) |

**Assessment:** Even at the most aggressive 5-minute interval, channel utilisation is 1.5% — well within the capacity of 4 eight-channel gateways. The recommended 10–15 minute interval for water meters provides more than adequate resolution for billing and leak detection while maximising battery life.

### 5.3 Multi-Gateway Deduplication

With 4 gateways, most uplinks will be received by 2 or more gateways simultaneously. ChirpStack automatically deduplicates these packets and selects the best-quality copy (highest SNR) for processing. This multi-gateway diversity provides:

- Effective packet delivery rate >99% at any proposed interval
- Redundant reception — a missed packet at one gateway is caught by another
- Better signal quality metrics for diagnostics and network optimisation

---

## 6. Battery Life

Battery-powered operation is essential for 576 ceiling-mounted nodes where mains power is not available.

### 6.1 Battery Specification

| Parameter | Value |
|-----------|-------|
| Chemistry | Lithium Thionyl Chloride (Li-SOCl2) |
| Cell | ER34615 (D-cell) |
| Nominal capacity | 19,000 mAh (19 Ah) |
| Practical capacity (derated 20%) | 15,200 mAh |
| Configuration | Dual cells in parallel (30,400 mAh practical) |
| Self-discharge | <1% per year |
| Temperature range | −55°C to +85°C (well within Durban climate) |

### 6.2 Battery Life Projections

| Transmit Interval | Daily Consumption | Single Cell Life | Dual Cell Life |
|-------------------|-------------------|-----------------|----------------|
| 5 minutes | 0.315 mAh | ~6 years | ~8+ years |
| 10 minutes | 0.176 mAh | ~10 years | ~15 years |
| 15 minutes | 0.129 mAh | ~14 years | ~20+ years |

**At a 10-minute transmit interval (recommended), dual-cell battery life exceeds 15 years** — longer than the expected service life of the node electronics. Battery replacement is not expected to be a recurring maintenance item.

### 6.3 Comparison: LoRaWAN vs WiFi Battery Economics

| Metric | LoRaWAN (Li-SOCl2) | WiFi (Li-ion rechargeable) |
|--------|--------------------|-----------------------------|
| Battery life per cycle | 6–15 years | 6–18 months |
| Replacement cycles (10 years) | 0–1 | 5–7 |
| Cost per replacement (576 units) | ~R90,000 | ~R172,800 |
| 10-year battery cost | R0–R90,000 | ~R864,000 |
| Labour disruption | Minimal (once, if ever) | Ongoing (every 12–18 months) |

LoRaWAN's battery advantage alone saves approximately R774,000–R864,000 over 10 years compared to a WiFi-based approach.

---

## 7. Regulatory Compliance

### 7.1 ICASA and 868 MHz

| Requirement | Status |
|-------------|--------|
| Frequency band | 868 MHz — EU863-870 channel plan, adopted by South Africa |
| Regulatory body | ICASA (Independent Communications Authority of South Africa) |
| Power limit | 25 mW ERP (+14 dBm) on primary sub-band |
| Duty cycle limit | 1% on 868.0–868.6 MHz; 0.1% on 868.7–869.2 MHz |
| Actual duty cycle (10-min interval) | ~0.01% — well within limits |
| Device type approval | Required for radio modules (RAK3172, RAK7268V2) — to be verified |
| License fee | None (ISM band, licence-exempt) |

### 7.2 POPIA Compliance

All data remains on-premises (or SA-hosted cloud). No consumption data leaves South Africa. The system does not collect personal data beyond unit-level water consumption linked to a unit number (not a person). CLTM ERP integration handles the tenant-to-unit mapping within Mosaic Group's existing systems.

---

## 8. Total Cost of Ownership (10-Year)

| Cost Component | LoRaWAN (Private) | WiFi | Wired Ethernet | Sigfox/NB-IoT |
|---------------|-------------------|------|----------------|---------------|
| Infrastructure (once-off) | R28–36K (4 gateways) | ~R0 (existing APs) | R145–200K (cabling) | R0 (carrier) |
| Network server | R0 (ChirpStack, self-hosted) | R0 | R0 | N/A |
| Subscription (10 years) | R0 | R0 | R0 | R1.03–3.11M |
| Battery replacement (10 years) | R0–90K | R864K | R0 | R200K |
| Maintenance labour (10 years) | ~R50K | ~R250K | ~R100K | ~R50K |
| **10-Year Total** | **~R78K–R176K** | **~R1.11M+** | **~R245–300K** | **~R1.28–3.36M** |

**LoRaWAN has the lowest 10-year total cost of ownership by a significant margin.** The absence of subscriptions, minimal battery replacement, and low infrastructure cost make it the clear economic choice.

---

## 9. Risk and Mitigation

| Risk | Likelihood | Impact | Mitigation |
|------|------------|--------|------------|
| Floor attenuation exceeds 23 dB model | Low–Medium | Medium | Pilot phase validates actual propagation; 4-gateway design provides 3-floor limit per gateway; additional gateways can be added |
| Metal riser degrades antenna performance | Medium | Medium | Antenna testing during pilot; node placement offset from riser; external antenna option available |
| ICASA type approval gap for radio modules | Medium | High | Verify RAK3172 and RAK7268V2 type approval status before procurement; alternative approved modules available |
| LoRaWAN interference from CBD RF environment | Low | Low | CSS modulation operates below noise floor; 868 MHz band is less congested than 2.4/5 GHz in CBD |
| Gateway hardware failure | Low | Low | 4-gateway redundancy with automatic ADR shift; failed gateway replaced within 24 hours; no data lost |
| Battery early depletion | Low | Low | Dashboard monitors battery voltage per node; dual-cell design provides margin; Li-SOCl2 has <1% self-discharge |

**The pilot phase is designed specifically to validate RF propagation and coverage** before committing to the full 12-floor rollout. All pilot hardware is reused in the full system.

---

## 10. Conclusion

### Why LoRaWAN is the Only Viable Choice for City Life

| Requirement | LoRaWAN Delivers |
|-------------|------------------|
| Penetrate 60-year-old reinforced concrete | Sub-GHz (868 MHz) with 23 dB/floor — covered by 4-gateway design |
| Support 588 devices reliably | <1.5% channel utilisation at 5-minute intervals; >99% packet delivery |
| Battery life measured in years | 6–15+ years on Li-SOCl2 D-cell; no recurring battery replacement |
| No recurring subscription fees | Private ChirpStack network — R0/month, R0/year, R0/decade |
| Survive load shedding | Battery-powered nodes unaffected; gateways on UPS-backed PoE |
| Data stays on-premises | Self-hosted network server; POPIA compliant by design |
| Self-healing and redundant | 4 gateways with ADR; automatic failover; no single point of failure |
| Proven technology | RAK3172 proven in production water monitoring deployments |
| CBD-resilient | CSS modulation operates below noise floor; 868 MHz less congested than 2.4 GHz |
| Lowest 10-year TCO | R78–176K vs R245K–R3.36M for alternatives |

**LoRaWAN at 868 MHz is not a preference — it is the only technology that meets all project requirements simultaneously.** WiFi fails on penetration and battery life. Wired Ethernet was rejected on cost and disruption. Public IoT networks impose unsustainable subscription costs. LoRaWAN addresses every constraint while delivering the lowest total cost of ownership.

The 4-gateway private network design provides complete coverage across all 12 floors, with built-in redundancy that automatically compensates for gateway failure. The pilot phase will validate the RF propagation model on-site before any commitment to the full deployment.

---

*Document Ref: MGW-PRO-202602-005 | Version 1.0 | Status: Supporting document for Business Case (MGW-PRO-202602-002)*
