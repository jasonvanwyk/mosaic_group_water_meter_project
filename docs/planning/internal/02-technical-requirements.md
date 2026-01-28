# Technical Requirements Gathering

**Project:** Mosaic Group Water Meter Monitoring (MGW)
**Date:** 29 January 2026
**Site:** 477 Anton Lembede Street, Durban

---

## Section A: Plumber Assessment

**Plumber Name:** ___________________________ **Contact:** _______________

### A1. Water Supply Configuration

| Question | Answer |
|----------|--------|
| Municipal supply entry point? | |
| Number of supply lines into building? | |
| Main supply pipe size? | |
| Main supply pipe material? | |
| Is there a bulk meter from municipality? | Yes / No |
| If yes, size and type? | |

### A2. Internal Distribution

| Question | Answer |
|----------|--------|
| Distribution system type? | Riser / Ring / Other: |
| Number of risers? | |
| Riser pipe sizes? | |
| Riser locations? | |
| Are risers accessible? | Yes / No |
| Riser access method? | Shaft / Ceiling / Other: |

### A3. Unit Connections

| Question | Answer |
|----------|--------|
| Pipe size to each unit? | |
| Pipe material to units? | |
| Where does pipe enter unit? | Kitchen / Bathroom / Utility |
| Isolation valve per unit? | Yes / No |
| Space for meter at entry point? | Yes / No |
| Typical straight pipe length available? | |

### A4. Bulk Meter Locations (11 required)

**Purpose:** Leak detection at key distribution points

| # | Proposed Location | Pipe Size | Accessible? | Space Available? | Notes |
|---|------------------|-----------|-------------|------------------|-------|
| 1 | Main incoming | | Yes / No | Yes / No | |
| 2 | | | Yes / No | Yes / No | |
| 3 | | | Yes / No | Yes / No | |
| 4 | | | Yes / No | Yes / No | |
| 5 | | | Yes / No | Yes / No | |
| 6 | | | Yes / No | Yes / No | |
| 7 | | | Yes / No | Yes / No | |
| 8 | | | Yes / No | Yes / No | |
| 9 | | | Yes / No | Yes / No | |
| 10 | | | Yes / No | Yes / No | |
| 11 | | | Yes / No | Yes / No | |

### A5. Installation Considerations

| Question | Answer |
|----------|--------|
| Can water be shut off per riser? | Yes / No |
| Shut-off duration acceptable? | |
| Best time for shut-offs? | |
| Any known plumbing issues? | |
| Recent plumbing work done? | |
| Plumber available for install support? | Yes / No |

### A6. Leak History

| Question | Answer |
|----------|--------|
| Frequency of leaks? | |
| Common leak locations? | |
| Biggest leak incident? | |
| Estimated water loss monthly? | |

---

## Section B: IT Technician Assessment

**IT Tech Name:** ___________________________ **Contact:** _______________

### B1. Network Infrastructure

| Question | Answer |
|----------|--------|
| Internet provider? | |
| Connection type? | Fibre / DSL / LTE / Other: |
| Bandwidth? | |
| Static IP available? | Yes / No |
| Firewall/router model? | |

### B2. Building Network

| Question | Answer |
|----------|--------|
| Ethernet throughout building? | Yes / No |
| If yes, coverage? | Full / Partial |
| CAT rating? | CAT5e / CAT6 / Other: |
| Number of network points? | |
| Managed switches? | Yes / No |
| VLANs in use? | Yes / No |

### B3. Comms Room / Server Room

| Question | Answer |
|----------|--------|
| Location? | Floor: _____ Room: _____ |
| Rack space available? | Yes / No |
| Power available (UPS)? | Yes / No |
| Air conditioned? | Yes / No |
| Access restrictions? | |

### B4. Wireless Infrastructure

| Question | Answer |
|----------|--------|
| Wi-Fi deployed? | Yes / No |
| Coverage? | Full / Partial / Common areas only |
| Wi-Fi for IoT acceptable? | Yes / No |
| Dedicated IoT SSID possible? | Yes / No |

