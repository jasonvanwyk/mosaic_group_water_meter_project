# Hosting Options Research — IoT Water Meter Monitoring Platform

**Project:** Mosaic Group Water Meter Monitoring (MGW)
**Document:** 03 — Hosting Options Research
**Date:** 30 January 2026
**Author:** Jason van Wyk, Precept Systems
**Status:** Research / Internal

---

## Table of Contents

1. [Context and Requirements](#1-context-and-requirements)
2. [Workload Profile](#2-workload-profile)
3. [Cloud Providers — South African Regions](#3-cloud-providers--south-african-regions)
4. [Local / South African Hosting Providers](#4-local--south-african-hosting-providers)
5. [On-Premises Options](#5-on-premises-options)
6. [Hybrid Architecture Options](#6-hybrid-architecture-options)
7. [South African Operational Considerations](#7-south-african-operational-considerations)
8. [Cost Comparison Summary](#8-cost-comparison-summary)
9. [Recommendations by Phase](#9-recommendations-by-phase)
10. [Decision Matrix](#10-decision-matrix)
11. [Conclusion and Next Steps](#11-conclusion-and-next-steps)

---

## 1. Context and Requirements

### 1.1 Client Preference

Mosaic Group's hosting preference (from site visit, 29 January 2026):
- **51% leaning cloud** (AWS, Google Cloud, Azure, or similar)
- **49% leaning on-premises**
- Open to hybrid approaches

### 1.2 System Components

The platform consists of the following components that need to be hosted:

| Component | Description | Resource Profile |
|-----------|-------------|-----------------|
| **ChirpStack LoRaWAN Network Server** | Open-source LoRaWAN NS; receives and processes all LoRa uplinks from gateways | CPU-moderate, RAM-moderate, requires PostgreSQL and Redis |
| **Web Application** | Node.js + Express backend, SPA frontend | CPU-light, RAM-light |
| **Database** | SQLite (pilot) or PostgreSQL (production) | Disk I/O moderate, storage grows with time |
| **MQTT Broker** | Message broker between ChirpStack and application | CPU-light, RAM-light, persistent |
| **Monitoring / Grafana** | Optional dashboards for system health | CPU-light, RAM-moderate |
| **Reverse Proxy** | Nginx or Caddy for HTTPS termination | CPU-light |

### 1.3 Scale Parameters

| Parameter | Pilot | Full Scale |
|-----------|-------|------------|
| Unit meters | 5 | 576 |
| Bulk meters | 1 (floor) | 12 (one per floor) |
| Total devices | 6 | 588 |
| Reporting interval | 5 minutes | 5 minutes |
| Uplinks per day | 1,728 | 169,344 |
| Uplinks per month | ~52,000 | ~5,080,000 |
| Data per uplink | ~10 bytes payload + metadata | ~10 bytes payload + metadata |
| Data ingestion per month | ~5 MB raw | ~500 MB raw |
| LoRa gateways | 1 | 5 (as per architecture research) |
| Concurrent dashboard users | 1-3 | 5-15 |
| API calls (ERP integration) | Minimal | ~17,000/day (billing sync) |

### 1.4 Reference Architecture

The Fairfield Water project (`/home/jason/Projects/fairfield-water`) runs:
- **Host:** LXC container on Proxmox (LXC 120, `pve` at 10.0.1.11)
- **OS:** Linux (Debian-based)
- **App:** Node.js + Express at `/opt/water-monitor/`
- **Database:** SQLite at `/opt/water-monitor/water_monitor.db`
- **Access:** Cloudflare Tunnel (no exposed ports, no static IP required)
- **URL:** https://fairfield.meter-tracker.com

---

## 2. Workload Profile

### 2.1 Minimum Server Specifications

Based on ChirpStack requirements and the application stack:

| Resource | Pilot (6 devices) | Full Scale (588 devices) |
|----------|-------------------|--------------------------|
| **CPU** | 2 vCPUs | 4 vCPUs |
| **RAM** | 2 GB | 4-8 GB |
| **Storage** | 20 GB SSD | 80-100 GB SSD |
| **Bandwidth** | 50 GB/month | 200 GB/month |
| **OS** | Debian 12 / Ubuntu 22.04 LTS | Debian 12 / Ubuntu 22.04 LTS |

### 2.2 Software Stack Requirements

ChirpStack v4 requires:
- **PostgreSQL** 14+ (with `pg_trgm` and `hstore` extensions)
- **Redis** 7+ (for device session caching and deduplication)
- **Mosquitto** (MQTT broker) or any MQTT 3.1.1+ broker

The application stack additionally requires:
- **Node.js** 18+ LTS
- **Nginx** or **Caddy** (reverse proxy)
- **Cloudflare Tunnel** (optional, for secure access without port exposure)

---

## 3. Cloud Providers -- South African Regions

### 3.1 Amazon Web Services (AWS) -- af-south-1 (Cape Town)

**Region:** Africa (Cape Town) — `af-south-1`
**Launched:** April 2020
**Availability Zones:** 3

**Key Points:**
- First hyperscaler with a dedicated South African region
- Full service availability including EC2, RDS, IoT Core, Lambda, S3, CloudWatch
- Data stays in South Africa (POPIA compliant by architecture)
- af-south-1 pricing carries a ~20-30% premium over EU/US regions
- Requires opt-in activation for new accounts (not enabled by default)

**Relevant Services:**

| Service | Use Case | Estimated Monthly Cost (ZAR) |
|---------|----------|------------------------------|
| EC2 t3.small (2 vCPU, 2 GB) | Pilot server | ~R650-R800 |
| EC2 t3.medium (2 vCPU, 4 GB) | Full-scale server | ~R1,300-R1,600 |
| EC2 t3.large (2 vCPU, 8 GB) | Full-scale with headroom | ~R2,600-R3,200 |
| RDS PostgreSQL db.t3.micro | Managed database (pilot) | ~R500-R700 |
| RDS PostgreSQL db.t3.small | Managed database (full) | ~R1,000-R1,400 |
| S3 (50 GB) | Backups and static assets | ~R50-R100 |
| Data transfer (200 GB out) | Egress | ~R600-R900 |
| IoT Core | MQTT broker (alternative) | ~R200-R400 (based on messages) |
| CloudWatch | Monitoring and alerts | ~R100-R200 |

**Estimated Monthly Total:**
- **Pilot:** R1,500-R2,300/month (~USD 80-125)
- **Full scale:** R3,500-R5,500/month (~USD 190-300)

**AWS IoT-Specific Offerings:**
- AWS IoT Core (managed MQTT broker with rules engine)
- AWS IoT Analytics (time-series analysis)
- AWS IoT Greengrass (edge computing, could run on-premises)
- AWS IoT SiteWise (industrial asset monitoring)

Note: AWS IoT Core could replace self-hosted MQTT but adds vendor lock-in and may not integrate natively with ChirpStack's MQTT expectations. Self-hosted MQTT on EC2 is simpler and more predictable for this use case.

**Reserved Instance Savings:** 1-year reserved instances offer ~30-40% discount. A 3-year term offers ~50-60% discount. This would bring full-scale costs closer to R2,000-R3,000/month.

---

### 3.2 Microsoft Azure -- South Africa North (Johannesburg) / South Africa West (Cape Town)

**Regions:**
- **South Africa North** (Johannesburg) — primary, full services
- **South Africa West** (Cape Town) — paired DR region, limited services

**Launched:** March 2019
**Key Points:**
- Two South African regions (best geo-redundancy option of any hyperscaler)
- South Africa North is the primary region with most services
- Azure IoT Hub is available in South Africa North
- Pricing comparable to or slightly higher than AWS af-south-1
- Strong enterprise integration (Active Directory, O365, etc.)
- POPIA compliance documentation available

**Relevant Services:**

| Service | Use Case | Estimated Monthly Cost (ZAR) |
|---------|----------|------------------------------|
| B2s VM (2 vCPU, 4 GB) | Full-scale server | ~R1,200-R1,500 |
| B1ms VM (1 vCPU, 2 GB) | Pilot server | ~R600-R800 |
| Azure Database for PostgreSQL Flexible (Burstable B1ms) | Managed database | ~R800-R1,200 |
| Azure Blob Storage (50 GB) | Backups | ~R40-R80 |
| Azure IoT Hub (S1) | Managed MQTT (8,000 msgs/day) | ~R800-R1,000 |
| Data transfer (200 GB out) | Egress | ~R500-R800 |

**Estimated Monthly Total:**
- **Pilot:** R1,500-R2,500/month
- **Full scale:** R3,000-R5,000/month

**Azure IoT-Specific Offerings:**
- Azure IoT Hub (device management, MQTT/AMQP)
- Azure IoT Edge (edge computing runtime)
- Azure Digital Twins (property modelling)
- Azure Stream Analytics (real-time event processing)
- Azure Time Series Insights (deprecated, replaced by Data Explorer)

---

### 3.3 Google Cloud Platform (GCP) -- africa-south1 (Johannesburg)

**Region:** africa-south1 (Johannesburg)
**Launched:** October 2023
**Availability Zones:** 3

**Key Points:**
- Newest hyperscaler region in South Africa
- Located in Johannesburg
- Core Compute Engine and Cloud SQL available
- IoT Core was deprecated by Google in August 2023 — no managed IoT service
- GCP generally has competitive pricing and strong data analytics tools
- Less mature South African presence compared to AWS and Azure

**Relevant Services:**

| Service | Use Case | Estimated Monthly Cost (ZAR) |
|---------|----------|------------------------------|
| e2-small (2 vCPU, 2 GB) | Pilot server | ~R500-R700 |
| e2-medium (2 vCPU, 4 GB) | Full-scale server | ~R1,000-R1,400 |
| Cloud SQL PostgreSQL (db-f1-micro) | Managed database (pilot) | ~R400-R600 |
| Cloud SQL PostgreSQL (db-g1-small) | Managed database (full) | ~R800-R1,200 |
| Cloud Storage (50 GB) | Backups | ~R30-R60 |
| Data transfer (200 GB out) | Egress | ~R500-R800 |

**Estimated Monthly Total:**
- **Pilot:** R1,200-R2,000/month
- **Full scale:** R2,800-R4,500/month

**Important Note:** Google deprecated its Cloud IoT Core service in August 2023. There is no managed IoT offering from GCP. This means ChirpStack would need to be self-hosted on a Compute Engine VM regardless — which is the same approach as the other providers. This actually reduces GCP's value proposition for this use case, as the others at least offer optional managed MQTT/IoT services.

---

### 3.4 Cloud Provider Comparison Summary

| Factor | AWS (Cape Town) | Azure (Johannesburg) | GCP (Johannesburg) |
|--------|-----------------|---------------------|-------------------|
| **SA Region** | af-south-1 (Cape Town) | South Africa North (JHB) + South Africa West (CPT) | africa-south1 (JHB) |
| **Launched** | April 2020 | March 2019 | October 2023 |
| **Availability Zones** | 3 | 3 (North) | 3 |
| **Managed IoT** | IoT Core (MQTT) | IoT Hub | None (deprecated) |
| **PostgreSQL Managed** | RDS | Flexible Server | Cloud SQL |
| **Pilot Cost/month** | R1,500-R2,300 | R1,500-R2,500 | R1,200-R2,000 |
| **Full-scale Cost/month** | R3,500-R5,500 | R3,000-R5,000 | R2,800-R4,500 |
| **POPIA Data Residency** | Yes (data in SA) | Yes (data in SA) | Yes (data in SA) |
| **Geo-redundancy** | Single SA region | Two SA regions (JHB + CPT) | Single SA region |
| **Maturity in SA** | Established | Most established | Newest |
| **Free Tier** | 12-month free tier (t3.micro) | 12-month free tier (B1s) | Always-free e2-micro |
| **Latency from Durban** | ~10-15ms (CPT) | ~5-10ms (JHB) | ~5-10ms (JHB) |

---

## 4. Local / South African Hosting Providers

These are South African companies with data centres physically located in South Africa. They offer significantly lower prices than hyperscalers and provide local support, ZAR billing, and inherent POPIA compliance. They are worth serious consideration for an IoT workload of this size.

### 4.1 Hetzner South Africa

**Data Centres:** Cape Town (primary), Johannesburg
**Website:** https://www.hetzner.co.za

Hetzner South Africa is the most popular local hosting provider for developers and small-to-medium businesses. They operate their own data centres in South Africa and are a separate entity from Hetzner Germany (though related).

**VPS (Cloud) Options:**

| Plan | vCPUs | RAM | Storage | Bandwidth | Estimated Price (ZAR/month) |
|------|-------|-----|---------|-----------|----------------------------|
| VPS CX11 (entry) | 1 | 2 GB | 20 GB SSD | 20 TB | R90-R120 |
| VPS CX21 | 2 | 4 GB | 40 GB SSD | 20 TB | R180-R250 |
| VPS CX31 | 2 | 8 GB | 80 GB SSD | 20 TB | R350-R450 |
| VPS CX41 | 4 | 16 GB | 160 GB SSD | 20 TB | R650-R800 |

**Dedicated Server Options:**

| Plan | CPU | RAM | Storage | Bandwidth | Estimated Price (ZAR/month) |
|------|-----|-----|---------|-----------|----------------------------|
| Entry Dedicated | Intel Xeon E-2136 | 32 GB | 2x 512 GB SSD | Unmetered | R1,500-R2,000 |
| Mid Dedicated | Intel Xeon E-2288G | 64 GB | 2x 1 TB SSD | Unmetered | R2,500-R3,500 |

**Key Advantages:**
- ZAR billing (no forex risk)
- Extremely competitive pricing — 5-10x cheaper than hyperscalers
- 20 TB bandwidth included (hyperscalers charge per GB egress)
- South African data centres (POPIA compliant)
- Local support (SA business hours)
- Unmanaged VPS = full root access, install anything
- DNS, backups, and monitoring tools available

**Key Disadvantages:**
- No managed database service (must self-manage PostgreSQL)
- No managed IoT services
- No auto-scaling (fixed resources)
- Limited SLA compared to hyperscalers
- Manual server management required
- Single-provider risk

**Estimated Monthly Total:**
- **Pilot:** R120-R250/month (VPS CX11 or CX21)
- **Full scale:** R350-R800/month (VPS CX31 or CX41)

**Verdict:** Outstanding value for money. A Hetzner CX31 (2 vCPU, 8 GB RAM, 80 GB SSD) at ~R400/month would comfortably run the entire platform at full scale. This is roughly 10x cheaper than equivalent hyperscaler resources.

---

### 4.2 Afrihost

**Data Centres:** Johannesburg (Teraco)
**Website:** https://www.afrihost.com

Afrihost is a well-known South African ISP and hosting provider. They offer cloud VPS hosting on infrastructure located at Teraco data centres in Johannesburg.

**Cloud VPS Options:**

| Plan | vCPUs | RAM | Storage | Bandwidth | Estimated Price (ZAR/month) |
|------|-------|-----|---------|-----------|----------------------------|
| Starter | 1 | 1 GB | 25 GB SSD | 1 TB | R99-R149 |
| Standard | 2 | 4 GB | 50 GB SSD | 2 TB | R299-R399 |
| Pro | 4 | 8 GB | 100 GB SSD | 3 TB | R599-R749 |

**Key Points:**
- ZAR billing
- Teraco data centres (premium SA facility)
- Less developer-focused than Hetzner
- Bandwidth limits are lower than Hetzner
- Phone and email support
- Suitable for smaller workloads

**Estimated Monthly Total:**
- **Pilot:** R149-R299/month
- **Full scale:** R399-R749/month

---

### 4.3 RSAWEB

**Data Centres:** Johannesburg, Cape Town
**Website:** https://www.rsaweb.co.za

RSAWEB is a South African connectivity and cloud provider with its own data centres.

**Cloud / VPS Options:**

| Plan | vCPUs | RAM | Storage | Estimated Price (ZAR/month) |
|------|-------|-----|---------|----------------------------|
| Small VM | 2 | 2 GB | 40 GB | R250-R400 |
| Medium VM | 2 | 4 GB | 80 GB | R450-R650 |
| Large VM | 4 | 8 GB | 160 GB | R750-R1,000 |

**Key Points:**
- South African data centres
- ZAR billing
- Enterprise-focused (may have higher pricing)
- Good connectivity infrastructure
- Less self-service, more engagement-based

---

### 4.4 Xneelo (formerly Hetzner)

**Note:** Xneelo is the rebranded consumer/SME arm of the original Hetzner South Africa. They focus on shared hosting, WordPress, and managed services. For VPS/cloud workloads, Hetzner South Africa (hetzner.co.za) remains the relevant option. Xneelo is not suitable for this use case.

---

### 4.5 Local Provider Comparison

| Factor | Hetzner SA | Afrihost | RSAWEB |
|--------|-----------|----------|--------|
| **Pilot Cost** | R120-R250 | R149-R299 | R250-R400 |
| **Full-scale Cost** | R350-R800 | R399-R749 | R750-R1,000 |
| **Data Centre** | CPT + JHB | JHB (Teraco) | JHB + CPT |
| **Bandwidth** | 20 TB included | 1-3 TB | Varies |
| **Managed DB** | No | No | No |
| **Root Access** | Yes | Yes | Yes |
| **ZAR Billing** | Yes | Yes | Yes |
| **Self-service** | Excellent | Good | Limited |
| **Developer Tools** | Good (API, CLI) | Basic | Basic |
| **Reputation** | Excellent (dev community) | Good (consumer) | Good (enterprise) |

---

## 5. On-Premises Options

The client has confirmed infrastructure suitable for on-premises hosting:
- Comms room with rack space, power, UPS, ethernet, and security
- Rooftop access for antenna placement
- Ubiquiti/UniFi managed network with Cat6, VLANs, 1 Gb fibre
- Sumir (in-house IT technician) for day-to-day monitoring

### 5.1 Hardware Options

#### Option A: Proxmox on Mini PC (Recommended for On-Prem)

Matches the Fairfield Water reference architecture. Run Proxmox virtualisation on a small-form-factor PC, with the application running in an LXC container.

**Hardware Candidates:**

| Device | CPU | RAM | Storage | Power Draw | Estimated Cost (ZAR) |
|--------|-----|-----|---------|------------|---------------------|
| **Intel NUC 13 Pro** (NUC13ANKi5) | Intel Core i5-1340P (12 cores) | 16-32 GB DDR4 (user-installed) | M.2 NVMe SSD (user-installed) | 28W TDP | R8,000-R12,000 (bare) + R1,500 RAM + R1,000 SSD = **R10,500-R14,500** |
| **Beelink SER5 Pro** | AMD Ryzen 5 5600H (6 cores) | 16 GB DDR4 (included) | 500 GB NVMe SSD (included) | 54W TDP | **R6,000-R8,000** |
| **Beelink EQ12** | Intel N100 (4 cores) | 16 GB DDR4 (included) | 500 GB NVMe SSD (included) | 6W TDP | **R3,500-R5,000** |
| **MinisForum UM690** | AMD Ryzen 9 6900HX (8 cores) | 32 GB DDR5 (included) | 512 GB NVMe SSD (included) | 45W TDP | **R10,000-R14,000** |

**Recommended On-Prem Configuration:**
- **Beelink SER5 Pro or Intel NUC 13 Pro** with 16 GB RAM and 500 GB NVMe SSD
- Install **Proxmox VE 8.x** as hypervisor
- Create LXC containers for:
  - ChirpStack (PostgreSQL + Redis + NS)
  - Water monitoring application (Node.js + Nginx)
  - MQTT broker (Mosquitto)
- Estimated total hardware cost: **R7,000-R12,000** (once-off)

**Why not Raspberry Pi 5?**
- 8 GB RAM maximum is insufficient for PostgreSQL + ChirpStack + application
- ARM architecture may have compatibility issues with some ChirpStack dependencies
- SD card storage is unreliable for database workloads (even with USB SSD)
- Limited to USB 3.0 for storage (no NVMe)
- Suitable for a gateway controller or lightweight edge device, but not for the main server
- Could be used for the LoRa gateway Packet Forwarder if needed

#### Option B: Rack-Mount Server (Overkill for Pilot, Suitable for Multi-Building)

If Mosaic Group plans to expand to additional buildings, a proper 1U/2U rack server could be justified.

| Device | Estimated Cost (ZAR) | Notes |
|--------|---------------------|-------|
| Dell PowerEdge R350 (refurbished) | R15,000-R25,000 | Xeon E-2300 series, 16-32 GB ECC RAM |
| HPE ProLiant DL20 Gen10+ | R20,000-R35,000 | Entry rackmount, ECC RAM, iLO management |

Not recommended for the current single-building scope. The mini PC approach is more appropriate.

### 5.2 On-Premises Cost Analysis

**Once-Off Capital Costs:**

| Item | Cost (ZAR) | Notes |
|------|-----------|-------|
| Mini PC (Beelink SER5 Pro or NUC) | R7,000-R12,000 | Server hardware |
| UPS (already available) | R0 | Client-provided |
| Ethernet (already available) | R0 | Cat6 infrastructure exists |
| Rack shelf/mount | R200-R500 | If needed for comms room |
| **Total CAPEX** | **R7,200-R12,500** | Once-off |

**Monthly Running Costs:**

| Item | Cost (ZAR/month) | Notes |
|------|------------------|-------|
| Electricity (~30W average) | R15-R30 | Marginal on building power |
| Cloudflare Tunnel (free tier) | R0 | Secure external access |
| Domain name (annual amortised) | R15-R20 | Already have meter-tracker.com |
| Backup storage (cloud, 10 GB) | R20-R50 | Off-site backup |
| **Total OPEX** | **R50-R100/month** | Ongoing |

**Break-even vs Cloud:** An on-prem setup at R10,000 CAPEX + R75/month OPEX vs Hetzner at R400/month breaks even at approximately **31 months**. Against a hyperscaler at R3,500/month, break-even is approximately **3 months**.

### 5.3 On-Premises Risks

| Risk | Severity | Mitigation |
|------|----------|------------|
| **Load shedding** | High | UPS covers comms room; mini PC draws <30W; UPS runtime adequate for Stage 4-6 |
| **Hardware failure** | Medium | Keep spare mini PC; automated cloud backup; Proxmox snapshot restore |
| **Internet outage** | Medium | ChirpStack + gateways continue to queue locally; meters store readings internally; data syncs when connectivity resumes |
| **Physical security** | Low | Comms room is access-controlled |
| **Maintenance burden** | Medium | Sumir (IT tech) handles daily; Jason handles updates remotely; Proxmox is low-maintenance |
| **No geo-redundancy** | Medium | Off-site backups (cloud); accept RPO of 24 hours for backups |
| **Fire/flood/theft** | Low | Building insurance; off-site backups; could redeploy in <4 hours to cloud |

---

## 6. Hybrid Architecture Options

Hybrid approaches split the workload between on-premises and cloud, optimising for latency, reliability, and cost. Three viable patterns exist for this project.

### 6.1 Pattern A: On-Prem ChirpStack + Cloud Application (Recommended Hybrid)

```
┌─────────────────────────────────────────────────────────────────────────┐
│  ON-PREMISES (Building Comms Room)                                     │
│                                                                        │
│  ┌─────────────┐    ┌─────────────────────┐    ┌────────────────────┐  │
│  │ LoRa        │───▶│ ChirpStack NS       │───▶│ MQTT Broker        │  │
│  │ Gateways    │    │ (PostgreSQL + Redis) │    │ (Mosquitto)        │  │
│  │ (x5)        │    │                     │    │                    │  │
│  └─────────────┘    └─────────────────────┘    └────────┬───────────┘  │
│                                                         │ MQTT/TLS     │
│                                                         │ via VPN or   │
│                                                         │ CF Tunnel    │
└─────────────────────────────────────────────────────────┼──────────────┘
                                                          │
                                              ┌───────────▼──────────────┐
                                              │  CLOUD (Hetzner / AWS)   │
                                              │                          │
                                              │  ┌────────────────────┐  │
                                              │  │ Web Application    │  │
                                              │  │ (Node.js + DB)     │  │
                                              │  │                    │  │
                                              │  │ API ──▶ CLTM ERP  │  │
                                              │  │ Dashboard          │  │
                                              │  │ Backups            │  │
                                              │  └────────────────────┘  │
                                              └──────────────────────────┘
```

**Rationale:** ChirpStack must be close to the LoRa gateways to minimise latency for the deduplication window (~200ms) and ADR adjustments. The web application and database can tolerate higher latency and benefit from cloud availability.

**Pros:**
- ChirpStack runs where the gateways are (lowest latency, most reliable)
- Web app in cloud = always available, even if building internet drops temporarily
- Cloud-hosted dashboard accessible from anywhere
- ERP integration runs from cloud (reliable API calls)
- Building-side operates independently if cloud is unreachable

**Cons:**
- Two environments to manage
- Requires reliable MQTT bridge between on-prem and cloud
- On-prem hardware still needed (mini PC)
- More complex deployment and monitoring

**Cost:**
- On-prem: R7,000-R12,000 CAPEX + R50-R100/month
- Cloud (Hetzner VPS): R250-R450/month
- **Total:** R7,000-R12,000 CAPEX + R300-R550/month OPEX

---

### 6.2 Pattern B: On-Prem Gateway + Cloud Everything (Simple Cloud)

```
┌─────────────────────────────────────────────────────────────────────────┐
│  ON-PREMISES (Building)                                                │
│                                                                        │
│  ┌─────────────┐    Packet Forwarder                                   │
│  │ LoRa        │──────────────────────────────────────────────┐        │
│  │ Gateways    │    (UDP/TCP to cloud ChirpStack)             │        │
│  │ (x5)        │                                              │        │
│  └─────────────┘                                              │        │
└───────────────────────────────────────────────────────────────┼────────┘
                                                                │
                                                    ┌───────────▼────────┐
                                                    │  CLOUD             │
                                                    │                    │
                                                    │  ChirpStack NS     │
                                                    │  Web Application   │
                                                    │  Database          │
                                                    │  MQTT Broker       │
                                                    │  Monitoring        │
                                                    │  ERP Integration   │
                                                    └────────────────────┘
```

**Rationale:** Simplest architecture. Gateways forward raw LoRa packets to a cloud-hosted ChirpStack instance. Everything else runs in the cloud.

**Pros:**
- Single environment to manage (cloud)
- No on-prem server hardware needed (gateways only)
- Simplest deployment and updates
- Matches client's 51% cloud preference
- Easiest for Jason to manage remotely

**Cons:**
- Gateway-to-cloud latency (~10-20ms to JHB/CPT) affects deduplication window — but this is acceptable for LoRaWAN (not a real-time control system)
- If internet drops, gateways cannot forward data — but ChirpStack gateways with local storage can buffer and replay
- All data transits the internet (encrypted but adds dependency)
- Monthly cloud costs ongoing

**Important Note on Latency:** For water metering with 5-minute reporting intervals, the 10-20ms latency to a South African cloud region is negligible. This is not a sub-millisecond industrial control system. The ChirpStack deduplication window (200ms) has plenty of margin for SA-based cloud hosting.

**Cost:**
- Cloud (Hetzner VPS CX31): R350-R450/month
- No on-prem server CAPEX
- **Total:** R0 CAPEX + R350-R450/month OPEX

---

### 6.3 Pattern C: Full On-Premises with Cloud Backup (Resilient On-Prem)

```
┌─────────────────────────────────────────────────────────────────────────┐
│  ON-PREMISES (Building Comms Room)                                     │
│                                                                        │
│  ┌─────────────┐    ┌─────────────────────┐    ┌────────────────────┐  │
│  │ LoRa        │───▶│ ChirpStack NS       │───▶│ Web Application   │  │
│  │ Gateways    │    │ (PostgreSQL + Redis) │    │ (Node.js + DB)    │  │
│  │ (x5)        │    │                     │    │ Dashboard         │  │
│  └─────────────┘    └─────────────────────┘    │ API ──▶ ERP      │  │
│                                                 └────────┬───────────┘  │
│                                                          │              │
│                                                  Cloudflare Tunnel      │
│                                                  (external access)      │
└──────────────────────────────────────────────────────────┼──────────────┘
                                                           │
                                               ┌───────────▼──────────────┐
                                               │  CLOUD (minimal)         │
                                               │                          │
                                               │  - Automated backups     │
                                               │  - DNS (Cloudflare)      │
                                               │  - Monitoring alerts     │
                                               │  - Disaster recovery     │
                                               └──────────────────────────┘
```

**Rationale:** Everything runs on-premises (matching Fairfield architecture). Cloud is used only for backups, DNS, and external access via Cloudflare Tunnel.

**Pros:**
- Lowest ongoing cost
- Full data ownership on-premises
- No dependency on cloud provider for core functionality
- System operates fully during internet outages
- Proven architecture (mirrors Fairfield Water)
- Fastest possible latency between gateways and server

**Cons:**
- Single point of failure (one server in one location)
- Hardware failure requires on-site intervention
- Load shedding risk (mitigated by UPS)
- Less accessible for remote management (though Cloudflare Tunnel helps)
- All eggs in one basket

**Cost:**
- On-prem: R7,000-R12,000 CAPEX + R50-R100/month OPEX
- Cloudflare (free tier): R0
- Cloud backup (50 GB): R20-R50/month
- **Total:** R7,000-R12,000 CAPEX + R70-R150/month OPEX

---

### 6.4 Hybrid Pattern Comparison

| Factor | Pattern A (Split) | Pattern B (Cloud) | Pattern C (On-Prem) |
|--------|-------------------|-------------------|---------------------|
| **CAPEX** | R7,000-R12,000 | R0 | R7,000-R12,000 |
| **Monthly OPEX** | R300-R550 | R350-R450 | R70-R150 |
| **Complexity** | High (2 environments) | Low (1 environment) | Low (1 environment) |
| **Internet dependency** | Partial | Full | Minimal |
| **Load shedding resilience** | High (core on UPS) | Low (internet-dependent) | High (UPS-backed) |
| **Remote management** | Easy (cloud portion) | Easiest | Moderate (via tunnel) |
| **Scalability** | Good | Best | Limited |
| **Data sovereignty** | On-prem + cloud | Cloud only | Fully on-prem |
| **Gateway-to-NS latency** | <1ms | 10-20ms | <1ms |
| **Recovery from failure** | Fast (cloud portion persists) | Fast (cloud restore) | Slow (hardware replacement) |
| **Matches Fairfield** | Partially | No | Yes |

---

## 7. South African Operational Considerations

### 7.1 Load Shedding (Eskom)

**Current Status (January 2026):** South Africa has experienced reduced load shedding through late 2025 and into 2026, but the grid remains fragile and stages 1-4 can be implemented at short notice. Durban (eThekwini Municipality) follows the national schedule.

**Impact Analysis:**

| Component | Impact of Power Loss | Mitigation |
|-----------|---------------------|------------|
| **LoRa gateways** | Stop receiving/forwarding | PoE from UPS-backed switches (confirmed on-site) |
| **On-prem server** | Shuts down | UPS-backed comms room; mini PC draws <30W = extended UPS runtime |
| **Smart meters** | Continue counting (battery-powered) | No impact; meters store readings locally |
| **Building internet** | Router/ONT may lose power | Depends on Vox OLT/ONT power; fibre ONTs need power |
| **Cloud server** | No impact | Data centre has generators |

**UPS Runtime Estimate:**
A typical 1500VA UPS (900W usable) powering a 30W mini PC + 100W of network switches would provide approximately **5-6 hours** of runtime. This covers Stage 4 load shedding (4.5 hours) with margin.

**Key Point:** The smart meters and LoRa nodes are battery-powered and continue recording consumption even during total power failure. When power returns, the gateways come online and receive the next uplink with the current cumulative reading. No data is lost — only the real-time visibility is temporarily interrupted.

For cloud hosting, the data centre has generator backup and is unaffected by load shedding. However, the building's internet connection and LoRa gateways are affected regardless of where the server is hosted.

### 7.2 Internet Connectivity

**Building Internet:** 1 Gb fibre from Vox (confirmed on-site). Vox is a reputable South African ISP with good uptime. Fibre is significantly more reliable than DSL or LTE.

**Durban Connectivity:** Durban is well-served by fibre infrastructure from multiple providers (Vumatel, Openserve, DFA, Octotel, MetroFibre). Vox uses these underlying networks.

**Risks:**

| Risk | Likelihood | Impact | Mitigation |
|------|-----------|--------|------------|
| ISP outage (Vox) | Low | Data not forwarded to cloud | On-prem buffering; meters store locally |
| Fibre cut (physical) | Very Low | Extended outage | Dual ISP (expensive); LTE failover (cheap) |
| Building ONT power loss | Medium (load shedding) | Internet down during outage | UPS for ONT; or LTE failover on gateway |
| DNS/Cloudflare outage | Very Low | Dashboard inaccessible | Direct IP access as backup |

**Recommendation:** Add a **4G/LTE failover dongle** (R200/month for data) to the primary gateway or server. This provides internet continuity during fibre outages and costs less than a second ISP line.

### 7.3 POPIA (Protection of Personal Information Act)

The system processes personal information (tenant names, unit numbers, water consumption data, billing data). POPIA compliance is mandatory.

**Requirements:**

| POPIA Requirement | Cloud Implication | On-Prem Implication |
|-------------------|-------------------|---------------------|
| **Data must be processed lawfully** | Same regardless of hosting | Same regardless of hosting |
| **Data must be stored securely** | Cloud provider handles physical security; encryption in transit and at rest required | Physical security of comms room; disk encryption required |
| **Data must remain in South Africa** | Must use SA-region cloud (AWS af-south-1, Azure SA North, GCP africa-south1, or SA local provider) | Inherently compliant (data on-premises in Durban) |
| **Cross-border transfer restrictions** | Backups must stay in SA; no US/EU replication without consent | No cross-border transfer by default |
| **Right to access and deletion** | API/tooling required regardless | API/tooling required regardless |
| **Responsible party obligations** | Mosaic Group is the responsible party; Jason/Precept is the operator | Same |
| **Operator agreement required** | Yes — between Mosaic Group and cloud provider, and Mosaic Group and Precept Systems | Yes — between Mosaic Group and Precept Systems |

**Key Point:** All three hyperscalers (AWS, Azure, GCP) and all local providers (Hetzner, Afrihost, RSAWEB) have South African data centres. POPIA data residency is satisfied by any of them. On-premises hosting is inherently compliant.

**Recommendation:** Include a POPIA operator agreement clause in the proposal. Regardless of hosting choice, ensure encryption at rest (disk encryption) and in transit (TLS/HTTPS).

### 7.4 Latency Requirements

**For water metering, latency is not critical.** The system reports every 5 minutes. Dashboard users can tolerate 100-200ms page loads without issue. API calls to the CLTM ERP system are asynchronous batch operations, not real-time.

The only latency-sensitive component is the **ChirpStack deduplication window** (200ms default). With a South African cloud region, the gateway-to-server latency is:

| Cloud Location | Latency from Durban | Fits in 200ms Window? |
|----------------|--------------------|-----------------------|
| Johannesburg | ~5-10ms | Yes (ample margin) |
| Cape Town | ~15-25ms | Yes (ample margin) |
| EU (Frankfurt) | ~150-180ms | Barely (risky) |
| US (Virginia) | ~250-300ms | No (unacceptable) |

**Conclusion:** Any South African hosting option provides acceptable latency. International hosting is not recommended due to deduplication timing and POPIA requirements.

---

## 8. Cost Comparison Summary

All costs in South African Rand (ZAR). Exchange rate assumption: R18.50/USD (January 2026).

### 8.1 Pilot Phase (6 devices, 1 floor + 5 units)

| Option | CAPEX (Once-Off) | Monthly OPEX | 12-Month Total |
|--------|------------------|--------------|----------------|
| **Hetzner SA VPS** | R0 | R120-R250 | R1,440-R3,000 |
| **Afrihost VPS** | R0 | R149-R299 | R1,788-R3,588 |
| **AWS EC2 (af-south-1)** | R0 | R1,500-R2,300 | R18,000-R27,600 |
| **Azure VM (SA North)** | R0 | R1,500-R2,500 | R18,000-R30,000 |
| **GCP Compute (africa-south1)** | R0 | R1,200-R2,000 | R14,400-R24,000 |
| **On-Prem (mini PC + Proxmox)** | R7,000-R12,000 | R70-R150 | R7,840-R13,800 |
| **Hybrid (On-prem CS + Hetzner app)** | R7,000-R12,000 | R300-R550 | R10,600-R18,600 |

### 8.2 Full Scale (588 devices, 12 floors)

| Option | CAPEX (Once-Off) | Monthly OPEX | 12-Month Total | 36-Month Total |
|--------|------------------|--------------|----------------|----------------|
| **Hetzner SA VPS** | R0 | R350-R800 | R4,200-R9,600 | R12,600-R28,800 |
| **Afrihost VPS** | R0 | R399-R749 | R4,788-R8,988 | R14,364-R26,964 |
| **AWS EC2 (af-south-1)** | R0 | R3,500-R5,500 | R42,000-R66,000 | R126,000-R198,000 |
| **Azure VM (SA North)** | R0 | R3,000-R5,000 | R36,000-R60,000 | R108,000-R180,000 |
| **GCP Compute (africa-south1)** | R0 | R2,800-R4,500 | R33,600-R54,000 | R100,800-R162,000 |
| **On-Prem (mini PC + Proxmox)** | R7,000-R12,000 | R70-R150 | R7,840-R13,800 | R9,520-R17,400 |
| **Hybrid (On-prem CS + Hetzner app)** | R7,000-R12,000 | R300-R550 | R10,600-R18,600 | R17,800-R31,800 |

### 8.3 Five-Year Total Cost of Ownership (Full Scale)

| Option | 5-Year TCO (ZAR) | Monthly Equivalent |
|--------|------------------|--------------------|
| **On-Prem (mini PC + Proxmox)** | R11,200-R21,000 | R187-R350 |
| **Hetzner SA VPS** | R21,000-R48,000 | R350-R800 |
| **Hybrid (On-prem + Hetzner)** | R25,000-R45,000 | R417-R750 |
| **Afrihost VPS** | R23,940-R44,940 | R399-R749 |
| **GCP** | R168,000-R270,000 | R2,800-R4,500 |
| **Azure** | R180,000-R300,000 | R3,000-R5,000 |
| **AWS** | R210,000-R330,000 | R3,500-R5,500 |

**Note:** On-prem TCO assumes one hardware replacement at year 3 (~R8,000). Hyperscaler costs assume on-demand pricing; reserved instances would reduce by 30-50%.

---

## 9. Recommendations by Phase

### 9.1 Pilot Phase (1 Floor + 5 Units)

**Recommended: Pattern C — Full On-Premises**

| Factor | Rationale |
|--------|-----------|
| **Why on-prem for pilot** | Lowest cost, fastest to deploy, mirrors proven Fairfield architecture |
| **Hardware** | Beelink SER5 Pro or Intel NUC (~R8,000) |
| **OS** | Proxmox VE 8.x with LXC containers |
| **Access** | Cloudflare Tunnel (free) for external dashboard access |
| **Cost** | ~R8,000 once-off + ~R100/month |
| **Risk** | Minimal — pilot is small scale; hardware failure during pilot is easily replaced |

The pilot validates the solution with 6 devices. The overhead of setting up a cloud environment is unnecessary at this stage. A mini PC in the comms room can be deployed in a day and provides immediate, low-latency access to the LoRa network.

If the pilot succeeds and the client prefers cloud for production, the on-prem mini PC can be repurposed as a dedicated ChirpStack/gateway server, and the application can be migrated to cloud (Pattern A hybrid).

### 9.2 Full Scale (576 Units + 12 Bulk Meters)

**Recommended: Pattern B — Cloud Hosted (Hetzner SA) with On-Prem Gateways**

| Factor | Rationale |
|--------|-----------|
| **Why Hetzner cloud** | Best cost-to-value for SA hosting; ZAR billing; no forex risk; POPIA compliant; low ongoing cost |
| **Why not hyperscaler** | 10x more expensive for equivalent resources; no managed IoT advantage (ChirpStack is self-hosted regardless); no material benefit for this workload |
| **Why not full on-prem** | Client's 51% cloud preference; better availability; easier remote management; no hardware replacement concerns |
| **Server** | Hetzner VPS CX31 (2 vCPU, 8 GB, 80 GB SSD) — ~R400/month |
| **Software** | ChirpStack + PostgreSQL + Redis + Node.js app + Nginx |
| **Gateways** | 5x LoRa gateways on-premises (these are always on-prem) |
| **Access** | HTTPS (Let's Encrypt or Cloudflare) |
| **Backups** | Automated to Hetzner Storage Box or S3-compatible storage |
| **ERP Integration** | API calls from cloud to CLTM ERP |

**Alternative if client insists on hyperscaler:** Azure South Africa North (Johannesburg) is the most cost-effective hyperscaler option, with two SA regions for geo-redundancy. Budget R3,000-R5,000/month.

### 9.3 Migration Path

```
Phase 1 (Pilot)          Phase 2 (Full Scale)        Phase 3+ (Multi-Building)
─────────────────        ──────────────────────      ────────────────────────────
On-Prem (mini PC)   ──▶  Hetzner VPS (cloud)    ──▶  Hetzner Dedicated or
                          + On-Prem gateways          Azure/AWS (if scale demands)
                                                      + On-Prem gateways per building
R8,000 + R100/mo         R400/mo                     R1,500-R5,000/mo
```

The architecture is portable. Node.js + PostgreSQL + ChirpStack run identically on any Linux environment. Moving from on-prem to cloud (or vice versa) requires:
1. PostgreSQL database dump and restore
2. ChirpStack configuration export
3. Application code deployment (git clone)
4. DNS/tunnel reconfiguration
5. Gateway packet forwarder reconfiguration (point to new server IP)

Estimated migration effort: **4-8 hours** for an experienced engineer.

---

## 10. Decision Matrix

Scoring: 1 (poor) to 5 (excellent). Weighted by relevance to this project.

| Criterion | Weight | Hetzner VPS | AWS af-south-1 | Azure SA North | On-Prem (Proxmox) | Hybrid (On-Prem + Hetzner) |
|-----------|--------|-------------|----------------|----------------|-------------------|---------------------------|
| **Cost (pilot)** | 20% | 5 | 2 | 2 | 4 | 3 |
| **Cost (full scale)** | 15% | 5 | 2 | 2 | 5 | 4 |
| **Ease of management** | 15% | 4 | 4 | 4 | 3 | 2 |
| **Reliability / uptime** | 15% | 4 | 5 | 5 | 3 | 4 |
| **POPIA compliance** | 10% | 5 | 5 | 5 | 5 | 5 |
| **Load shedding resilience** | 10% | 3 | 3 | 3 | 5 | 4 |
| **Scalability** | 5% | 4 | 5 | 5 | 2 | 4 |
| **Data sovereignty** | 5% | 5 | 5 | 5 | 5 | 5 |
| **Latency** | 5% | 4 | 4 | 4 | 5 | 5 |
| **Weighted Total** | 100% | **4.30** | **3.35** | **3.35** | **3.85** | **3.60** |

**Ranking:**
1. **Hetzner SA VPS** — 4.30 (best overall value)
2. **On-Premises** — 3.85 (best TCO, highest resilience)
3. **Hybrid** — 3.60 (good balance, more complex)
4. **AWS / Azure** — 3.35 (best availability, highest cost)

---

## 11. Conclusion and Next Steps

### Summary of Findings

1. **Hyperscalers are expensive and over-specified** for this workload. AWS, Azure, and GCP all have South African regions, but their pricing (R3,000-R5,500/month) is disproportionate to the requirements. Their managed IoT services do not add value when ChirpStack is the chosen network server.

2. **Hetzner South Africa offers the best cost-to-value ratio** for cloud hosting. At R350-R800/month for a full-scale deployment, it is 5-10x cheaper than hyperscalers while providing equivalent compute resources, South African data residency, ZAR billing, and generous bandwidth.

3. **On-premises hosting is the cheapest long-term option** and matches the proven Fairfield architecture. A mini PC (~R8,000) running Proxmox with LXC containers provides ample resources for 588 devices with minimal ongoing costs (~R100/month).

4. **The client's 51/49 split** suggests the decision should be framed not as cloud vs on-prem, but as **"where does each component run best?"** The gateways are always on-premises. ChirpStack works well in either location. The web application benefits from cloud availability.

5. **POPIA compliance is satisfied** by any South African hosting option. The key requirement is that personal data (tenant information, consumption records) remains within South African borders.

6. **Load shedding is manageable** with UPS infrastructure already in place. Battery-powered smart meters are unaffected. The on-prem mini PC draws minimal power. Data is never lost — only temporarily delayed.

### Recommended Approach

**Pilot:** Full on-premises (Pattern C) — deploy a mini PC in the comms room running Proxmox with ChirpStack and the web application. Access via Cloudflare Tunnel. Cost: ~R8,000 + R100/month.

**Full Scale:** Evaluate after pilot. If the client prefers cloud, migrate the web application to Hetzner SA VPS (Pattern B). Keep gateways on-premises (always). The mini PC can remain as a local ChirpStack server or be repurposed.

**Present to client as:** "Start on-premises to validate the solution at lowest cost. After pilot, choose the production hosting that best fits your operational preferences. The architecture is portable — moving to cloud is a straightforward migration."

### Next Steps

1. Include hosting options summary in the technical proposal
2. Present Hetzner SA VPS as the recommended cloud option (if cloud is chosen)
3. Present on-prem Proxmox as the recommended on-prem option (if on-prem is chosen)
4. Price both options in the proposal for Tanya to compare
5. Research CLTM ERP integration requirements (separate action item)
6. Confirm whether Vox fibre ONT at the building has UPS backup

---

## Appendix A: ChirpStack System Requirements (Reference)

ChirpStack v4 minimum requirements for 588 devices with 5-minute reporting:

| Component | Requirement |
|-----------|-------------|
| **CPU** | 2+ cores (ARM64 or x86_64) |
| **RAM** | 2 GB minimum, 4 GB recommended |
| **Disk** | 20 GB minimum (grows ~1 GB/year with 588 devices) |
| **PostgreSQL** | v14+ with `pg_trgm` and `hstore` extensions |
| **Redis** | v7+ |
| **OS** | Debian 12, Ubuntu 22.04 LTS, or any modern Linux |
| **Docker** | Optional but recommended (Docker Compose deployment) |
| **Network** | Ports 8080 (API), 1700 (UDP gateway), 1883 (MQTT), 8883 (MQTT/TLS) |

## Appendix B: Hetzner SA vs Hyperscaler Quick Reference

For the specific workload of this project (2 vCPU, 4-8 GB RAM, 80 GB SSD, 200 GB bandwidth):

| Provider | Product | Monthly Cost (ZAR) | Notes |
|----------|---------|-------------------|-------|
| Hetzner SA | VPS CX31 | ~R400 | 2 vCPU, 8 GB, 80 GB, 20 TB BW |
| AWS af-south-1 | t3.medium + EBS + egress | ~R3,500 | 2 vCPU, 4 GB, 80 GB, 200 GB egress |
| Azure SA North | B2s + disk + egress | ~R3,000 | 2 vCPU, 4 GB, 80 GB, 200 GB egress |
| GCP africa-south1 | e2-medium + disk + egress | ~R2,800 | 2 vCPU, 4 GB, 80 GB, 200 GB egress |

The hyperscaler premium is approximately **7-9x** for equivalent resources. This premium buys managed services, SLAs, auto-scaling, and enterprise support — none of which are critical requirements for this project.

## Appendix C: Vendor Pricing Verification Notes

Cost estimates in this document are based on published pricing (as of January 2026) and domain knowledge. Actual pricing should be confirmed by:
1. Creating trial accounts on AWS, Azure, and GCP to access their pricing calculators
2. Requesting a quote from Hetzner SA for the specific VPS configuration
3. Confirming current ZAR/USD exchange rates at time of proposal

All ZAR estimates assume an exchange rate of approximately R18.50/USD. Hyperscaler pricing is typically in USD and subject to forex fluctuation. Hetzner SA prices in ZAR directly.
