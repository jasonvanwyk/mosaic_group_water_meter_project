# RF Engineering Assessment — 477 Anton Lembede Street

**Project:** MGW — Mosaic Group Water Meter Monitoring
**Document:** 04-rf-engineering-assessment
**Date:** 2026-02-04
**Author:** Jason van Wyk, Precept Systems
**Scope:** 12 floors, 588 meters (576 unit + 12 bulk), City Life

---

## Table of Contents

1. [Purpose and Scope](#1-purpose-and-scope)
2. [Architecture Under Review](#2-architecture-under-review)
3. [Regulatory Compliance — ICASA 868 MHz](#3-regulatory-compliance--icasa-868-mhz)
4. [RF Propagation Analysis](#4-rf-propagation-analysis)
5. [Gateway Placement — 4-Gateway Design](#5-gateway-placement--4-gateway-design)
6. [Network Capacity Analysis](#6-network-capacity-analysis)
7. [Node Design and Battery Strategy](#7-node-design-and-battery-strategy)
8. [Codec and Payload Specification](#8-codec-and-payload-specification)
9. [Reliability and Redundancy](#9-reliability-and-redundancy)
10. [Risk Register](#10-risk-register)
11. [Recommended Test Equipment](#11-recommended-test-equipment)
12. [Conclusions](#12-conclusions)
13. [Action Items](#13-action-items)
14. [References](#14-references)

---

## 1. Purpose and Scope

This document provides an objective RF engineering assessment of the proposed LoRaWAN solution for water meter monitoring at City Life, 477 Anton Lembede Street, Durban. The assessment evaluates whether the proposed architecture will work **reliably and legally** in this specific environment.

The review covers:
- Regulatory compliance under ICASA for the 868 MHz ISM band
- RF propagation through a 60-year-old reinforced concrete high-rise
- Gateway placement and coverage for 12 floors
- Network capacity for 588 nodes at 5-15 minute uplink intervals
- Node design, battery strategy, and firmware approach
- Recommended test and measurement equipment

### Key Design Clarifications

The following design decisions are confirmed for this assessment:

| Decision | Detail |
|----------|--------|
| **LoRa radio source** | Precept-designed custom nodes (RAK3172-based), NOT the Precision Meters integrated LoRaWAN radio |
| **Meter interface** | Pulse output from Precision Meters ultrasonic meters — each Precept node reads the meter's pulse signal |
| **Codec and payload** | Precept-designed from scratch — own payload specification, own ChirpStack codec |
| **Firmware** | Precept-developed, with configurable uplink intervals (5 / 10 / 15 minutes) |
| **Battery strategy** | Dual Li-SOCl2 batteries in parallel acceptable; 2-3 year replacement cycle is acceptable |
| **Gateway count** | 4 gateways from day one — not iterating from 3 |

This approach gives Precept full control over the communications layer, uplink intervals, payload format, and firmware updates — independent of Precision Meters' radio implementation.

---

## 2. Architecture Under Review

```
  UNIT METERS (576x) + BULK METERS (12x):

  [Precision Meters Ultrasonic 15mm / 50mm]
     (pulse output)
              |
              | Pulse wire (short run, co-located)
              v
  [Precept LoRa Node (RAK3172)]
     (custom PCB, custom firmware)
     (valve-ready connectors, dual battery)
              |
              | 868 MHz LoRaWAN uplink (5/10/15 min configurable)
              v
  [LoRaWAN Gateway x4 (RAK7268V2)]
     (Floors 3, 6, 9, 12 — PoE from UniFi switch)
              |
              | Ethernet (existing Cat6, dedicated VLAN)
              v
  [ChirpStack Network Server]
     (LXC on Proxmox / cloud VM)
              |
              | MQTT / gRPC API
              v
  [Water Monitoring Platform]
     (Node.js + Express + SQLite)
              |
              | REST API (Postman collection)
              v
  [CLTM ERP Integration]
```

### Why Precept Nodes Instead of Meter-Integrated LoRaWAN

The Precision Meters ultrasonic meters include an integrated LoRaWAN radio (confirmed by Garth Le Roux, 3 Feb 2026). However, using Precept-designed nodes reading the meter's pulse output is preferred for the following reasons:

| Factor | Meter-Integrated LoRaWAN | Precept Custom Node |
|--------|--------------------------|---------------------|
| Minimum uplink interval | 4 hours (confirmed by Garth) | 5 minutes (firmware configurable) |
| Uplink configuration | Pre-dispatch only (not field-configurable) | Field-configurable via NFC or downlink |
| ChirpStack codec | Not supplied, payload format unknown | Designed by Precept, fully documented |
| Firmware updates | Not possible (sealed meter) | OTA via LoRaWAN Class A downlink or NFC |
| Valve control (future) | Not possible from meter radio | Built into node PCB (valve-ready) |
| Battery management | Tied to meter's 16-year battery | Independent, replaceable, dual-cell option |
| Sensor expansion | None — meter data only | Can add temperature, tamper, accelerometer |
| Payload optimisation | Fixed by manufacturer | Optimised for minimum ToA |
| Real-time monitoring | 4-hour minimum resolution | 5-minute resolution achievable |

**Verdict:** Using Precept nodes eliminates the 4-hour interval constraint, gives full firmware and payload control, enables future valve integration, and keeps the platform meter-agnostic at the communications layer.

---

## 3. Regulatory Compliance — ICASA 868 MHz

### 3.1 Legal Status

**LoRaWAN at 868 MHz is legal for unlicensed operation in South Africa.**

The governing regulation is the **Radio Frequency Spectrum Regulations, 2015** (Government Notice No. 279, Government Gazette No. 38641, dated 30 March 2015), Annexure B — Licence-Exempt Equipment. ICASA follows the EU863-870 channel plan, referencing CEPT/ERC/REC 70-03 and ETSI EN 300 220.

### 3.2 Power and Duty Cycle Limits

| Sub-Band (MHz) | Application | Max ERP | Duty Cycle | LoRaWAN Channels |
|-----------------|-------------|---------|------------|------------------|
| 868.0–868.6 | Non-Specific SRD | 25 mW (+14 dBm) | 1% | 868.1, 868.3, 868.5 (3 mandatory join channels) |
| 868.7–869.2 | Non-Specific SRD | 25 mW (+14 dBm) | 0.1% | Additional channels |
| 869.4–869.65 | Non-Specific SRD | 500 mW (+27 dBm) | 10% | RX2 window / high-power channel |
| 869.7–870.0 | Non-Specific SRD | 25 mW (+14 dBm) | 1% | Additional channels |

### 3.3 Compliance Verification — Precept Nodes at 5-Minute Interval

| Parameter | Limit | Precept Node Design | Compliant? |
|-----------|-------|---------------------|------------|
| TX power | 25 mW ERP (+14 dBm) | RAK3172 configured at 14 dBm EIRP (~11.85 dBm ERP) | Yes |
| Duty cycle (868.0–868.6) | 1% = max 36s airtime/hr | SF7, 10-byte payload: ~61ms ToA × 12 TX/hr = 0.73s/hr (0.02%) | Yes |
| Duty cycle (5-min interval) | 1% | 0.02% per device | Yes — 50x headroom |
| Duty cycle (10-min interval) | 1% | 0.01% per device | Yes — 100x headroom |
| Encryption | Not mandated | AES-128 (LoRaWAN 1.0.x standard) | Yes |
| Frequency hopping | Not mandated (recommended) | 8-channel hopping via LoRaWAN protocol | Yes |

### 3.4 ICASA Type Approval Requirements

All RF-transmitting devices deployed in South Africa must be ICASA type-approved under Section 35 of the Electronic Communications Act (No. 36 of 2005).

| Device | Type Approval Required | Action |
|--------|----------------------|--------|
| Precept LoRa node (RAK3172 module) | Yes — the RAK3172 module must be type-approved | Verify RAKwireless holds ICASA TA for RAK3172. If not, apply (~R5,100, 6-12 weeks). ICASA accepts EU CE test reports from ISO 17025 labs. |
| LoRaWAN gateway (RAK7268V2) | Yes | Verify RAKwireless holds ICASA TA for RAK7268V2. Same process if not. |
| Precision Meters ultrasonic meter | Only the LoRaWAN radio module needs TA — but since we are NOT using the meter's radio, the meter itself does not require RF type approval | Meter requires SANS 1529-1:2019 compliance for legal metrology (confirm with Precision Meters) |

**Key point:** Because Precept nodes provide the LoRaWAN radio (not the meter's built-in module), the type approval burden falls on the RAK3172 module and the gateway — not on the Precision Meters hardware for RF purposes. The meter's built-in LoRaWAN radio remains dormant/unused.

### 3.5 Regulatory Constraints Summary

| Constraint | Impact | Mitigation |
|------------|--------|------------|
| 1% duty cycle on primary sub-band | Limits airtime per device to 36s/hr | At 5-min intervals with SF7, usage is 0.73s/hr (0.02%). No impact. |
| 14 dBm ERP max on primary channels | Limits range per transmission | Sub-GHz penetration is excellent. 4 gateways provide adequate coverage for 12 floors. |
| ICASA type approval required | Must hold TA certificate before deployment | Verify RAK3172 and RAK7268V2 TA status. Budget 6-12 weeks if application needed. |
| Non-interference basis | No protection from other users of the band | Private building deployment — interference from external 868 MHz sources unlikely in a concrete building. |

---

## 4. RF Propagation Analysis

### 4.1 Building Characteristics

| Parameter | Value | RF Impact |
|-----------|-------|-----------|
| Age | ~60 years | Denser concrete, higher steel content → higher attenuation per floor |
| Construction | Reinforced concrete, brick, steel | 22-28 dB attenuation per floor slab (868 MHz) |
| Floors in scope | 12 | Cannot be covered by a single gateway |
| Units per floor | 48 | High node density per floor (manageable with LoRaWAN) |
| Node mounting | Passage ceiling, above unit front door | Good — open corridor, not inside sealed apartments |
| Riser | Single 75mm steel riser + piping cluster | 5-10 dB additional attenuation near riser; also potential vertical waveguide effect |
| Stairwells/lift shafts | Present | Provide partial vertical propagation paths (reduces effective floor attenuation) |
| Cable basket | Cat6, power cables in passage ceiling | 1-3 dB additional (minor) |

### 4.2 Measured Attenuation Values (868 MHz, Published Literature)

| Material | Typical Thickness | Attenuation (dB) | Source |
|----------|-------------------|-------------------|--------|
| Reinforced concrete floor slab | 150-250mm | 18-25 | ITU-R P.2040, field studies |
| Reinforced concrete (1960s-era, denser) | 200-300mm | 22-28 | Field studies in period buildings |
| Interior brick/masonry wall | 110mm | 3-5 | ITU-R P.2040 |
| Exterior masonry/stone wall | 200mm+ | 10-15 | ITU-R P.2040 |
| Steel fire door | 45mm | 15-18 | Field measurements |
| Plumbing riser (metal piping cluster) | Varies | 5-10 | Empirical |
| Cable basket / cable tray | Varies | 1-3 | Empirical |

**Design attenuation per floor:** 23 dB (conservative mid-range for 1960s reinforced concrete)

### 4.3 Link Budget Analysis

**LoRaWAN link budget (EU868, SF7 to SF12):**

| Parameter | SF7 | SF10 | SF12 |
|-----------|-----|------|------|
| TX power (node) | +14 dBm | +14 dBm | +14 dBm |
| Receiver sensitivity (gateway) | -124 dBm | -132 dBm | -137 dBm |
| **Maximum path loss** | **138 dB** | **146 dB** | **151 dB** |

**Propagation model for vertical penetration:**

```
Total path loss = (N × 23 dB) + 35 dB + fading margin

Where:
  N = number of concrete floors penetrated
  23 dB = attenuation per floor (1960s reinforced concrete)
  35 dB = horizontal path loss on same floor (~15m at 868 MHz, including walls and corridor)
  Fading margin = 10 dB (accounts for multipath, temporal variation, and material inconsistency)
```

| Floors Penetrated | Floor Loss | Horizontal + Margin | Total Path Loss | Available at SF7 | Available at SF12 | Status |
|-------------------|------------|---------------------|-----------------|-------------------|--------------------| -------|
| 0 (same floor) | 0 dB | 45 dB | 45 dB | 93 dB margin | 106 dB margin | Excellent |
| 1 | 23 dB | 45 dB | 68 dB | 70 dB margin | 83 dB margin | Excellent |
| 2 | 46 dB | 45 dB | 91 dB | 47 dB margin | 60 dB margin | Good |
| 3 | 69 dB | 45 dB | 114 dB | 24 dB margin | 37 dB margin | Good |
| 4 | 92 dB | 45 dB | 137 dB | 1 dB margin | 14 dB margin | SF7 marginal, SF10+ OK |
| 5 | 115 dB | 45 dB | 160 dB | — | — | Unreliable |

**Practical reliable range per gateway: 3 floors in each direction (above and below), 4 floors with ADR shift to SF10+.**

This model is conservative — real-world performance may be slightly better due to vertical propagation through stairwells and lift shafts. The pilot will validate these figures with actual measurements.

---

## 5. Gateway Placement — 4-Gateway Design

### 5.1 Placement Strategy

4 gateways deployed from day one, evenly distributed across 12 floors:

| Gateway | Location | Primary Coverage | Overlap Zones | Backhaul |
|---------|----------|------------------|---------------|----------|
| GW-1 | Floor 3 | Floors 1-5 | Floors 5-6 (with GW-2) | PoE to floor UniFi switch |
| GW-2 | Floor 6 | Floors 4-8 | Floors 4-5 (with GW-1), 8-9 (with GW-3) | PoE to floor UniFi switch |
| GW-3 | Floor 9 | Floors 7-11 | Floors 7-8 (with GW-2), 11-12 (with GW-4) | PoE to floor UniFi switch |
| GW-4 | Floor 12 | Floors 10-12 | Floors 10-11 (with GW-3) | PoE to floor UniFi switch |

### 5.2 Coverage Map

| Floor | GW-1 (F3) | GW-2 (F6) | GW-3 (F9) | GW-4 (F12) | Gateways in Range | Redundancy |
|-------|-----------|-----------|-----------|------------|-------------------|------------|
| 1 | 2↓ | 5↓ | 8↓ | 11↓ | 1 (GW-1) | Single |
| 2 | 1↓ | 4↓ | 7↓ | 10↓ | 1-2 (GW-1, GW-2 marginal) | Single+ |
| 3 | **0** | 3↓ | 6↓ | 9↓ | 1-2 (GW-1, GW-2) | Dual |
| 4 | 1↑ | 2↓ | 5↓ | 8↓ | 2 (GW-1, GW-2) | Dual |
| 5 | 2↑ | 1↓ | 4↓ | 7↓ | 2 (GW-1, GW-2) | Dual |
| 6 | 3↑ | **0** | 3↓ | 6↓ | 2-3 (GW-1 marginal, GW-2, GW-3) | Dual+ |
| 7 | 4↑ | 1↑ | 2↓ | 5↓ | 2 (GW-2, GW-3) | Dual |
| 8 | 5↑ | 2↑ | 1↓ | 4↓ | 2 (GW-2, GW-3) | Dual |
| 9 | 6↑ | 3↑ | **0** | 3↓ | 2-3 (GW-2 marginal, GW-3, GW-4) | Dual+ |
| 10 | 7↑ | 4↑ | 1↑ | 2↓ | 2 (GW-3, GW-4) | Dual |
| 11 | 8↑ | 5↑ | 2↑ | 1↓ | 2 (GW-3, GW-4) | Dual |
| 12 | 9↑ | 6↑ | 3↑ | **0** | 1-2 (GW-3 marginal, GW-4) | Single+ |

**Key improvements over 3-gateway design:**
- Every floor has at least 1 gateway within 3 floors (reliable at SF7)
- 8 of 12 floors have dual-gateway coverage (floors 3-11)
- Maximum gateway distance is 3 floors (floors 1 and 12)
- No floor relies on a 4+ floor penetration path at SF7
- If any single gateway fails, no floor loses all coverage — adjacent gateways cover the gap without needing SF12

### 5.3 Gateway Hardware

| Item | Specification |
|------|---------------|
| Model | RAK7268V2 (WisGate Edge Lite 2) |
| Channels | 8 (EU868 channel plan) |
| Backhaul | Ethernet (PoE 802.3af) |
| ChirpStack | Native support (Semtech UDP Packet Forwarder or MQTT) |
| Power | PoE from floor UniFi switch (single Cat6 cable) |
| Mounting | Indoor, wall-mount or shelf in comms closet / switch room |
| Quantity | 4 |
| Est. unit cost | R7,000-R9,000 |
| Est. total | R28,000-R36,000 |

### 5.4 Rationale for 4 Gateways from Day One

At R28-36K total for 4 gateways, this represents approximately 2-3% of the total project cost (meters + platform). The benefits of deploying 4 from the start:

- No floor relies on marginal 4+ floor penetration at SF7
- Dual coverage on most floors enables immediate self-healing if any gateway fails
- ADR keeps all nodes at SF7 (shortest ToA, longest battery life, lowest collision probability)
- Eliminates the cost and disruption of retrofitting a 4th gateway later
- Provides confidence to the client that the network is robust from day one

---

## 6. Network Capacity Analysis

### 6.1 Traffic Model

| Parameter | At 5-min Interval | At 10-min Interval | At 15-min Interval |
|-----------|-------------------|--------------------|--------------------|
| Nodes | 588 | 588 | 588 |
| Uplinks per hour (total) | 7,056 | 3,528 | 2,352 |
| Uplinks per hour per channel (8 ch) | 882 | 441 | 294 |
| ToA per uplink (SF7, 10 bytes) | 61 ms | 61 ms | 61 ms |
| Channel utilisation per channel | 1.5% | 0.75% | 0.5% |
| Duty cycle per device | 0.02% | 0.01% | 0.007% |

### 6.2 Collision Probability (Pure ALOHA Model)

The ALOHA collision model gives the probability of a successful transmission as:

```
P(success) = e^(-2G)

Where G = channel utilisation (fraction of time the channel is occupied)
```

| Interval | G per channel | P(success) | Expected PDR (with 4-GW diversity) |
|----------|---------------|------------|-------------------------------------|
| 5 min | 0.015 | 97.0% | >99% |
| 10 min | 0.0075 | 98.5% | >99.5% |
| 15 min | 0.005 | 99.0% | >99.7% |

With 4 gateways providing spatial diversity, a node's uplink may be received by 2-3 gateways simultaneously. ChirpStack deduplicates and selects the best packet. The effective packet delivery ratio with multi-gateway diversity exceeds 99% even at 5-minute intervals.

### 6.3 Capacity Verdict

**No capacity concern at any of the proposed intervals.** The network can comfortably handle 588 nodes at 5-minute intervals on 8 channels across 4 gateways. There is significant headroom for future expansion (e.g., adding the upper 15 floors if Mosaic Group extends the scope).

---

## 7. Node Design and Battery Strategy

### 7.1 Node Architecture (Precept Custom — Rev 1.0)

Since all 588 nodes are Precept-designed (reading meter pulse output), the node design is unified:

| Component | Specification | Purpose |
|-----------|---------------|---------|
| MCU + LoRa | RAK3172 (STM32WLE5 + SX1262) | LoRaWAN Class A, EU868, configurable SF/TX |
| Pulse input | Dry contact (reed switch) from meter | Counts pulses, calculates cumulative volume |
| Battery | 1× ER34615 (D-cell Li-SOCl2, 3.6V, 19Ah) — dual holder on PCB for optional 2nd cell | 19Ah default, 38Ah with dual cells |
| H-bridge driver | DRV8837 or equivalent | Valve-ready (Phase 2) |
| Valve connectors | 2× JST-XH 2-pin | 12V input + motor output |
| Feedback input | Optocoupler | Valve position (Phase 2) |
| Antenna | PCB trace (internal) or SMA whip (external option) | Tuned for 868 MHz |
| Enclosure | ABS IP54, ~100×70×40mm | Passage ceiling mount |

### 7.2 Battery Life Analysis

**Single ER34615: 19Ah at 3.6V**
**Dual ER34615 in parallel: 2 × 19Ah = 38Ah total at 3.6V**

```
Per uplink cycle (SF7, 10-byte payload):
  TX:     40 mA × 61 ms    = 0.68 µAh
  RX1:     5 mA × 100 ms   = 0.14 µAh
  RX2:     5 mA × 100 ms   = 0.14 µAh
  Processing: 10 mA × 5 ms = 0.014 µAh
  Total per cycle:          ≈ 0.97 µAh

Sleep current (RAK3172): 1.5 µA continuous = 36 µAh/day

Pulse counting (interrupt-driven): ~0.1 µA average
```

| Interval | Cycles/Day | Active µAh/Day | Sleep µAh/Day | Total µAh/Day | Total mAh/Day |
|----------|------------|----------------|---------------|----------------|----------------|
| 5 min | 288 | 279 | 36 | 315 | 0.315 |
| 10 min | 144 | 140 | 36 | 176 | 0.176 |
| 15 min | 96 | 93 | 36 | 129 | 0.129 |

**Note:** At 5-minute intervals, active current (TX/RX) accounts for ~89% of the power budget. Increasing the interval from 5 to 15 minutes reduces daily consumption by 2.4×, roughly doubling battery life. The uplink interval is a meaningful design choice for battery longevity.

**Battery life projections (single ER34615, 19,000 mAh):**

Self-discharge for Li-SOCl2 is ~1%/year = 190 mAh/year.

| Interval | Active mAh/Year | Self-Discharge mAh/Year | Total mAh/Year | Theoretical Life | Practical (÷10 derating) |
|----------|-----------------|-------------------------|-----------------|-------------------|--------------------------|
| 5 min | 115 | 190 | 305 | 62 years | **~6 years** |
| 10 min | 64 | 190 | 254 | 75 years | **~7.5 years** |
| 15 min | 47 | 190 | 237 | 80 years | **~8 years** |

**Battery life projections (dual ER34615, 38,000 mAh):**

| Interval | Total mAh/Year | Theoretical Life | Practical (÷10 derating) |
|----------|----------------|-------------------|--------------------------|
| 5 min | 495 | 77 years | **~8 years** |
| 10 min | 254 | 150 years | **~15 years** |
| 15 min | 427 | 89 years | **~9 years** |

The ÷10 derating factor is conservative and accounts for: temperature variation, end-of-life voltage sag, manufacturing variance, passivation effects in Li-SOCl2 cells, and engineering margin. Real-world performance in a stable indoor environment (passage ceiling) is likely better than these figures.

**Conclusion:** A single ER34615 at 5-minute intervals achieves ~6 years practical life. Dual cells extend this to ~8+ years. At 10-minute intervals, even a single cell approaches 8 years. The dual-cell option provides comfortable margin for the 10-year project horizon, especially at 10-15 minute intervals.

**Design decision:** Include dual battery holders on the PCB (low cost, adds flexibility), but deploy with a single cell initially. If field measurements show higher-than-modelled consumption, the second cell can be added without hardware changes.

### 7.3 Battery Replacement Strategy

With single-cell deployment at 5-minute intervals (~6 year life), battery replacement is an infrequent event:

| Parameter | Single Cell (5-min) | Dual Cell (10-min) |
|-----------|--------------------|--------------------|
| Replacement interval | ~6 years | ~15 years (exceeds project horizon) |
| Nodes per cycle | 588 | 588 |
| Battery cost per node | ~R100 (1× ER34615) | ~R200 (2× ER34615) |
| Labour (Sumir + assistant, 5 min/node) | ~R50/node | ~R50/node |
| Total per replacement cycle | ~R88,000 | ~R147,000 |
| Annualised battery cost | ~R15,000/year | Negligible (beyond project horizon) |

Even with single-cell deployment and a conservative 6-year replacement cycle, the annualised cost is ~R15K/year — negligible relative to the R1.2M/month water bill.

### 7.4 Battery Optimisation Options (Future)

If longer battery life is desired in later revisions:

| Option | Impact | Complexity |
|--------|--------|------------|
| Increase uplink interval to 10-15 min | Extends single-cell life to ~7.5-8 years | Firmware change only |
| Deploy dual cells from day one | Extends life to ~8-15 years depending on interval | Hardware provision on Rev 1.0 PCB |
| Lower sleep current MCU (e.g., STM32WL55 bare chip, ~0.3 µA stop mode) | Extends life further by reducing sleep drain | Requires custom PCB redesign (no module) |
| Switch to Class C with external power (Phase 2, when valve power installed) | Unlimited battery life (mains powered) | Requires 12V DC infrastructure per floor |

For Rev 1.0, design the PCB with dual battery holders. Deploy with a single cell initially. Add the second cell if field measurements warrant it or if valve power infrastructure (Phase 2) is delayed beyond the first battery cycle.

---

## 8. Codec and Payload Specification

### 8.1 Ownership and Control

Because Precept designs the node firmware AND the ChirpStack codec, the entire data pipeline is under Precept's control:

```
[Precept Node Firmware]  →  encodes payload
         |
         | LoRaWAN uplink (binary payload)
         v
[ChirpStack Network Server]  →  decrypts LoRaWAN frame
         |
         | passes payload to codec
         v
[Precept JavaScript Codec]  →  decodes binary → JSON
         |
         | MQTT / gRPC
         v
[Water Monitoring Platform]  →  stores, displays, alerts
```

No dependency on Precision Meters for codec, payload format, or data interpretation. The meter provides pulses; Precept handles everything else.

### 8.2 Recommended Payload Structure

A compact binary payload minimises Time-on-Air and collision probability:

**Uplink payload (10 bytes):**

| Byte | Field | Type | Description |
|------|-------|------|-------------|
| 0-3 | Cumulative pulse count | uint32 (big-endian) | Total pulses since installation |
| 4-5 | Instantaneous flow | uint16 (big-endian) | Pulses in last measurement period (for flow rate calculation) |
| 6 | Battery voltage | uint8 | (raw_value + 200) / 100 = voltage (e.g., 160 → 3.60V) |
| 7 | Status flags | uint8 | Bit 0: leak alarm, Bit 1: tamper, Bit 2: valve open, Bit 3: valve closed, Bit 4: low battery, Bit 5-7: reserved |
| 8 | Temperature | int8 | Signed, degrees Celsius (MCU internal sensor) |
| 9 | Reserved | uint8 | Future use (e.g., firmware version, error code) |

**Time-on-Air at SF7/125kHz:** ~61 ms for 10 bytes (including LoRaWAN overhead).

### 8.3 Downlink Commands (Class A)

| FPort | Command | Payload | Description |
|-------|---------|---------|-------------|
| 1 | Set uplink interval | 1 byte (minutes: 5, 10, 15, 30, 60) | Change reporting frequency |
| 2 | Valve open | 0 bytes | Actuate valve to open position |
| 3 | Valve close | 0 bytes | Actuate valve to close position |
| 4 | Reset pulse counter | 0 bytes | Reset cumulative count (maintenance) |
| 5 | Request status | 0 bytes | Force immediate uplink with full status |

### 8.4 ChirpStack Device Profile

| Parameter | Value |
|-----------|-------|
| Region | EU868 |
| MAC version | LoRaWAN 1.0.3 |
| Regional parameters revision | RP001 Regional Parameters 1.0.3 Revision A |
| ADR algorithm | Default (LoRa only) |
| Expected uplink interval | 300s (5 min) — configurable |
| Device-status request frequency | Every 24 uplinks |
| Codec | JavaScript (custom Precept decoder/encoder) |

---

## 9. Reliability and Redundancy

### 9.1 Single Gateway Failure Scenario

With 4 gateways at 3-floor spacing, the loss of any single gateway is tolerable:

| Failed Gateway | Affected Floors | Coverage from Remaining | ADR Adjustment Needed |
|----------------|-----------------|------------------------|-----------------------|
| GW-1 (F3) | Floors 1-2 lack primary | GW-2 (F6): 3-5 floors away → SF10+ | Yes — floors 1-2 shift to SF10 |
| GW-2 (F6) | Floors 4-5 and 7-8 | GW-1 + GW-3 cover with 2-3 floor gap | Minimal — most floors still within 3 of another GW |
| GW-3 (F9) | Floors 7-8 and 10-11 | GW-2 + GW-4 cover with 2-3 floor gap | Minimal |
| GW-4 (F12) | Floor 12 lacks primary | GW-3 (F9): 3 floors away → still SF7 | None |

**Worst case:** GW-1 failure leaves floors 1-2 dependent on GW-2 at 3-5 floors distance. ChirpStack ADR automatically increases SF to maintain connectivity. Battery life on affected floors decreases slightly due to longer ToA, but service continues uninterrupted.

### 9.2 Node Failure Modes

| Failure | Detection | Recovery |
|---------|-----------|----------|
| Battery depletion | Low battery flag in status byte; voltage monitoring in dashboard | Scheduled replacement during maintenance window |
| Node hardware failure | Missing uplinks (ChirpStack frame counter stops) | Dashboard alert after N missed uplinks; physical replacement |
| Pulse wire disconnect | Zero flow detected while bulk meter shows consumption | Dashboard reconciliation alarm; physical inspection |
| Antenna damage | RSSI/SNR degradation visible in ChirpStack | Dashboard alert; physical replacement |

### 9.3 Data Resilience

| Layer | Mechanism |
|-------|-----------|
| Meter internal logger | Ultrasonic meters log hourly, daily, monthly values internally — data survives total network outage |
| Node pulse counter | Cumulative count stored in non-volatile memory (STM32 flash) — survives node reboot |
| LoRaWAN confirmed uplinks | Optional: use confirmed uplinks for critical data (at cost of additional airtime) |
| ChirpStack deduplication | Multi-gateway reception ensures packet delivery even if one gateway misses a frame |
| Platform database | SQLite with WAL mode, regular backups |

---

## 10. Risk Register

| # | Risk | Likelihood | Impact | Mitigation |
|---|------|------------|--------|------------|
| 1 | **ICASA type approval not held for RAK3172 or RAK7268V2** | Medium | High — cannot legally deploy | Verify before ordering. Budget 6-12 weeks if application needed. |
| 2 | **Actual floor attenuation exceeds 23 dB estimate** | Low-Medium | Medium — some floors may need SF10+ | 4 gateways provide margin. Pilot RF survey will validate. Can add 5th gateway if needed. |
| 3 | **Metal riser proximity degrades node antenna performance** | Medium | Medium — reduced range for nodes near riser | Test during pilot. If problematic, use external SMA antenna option (short whip, positioned away from pipes). |
| 4 | **Precision Meters pulse output incompatible or undocumented** | Low | High — node cannot read meter | Confirm pulse specification (reed switch or wired, litres per pulse) before PCB design. Request from Bradley. |
| 5 | **WiFi interference from 13-15 APs per floor bleeds into sub-GHz** | Very Low | Low — different frequency bands | 868 MHz is far from 2.4/5 GHz. No practical risk of interference. |
| 6 | **Concrete stairwell/lift shaft propagation creates unexpected multipath** | Medium | Low — may improve or degrade signal | ADR handles automatically. Multi-gateway diversity provides resilience. |
| 7 | **Battery life shorter than 6 years in practice** | Low | Medium — brings forward replacement cycle | Dual-cell holder on PCB allows adding second cell. Uplink interval adjustable via downlink. Monitor voltage via dashboard. |
| 8 | **ChirpStack ADR oscillation ("flapping") in marginal zones** | Low | Low — affects battery life, not reliability | Set conservative ADR installation margin (10 dB). Fix SF for pilot nodes to validate. |

---

## 11. Recommended Test Equipment

### 11.1 Essential — Required for Pilot and Deployment

| Equipment | Purpose | Est. Cost (ZAR) | Recommended Model |
|-----------|---------|------------------|-------------------|
| **LoRa field tester** | Verify gateway coverage per floor, measure RSSI/SNR, validate link budget across all 12 floors before and during deployment | R3,000-R6,000 | **Adeunis FTD2 (Field Test Device)** — dedicated LoRaWAN 868 MHz tester with built-in GPS, OLED display, configurable SF, real-time RSSI/SNR readout. Sends test uplinks and displays gateway response. Industry standard for LoRaWAN site surveys. Alternative: **RAK10701 Field Tester** (R2,500-R4,000) — lower cost, ChirpStack compatible, good for basic coverage mapping. |
| **Digital multimeter** | Battery voltage verification, pulse signal testing, continuity checks on node wiring | R500-R1,500 | **Fluke 117** or equivalent true-RMS DMM. Needed for verifying battery voltage under load, testing pulse output from meters, and general troubleshooting. |
| **Oscilloscope (portable)** | Verify meter pulse signal waveform (pulse width, voltage level, bounce/debounce), validate node pulse counting accuracy, debug firmware timing | R3,000-R8,000 | **Rigol DS1054Z** (4-channel, 50 MHz, upgradeable to 100 MHz) — excellent value. Needed to characterise the Precision Meters pulse output waveform before finalising firmware debounce logic. Alternative: **Hantek 2D72** (handheld, R2,000-R3,000) — portable for field use but lower bandwidth. |
| **SWR / antenna analyser** | Verify node PCB antenna tuning at 868 MHz, check for detuning caused by proximity to metal pipes and building structure | R2,000-R5,000 | **NanoVNA V2 Plus4** — covers 50 kHz to 4.4 GHz, portable, USB-C, open-source software. Measures SWR, impedance, Smith chart, return loss. Essential for validating that the PCB trace antenna is properly tuned at 868 MHz in the actual installation environment (near metal riser pipes). |

### 11.2 Highly Recommended — Improves Confidence and Troubleshooting

| Equipment | Purpose | Est. Cost (ZAR) | Recommended Model |
|-----------|---------|------------------|-------------------|
| **Spectrum analyser (portable)** | Survey the 868 MHz band at the building for existing RF interference before deployment. Identify any co-channel users. Verify node TX power and spectral purity. | R8,000-R25,000 | **TinySA Ultra** (R3,000-R5,000) — covers 100 kHz to 5.3 GHz, portable, adequate for ISM band survey. For more rigorous work: **RF Explorer 6G Combo** (R8,000-R12,000) — better dynamic range, PC software for logging. |
| **LoRaWAN protocol analyser / sniffer** | Capture raw LoRa frames off-air for debugging join issues, payload verification, and ADR behaviour analysis | R2,000-R4,000 | **Heltec LoRa 32 V3** flashed with **LoRa sniffer firmware** + laptop running **Wireshark with LoRaWAN dissector**. Alternative: use a spare RAK3172 dev board in promiscuous receive mode. This is a development tool for debugging, not a deployment tool. |
| **Thermal camera** | Identify battery heating issues, detect poor electrical connections, verify node is not overheating in enclosed passage ceiling | R4,000-R10,000 | **FLIR C5** (R8,000-R10,000) or **UNI-T UTi260B** (R4,000-R6,000). Also useful for detecting water leaks (temperature differential in walls/ceilings) — dual-purpose with the water monitoring mission. |
| **Cable tester** | Verify Cat6 patch cables for gateway backhaul, check for wiring faults | R500-R1,500 | **Fluke MicroScanner2** or equivalent. Basic continuity and wiremap testing for the 4 gateway Ethernet connections. |

### 11.3 Nice to Have — For Rigorous RF Characterisation

| Equipment | Purpose | Est. Cost (ZAR) | Recommended Model |
|-----------|---------|------------------|-------------------|
| **Calibrated RF attenuator set** | Test gateway receiver sensitivity at known signal levels; simulate various floor attenuation scenarios in a lab before field deployment | R1,000-R3,000 | **SMA attenuator set** (10, 20, 30, 40, 50 dB steps). Used with a LoRa test transmitter to verify gateway reception at expected signal levels. |
| **Signal generator (868 MHz)** | Generate known-power 868 MHz signals for calibrating test equipment and validating antenna performance | R5,000-R15,000 | Typically not needed if NanoVNA is available. Only for advanced RF lab work. |

### 11.4 Recommended Minimum Kit for This Project

For a project of this scale (588 nodes, R1.3M+ in meters), the following minimum test kit is recommended:

| Item | Est. Cost (ZAR) |
|------|------------------|
| Adeunis FTD2 or RAK10701 field tester | R3,000-R6,000 |
| Fluke 117 multimeter | R1,000 |
| Rigol DS1054Z oscilloscope | R5,000 |
| NanoVNA V2 Plus4 antenna analyser | R3,000 |
| TinySA Ultra spectrum analyser | R4,000 |
| Cable tester | R500 |
| **Total test equipment investment** | **R16,500-R19,500** |

This investment is approximately 1.5% of the meter hardware cost and provides the tools needed to:
1. Survey the RF environment before deployment
2. Validate antenna tuning in the actual installation environment
3. Map gateway coverage across all 12 floors
4. Characterise meter pulse output for firmware development
5. Debug and troubleshoot during pilot and rollout
6. Provide measured evidence to the client that the network is performing to specification

---

## 12. Conclusions

### 12.1 Will It Work?

**Yes.** The proposed LoRaWAN solution at 868 MHz is technically sound for this building. Specifically:

| Question | Answer |
|----------|--------|
| Will 868 MHz signals penetrate the concrete floors? | Yes — 3 floors reliably at SF7, 4 floors at SF10. 4 gateways cover all 12 floors with margin. |
| Is it legal under ICASA? | Yes — 868 MHz is licence-exempt in SA. Per-device duty cycle at 5-min intervals is 0.02% (limit: 1%). Power at 14 dBm ERP is within 25 mW limit. |
| Can 588 nodes share the network without collisions? | Yes — channel utilisation at 5-min intervals is 1.5% per channel. With 4-gateway diversity, PDR exceeds 99%. |
| Is 4 gateways sufficient? | Yes — provides dual coverage on most floors, single coverage on floors 1-2 and 12, and self-healing if any gateway fails. |
| Is the battery life acceptable? | Yes — single D-cell Li-SOCl2 provides ~6 years at 5-minute intervals. Dual cells extend to 8+ years. Well within acceptable range for the 10-year project horizon. |
| Can Precept control the uplink interval? | Yes — own firmware on RAK3172, fully configurable (5/10/15 min). No dependency on Precision Meters radio. |
| Can Precept write the codec? | Yes — own payload specification, own ChirpStack JavaScript codec. Full control of the data pipeline. |

### 12.2 What Could Go Wrong?

The primary risks, in order of likelihood and impact:

1. **ICASA type approval gap** — If RAK3172 or RAK7268V2 lacks ICASA TA, deployment is delayed 6-12 weeks. Verify immediately.
2. **Metal riser antenna detuning** — Node antennas may underperform near metal pipes. Mitigated by NanoVNA testing during pilot and external antenna option.
3. **Floor attenuation exceeds model** — Some floors may have unexpected construction differences. Mitigated by 4-gateway design and ADR.
4. **Pulse output specification unknown** — Need to confirm reed switch type, pulse value (litres/pulse), and voltage levels from Precision Meters before PCB design.

### 12.3 Overall Assessment

| Aspect | Rating | Notes |
|--------|--------|-------|
| Protocol choice (LoRaWAN 868 MHz) | **Correct** | Only viable wireless protocol for this environment |
| Regulatory compliance (ICASA) | **Compliant** (subject to type approval verification) | EU868 channel plan, duty cycle, power limits all met |
| RF propagation model | **Sound** | Conservative estimates, validated by published literature |
| Gateway design (4 units) | **Robust** | Dual coverage on most floors, self-healing capability |
| Network capacity | **No concern** | 588 nodes at 5-min easily within limits |
| Node design (Precept custom) | **Correct** | Full control of interval, payload, codec, valve-readiness |
| Battery strategy (single D-cell, dual option) | **Good** | ~6 years at 5-min interval; dual cells or longer interval extends to 8-15 years |
| Test equipment plan | **Adequate** | ~R17K investment covers survey, validation, and troubleshooting |

---

## 13. Action Items

| # | Action | Priority | Owner | Status |
|---|--------|----------|-------|--------|
| 1 | Verify ICASA type approval for RAK3172 module | Critical | Jason | Pending |
| 2 | Verify ICASA type approval for RAK7268V2 gateway | Critical | Jason | Pending |
| 3 | Confirm Precision Meters pulse output specification (reed/wired, litres/pulse, voltage) | Critical | Jason (ask Bradley) | Pending |
| 4 | Confirm Precision Meters SANS 1529-1:2019 compliance certificate | High | Jason (ask Bradley) | Pending |
| 5 | Procure NanoVNA V2 Plus4 for antenna validation | High | Jason | Pending |
| 6 | Procure LoRa field tester (Adeunis FTD2 or RAK10701) | High | Jason | Pending |
| 7 | Procure Rigol DS1054Z for pulse characterisation | High | Jason | Pending |
| 8 | Design Rev 1.0 PCB (RAK3172 + dual D-cell + valve-ready) | High | Jason | Pending |
| 9 | Develop Precept payload specification (finalise byte layout) | Medium | Jason | Pending |
| 10 | Develop ChirpStack JavaScript codec | Medium | Jason | Pending |
| 11 | RF site survey during pilot (using field tester) | Medium | Jason | Pending (pilot phase) |

---

## 14. References

### Regulatory
- ICASA Radio Frequency Spectrum Regulations 2015, GG 38641
- ICASA Annexure B Amendment 2021, GG 45690
- CEPT/ERC/REC 70-03 — Short Range Devices
- ETSI EN 300 220-1 — Non-specific SRD technical standard
- Electronic Communications Act No. 36 of 2005, Section 35 (Type Approval)

### RF Propagation
- ITU-R P.2040 — Effects of building materials and structures on radiowave propagation above about 100 MHz
- ITU-R P.1238 — Propagation data and prediction methods for indoor radio communication systems

### LoRaWAN
- LoRa Alliance — LoRaWAN 1.0.3 Specification
- LoRa Alliance — RP001 Regional Parameters (EU863-870)
- Semtech — Predicting LoRaWAN Capacity
- ChirpStack v4 Documentation — https://www.chirpstack.io/docs/

### Project Documents
- `docs/planning/internal/Research/03-solution-design-research.md` — Solution design
- `docs/planning/internal/Research/Smart Metering Architecture for High-Rise.md` — High-rise AMI analysis
- `docs/planning/site-visits/2026-01-29-city-life-site-visit-report.md` — Site visit report
- `docs/planning/correspondence/2026-02-03-garth-meter-questions-reply.md` — Precision Meters confirmations
