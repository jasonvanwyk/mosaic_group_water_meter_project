# Email: Tanya Follow-Up Questions — Scalability, Hosting, Deployment, Operating Costs

**Reference:** MGW-COR-202602-018
**Date:** 2026-02-23, 08:37
**From:** Tanya Dowley (tanya@mosaicgroup.co.za)
**To:** Jason van Wyk (jason@precept.co.za)
**Subject:** RE: City Life Water Metering — Documents for Review
**In Reply To:** MGW-COR-202602-017 (Jason's reply, 9 Feb 2026)
**Status:** RECEIVED

---

Dear Jason,

I hope this mail finds you well.

Just a few more questions:

1. Is there a limit on the number of meters the system can handle. Will I be able to install this software at other sites without an additional cost.
2. There is reference to hosting costs- considering we have this is place onsite, I assume this cost falls away. Forgive me if I have not fully understood this cost.
3. Please relook at the deployment and Training amounts.
4. What is the annual operating amount of R12k-18K for.

Kind Regards,

---

## Internal Notes

### Analysis

**Tone:** Professional, detail-oriented, cost-conscious. She's scrutinising the numbers — strong buying signal. She's also thinking about multi-site rollout (Question 1) which suggests she sees this beyond City Life.

### Question 1 — Meter Limit / Multi-Site
- No practical limit on meter count. Platform is database-backed (SQLite/PostgreSQL), scales to thousands.
- **IP ownership (Section 10):** She owns all source code, schematics, firmware at project completion. No licensing fees.
- **Multi-site:** The software can be deployed to additional Mosaic properties at no additional software/licensing cost. Each new site would still require: site assessment, hardware (meters, nodes, gateways), site-specific configuration, installation, and commissioning.
- This is a major selling point — contrast with SaaS alternatives that charge per-meter or per-site subscriptions.

### Question 2 — Hosting Costs
- She's seen the cloud hosting option (R1,800-R3,000/month) in Section 4.7 and thinks her on-site infrastructure eliminates this.
- **She's mostly right.** The on-premises option (R1,000-R1,500/month) already avoids cloud VPS fees. That residual R1K-R1.5K/month is not "hosting" — it's monitoring, off-site backup, and maintenance reserve (see Q4).
- The server hardware is a one-time CAPEX item (R12K-R18K in Section 4.5), already included.
- Clarify: her existing Ubiquiti/UPS/fiber infrastructure is an advantage — we use it, don't duplicate it.

### Question 3 — Deployment & Training (R31,800-R48,000)
- Breakdown from Section 4.4:
  - Gateway config and network deployment: R9,000-R12,000 (15-20 hrs)
  - On-site installation support and commissioning: R18,000-R30,000 (30-50 hrs)
  - Training (Sumir, building management): R4,800-R6,000 (8-10 hrs)
- She may feel this is high since Ryan does the physical plumbing.
- **What Precept's portion covers:** IoT node configuration and pairing (588 devices), gateway placement and RF validation, ChirpStack device provisioning, system commissioning per floor, integration testing with CLTM, training Sumir on platform administration.
- **Consider:** Could reduce the estimate slightly or reframe. The 30-50 hrs on-site is for 12 floors × 48 units = 576 units. That's roughly 3-5 minutes per unit for node commissioning alone, plus floor-level gateway and bulk meter commissioning.
- **Action:** Offer to review and tighten in the formal proposal. Don't cut too aggressively — this is real work.

### Question 4 — Annual Operating R12K-18K
- From Section 4.7 (on-premises):
  - Platform monitoring (remote health checks, alerts, basic troubleshooting): R500/month
  - Cloud backup storage (off-site, SA-hosted): R100-R200/month
  - Maintenance reserve: R400-R800/month
- **Total: R1,000-R1,500/month = R12,000-R18,000/year**
- Explain each line item clearly.
