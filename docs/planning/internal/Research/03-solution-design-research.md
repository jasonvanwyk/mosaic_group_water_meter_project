# Solution Design Research: 477 Anton Lembede Street

**Project:** MGW - Mosaic Group Water Meter Monitoring
**Document:** 03-solution-design-research
**Date:** 2026-01-30
**Author:** Jason van Wyk, Precept Systems
**Scope:** First 12 floors, 576 units (48/floor), City Life

---

## Table of Contents

1. [LoRaWAN Feasibility in High-Rise Concrete](#1-lorawan-feasibility-in-high-rise-concrete)
2. [Meter Selection (Precision Meters)](#2-meter-selection-precision-meters)
3. [Motorised Ball Valve Research](#3-motorised-ball-valve-research)
4. [Communications Architecture Options](#4-communications-architecture-options)
5. [Node Design Considerations](#5-node-design-considerations)
6. [Pilot Design](#6-pilot-design)

---

## 1. LoRaWAN Feasibility in High-Rise Concrete

### 1.1 Signal Propagation Through Reinforced Concrete

The building at 477 Anton Lembede Street is approximately 60 years old, constructed from reinforced concrete, brick, and steel. LoRaWAN operates in the sub-GHz ISM band -- South Africa uses 868 MHz (ICASA-regulated, aligned with EU863-870 channel plan). Sub-GHz signals have significantly better penetration through dense building materials compared to 2.4 GHz (WiFi).

**Measured Attenuation Values (868 MHz band, from published literature and field data):**

| Material | Typical Thickness | Attenuation per Layer (dB) | Source |
|----------|-------------------|---------------------------|--------|
| Reinforced concrete floor slab | 150-250mm | 18-25 dB | ITU-R P.2040, field studies |
| Reinforced concrete floor slab (older, denser) | 200-300mm | 22-28 dB | Field studies in 1960s-era buildings |
| Interior brick/masonry wall | 110mm | 3-5 dB | ITU-R P.2040 |
| Exterior masonry/stone wall | 200mm+ | 10-15 dB | ITU-R P.2040 |
| Steel fire door | 45mm | 15-18 dB | Field measurements |
| Plumbing riser (metal piping cluster) | varies | 5-10 dB additional | Empirical |
| Cable basket / cable tray | varies | 1-3 dB | Empirical |

**Key constraint:** The existing research document (see `Smart Metering Architecture for High-Rise.md`) correctly identifies that a signal attempting to penetrate from ground to floor 12 through floor slabs alone would encounter approximately 12 x 23 dB = 276 dB of cumulative attenuation. The maximum LoRaWAN link budget is approximately 155-157 dB (at SF12, 14 dBm TX power, -137 dBm receiver sensitivity). This means a single gateway cannot cover 12 floors through direct vertical floor penetration. The signal can realistically penetrate 5-7 reinforced concrete floors under favourable conditions (approximately 5 x 23 = 115 dB through floors, leaving margin for path loss and fading within the ~157 dB link budget).

**Practical floor coverage per gateway:** 3-5 floors in each direction (above and below) through reinforced concrete, assuming nodes are in the corridor/passage (line of sight to corridor, not inside sealed apartments). This is achievable because:
- Nodes are mounted in the passage ceiling, not inside enclosed units
- The passage provides a relatively open corridor for horizontal signal propagation on the same floor
- Vertical penetration only needs to pass through floor slabs, not through walls

### 1.2 Gateway Placement Strategy for 12 Floors

**Recommended: 3 gateways for 12 floors (with rooftop as optional 4th for redundancy)**

| Gateway | Location | Floors Covered (Primary) | Floors Covered (Backup) | Backhaul |
|---------|----------|--------------------------|------------------------|----------|
| GW-1 | Floor 3 comms closet/switch room | Floors 1-6 | Floors 7-8 | PoE to UniFi switch |
| GW-2 | Floor 8 comms closet/switch room | Floors 5-12 | Floors 3-4 | PoE to UniFi switch |
| GW-3 | Floor 12 or Rooftop | Floors 9-12 + riser top | Floors 7-8 | PoE to UniFi switch or rooftop ethernet |

**Rationale for 3 gateways (not 5):**
- The existing research document recommended 5 gateways for a 15-floor building (all 720 units). With only 12 floors in scope, 3 gateways provide adequate coverage.
- Each gateway covers ~4-5 floors primary, with 2-3 floors overlap for redundancy.
- Nodes are in the passage (not inside sealed apartments), improving signal path.
- ChirpStack deduplication handles multi-gateway reception -- if a node on floor 6 is heard by both GW-1 and GW-2, ChirpStack selects the best packet.
- The overlap zone (floors 5-6 covered by both GW-1 and GW-2) provides self-healing redundancy.

**Why not fewer gateways:**
- 2 gateways would leave floors 5-8 in a marginal zone where both gateways are at the edge of their range through 4+ concrete floors. Risk of packet loss too high.
- 1 gateway is physically impossible for 12 concrete floors.

**Why not more:**
- 4 or 5 gateways would work but is over-provisioned for 12 floors. Each gateway costs approximately R8,000-R15,000. Adding a 4th on the rooftop is sensible only if the riser bulk meters extend above floor 12.
- Recommend starting the pilot with 1 gateway and adding the second during the first full-floor rollout. The third is added when deploying beyond floor 8.

**Gateway hardware options:**

| Gateway | Type | Channels | Backhaul | Price (est. ZAR) | Notes |
|---------|------|----------|----------|-------------------|-------|
| RAK7268V2 (WisGate Edge Lite 2) | Indoor | 8-channel | Ethernet + WiFi | R6,000-R8,000 | ChirpStack compatible, PoE, compact |
| RAK7289V2 (WisGate Edge Pro) | Indoor/Outdoor | 8/16-channel | Ethernet + 4G | R12,000-R18,000 | Built-in 4G failover |
| Kerlink iStation | Indoor | 8-channel | Ethernet | R10,000-R14,000 | Industrial grade, managed |
| Milesight UG65 | Indoor | 8-channel | Ethernet + WiFi + 4G | R8,000-R12,000 | ChirpStack compatible |
| DIY: RAK2287 concentrator + RPi4 | Indoor | 8-channel | Ethernet | R4,000-R6,000 | Cheapest, but less reliable |

**Recommendation:** RAK7268V2 for all three positions. It is the most cost-effective 8-channel gateway with Ethernet backhaul and direct ChirpStack support. Each floor has a UniFi managed switch with PoE, so the gateway can be powered and connected with a single Cat6 cable. No additional infrastructure required.

### 1.3 LoRa vs Alternatives for This Building

Given the existing UniFi WiFi infrastructure, the following alternatives were evaluated:

**Why LoRaWAN is still preferred despite excellent WiFi infrastructure:**

| Factor | LoRaWAN (868 MHz) | WiFi (2.4/5 GHz) | Wired Ethernet |
|--------|-------------------|-------------------|----------------|
| Building penetration | Excellent (sub-GHz) | Poor in concrete (2.4 GHz); very poor (5 GHz) | N/A (wired) |
| Power consumption | Ultra-low (~10mA TX, <1uA sleep) | High (~100-200mA TX) | N/A (mains powered) |
| Battery life (5-min interval) | 8-16 years (Li-SOCl2) | 6-18 months (AA batteries) | No battery needed |
| WiFi congestion impact | None (separate band) | Severe (13-15 APs/floor, tenants + IoT competing) | None |
| Infrastructure cost per floor | R0 (existing ethernet for gateway backhaul) | R0 (existing APs) | High (new Cat6 runs to each meter) |
| Infrastructure cost per node | ~R200-400 (LoRa module) | ~R50-100 (ESP32 WiFi) | ~R200-500 (ethernet port + cable) |
| Node cost (total) | R800-1,500 per node | R400-800 per node | R1,500-3,000 per node |
| Reliability (utility grade) | High (CSS modulation, below noise floor) | Low (shared spectrum, interference) | Very high (wired) |
| Scalability | Excellent (576+ nodes per 3 gateways) | Problematic (AP capacity limits) | Limited (switch port counts) |
| Installation disruption | Low (wireless) | Low (wireless) | Very high (cable runs to 576 locations) |
| Maintenance (battery replacement) | None for 10+ years | Every 12-18 months (576 units) | None (but cable faults) |
| Motorised valve support | Requires separate power | Requires separate power | Can supply power via PoE |
| Client constraint | Explicitly: "ethernet cabling is too much infrastructure cost" | Uses existing infrastructure | Explicitly rejected by client |

**Critical factor -- client rejection of wired infrastructure:** The site visit notes record that Mosaic Group explicitly stated "ethernet cabling is too much of an infrastructure cost." This eliminates Option C (wired Ethernet per node) from consideration. The only viable wireless options are LoRaWAN and WiFi.

**Critical factor -- WiFi congestion:** With 13-15 UniFi APs per floor (for tenant internet), the 2.4 GHz spectrum is extremely congested. Adding 48 WiFi-based IoT devices per floor (576 total) to an already saturated environment creates reliability risk. WiFi is a contention-based protocol (CSMA/CA) -- every additional device increases the probability of collision and retry, which increases power consumption and reduces battery life further.

**Critical factor -- battery life and maintenance cost:** At a 5-minute reporting interval, a WiFi-based node (ESP32) drawing ~100mA during TX and ~20mA during association would need battery replacement every 12-18 months. For 576 nodes at an estimated R150/replacement (battery + labour), that is R86,400/year in ongoing maintenance. Over 10 years: R864,000. A LoRaWAN node with Li-SOCl2 battery at SF7 can achieve 10+ years with no replacement.

**Verdict:** LoRaWAN remains the clear choice for unit meters. However, the existing Ethernet infrastructure creates an opportunity for a **hybrid approach** for the 12 bulk floor meters, which are located at fixed, accessible positions near switches and have power available. See Section 4 for the full architecture options.

### 1.4 Private LoRaWAN (ChirpStack) vs Public Network

| Factor | ChirpStack (Private) | TTN (The Things Network) | Loriot | Sigfox/NB-IoT |
|--------|---------------------|--------------------------|--------|---------------|
| Monthly cost | R0 | R0 (Fair Use limits) | R15-30/device/month | R15-45/device/month |
| Device limit | Unlimited | Fair Use: 30s airtime/device/day | Unlimited (paid) | Unlimited (paid) |
| 5-min interval feasible | Yes (own rules) | Borderline (Fair Use may limit SF12) | Yes | No (Sigfox: 140 msgs/day max) |
| Data sovereignty | Full (on-premise or own cloud) | EU-hosted cloud | Cloud | Cloud |
| Downlink capability | Full (Class A/B/C) | Limited (Class A, 10/day) | Full | Very limited |
| Remote valve control | Yes (Class C, near-instant) | Impractical (10 downlinks/day) | Yes | No |
| Customisation | Full API (gRPC, REST, MQTT) | Limited | API available | Limited |
| Self-hosted | Yes | No (community) / Yes (Enterprise) | No | No |
| Annual cost (576 nodes) | R0 + hosting | R0 (if within Fair Use) | R103,680-R207,360 | R103,680-R311,040 |
| 10-year cost (576 nodes) | ~R50,000 (hosting + power) | R0 or R500,000+ (Enterprise) | R1M-R2M | R1M-R3M |

**Verdict:** Private ChirpStack is the only option that meets all requirements:
- Zero recurring subscription cost (critical for price-sensitive client)
- Full downlink support for future motorised valve control
- Full data ownership (required for CLTM ERP integration)
- No device or message limits
- Self-hosted, aligning with the Fairfield Water reference architecture

**ChirpStack hosting:** Can be hosted on the same infrastructure as the monitoring platform (LXC container on Proxmox, or cloud VM). The existing Fairfield deployment uses this approach. ChirpStack v4 runs on a single server with PostgreSQL + Redis + MQTT broker.

---

## 2. Meter Selection (Precision Meters)

### 2.1 Unit Meters (16mm Feed) -- Precision Meters Ultrasonic 15mm

**Recommended product: Precision Meters Ultrasonic 15mm**

Based on the vendor spec sheets at `/home/jason/Projects/mosaic_group/docs/planning/internal/Vendors/`:

| Specification | Value | Notes |
|---------------|-------|-------|
| Meter size | 15mm | Fits 16mm unit feed pipe with standard BSP adaptor |
| Body length | 110mm | Compact -- fits in passage ceiling space above front door |
| Overload flow (Qs) | 2 m3/h | Adequate for single apartment |
| Permanent flow (Qp) | 1 m3/h | Typical apartment peak flow |
| Transitional flow (Qt) | 11.5 L/h (Class C) / 15 L/h (Class D) | Good low-flow sensitivity |
| Minimum flow (Qmin) | 7.5 L/h (Class C) / 10 L/h (Class D) | Detects dripping taps (~4 L/h drip may be below threshold) |
| Sensitive to | 1 L/h | Per spec sheet "sensitive and accurate in low flows, down to 1 L/h" |
| Technology | Ultrasonic (static, no moving parts) | Critical for Durban water quality (grit/sediment) |
| IP rating | IP68 | Fully submersible -- passage ceiling is not wet but provides margin |
| Installation | Any position (horizontal/vertical), no straight sections (U0D0) | Critical: pipes in passage ceiling may not allow upstream/downstream straight runs |
| Battery life | >16 years | Exceeds 10-year project horizon |
| Data logger | Hourly, daily, monthly values stored internally | Provides data resilience if comms fail |
| Standards | SANS 1529-1:2019 Class C or D | South African legal metrology compliant |
| AMR/AMI ready | wM-Bus 433/868 MHz, LoRaWAN (EU863-870), NB-IoT, NFC | Direct LoRaWAN support -- no separate pulse counter node needed |
| Alarms | Leakage, burst, backflow, empty pipe, tamper | Built-in leak detection |
| Bi-directional | Yes | Detects backflow/reverse flow |

**Critical finding: The Precision Meters ultrasonic 15mm meter has INTEGRATED LoRaWAN.** This means the meter itself can transmit directly to a ChirpStack gateway without needing a separate LoRa node/pulse counter. This simplifies the architecture significantly:

- No separate PCB/node needed per unit meter (for metering function only)
- The meter handles its own LoRaWAN join, transmission, and data encoding
- Internal data logger provides resilience
- Built-in leak detection, backflow detection, and tamper alarms are transmitted via LoRaWAN
- Battery life >16 years (meter's own battery powers both metering and LoRaWAN radio)

**Implication for node design:** If using the integrated LoRaWAN radio in the meter, a separate Precept node per unit is ONLY needed if:
1. Motorised ball valve control is required (the meter cannot control an external valve)
2. Additional sensors are needed beyond what the meter provides
3. The meter's LoRaWAN implementation is incompatible with ChirpStack (needs verification with Bradley/Precision Meters)

**ACTION ITEM:** Confirm with Bradley Cassani (Precision Meters) the following:
- Is the LoRaWAN radio module an optional add-on or standard?
- What is the price difference between base meter and LoRaWAN-enabled meter?
- What LoRaWAN device profile/codec is used? (Is there a ChirpStack device profile available?)
- What channel plan is configured? (Must be EU863-870 for South Africa)
- What is the default uplink interval? (Can it be configured to 5 minutes?)
- What data is included in the LoRaWAN uplink payload? (Total volume, flow rate, alarms?)
- Is the LoRaWAN module field-replaceable?
- Can NFC be used for local configuration/commissioning?
- What is the unit price at volumes of 50, 100, 576?

**Fallback: If LoRaWAN radio is not available or too expensive in the 15mm meter**, the meter still has a pulse output (per the spec sheet: "Ready for AMI/AMR"). In this case, a separate pulse counter node (e.g., Precept custom node with RAK3172 LoRa module) reads the meter's pulse output and transmits via LoRaWAN. The pulse output specification is not listed on the 15mm spec sheet -- confirm with Precision Meters whether it is reed switch or wired pulse, and what the pulse value is (litres per pulse).

**Alternative considered and rejected:**

| Product | Reason for Rejection |
|---------|---------------------|
| ASM Polymer Volumetric Rotary Piston (15mm) | Mechanical -- moving parts will fail on Durban water. Pulse prepared (0.5L/pulse) but no integrated LoRaWAN. Lower accuracy at low flows. Legal requirement for meter box enclosure. |
| Ultrasonic 20mm | Oversized for 16mm pipe. Same meter body (105mm length) but requires 25mm BSP adaptor -- unnecessary cost and space. |

### 2.2 Bulk Floor Meters (50mm Branch) -- Two Options

**Option A (Recommended): Precision Meters Ultrasonic 40mm (flanged)**

From the 32-50mm ultrasonic spec sheet, the closest match to a 50mm pipe is the 40mm flanged version. Note: The spec sheet header says "32mm - 50mm" but the actual data table shows 32mm, 40mm, and a second 40mm column (likely mislabelled; the third column appears to be 50mm based on the flow rates).

| Specification | 40mm (Column 2) | "40mm" (Column 3, likely 50mm) |
|---------------|-----------------|-------------------------------|
| Overload flow (Qs) | 12 m3/h | 24 m3/h |
| Permanent flow (Qp) | 6 m3/h | 12 m3/h |
| Transitional flow (Qt) | 90 L/h | 180 L/h |
| Minimum flow (Qmin) | 60 L/h | 72 L/h |
| Body length | 300mm | 255mm |

**ACTION ITEM:** Confirm with Bradley Cassani which size is appropriate for a 50mm floor branch pipe, and whether the third column in the spec sheet is indeed a 50mm meter (the header shows "40mm" twice but the flow rates double, suggesting DN50).

The ultrasonic bulk meter offers:
- Same benefits as the 15mm: no moving parts, IP68, internal data logger, LoRaWAN ready
- Built-in leak detection, burst, backflow alarms
- >16 year battery life

**Option B (Alternative): Infinity Evo Class C Woltmann 50mm**

| Specification | 50mm |
|---------------|------|
| Overload flow (Qs) | 80 m3/h |
| Permanent flow (Qp) | 40 m3/h |
| Transitional flow (Qt) | 0.600 m3/h (600 L/h) |
| Minimum flow (Qmin) | 0.240 m3/h (240 L/h) |
| Starting flow | 0.125 m3/h (125 L/h) |
| Dimensions | 200mm L x 209mm H |
| Pulse output (inductive) | 10 litres/pulse |
| Pulse output (reed switch) | 100 litres/pulse |

The Woltmann is a mechanical (turbine) meter. It has significantly higher maximum flow capacity (80 m3/h vs 24 m3/h) but much worse low-flow sensitivity (240 L/h minimum vs 72 L/h for ultrasonic). For leak detection on a floor branch, the ultrasonic meter is vastly superior because it can detect small overnight flows that indicate hidden leaks in apartments.

**The Woltmann meter does NOT have integrated LoRaWAN** -- it uses pulse output only (inductive or reed switch). This means it always requires a separate pulse counter node.

**Recommendation:** Use the Ultrasonic 50mm (or 40mm, pending confirmation) for floor bulk meters. The low-flow sensitivity is critical for leak detection, which is a primary business driver.

### 2.3 Meter Count Summary

| Meter Type | Pipe Size | Product | Quantity | Purpose |
|------------|-----------|---------|----------|---------|
| Unit meter | 15mm (on 16mm feed) | Ultrasonic 15mm | 576 | Per-apartment consumption metering |
| Floor bulk meter | 50mm | Ultrasonic 40/50mm (TBC) | 12 | Per-floor leak detection and reconciliation |
| **Total** | | | **588** | |

---

## 3. Motorised Ball Valve Research

### 3.1 Requirements

From site visit notes:
- Not required for Phase 1 but design should accommodate future addition
- Purpose: throttle or shut off water to non-paying tenants, emergency leak shutoff per unit/floor
- Power source needed (12V DC noted as likely)
- PCBs must be designed valve-ready with connectors

### 3.2 Product Specifications

For 16mm (1/2 inch) unit feed pipes, motorised ball valves are available in DN15 (1/2" BSP).

**Typical 12V DC Motorised Ball Valve Specifications (DN15/1/2"):**

| Specification | Typical Range | Notes |
|---------------|---------------|-------|
| Nominal size | DN15 (1/2" BSP) | Matches 16mm pipe with BSP adaptor |
| Voltage | 12V DC (also available: 24V DC, 24V AC, 220V AC) | 12V DC preferred for safety and PoE compatibility |
| Power consumption | 3-5W during actuation | Only draws power when opening/closing (5-10 seconds) |
| Standby power | 0W (latching type) or <0.5W (non-latching) | Latching type holds position without power |
| Actuation time | 3-10 seconds (full open to full close) | Adequate for this application |
| Working pressure | 1.0-1.6 MPa (10-16 bar) | Adequate for building reticulation |
| Working temperature | 0-80 degrees C | Adequate for cold water |
| IP rating | IP65-IP67 | Suitable for passage ceiling (dry location) |
| Body material | Brass (CW617N) or Stainless Steel 304 | Brass is standard for potable water |
| Connection | 1/2" BSP Female-Female | Standard plumbing connection |
| Feedback signal | Open/close position feedback (2-wire or 3-wire) | Allows node to confirm valve state |

**Valve types:**

| Type | Behaviour | Power Requirement | Suitability |
|------|-----------|-------------------|-------------|
| Latching (bistable) | Holds position when power removed. Requires pulse to change state. | Very low (only during state change) | Best for battery-powered nodes. Fails in current position. |
| Non-latching (spring return) | Returns to default position (normally open or normally closed) when power removed. | Continuous holding current OR spring return. | Best for safety (NO = water flows if power fails). Higher power. |
| Servo-type | Can hold any intermediate position (0-100%). | Continuous power for position hold. | Throttling. Highest power. Not recommended for battery nodes. |

**Recommendation: Normally Open (NO), latching ball valve.** Rationale:
- If power fails, the valve remains in its last state (if open, stays open -- tenants keep water)
- Only draws power during the 5-10 second actuation cycle
- Can be actuated by the node using a simple H-bridge motor driver
- Compatible with battery-powered or externally powered nodes

### 3.3 Brands and Products Available in South Africa

| Brand/Product | Type | Voltage | Size | Approx Price (ZAR) | Availability in SA |
|---------------|------|---------|------|--------------------|--------------------|
| U.S. Solid DN15 12V DC | Latching, brass | 12V DC | 1/2" BSP | R400-R600 | Import (AliExpress, Amazon) |
| Bacoeng DN15 12V DC | Non-latching (NO), brass | 12V DC | 1/2" BSP | R500-R800 | Import (Amazon, direct) |
| HSH-Flo DN15 12V DC | Latching, brass/SS | 12V DC | 1/2" BSP | R450-R700 | Import (AliExpress) |
| BKV DN15 (SA distributor) | Various | 12/24V DC | 1/2" BSP | R800-R1,200 | Local distributor (Johannesburg) |
| ACT Valve (KZN-based) | Industrial | 24V DC | 1/2"-2" | R1,500-R3,000 | Local manufacturer (Pinetown) |
| Lesira-Teq WMU integrated | Built-in to meter | 12V DC | 15-25mm | R2,500-R4,000 | SA manufacturer (Johannesburg) |

**Import vs local:** For 576+ valves, bulk import from China (via AliExpress, Alibaba, or direct manufacturer) is significantly cheaper than local supply. Typical landed cost (including shipping, customs, VAT) for a brass DN15 12V DC motorised ball valve: R350-R500 per unit at volume (100+).

**Volume pricing estimate (576 units):**

| Source | Unit Price | Total (576 units) | Lead Time |
|--------|-----------|-------------------|-----------|
| AliExpress (bulk) | R350-R450 | R201,600-R259,200 | 4-6 weeks |
| Direct China manufacturer (Alibaba) | R250-R350 | R144,000-R201,600 | 6-8 weeks |
| SA distributor | R800-R1,200 | R460,800-R691,200 | 1-2 weeks |
| Pilot (5 units, AliExpress) | R500-R600 each | R2,500-R3,000 | 2-3 weeks |

### 3.4 Power Options for Motorised Ball Valves

The valve only draws power during actuation (3-5W for 5-10 seconds). Between actuations, it draws zero (latching type). The challenge is delivering 12V DC to 48 locations per floor.

**Option A: Centralised 12V DC supply per floor (RECOMMENDED)**

| Item | Details |
|------|---------|
| Concept | One 12V DC switched-mode power supply (SMPS) per floor, located at the floor's switch room / comms closet. 12V DC distributed via 2-core cable to each valve along the passage cable basket. |
| Power supply | 12V DC, 10A (120W) DIN-rail SMPS -- e.g., MeanWell HDR-100-12 or NDR-120-12 |
| Cable | 2-core 0.75mm2 or 1.0mm2 (adequate for 5W load at <50m run, voltage drop <0.5V) |
| Cable routing | Along existing passage ceiling cable basket (already has Cat6 and other cables) |
| Advantages | Low cost per unit, centralised maintenance, UPS-protected if connected to floor switch UPS |
| Disadvantages | Requires new cable runs (48 per floor), SMPS per floor, additional fire load in cable basket |
| Cost per floor | R2,500-R4,000 (PSU) + R3,000-R5,000 (cable + connectors) = R5,500-R9,000 |
| Cost for 12 floors | R66,000-R108,000 |

**Option B: PoE-powered nodes with 12V valve output**

| Item | Details |
|------|---------|
| Concept | Each node is powered via PoE (802.3af, 48V DC) from the floor's UniFi switch. Node has a DC-DC converter to provide 12V to the valve. |
| PoE source | Existing UniFi managed switches (verify PoE port count and budget per switch) |
| Cable | Cat6 (existing infrastructure to APs, but new runs needed to each meter location) |
| Advantages | Single cable provides data + power, leverages existing infrastructure |
| Disadvantages | Requires Cat6 cable run to every meter location (client explicitly rejected infrastructure cabling cost). Consumes PoE ports. Fundamentally conflicts with LoRaWAN wireless approach. |
| Cost per floor | Very high: 48 x Cat6 runs + PoE ports |

**Option C: Local AC-DC adapter per unit**

| Item | Details |
|------|---------|
| Concept | Small 12V DC wall adapter (like a phone charger) at each unit, drawing from the passage lighting circuit or a dedicated circuit. |
| Advantages | No long cable runs, independent per unit |
| Disadvantages | 576 individual adapters. Passage ceiling typically has no accessible AC outlet per unit. Would require electrician to install junction box + outlet per unit. Extremely labour-intensive. |
| Cost per unit | R150-R300 (adapter) + R500-R1,000 (electrical installation) = R650-R1,300 |
| Cost for 576 units | R374,400-R748,800 |

**Option D: Battery-powered valve actuation**

| Item | Details |
|------|---------|
| Concept | Valve is powered by the node's battery. Latching valve actuated by a pulse from the node's MCU via an H-bridge driver. |
| Battery | 3.6V Li-SOCl2 (e.g., ER34615 D-cell, 19Ah) or 2x AA Li-SOCl2 in series (7.2V) with boost to 12V |
| Actuation energy | ~5W x 10s = 50J per actuation = ~3.8Ah at 3.6V per actuation... **this is problematic.** |
| Problem | Li-SOCl2 batteries have very high energy density but LOW peak current capability (~100-200mA continuous). A 12V/5W valve draws ~420mA. This exceeds the continuous current rating of typical Li-SOCl2 cells. Even with a supercapacitor buffer, frequent valve actuations would severely reduce battery life. |
| Verdict | NOT VIABLE for regular valve actuation from a battery-powered node. Could work for rare emergency shutoffs (once or twice per year) with a supercapacitor buffer, but not for routine throttling. |

**Recommendation: Option A (centralised 12V DC per floor) for Phase 2+.**

For Phase 1 (pilot), valves are not installed. The node PCB should include:
- A 2-pin JST connector for 12V DC input (from future centralised PSU)
- A 2-pin JST connector for valve motor output (to future valve)
- An H-bridge motor driver IC on the PCB (e.g., DRV8837, TB6612, or dual MOSFET pair) -- very low cost (<R5 per unit) to include from the start
- A digital input for valve position feedback (open/close status)

This makes the node "valve-ready" without any hardware redesign when valves are deployed.

### 3.5 Wiring Considerations in Existing Building

The passage ceiling at City Life already has:
- Water pipes (16mm feeds to each unit)
- Cable basket with Cat6 data cables
- Possibly lighting/power cables

Adding valve power cables (2-core 0.75mm2) to the existing cable basket is feasible:
- 2-core 0.75mm2 cable is thin (approx 5mm diameter) and flexible
- Can be cable-tied to the existing cable basket
- Total cable run per floor: approximately 48 units x average 15m = 720m of 2-core cable
- Can use a trunk/ring topology: single 2-core cable along the passage with short taps to each valve

**Fire risk consideration:** Adding cables to the cable basket increases fire load. Use LSZH (Low Smoke Zero Halogen) cable to comply with SANS 10142-1 and building fire regulations. The cable carries 12V DC (SELV -- Safety Extra Low Voltage) so does not require the same installation standards as mains wiring, but must still be properly installed by a qualified electrician.

### 3.6 Safety Considerations and Regulations (South Africa)

| Regulation | Applicability | Notes |
|------------|---------------|-------|
| SANS 10252-1:2012 (Water supply installations) | Meter and valve installation | Plumbing work must be done by registered plumber (Ryan, in-house) |
| SANS 10142-1:2017 (Wiring of premises) | Electrical installation for valve power | SELV (12V DC) exempted from most requirements but must be installed safely |
| National Building Regulations (SANS 10400) | Building modifications | Passage ceiling work may require notification to body corporate or building management |
| Water Services Act 108 of 1997 | Right to water | Cannot disconnect water supply without following legal process. Throttling is a grey area. |
| Consumer Protection Act 68 of 2008 | Tenant rights | Must give adequate notice before any water restriction |
| Rental Housing Act 50 of 1999 | Tenant/landlord relationship | Landlord cannot arbitrarily restrict services. Valve use must be governed by lease terms. |

**CRITICAL LEGAL NOTE:** Remotely shutting off a tenant's water supply has significant legal implications under South African law. The Water Services Act and Consumer Protection Act protect the right to basic water supply. Any valve shutoff capability must be governed by:
1. Clear lease terms that tenants sign
2. A formal process with notice periods
3. Legal review before deployment
4. The system should default to "open" (fail-safe) -- if anything goes wrong, tenants keep water

**Recommendation:** The valve capability should be presented to Tanya as a "future capability" with the caveat that legal review is essential before activation. The hardware can be valve-ready from Day 1, but the software and operational procedures for valve control should be developed separately with legal input.

### 3.7 Should PCBs Be Designed Valve-Ready from the Start?

**Yes, absolutely.** The incremental cost is minimal:

| Component | Unit Cost (ZAR) | Purpose |
|-----------|-----------------|---------|
| H-bridge motor driver (DRV8837 or similar) | R3-R5 | Drives valve motor forward/reverse |
| 2x JST-XH 2-pin connectors | R2-R3 | 12V input + valve motor output |
| 1x digital input (optocoupler) | R2-R3 | Valve position feedback |
| PCB area | ~5mm x 10mm additional | Negligible |
| **Total additional cost per PCB** | **R7-R11** | |
| **Total for 576 units** | **R4,000-R6,300** | |

Not including valve-ready capability from the start would require a PCB redesign and remanufacture later, costing far more than the R4,000-R6,300 upfront investment. Include it in Rev 1.0 of the PCB.

---

## 4. Communications Architecture Options

### 4.1 Option A: Private LoRaWAN Only (ChirpStack)

```
  [Precision Meters Ultrasonic 15mm]
     (integrated LoRaWAN radio)
              |
              | 868 MHz LoRaWAN uplink
              v
     [LoRaWAN Gateway (RAK7268V2)]
         (3x gateways, floors 3/8/12)
              |
              | Ethernet (PoE, via UniFi switch)
              v
     [ChirpStack Network Server]
         (LXC on Proxmox / cloud VM)
              |
              | MQTT / gRPC API
              v
     [Water Monitoring Platform]
         (Node.js + Express + SQLite)
              |
              | REST API
              v
     [CLTM ERP Integration]
```

| Metric | Value |
|--------|-------|
| Cost per unit meter node | R0 (integrated LoRaWAN in meter -- verify with Precision Meters) |
| Cost per bulk meter node | R800-R1,200 (separate LoRa pulse counter node) |
| Gateway cost (3x) | R18,000-R24,000 |
| Server cost | R0-R2,000 (existing Proxmox infrastructure or cloud VM) |
| Total infrastructure | R18,000-R24,000 + server |
| Power per unit node | Battery (in meter, 16+ year life) |
| Reliability | High -- CSS modulation, sub-GHz penetration |
| Scalability | 576 nodes well within 3-gateway capacity |
| Maintenance | Very low -- no batteries to replace if meter LoRaWAN is used |
| Security | AES-128 encryption (LoRaWAN 1.0.x) or AES-128 + AES-128 (LoRaWAN 1.1) |
| Valve support | Requires separate power infrastructure (Phase 2) |

**Advantages:**
- Simplest architecture if meters have integrated LoRaWAN
- Lowest infrastructure cost
- Lowest maintenance cost
- Proven approach (Fairfield Water reference)
- No dependency on building WiFi/Ethernet for node communications
- ChirpStack is already in use for Fairfield project

**Disadvantages:**
- If meters do NOT have LoRaWAN (or cost is prohibitive), 576 separate nodes are needed
- No valve control without additional power infrastructure
- LoRa is one-way for most use cases (Class A: node initiates; downlinks only after uplink)
- Less bandwidth than WiFi/Ethernet (but adequate for metering data)

### 4.2 Option B: WiFi-Based (Leverage Existing UniFi APs)

```
  [ESP32-based Node + Pulse Counter]
     (connected to meter pulse output)
              |
              | 2.4 GHz WiFi (dedicated IoT SSID/VLAN)
              v
     [Existing UniFi Access Points]
         (13-15 per floor)
              |
              | Ethernet (existing Cat6)
              v
     [Water Monitoring Platform]
         (Node.js + Express + SQLite)
```

| Metric | Value |
|--------|-------|
| Cost per unit node | R200-R400 (ESP32 + pulse counter + enclosure) |
| Cost per bulk meter node | R200-R400 (same) |
| Gateway cost | R0 (existing APs) |
| Server cost | R0-R2,000 |
| Total node cost (588) | R117,600-R235,200 |
| Total infrastructure | R0 |
| Power per unit node | Battery: 12-18 month replacement cycle (high WiFi power draw) |
| Battery replacement cost/year | ~R86,400 (576 nodes x R150 labour+parts) |
| 10-year battery cost | ~R864,000 |
| Reliability | LOW -- 2.4 GHz congestion from 13-15 APs per floor + tenant devices |
| Scalability | Problematic -- 576 devices compete with tenant traffic |
| Maintenance | HIGH -- battery replacements every 12-18 months |
| Security | WPA2/WPA3 on dedicated VLAN (Sumir can configure) |
| Valve support | Same power problem as LoRaWAN |

**Advantages:**
- Zero infrastructure cost (existing APs)
- Lower per-node hardware cost
- Higher bandwidth available (if needed)
- Easier firmware updates (OTA via WiFi)

**Disadvantages:**
- 2.4 GHz spectrum saturation (13-15 APs/floor, hundreds of tenant devices)
- Extremely high maintenance burden (battery replacement every 12-18 months for 576 nodes)
- WiFi association/reconnection overhead kills battery life
- Tenant WiFi changes (password resets, AP reboots) could disrupt IoT nodes
- ESP32 sleep current (~10 uA) is good, but WiFi TX + association (~150mA for 2-5 seconds) is 15-50x higher than LoRa TX (~10mA for 60ms)
- Not utility-grade reliable in this environment
- 10-year total cost of ownership is significantly higher than LoRaWAN

**Verdict:** NOT RECOMMENDED for unit meters. The battery replacement burden alone makes this uneconomical.

### 4.3 Option C: Wired Ethernet (Cat6)

```
  [Ethernet Node (ESP32 + W5500/LAN8720)]
     (connected to meter pulse output)
              |
              | Cat6 Ethernet (new cable runs)
              v
     [Floor UniFi Switch]
         (existing, dedicated VLAN)
              |
              | Ethernet backbone
              v
     [Water Monitoring Platform]
```

| Metric | Value |
|--------|-------|
| Cost per unit node | R300-R600 (ESP32 + Ethernet PHY + enclosure) |
| Infrastructure per floor | R20,000-R40,000 (48 x Cat6 runs, patch panel, switch ports) |
| Infrastructure (12 floors) | R240,000-R480,000 |
| Total cost | R412,800-R832,800 |
| Power per unit node | PoE (802.3af) from switch -- no batteries |
| Reliability | VERY HIGH (wired, no RF issues) |
| Scalability | Limited by switch port count (48-port switches needed per floor) |
| Maintenance | Very low (wired connections rarely fail) |
| Valve support | PoE can power the valve directly (12V from DC-DC on node) |

**Advantages:**
- Most reliable communications (wired)
- PoE provides power for node AND valve -- no separate power infrastructure
- No battery replacement ever
- No RF concerns

**Disadvantages:**
- EXPLICITLY REJECTED BY CLIENT ("ethernet cabling is too much of an infrastructure cost")
- Requires 576 new Cat6 cable runs to meter locations
- Requires new 48-port PoE switches per floor (existing switches may not have capacity)
- Highest infrastructure cost
- Most disruptive installation (cable routing through passage ceiling)

**Verdict:** ELIMINATED. Client explicitly rejected ethernet cabling cost during site visit.

### 4.4 Option D: Hybrid (Recommended)

```
  UNIT METERS (576x):
  [Precision Meters Ultrasonic 15mm]
     (integrated LoRaWAN radio)
              |
              | 868 MHz LoRaWAN
              v
     [LoRaWAN Gateway (RAK7268V2)]
         (3x gateways, PoE from UniFi switch)
              |
              | Ethernet (existing Cat6 to switch)
              v
     [ChirpStack + Monitoring Platform]

  FLOOR BULK METERS (12x):
  [Precision Meters Ultrasonic 50mm]
     (pulse output)
              |
              | Pulse wire (short run, same room)
              v
     [LoRa Pulse Counter Node (RAK3172)]
              |
              | 868 MHz LoRaWAN
              v
     [Same LoRaWAN Gateways]
              |
              v
     [Same ChirpStack + Monitoring Platform]
```

The hybrid approach uses:
- **LoRaWAN for all 576 unit meters** (via integrated meter radio or separate pulse counter nodes)
- **LoRaWAN for all 12 bulk meters** (via pulse counter nodes, since Woltmann/large ultrasonic meters may not have integrated LoRaWAN)
- **Existing Ethernet infrastructure ONLY for gateway backhaul** (3 gateways connect to 3 floor switches)
- **Existing UniFi infrastructure unchanged** (no additional APs, no IoT SSID needed)
- **ChirpStack as the unified network server** for all devices

This is functionally the same as Option A (LoRaWAN only) but recognises that:
1. The gateway backhaul uses existing wired Ethernet (not WiFi or 4G)
2. Bulk meters near switches could optionally use Ethernet directly, but for consistency and simplicity, all meters use the same LoRaWAN path
3. The monitoring platform server could be hosted on-premises (using existing rack space and internet) or in the cloud, connected via VPN

### 4.5 Architecture Comparison Summary

| Factor | A: LoRaWAN Only | B: WiFi | C: Ethernet | D: Hybrid (Rec.) |
|--------|-----------------|---------|-------------|-------------------|
| Infrastructure cost | R18-24K | R0 | R240-480K | R18-24K |
| Per-node cost | R0-R1,200 | R200-400 | R300-600 | R0-R1,200 |
| 10-year battery cost | R0-R45K | R864K | R0 | R0-R45K |
| Reliability | High | Low | Very High | High |
| Installation disruption | Low | Low | Very High | Low |
| Valve support (future) | Needs power infra | Needs power infra | PoE built-in | Needs power infra |
| Client acceptance | Yes | Possible | No (rejected) | Yes |
| Fairfield compatibility | Yes | No | No | Yes |
| **10-year TCO (est.)** | **R100-200K** | **R1M+** | **R500K-900K** | **R100-200K** |

**Recommendation: Option D (Hybrid) is the way forward.** It is architecturally identical to Option A for this deployment but acknowledges that the Ethernet infrastructure serves as reliable backhaul. The entire monitoring stack is consistent with Fairfield Water.

---

## 5. Node Design Considerations

### 5.1 Two Node Scenarios

**Scenario 1: Meter has integrated LoRaWAN (preferred)**
In this case, the Precision Meters ultrasonic 15mm meter handles all communication. No separate Precept node is needed per unit for metering. A Precept node is only needed:
- Per bulk meter (pulse counter)
- Per unit IF motorised valve control is added (Phase 2+)

**Scenario 2: Meter has pulse output only (fallback)**
If the LoRaWAN variant is not available or cost-prohibitive, each unit needs a Precept pulse counter node. This is the same architecture as Fairfield Water Phase 2.

### 5.2 Data Captured Per Node

| Data Point | Source | Purpose | Priority |
|------------|--------|---------|----------|
| Cumulative volume (m3) | Meter (LoRaWAN) or pulse count | Billing, usage tracking | Critical |
| Instantaneous flow rate (m3/h) | Meter (LoRaWAN) or derived from pulse timing | Real-time monitoring, leak detection | High |
| Leak alarm | Meter (built-in, ultrasonic) | Automatic leak detection | High |
| Burst alarm | Meter (built-in) | Emergency alert | High |
| Backflow alarm | Meter (built-in) | Tampering/plumbing fault | High |
| Tamper detection | Meter (magnetic/NFC) or node (tilt/accelerometer) | Security | High |
| Battery voltage | Meter or node | Maintenance planning | Medium |
| RSSI / SNR | Gateway (ChirpStack metadata) | Network health monitoring | Medium |
| Valve status (open/close) | Node (digital input from valve feedback) | Valve control verification | Phase 2 |
| Valve command (open/close) | Node (downlink from ChirpStack) | Remote valve control | Phase 2 |
| Water temperature | Meter (built-in, ultrasonic) | Water quality monitoring | Low |
| Node internal temperature | Node (MCU internal sensor) | Environmental monitoring | Low |

### 5.3 Power Source Analysis

**For meters with integrated LoRaWAN:**
- Power: Internal meter battery (Li-SOCl2, >16 year life per spec sheet)
- No external power needed for metering function
- Valve control (Phase 2) requires external 12V DC

**For separate pulse counter nodes (if needed):**

| Power Option | Battery Type | Capacity | Estimated Life (5-min TX) | Cost (ZAR) | Notes |
|--------------|-------------|----------|---------------------------|------------|-------|
| ER34615 (D-cell Li-SOCl2) | 3.6V, 19Ah | 19,000 mAh | 10-15 years (SF7, 10-byte payload) | R80-R120 | Best option. High capacity, long life. Low peak current (~200mA max). |
| 2x ER14505 (AA Li-SOCl2) | 3.6V, 2.6Ah each | 5,200 mAh (parallel) | 3-5 years | R40-R60 each | Smaller form factor, shorter life. Good for pilot. |
| ER26500 (C-cell Li-SOCl2) | 3.6V, 8.5Ah | 8,500 mAh | 6-10 years | R60-R90 | Middle ground. |
| CR123A (Lithium primary) | 3.0V, 1.5Ah | 1,500 mAh | 1-2 years | R25-R40 | Too short. Not recommended. |

**Battery life calculation (LoRaWAN, SF7, 5-minute interval, RAK3172):**

```
TX power consumption: ~40mA for ~60ms (SF7, 10-byte payload) = 0.67 uAh per TX
TX every 5 minutes: 288 TX/day x 0.67 uAh = 192.7 uAh/day
RX window (Class A): ~5mA for ~100ms x 2 windows = 0.28 uAh per cycle
RX daily: 288 x 0.28 = 80.6 uAh/day
Sleep current: ~1.5 uA x 24h = 36 uAh/day
Pulse counting (interrupt-based): negligible

Total daily: ~310 uAh/day = 0.31 mAh/day
Yearly: 113 mAh/year

ER34615 (19,000 mAh): 19,000 / 113 = 168 years (theoretical)
Reality factor (self-discharge ~1%/year, temperature, margin): divide by 10-15
Practical estimate: 11-17 years
```

This confirms that a D-cell Li-SOCl2 battery easily achieves the 10-year target, even with conservative margins.

### 5.4 Enclosure Requirements

| Requirement | Specification | Notes |
|-------------|---------------|-------|
| Mounting location | Passage ceiling, above unit front door | Alongside existing pipes and cables |
| Mounting method | DIN rail clip, screw mount, or cable tie to cable basket | Must be secure but removable for maintenance |
| IP rating | IP54 minimum | Indoor, dry location but possible condensation from pipes |
| Material | ABS plastic | Lightweight, flame-retardant (UL94 V-0) |
| Size (pulse counter node) | ~80mm x 50mm x 30mm | Small enough to fit alongside pipes |
| Size (valve controller node) | ~100mm x 70mm x 40mm | Larger to accommodate H-bridge driver and connectors |
| Cable glands | 2-3x M12 or PG7 | Pulse input, antenna (if external), valve connections |
| Antenna | Internal PCB antenna or short SMA whip (50mm) | Internal antenna preferred for aesthetics, external if signal is marginal |
| Colour | White or light grey | Blends with passage ceiling |
| Labelling | Unit number, QR code (links to dashboard) | For maintenance identification |

### 5.5 MCU Options

| MCU / Module | Radio | Interface | Price (ZAR) | Notes |
|--------------|-------|-----------|-------------|-------|
| **RAK3172 (STM32WLE5)** | LoRa SX1262 (868 MHz) | UART, SPI, I2C, GPIO | R80-R120 | **Recommended.** Same as Fairfield. LoRaWAN stack built-in (AT commands or custom firmware). Ultra-low power. |
| RAK11720 (Ambiq Apollo3 Blue) | LoRa SX1262 | UART, SPI, I2C, GPIO, BLE | R100-R150 | BLE for local config. Higher cost. |
| Heltec CubeCell (ASR6501) | LoRa SX1262 | UART, I2C, GPIO | R60-R90 | Very low cost but less community support. |
| ESP32 + SX1276 module | WiFi + BLE + LoRa | Full ESP32 IO | R80-R120 | Higher power (ESP32 always draws more in sleep). Only if WiFi/BLE needed. |
| ESP32-S3 (WiFi only) | WiFi + BLE | Full ESP32 IO | R50-R80 | WiFi option. Not recommended (see Section 4.2). |
| STM32WL55 (bare chip) | LoRa SX1262 | Full STM32 IO | R40-R60 | Custom PCB required. Lowest BOM cost at volume. |

**Recommendation: RAK3172** for all nodes. Reasons:
- Proven in Fairfield Water project
- Integrated STM32WLE5 + SX1262 in a single module (smallest footprint)
- AT command firmware available (fast development) or custom firmware via STM32CubeIDE
- Ultra-low sleep current (~1.5 uA)
- Castellated pads for PCB integration
- Good availability from RAKwireless (direct or via Mouser/Digikey)
- Unit cost at volume (100+): approximately R80-R100

### 5.6 Node BOM Estimate (Pulse Counter + Valve-Ready)

| Component | Description | Qty | Unit Cost (ZAR) | Extended (ZAR) |
|-----------|-------------|-----|-----------------|----------------|
| RAK3172 | LoRa module (STM32WLE5 + SX1262) | 1 | R90 | R90 |
| Custom PCB | 2-layer, 50x40mm, fabricated (JLCPCB) | 1 | R8-R15 | R12 |
| ER34615 | D-cell Li-SOCl2 3.6V 19Ah battery | 1 | R100 | R100 |
| Battery holder | D-cell spring clip | 1 | R5 | R5 |
| Pulse input | Screw terminal or JST-XH 2-pin | 1 | R3 | R3 |
| H-bridge driver | DRV8837 or TB6612 (valve-ready) | 1 | R5 | R5 |
| Valve connectors | JST-XH 2-pin (12V in + motor out) | 2 | R3 | R6 |
| Feedback input | Optocoupler (valve position) | 1 | R3 | R3 |
| Voltage regulator | LDO 3.3V (for battery input) | 1 | R3 | R3 |
| Capacitors, resistors | Passives (decoupling, pull-ups) | misc | R5 | R5 |
| Enclosure | ABS IP54, 80x50x30mm | 1 | R25-R40 | R35 |
| Antenna | PCB trace antenna (on-board) | 1 | R0 | R0 |
| Cable gland | M12, 2x | 2 | R5 | R10 |
| Label / QR code | Printed sticker | 1 | R2 | R2 |
| Assembly | SMD reflow + through-hole (JLCPCB assembly or local) | 1 | R30-R50 | R40 |
| **Total per node** | | | | **R319** |

**At volume (576 nodes):** R183,744 total, or approximately R319/node.

**Note:** This is the cost of a SEPARATE pulse counter + valve-ready node. If the Precision Meters ultrasonic 15mm has integrated LoRaWAN, these nodes are only needed for:
- 12 bulk meter pulse counters: 12 x R319 = R3,828
- Future valve controller nodes (576, only when valves are deployed)

---

## 6. Pilot Design

### 6.1 Recommended Pilot Scope

Per client preference (Tanya Dowley): **1 floor + 5 units.**

**Recommended pilot configuration:**

| Item | Quantity | Purpose |
|------|----------|---------|
| Unit meters (15mm ultrasonic) | 5 | 5 representative apartments on 1 floor |
| Bulk floor meter (50mm ultrasonic or Woltmann) | 1 | Floor-level consumption and leak detection |
| LoRaWAN gateway | 1 | RAK7268V2, installed on the pilot floor |
| ChirpStack server | 1 | LXC container on existing Proxmox (same as Fairfield) or cloud VM |
| Monitoring platform | 1 | Instance of the water monitoring app (same codebase as Fairfield) |
| Pulse counter nodes (if meters lack LoRaWAN) | 5 + 1 = 6 | One per meter |

**Which floor for the pilot?** Recommend Floor 3 or Floor 4.

| Reason | Details |
|--------|---------|
| Mid-low building position | Avoids extremes (ground floor has unique plumbing, high floors are furthest from ground comms) |
| Gateway coverage test | A gateway on Floor 3 allows testing signal reach to Floor 1 (below) and Floor 6 (above) during the pilot |
| Representative | Mid-floor apartments have typical plumbing configurations |
| Accessible | Low enough for easy access during pilot troubleshooting |
| Scalable | When scaling, this floor's gateway becomes GW-1 in the 3-gateway plan |

### 6.2 Pilot Infrastructure

| Component | Specification | Cost (est. ZAR) | Notes |
|-----------|---------------|-----------------|-------|
| 5x Ultrasonic 15mm meter | Precision Meters | TBC (request pricing from Bradley) | Must confirm LoRaWAN variant |
| 1x Ultrasonic 50mm bulk meter | Precision Meters | TBC (request pricing from Bradley) | OR Woltmann 50mm |
| 1x RAK7268V2 gateway | 8-channel indoor LoRaWAN gateway | R7,000-R9,000 | PoE from floor switch |
| 6x Precept nodes (if needed) | RAK3172 pulse counter + valve-ready | 6 x R319 = R1,914 | Only if meters lack LoRaWAN |
| ChirpStack server | LXC on Proxmox or cloud VM | R0-R500/month | Shared with Fairfield infrastructure |
| Monitoring platform | Node.js + Express + SQLite | R0 (development cost only) | Fork of Fairfield Water codebase |
| Cat6 patch cable (1x) | Gateway to floor switch | R50 | 1-3m, existing infrastructure |
| Installation labour | Ryan (plumbing) + Jason (system) + Sumir (learning) | R0 (in-house) + Jason consulting | Estimated 1 hour per unit |
| **Total pilot hardware** | | **R9,000-R11,000 + meters** | Excluding meter cost (TBC from Precision) |

### 6.3 Pilot-to-Scale Design Principles

The pilot must be designed so that **nothing is thrown away or reworked** when scaling to the full 576-unit deployment.

| Principle | Implementation |
|-----------|----------------|
| **Same meter type throughout** | Pilot uses the same 15mm ultrasonic meter that will be deployed in all 576 units. No "pilot-only" hardware. |
| **Same gateway model** | The pilot gateway (RAK7268V2) becomes GW-1 in the 3-gateway production deployment. Same firmware, same configuration. |
| **Same ChirpStack instance** | Pilot devices are registered in the same ChirpStack instance that will manage all 576 devices. Device profiles, applications, and integrations are production-grade from Day 1. |
| **Same monitoring platform** | Pilot uses the same web application that will serve 576 units. Database schema, API endpoints, and dashboard designed for full scale from the start. |
| **Same node hardware (if applicable)** | Pilot nodes use Rev 1.0 of the Precept PCB, which is valve-ready. Same PCB is used for all subsequent deployments. |
| **Same VLAN** | Pilot gateway connects to the dedicated IoT VLAN (configured by Sumir). All future gateways use the same VLAN. |
| **Same addressing scheme** | ChirpStack device EUI and application addressing scheme designed for 588 devices from the start (e.g., device naming convention: MGW-F03-U01 for Floor 3 Unit 1). |
| **Same reporting interval** | 5-minute uplink from Day 1. No "pilot mode" with different settings. |

### 6.4 Pilot Success Criteria

The pilot should prove the following before scaling:

| Test | Success Criteria | Method |
|------|------------------|--------|
| LoRaWAN connectivity | All 6 devices deliver >98% of expected uplinks over 2 weeks | ChirpStack frame counter analysis |
| Signal quality | All devices maintain RSSI > -120 dBm and SNR > -5 dB | ChirpStack device status |
| Metering accuracy | Meter readings match manual readings within 2% over 1 month | Compare meter totaliser vs manual bucket test |
| Leak detection | System correctly identifies a simulated slow leak (50 L/h) | Open a tap to drip rate, verify alarm in dashboard |
| Dashboard functionality | All data visible in real-time, historical charts correct | Visual inspection, data export |
| Multi-floor coverage | Test signal from pilot floor to adjacent floors (above/below) | Place a test node 2-3 floors above and below the gateway |
| Battery life projection | Measured daily consumption matches calculated estimates | Measure battery voltage over 30 days, extrapolate |
| Tamper detection | System alerts if meter is removed or disturbed | Physically disturb a meter, verify alert |
| NFC commissioning | Meters can be configured/read via NFC on smartphone | Test with NFC app during installation |
| ChirpStack ADR | Network automatically optimises SF and TX power | Monitor SF distribution over 2 weeks |

### 6.5 Pilot Timeline Estimate

| Week | Activity |
|------|----------|
| Week 1 | Order meters from Precision Meters (confirm LoRaWAN availability, pricing). Order gateway. Manufacture/assemble nodes (if needed). |
| Week 2 | Set up ChirpStack server. Configure monitoring platform. Create device profiles. |
| Week 3 | Install gateway on pilot floor (connect to UniFi switch). Register devices in ChirpStack. |
| Week 3-4 | Install meters and nodes on 5 units + 1 bulk meter (coordinate with Ryan for plumbing, Sumir for network). |
| Week 4 | Commission and verify all devices online. Run initial tests. |
| Week 5-8 | Monitoring period. Collect data. Validate accuracy. Test leak detection. |
| Week 8 | Pilot review with Tanya. Present results. Decision on full rollout. |

### 6.6 Full Rollout Estimate (Post-Pilot)

After successful pilot, the full 576-unit rollout would proceed floor by floor:

| Phase | Floors | Units | Meters | Bulk Meters | Additional Gateways | Cumulative Cost (est.) |
|-------|--------|-------|--------|-------------|---------------------|----------------------|
| Pilot | 1 floor (3 or 4) | 5 | 5 | 1 | 1 (GW-1) | R9-11K + meters |
| Phase 1 | Pilot floor remaining | 43 | 43 | 0 | 0 | +R13-43K + meters |
| Phase 2 | Floors 1-4 | 192 | 192 | 4 | 0 (GW-1 covers) | +R58-192K + meters |
| Phase 3 | Floors 5-8 | 192 | 192 | 4 | 1 (GW-2) | +R67-201K + meters |
| Phase 4 | Floors 9-12 | 192 | 192 | 3 | 1 (GW-3) | +R67-201K + meters |
| **Total** | **12 floors** | **576** | **576** | **12** | **3** | **R214K-648K + meters** |

Note: The wide cost range depends on whether separate Precept nodes are needed (R319/each) or whether the meter's integrated LoRaWAN eliminates the need for separate nodes. The meter cost itself is the largest unknown and must be confirmed with Precision Meters.

---

## Action Items

| # | Action | Priority | Owner | Status |
|---|--------|----------|-------|--------|
| 1 | Confirm with Bradley Cassani: Does the 15mm ultrasonic have integrated LoRaWAN? What is the price difference? What ChirpStack device profile is used? | CRITICAL | Jason | Pending |
| 2 | Confirm with Bradley: Is the third column in the 32-50mm spec sheet actually 50mm? What is the 50mm price? | High | Jason | Pending |
| 3 | Confirm with Bradley: What is the pulse output specification of the 15mm meter (if LoRaWAN not available)? Litres per pulse? Reed or wired? | High | Jason | Pending |
| 4 | Request volume pricing from Precision Meters for: 576x 15mm ultrasonic + 12x 50mm ultrasonic (LoRaWAN variants if available) | High | Jason | Pending |
| 5 | Order 1x RAK7268V2 gateway for pilot | Medium | Jason | Pending |
| 6 | Design Rev 1.0 PCB for Precept pulse counter + valve-ready node (RAK3172 based) | Medium | Jason | Pending |
| 7 | Source pilot motorised ball valves (5x DN15 12V DC) for testing even if not Phase 1 | Low | Jason | Pending |
| 8 | Research CLTM ERP system API capabilities for billing integration | High | Jason | Pending (separate doc) |
| 9 | Confirm with Sumir: available PoE ports and power budget on floor switches | Medium | Jason | Pending |
| 10 | Confirm with Sumir: VLAN allocation for IoT network | Medium | Jason | Pending |

---

## References

### Existing Project Documents
- `docs/planning/internal/Research/Smart Metering Architecture for High-Rise.md` -- Comprehensive AMI analysis (LoRaWAN, gateway placement, TCO)
- `docs/planning/internal/Vendors/` -- Precision Meters spec sheets (4 products)
- `docs/planning/site-visits/2026-01-29-city-life-site-visit-report.md` -- Full site visit report
- `docs/planning/internal/site-visit-29012026-notes.md` -- Raw site visit notes

### Technical Standards
- SANS 1529-1:2019 -- Water meters for cold potable water (South African standard)
- SANS 10252-1:2012 -- Water supply installations for buildings
- SANS 10142-1:2017 -- Wiring of premises (electrical installations)
- ICASA Radio Frequency Spectrum Regulations -- 868 MHz ISM band (South Africa)
- ITU-R P.2040 -- Effects of building materials and structures on radiowave propagation

### LoRaWAN and ChirpStack
- ChirpStack v4 documentation: https://www.chirpstack.io/docs/
- LoRaWAN specification (LoRa Alliance): https://lora-alliance.org/about-lorawan/
- RAK7268V2 product page: https://store.rakwireless.com/products/rak7268-wisgate-edge-lite-2
- RAK3172 module: https://store.rakwireless.com/products/rak3172-lora-module

### Vendor Contacts
- Precision Meters: Bradley Cassani (061 864 0536, bradley.cassani@precisionmeters.co.za)
- Precision Meters: Garth Le Roux (072 469 8133)
- RAKwireless: https://store.rakwireless.com/ (or via Mouser/Digikey for SA delivery)