### B5. LoRa/IoT Feasibility

| Question | Answer |
|----------|--------|
| Building construction? | Concrete / Brick / Steel / Mixed |
| Number of floors? | |
| Floor plate size (approx)? | |
| Basement levels? | |
| RF interference concerns? | |
| Rooftop access for antenna? | Yes / No |

### B6. Gateway Placement Options

| Location | Power? | Ethernet? | Secure? | Coverage Est. | Notes |
|----------|--------|-----------|---------|---------------|-------|
| Comms room | Yes/No | Yes/No | Yes/No | | |
| Rooftop | Yes/No | Yes/No | Yes/No | | |
| Floor _____ | Yes/No | Yes/No | Yes/No | | |
| Floor _____ | Yes/No | Yes/No | Yes/No | | |
| Basement | Yes/No | Yes/No | Yes/No | | |

### B7. Data & Security Requirements

| Question | Answer |
|----------|--------|
| Data hosting preference? | Cloud / On-premises / Hybrid |
| Security policies to comply with? | |
| VPN required? | Yes / No |
| Integration with existing systems? | |
| Which systems? | |
| API access needed? | Yes / No |

### B8. IT Support

| Question | Answer |
|----------|--------|
| IT support for installation? | Yes / No |
| Network changes permitted? | Yes / No |
| Change control process? | |
| Monitoring/alerting systems in use? | |

---

## Section C: Combined Technical Summary

### Recommended Architecture

Based on site assessment:

| Component | Recommendation | Notes |
|-----------|---------------|-------|
| Connectivity | LoRa / Ethernet / Hybrid | |
| Gateway quantity | | |
| Gateway locations | | |
| Server/hosting | Cloud / On-prem | |

### Technical Risks

| Risk | Likelihood | Impact | Mitigation |
|------|------------|--------|------------|
| | H / M / L | H / M / L | |
| | H / M / L | H / M / L | |
| | H / M / L | H / M / L | |

### Bill of Materials (Preliminary)

| Item | Quantity | Notes |
|------|----------|-------|
| 50mm bulk meters | 11 | Precision Meters |
| 15mm unit meters | 500 | Precision Meters, phased |
| LoRa gateways | | |
| LoRa transmitters | | |
| Ethernet nodes | | |
| Cabling (m) | | |
| Enclosures | | |

---

## Section D: Drawings / Sketches

### Building Layout (rough sketch)

```
+------------------------------------------------------------------+
|                                                                  |
|                                                                  |
|                                                                  |
|                                                                  |
|                                                                  |
|                                                                  |
|                                                                  |
|                                                                  |
|                                                                  |
|                                                                  |
+------------------------------------------------------------------+
```

### Riser Diagram

```
+------------------------------------------------------------------+
|                                                                  |
|                                                                  |
|                                                                  |
|                                                                  |
|                                                                  |
|                                                                  |
|                                                                  |
|                                                                  |
|                                                                  |
|                                                                  |
+------------------------------------------------------------------+
```

---

## Section E: Documents to Request

- [ ] Building floor plans
- [ ] Plumbing as-built drawings
- [ ] Network infrastructure diagram
- [ ] Unit layout drawings
- [ ] Previous water bills (12 months)
- [ ] Tenant schedule / unit list

---

## Section F: Photo Log

| # | Description | Taken? |
|---|-------------|--------|
| 1 | Main water supply entry | [ ] |
| 2 | Municipal meter (if exists) | [ ] |
| 3 | Riser shaft access | [ ] |
| 4 | Typical riser configuration | [ ] |
| 5 | Typical unit entry point | [ ] |
| 6 | Comms room | [ ] |
| 7 | Potential gateway location 1 | [ ] |
| 8 | Potential gateway location 2 | [ ] |
| 9 | Rooftop (if accessible) | [ ] |
| 10 | Problem areas | [ ] |

---

## Notes

_____________________________________________________________

_____________________________________________________________

_____________________________________________________________

_____________________________________________________________

_____________________________________________________________

_____________________________________________________________

_____________________________________________________________

_____________________________________________________________
