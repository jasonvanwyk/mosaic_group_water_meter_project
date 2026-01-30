# Site Assessment Report

**City Life (Delta Towers) — 477 Anton Lembede Street, Durban CBD**

---

| | |
|---|---|
| **Document** | MGW-SAR-2026-01-30 |
| **Prepared by** | Precept Systems (Pty) Ltd |
| **Author** | Jason van Wyk, Managing Director |
| **Date** | 30 January 2026 |
| **Prepared for** | Precision Meters (Pty) Ltd — Bradley Cassani, Garth Le Roux |
| **Client** | Mosaic Group — Tanya Dowley (Part Owner) |
| **Classification** | Commercial in Confidence |

---

**Precept Systems (Pty) Ltd**
3377 Nguni Drive, St. Johns Village, Howick 3290
Tel: +27 83 288 9052 | Email: jason@precept.co.za | Web: www.precept.co.za
Reg: 2024/507423/07

---

## Table of Contents

1. [Executive Summary](#1-executive-summary)
2. [Building Profile](#2-building-profile)
3. [Water Infrastructure](#3-water-infrastructure)
4. [Current Water Operations](#4-current-water-operations)
5. [Problems Identified](#5-problems-identified)
6. [Meter Requirements](#6-meter-requirements)
7. [Recommended Meter Selection](#7-recommended-meter-selection)
8. [Communications Architecture — Why LoRaWAN](#8-communications-architecture--why-lorawan)
9. [IT Infrastructure Assessment](#9-it-infrastructure-assessment)
10. [Installation Considerations](#10-installation-considerations)
11. [Phased Rollout Plan](#11-phased-rollout-plan)
12. [Next Steps](#12-next-steps)

---

## 1. Executive Summary

Precept Systems conducted a site visit at City Life (Delta Towers), 477 Anton Lembede Street, Durban CBD on 29 January 2026. The building is a 27-storey residential high-rise owned by Mosaic Group, a Durban-based property developer managing 2,500+ residential units and 9,000+ tenants across multiple brands.

The scope of the proposed water metering solution covers the first 12 floors (576 units at 48 per floor). The upper 15 floors accommodate a different tenant type and are excluded from this phase, though future expansion is possible.

The building currently has no individual water metering. Tenants pay a flat R400/month regardless of consumption, while Mosaic Group absorbs the full municipal water bill. The client requires:

- **576 unit meters** (15mm) for per-apartment consumption metering and billing
- **12 bulk floor meters** (50mm) for per-floor leak detection and water balance reconciliation
- **Automated telemetry** — manual reading of 588 meters is impractical
- **ERP integration** — metering data must flow into the building's tenant management system

Based on our assessment of the site conditions, building construction, and existing IT infrastructure, we recommend:

- **Precision Meters Ultrasonic 15mm meters** with integrated LoRaWAN radio for all 576 unit meters
- **Precision Meters Ultrasonic 50mm meters** with integrated LoRaWAN radio for all 12 bulk meters
- **Private LoRaWAN network** (868 MHz, EU863-870) using ChirpStack network server, with Ethernet backhaul via the building's existing UniFi managed network

This report details our findings, the rationale for these recommendations, and the proposed phased deployment approach.

---

## 2. Building Profile

| Item | Details |
|------|---------|
| **Building name** | City Life (Delta Towers) |
| **Address** | 477 Anton Lembede Street, Durban CBD |
| **Owner** | Mosaic Group |
| **Total floors** | 27 |
| **Floors in scope** | 12 (first 12 floors) |
| **Units per floor** | 48 |
| **Total units in scope** | 576 |
| **Building age** | Approximately 60 years |
| **Construction** | Reinforced concrete, brick, and steel |
| **Basements** | None |
| **Floor slab thickness** | Estimated 200–300mm reinforced concrete (1960s-era construction) |

### Building Context

Mosaic Group is a significant property developer in Durban with an extensive portfolio including City Life (2,500+ residential units), Ole (student housing for 2,000+ students), Luna (furnished apartments), and supporting commercial operations. The water monitoring requirement at City Life has the potential to extend to other Mosaic Group properties.

---

## 3. Water Infrastructure

### 3.1 Municipal Supply

| Item | Details |
|------|---------|
| Municipal mainline | 110mm |
| Municipal meter | Ground level, pavement outside building |
| Supply entry point | Top of building (rooftop tanks) |
| Feed direction | Gravity-fed from rooftop down the riser |
| Pumping | Lower floors are pump-assisted from the riser |

### 3.2 Building Reticulation

```
                    ┌──────────────────┐
                    │  Rooftop Tanks   │
                    │  (Municipal In)  │
                    └────────┬─────────┘
                             │ 75mm Main Riser
                             │ (gravity-fed, top to bottom)
         ┌───────────────────┼───────────────────┐
         │                   │                   │
   Floor 12            Floor 6             Floor 1
   ┌─────┴─────┐      ┌─────┴─────┐      ┌─────┴─────┐
   │  50mm     │      │  50mm     │      │  50mm     │
   │  Branch   │      │  Branch   │      │  Branch   │
   └──┬──┬──┬──┘      └──┬──┬──┬──┘      └──┬──┬──┬──┘
      │  │  │             │  │  │             │  │  │
     16mm per unit       16mm per unit       16mm per unit
     (48 per floor)      (48 per floor)      (48 per floor)
```

| Section | Pipe Size | Notes |
|---------|-----------|-------|
| Main riser | 75mm | Single riser through entire building |
| Floor branch | 50mm | Branches off riser at each of 12 floors |
| Unit feed | 16mm | From floor branch to each apartment |

The system is a single-riser configuration (not a ring feed). Water enters each apartment above the front door in the passage ceiling. Each unit has an existing isolation valve. There is accessible space at the pipe entry point for meter installation. Water can be isolated per riser on each floor, allowing installation without disrupting the entire building.

---

## 4. Current Water Operations

| Item | Details |
|------|---------|
| Monthly municipal water bill | R1.2M/month (entire 27-storey building) |
| Municipal water tariff | R65.91/kL |
| Monthly consumption | ~10,500 kL/month (entire building, all 27 floors) |
| Tenant billing model | Flat R400/month — no usage-based billing |
| Leak detection | Visual observation or tenant reporting only |
| Common leak points | Angle joints |
| Estimated water loss | Unknown — no measurement capability |

The gap between the flat tenant charge and the actual cost of water is the primary business driver for this project.

---

## 5. Problems Identified

| # | Problem | Impact |
|---|---------|--------|
| 1 | **No usage visibility** — No individual consumption measurement | Cannot identify waste, allocate costs fairly, or detect issues |
| 2 | **No tenant incentive** — Flat charge regardless of usage | No motivation to conserve or report leaks |
| 3 | **No leak detection** — Hidden leaks (running toilets, dripping taps) undetectable | Ongoing water loss and building damage |
| 4 | **Manual readings impractical** — 576 meters cannot be manually read at useful frequency | Need for automated telemetry |
| 5 | **Building bears full cost** — No cost recovery tied to actual consumption | Financial exposure for Mosaic Group |
| 6 | **Water restrictions unenforceable** — No mechanism during municipal restrictions | Regulatory risk and penalties |
| 7 | **System silos** — Billing data must sync with tenant management ERP | Administrative overhead |

---

## 6. Meter Requirements

| Type | Application | Pipe Size | Quantity | Purpose |
|------|-------------|-----------|----------|---------|
| Unit meter | Apartment feed | 15mm (on 16mm pipe) | 576 | Per-unit consumption metering and billing |
| Bulk floor meter | Floor branch | 50mm | 12 | Per-floor leak detection and water balance |
| **Total** | | | **588** | |

### Site-Specific Constraints on Meter Selection

| Constraint | Implication |
|------------|-------------|
| **Durban water quality** — sediment, grit, suspended particles common | Mechanical meters (rotary piston, turbine) at risk of fouling and premature failure. Ultrasonic (no moving parts) strongly preferred. |
| **Installation position** — passage ceiling, mixed horizontal/vertical orientation | Meters must be position-independent (U0D0). Ultrasonic meters are rated for any orientation; mechanical meters may lose accuracy in non-horizontal positions. |
| **No straight pipe sections** — limited space in passage ceiling, pipes may have bends/fittings near meter location | Meters requiring upstream/downstream straight runs (e.g., some turbine meters) are unsuitable. U0D0 rated meters essential. |
| **576 meters at scale** — maintenance-free operation critical | Battery life >10 years and no moving parts essential to avoid ongoing replacement/service costs across 576 locations |
| **Automated reading** — manual reading impractical | Integrated radio communication (LoRaWAN) required to avoid separate pulse counter hardware at each of 576 locations |
| **Leak detection** — primary business driver | Low-flow sensitivity critical, particularly for bulk meters detecting overnight hidden leaks |

---

## 7. Recommended Meter Selection

### 7.1 Unit Meters — Precision Meters Ultrasonic 15mm

Based on the Precision Meters product datasheets (ref: *Ultrasonic Water Meters 15mm–25mm*), the ultrasonic 15mm is the recommended unit meter.

| Specification | Value | Site Relevance |
|---------------|-------|----------------|
| Technology | Ultrasonic (static, no moving parts) | Resistant to Durban sediment/grit |
| Body length | 110mm | Fits available passage ceiling space |
| Class C: Qmin | 7.5 L/h | Detects very low flows |
| Class D: Qmin | 10 L/h | |
| Sensitive to | 1 L/h | Dripping tap detection |
| Overload flow (Qs) | 2 m³/h | Adequate for single apartment |
| Permanent flow (Qp) | 1 m³/h | |
| Installation | Any position (U0D0) | Passage ceiling: mixed orientation |
| IP rating | IP68 | Fully submersible |
| Battery life | >16 years | Exceeds 10-year project horizon |
| Data logger | Hourly, daily, monthly | Data resilience if comms fail |
| Standards | SANS 1529-1:2019 Class C or D | SA legal metrology compliant |
| Bi-directional | Yes | Detects backflow |
| **AMR/AMI** | **LoRaWAN (EU863-870), wM-Bus 433/868 MHz, NB-IoT, NFC** | **Integrated LoRaWAN eliminates need for separate telemetry hardware per meter** |

**Key advantage — integrated LoRaWAN radio:** The datasheet confirms integrated radio communication supporting LoRaWAN on the EU863-870 channel plan (correct for South Africa under ICASA regulations). This means each meter can transmit directly to a LoRaWAN gateway without requiring a separate pulse counter node. For 576 meters, this eliminates significant hardware cost, installation complexity, and maintenance burden.

**Built-in intelligence:** The meter provides total volume, forward/reverse volume, max/min flow rate with timestamps, leakage detection, burst detection, backflow detection, empty pipe, tamper, and water temperature — all transmitted over the integrated radio.

### 7.2 Bulk Floor Meters — Precision Meters Ultrasonic 50mm

Based on the Precision Meters product datasheets (ref: *Ultrasonic 32 Water Meters 32mm–50mm*), the ultrasonic range is recommended for the 12 bulk floor meters.

**Note on spec sheet:** The 32–50mm datasheet shows three columns labelled 32mm, 40mm, and 40mm. The third column has doubled flow rates (Qs 24 m³/h, Qp 12 m³/h) and a shorter body length (255mm vs 300mm). We believe this third column represents the 50mm variant and request confirmation from Precision Meters.

| Specification | Column 3 (believed 50mm) | Site Relevance |
|---------------|--------------------------|----------------|
| Overload flow (Qs) | 24 m³/h | Adequate for 48-unit floor branch |
| Permanent flow (Qp) | 12 m³/h | |
| Transitional flow (Qt) | 180 L/h | |
| **Minimum flow (Qmin)** | **72 L/h** | **Critical for overnight leak detection** |
| Body length | 255mm | Fits floor branch location |
| AMR/AMI | LoRaWAN (EU863-870), wM-Bus, NB-IoT, NFC | Same integrated radio as 15mm |

### 7.3 Why Not the Infinity Evo Woltmann for Bulk Meters?

The Woltmann 50mm (ref: *Infinity Evo Class C Woltmann Meter*) was considered for the bulk meters but is not recommended:

| Factor | Ultrasonic 50mm | Woltmann 50mm |
|--------|----------------|---------------|
| Technology | Static (no moving parts) | Mechanical turbine |
| **Minimum flow (Qmin)** | **72 L/h** | **240 L/h** |
| Leak detection sensitivity | Excellent — detects flows 3.3x lower | Poor for small leak detection |
| LoRaWAN integrated | Yes | No — pulse output only |
| Node required | No (integrated radio) | Yes (separate pulse counter) |
| Battery life | >16 years (internal) | N/A (external node battery) |
| Maintenance | None (no moving parts) | Bearing wear, sediment sensitivity |

The 3.3x lower minimum flow threshold of the ultrasonic meter is critical. The primary value of bulk floor meters is detecting hidden leaks (running toilets, slow drips) that manifest as measurable overnight flow. At 2:00 AM, a floor with 48 apartments should have near-zero flow. A concealed running toilet flowing at ~100 L/h would be detected by the ultrasonic meter (Qmin 72 L/h) but missed entirely by the Woltmann (Qmin 240 L/h).

### 7.4 Why Not the ASM Polymer Rotary Piston for Unit Meters?

| Factor | Ultrasonic 15mm (W1) | ASM Polymer 15mm |
|--------|---------------------|-------------------|
| Technology | Static (no moving parts) | Mechanical (rotary piston) |
| Durban water quality resilience | Excellent — no fouling | At risk — moving parts affected by grit/sediment |
| Minimum flow (Qmin) | 7.5 L/h (Class C) | 15 L/h |
| Low flow sensitivity | Down to 1 L/h | Not specified below Qmin |
| LoRaWAN integrated | Yes | No — pulse output only (0.5 L/pulse) |
| Enclosure required | No (IP68 composite body) | Yes — legal requirement for polymer body meters |
| Installation in passage ceiling | No enclosure = simpler, smaller | Meter box enclosure = larger, more complex |
| Battery life | >16 years | N/A (mechanical register) |

The combination of Durban water quality, the passage ceiling mounting location, the scale (576 units), and the need for integrated LoRaWAN makes the Precision Meters ultrasonic 15mm the clear choice for unit meters.

---

## 8. Communications Architecture — Why LoRaWAN

### 8.1 The Challenge

The building is a ~60-year-old, 27-storey reinforced concrete high-rise. Any telemetry solution must:

- Reliably collect data from 588 meters across 12 concrete floors
- Operate for 10+ years without battery replacement at 576 locations
- Avoid new cabling to individual meter locations (explicitly rejected by the client on cost grounds)
- Not interfere with or depend on the building's tenant WiFi infrastructure
- Support future expansion (motorised valve control, additional floors)

### 8.2 Why 868 MHz LoRaWAN Is Optimal for This Building

LoRaWAN operates in the sub-GHz ISM band — South Africa uses 868 MHz under ICASA regulation (aligned with the EU863-870 channel plan). This frequency offers fundamental physical advantages over 2.4 GHz (WiFi) for penetrating dense building materials.

**Measured signal attenuation through building materials at 868 MHz:**

| Material | Typical Thickness | Attenuation per Layer | Source |
|----------|-------------------|-----------------------|--------|
| Reinforced concrete floor slab | 150–250mm | 18–25 dB | ITU-R P.2040, field studies |
| Reinforced concrete (1960s-era, denser) | 200–300mm | 22–28 dB | Field studies |
| Interior brick/masonry wall | 110mm | 3–5 dB | ITU-R P.2040 |
| Exterior masonry/stone wall | 200mm+ | 10–15 dB | ITU-R P.2040 |
| Steel fire door | 45mm | 15–18 dB | Field measurements |
| Plumbing riser (metal cluster) | Varies | 5–10 dB | Empirical |

**LoRaWAN link budget analysis for City Life:**

| Parameter | Value |
|-----------|-------|
| TX power | +14 dBm (ICASA limit for 868 MHz) |
| Receiver sensitivity (SF12) | -137 dBm |
| **Maximum link budget** | **~157 dB** |
| Attenuation per concrete floor slab (conservative) | 23 dB |
| Floors penetrable from single gateway | 5–7 (23 dB x 6 = 138 dB, within budget) |

At SF12 (maximum spreading factor), a LoRaWAN signal has approximately 157 dB of link budget. With a conservative estimate of 23 dB attenuation per reinforced concrete floor slab, a single gateway can reliably cover 5–7 floors — accounting for additional path losses from walls, piping, and fading.

**Gateway placement strategy for 12 floors:**

| Gateway | Location | Primary Coverage | Overlap Coverage |
|---------|----------|-----------------|------------------|
| GW-1 | Floor 3 | Floors 1–6 | Floors 7–8 |
| GW-2 | Floor 8 | Floors 5–12 | Floors 3–4 |
| GW-3 | Floor 12 / Rooftop | Floors 9–12 | Floors 7–8 |

Three gateways provide full coverage with deliberate overlap zones. The LoRaWAN network server (ChirpStack) handles deduplication — if a meter on Floor 6 is received by both GW-1 and GW-2, the server selects the best packet. This overlap provides self-healing redundancy.

Meter nodes are located in the passage ceiling (not inside sealed apartments), which provides a relatively open corridor for horizontal signal propagation on the same floor. Vertical penetration only passes through floor slabs, not apartment walls.

### 8.3 Why Not WiFi?

The building has excellent WiFi infrastructure (Ubiquiti UniFi, 13–15 APs per floor, Cat6 copper, 1 Gb fibre). Despite this, WiFi is not recommended for meter telemetry:

| Factor | LoRaWAN (868 MHz) | WiFi (2.4/5 GHz) |
|--------|-------------------|-------------------|
| **Building penetration** | Excellent (sub-GHz, 157 dB link budget) | Poor in concrete (2.4 GHz); very poor (5 GHz) |
| **TX power consumption** | ~10 mA for ~60 ms | ~100–200 mA for 2–5 seconds |
| **Sleep current** | <1.5 uA | ~10–20 uA (maintaining association) |
| **Battery life (5-min interval)** | **10–16 years** (Li-SOCl2 D-cell) | **12–18 months** (AA batteries) |
| **Spectrum congestion** | None (dedicated sub-GHz band) | Severe (13–15 APs/floor + tenant devices) |
| **Annual battery cost (576 meters)** | R0 | ~R86,400 (R150/replacement x 576) |
| **10-year battery cost** | R0 | ~R864,000 |
| **Infrastructure cost** | R0 per meter (existing ethernet for gateway backhaul) | R0 per meter (existing APs) |
| **Reliability** | High (CSS modulation, operates below noise floor) | Low (shared spectrum, contention-based CSMA/CA) |
| **Scalability** | 576+ nodes per 3 gateways | Problematic (AP capacity limits with tenant traffic) |
| **Interference** | Independent of building WiFi | Competes with 13–15 APs/floor and all tenant devices |
| **Protocol** | Purpose-built for IoT (small payloads, low power) | Designed for high-bandwidth data (excess overhead for metering) |

**The critical factor is battery life.** At a 5-minute reporting interval, a WiFi-based node (ESP32) draws ~100–200 mA during transmission and association. Battery replacement every 12–18 months across 576 locations creates an ongoing operational cost of ~R86,400/year — totalling ~R864,000 over 10 years in batteries and labour alone. A LoRaWAN node with a Li-SOCl2 D-cell battery achieves 10+ years with zero replacement.

However, with the ultrasonic meter's integrated LoRaWAN radio, the meter's own internal battery (>16 year life) powers both the metering function and radio transmission. No external battery, no external node, no replacement schedule.

### 8.4 Why Not Wired Ethernet?

| Factor | LoRaWAN | Wired Ethernet |
|--------|---------|----------------|
| Infrastructure per floor | R0 (wireless) | R20,000–R40,000 (48 Cat6 runs) |
| Infrastructure for 12 floors | R0 | R240,000–R480,000 |
| Installation disruption | Low (wireless, no cabling) | Very high (cable runs to 576 locations) |
| Power | Battery (in meter) | PoE from switch (new ports needed) |
| **Client acceptance** | **Accepted** | **Explicitly rejected** (infrastructure cabling cost) |

The client specifically ruled out new ethernet cabling to individual meters during the site visit. This eliminates wired Ethernet from consideration.

### 8.5 Why Private LoRaWAN (ChirpStack) vs Public Network?

| Factor | ChirpStack (Private) | Public Network (TTN, Loriot, etc.) |
|--------|---------------------|-------------------------------------|
| Monthly subscription | R0 | R0 (TTN, with limits) or R15–45/device/month |
| Annual cost (588 devices) | R0 + hosting | R0 (TTN) or R106,000–R318,000 |
| 10-year cost | ~R50,000 (hosting) | R0 (TTN) or R1M–R3M |
| Device/message limits | Unlimited | TTN: Fair Use limits; others: per-device billing |
| Data sovereignty | Full (SA-hosted) | Cloud (EU/US hosted) |
| Downlink (for future valve control) | Full (Class A/B/C) | Limited (TTN: 10/day) |
| Customisation | Full API (gRPC, REST, MQTT) | Limited |
| Self-hosted | Yes | No (community) |

A private ChirpStack network is the only option that provides zero recurring subscription cost, unlimited devices, full data sovereignty (SA-hosted, POPIA compliant), and unrestricted downlink for future motorised valve control.

Precept Systems already operates ChirpStack in production for the Fairfield Water monitoring project — the same architecture is proven and portable.

### 8.6 Architecture Summary

```
  ┌─────────────────────────────────────────────────────┐
  │  UNIT METERS (576x)                                 │
  │  Precision Meters Ultrasonic 15mm                    │
  │  (integrated LoRaWAN EU863-870)                     │
  └──────────────────────┬──────────────────────────────┘
                         │ 868 MHz LoRaWAN uplink
  ┌──────────────────────┼──────────────────────────────┐
  │  BULK METERS (12x)   │                              │
  │  Precision Meters     │                              │
  │  Ultrasonic 50mm      │                              │
  │  (integrated LoRaWAN) │                              │
  └──────────┬────────────┘                              │
             │ 868 MHz LoRaWAN uplink                    │
             ▼                                           ▼
  ┌─────────────────────────────────────────────────────┐
  │  LoRaWAN GATEWAYS (3x RAK7268V2)                   │
  │  Floors 3, 8, 12 — PoE from UniFi switch           │
  └──────────────────────┬──────────────────────────────┘
                         │ Ethernet (existing Cat6 + UniFi switches)
                         ▼
  ┌─────────────────────────────────────────────────────┐
  │  ChirpStack LoRaWAN Network Server                  │
  │  (self-hosted, SA data centre or on-premise)        │
  └──────────────────────┬──────────────────────────────┘
                         │ MQTT / API
                         ▼
  ┌─────────────────────────────────────────────────────┐
  │  WATER MONITORING PLATFORM                          │
  │  Real-time dashboard, billing, alerts, reports      │
  └──────────────────────┬──────────────────────────────┘
                         │ REST API / CSV / PDF
                         ▼
  ┌─────────────────────────────────────────────────────┐
  │  TENANT MANAGEMENT / ERP INTEGRATION                │
  │  (automated billing data export)                    │
  └─────────────────────────────────────────────────────┘
```

---

## 9. IT Infrastructure Assessment

The building's existing IT infrastructure is well-suited to support a LoRaWAN deployment. No new IT infrastructure is required beyond 3 LoRaWAN gateways.

| Item | Details | Relevance |
|------|---------|-----------|
| Network | Ubiquiti / UniFi managed | Professional-grade, VLAN-capable |
| APs per floor | 13–15 | Extensive coverage (for tenant use, not metering) |
| Internet | 1 Gb fibre (Vox) | More than adequate for gateway backhaul |
| Internal cabling | Cat6 solid copper throughout | Gateway connects to nearest floor switch |
| Switches | UniFi managed per floor | PoE available for gateway power |
| VLANs | Configured | Dedicated IoT VLAN can be allocated |
| UPS | Covers comms room and floor switches | Gateways inherit UPS protection via PoE |
| Comms room | Power, ethernet, security, rack space | Available for on-premise server if needed |
| Rooftop | Power, ethernet, security | Available for optional 4th gateway |
| IT administrator | Sumir (on-site) | Capable of VLAN config and ongoing support |

**Gateway infrastructure requirement:** Each of the 3 gateways needs a single Cat6 cable to the nearest floor switch (PoE provides both power and data). No additional cabling, power supplies, or network equipment is required.

---

## 10. Installation Considerations

| Factor | Details |
|--------|---------|
| Meter mounting | Passage ceiling, above unit front door, alongside existing pipes |
| Existing isolation valve | Yes — each unit already has a shut-off valve |
| Space available | Adequate for meter installation at pipe entry point |
| Orientation | Mixed (horizontal/vertical) — requires U0D0-rated meters |
| Cable basket | Existing in passage ceiling (Cat6 and other cables) |
| Water shutoff method | Per riser on each floor |
| Estimated install time | ~1 hour per unit |
| Preferred installation window | 10:00–14:00 (tenants typically out) |
| Plumbing labour | Ryan (Mosaic Group in-house plumber) |
| IT/network support | Sumir (Mosaic Group in-house IT) |
| Tenant notification | Mosaic Group will inform tenants of scheduled shutdowns |

**Key advantage of integrated LoRaWAN meters:** Installation is purely plumbing. Ryan installs the meter inline on the 16mm feed, and the meter's integrated radio begins transmitting immediately once water flows (or upon NFC activation). No separate node, no wiring, no configuration per unit beyond gateway registration.

---

## 11. Phased Rollout Plan

The client has confirmed a phased approach. The pilot validates the solution before committing to full-scale deployment.

| Phase | Scope | Meters |
|-------|-------|--------|
| **Pilot** | 1 floor + 5 selected units | 5x 15mm + 1x 50mm |
| **Phase 1** | Complete pilot floor | +43x 15mm |
| **Phase 2** | Floors 1–4 | +192x 15mm + 4x 50mm |
| **Phase 3** | Floors 5–8 | +192x 15mm + 4x 50mm |
| **Phase 4** | Floors 9–12 | +192x 15mm + 3x 50mm |
| **Total** | 12 floors, 576 units | **576x 15mm + 12x 50mm = 588 meters** |

### Pilot Design

The pilot is designed so that nothing is discarded when scaling:

- Same meter model as full deployment
- Gateway installed during pilot becomes the permanent GW-1
- ChirpStack and monitoring platform are production-grade from day one
- Pilot floor recommended: **Floor 3 or 4** (mid-low position allows signal testing to adjacent floors)

### Gateway Rollout (aligned with meter phases)

| Gateway | Installed During | Location | Covers |
|---------|-----------------|----------|--------|
| GW-1 | Pilot | Floor 3 | Floors 1–6 |
| GW-2 | Phase 3 | Floor 8 | Floors 5–12 |
| GW-3 | Phase 4 | Floor 12 / Rooftop | Floors 9–12 |

---

## 12. Next Steps

| # | Action | Owner |
|---|--------|-------|
| 1 | Confirm 15mm ultrasonic LoRaWAN availability (standard or optional module) | Precision Meters |
| 2 | Confirm 50mm ultrasonic variant (3rd column in spec sheet) | Precision Meters |
| 3 | Provide volume pricing for 15mm and 50mm meters (pilot and full quantities) | Precision Meters |
| 4 | Provide ChirpStack device profile / payload codec for ultrasonic 15mm | Precision Meters |
| 5 | Confirm LoRaWAN uplink interval and NFC configuration capability | Precision Meters |
| 6 | Clarify supply chain arrangement (direct to client or via Precept) | Precision Meters / Precept |
| 7 | Develop technical proposal and business case for Mosaic Group | Precept Systems |
| 8 | Submit proposal to client | Precept Systems |

---

**Document Reference:** MGW-SAR-2026-01-30
**Precept Systems (Pty) Ltd** | www.precept.co.za | jason@precept.co.za | +27 83 288 9052
