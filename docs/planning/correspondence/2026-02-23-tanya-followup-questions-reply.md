# Email: Reply to Tanya — Scalability, Hosting, Deployment, Operating Costs

**Reference:** MGW-COR-202602-019
**Date:** 2026-02-23
**From:** Jason van Wyk (jason@precept.co.za)
**To:** Tanya Dowley (tanya@mosaicgroup.co.za)
**CC:** Garth Le Roux (garth@precisionmeters.co.za), Bradley Cassani (bradley.cassani@precisionmeters.co.za)
**Subject:** RE: City Life Water Metering — Documents for Review
**In Reply To:** MGW-COR-202602-018 (Tanya's follow-up, 23 Feb 2026)
**Status:** SENT

---

Dear Tanya,

Good to hear from you — happy to clarify - apologies for the long email.

METER LIMIT AND OTHER SITES

There is no practical limit on the number of meters the system can handle. The platform is designed to scale — whether it's 7 meters for the pilot, 588 for City Life, or thousands across multiple buildings (bear in mind, scaling at some point does require upgrades in hardware - like bigger servers etc).

On the multi-site question — as noted in the business case (Section 10), Mosaic Group receives full ownership of the intellectual property: all source code, firmware, schematics, and documentation. There is no software licensing fee, per-meter charge, or per-site subscription. The platform is yours.

If you wanted to deploy the system at another Mosaic property, the software itself carries no additional cost — it's already built and you own it. What each new site would require is the physical hardware (meters, IoT nodes, gateways), a site assessment to understand the building layout and RF environment, and on-site installation and commissioning. But the platform, dashboards, reporting, and all the development work — that's a once-off investment that serves every site you roll it out to. Multi-site support is built into the platform design, so adding a new building is a matter of configuration rather than new development.

This is one of the key advantages of a custom-built, IP-owned solution over a SaaS subscription model, where you'd typically pay a recurring per-meter or per-site fee that grows with every new building.

HOSTING COSTS

You're right — because the system is hosted on-premises using your existing infrastructure, there is no monthly server rental or cloud hosting fee (i include this to give you an estimation of typical costs involved for this option). The server hardware (R12,000–R18,000 - also dependent on current hardware costs) is a once-off capital item already included in the infrastructure budget (Section 4.5), and it sits in your building on your network.

Your existing Ubiquiti network, fibre connection, UPS, and VLAN infrastructure is a real advantage here — we leverage what's already in place rather than building a parallel network.

That said, I do want to be transparent about what on-premises hosting means in terms of ongoing responsibility. The server is a physical machine in your comms room, and with that comes a few considerations:

Power and UPS — the server needs to stay powered. If the building UPS fails during load shedding, the platform goes offline until power is restored. The smart meters buffer their readings internally, so no data is lost — but live monitoring and alerts pause until the server is back up.
Internet connectivity — the platform needs your fibre connection for off-site cloud backups, remote monitoring access, and if you'd like to view dashboards from head office or other sites.
Network security — the server should sit on its own VLAN (which Sumir can configure) and not be exposed directly to the internet. Access from outside the building would go through a secure tunnel.
Hardware failure — if the server fails, it needs to be replaced. The system can be restored from backup, but there's a recovery window of a few hours while that happens.

None of this is unusual — it's the same as any on-site server — and your building is well set up for it. The three-tier backup strategy (local NAS, off-site cloud, and meter-level storage) is specifically designed so that no data is lost even if the server has a bad day.

The residual monthly amount I've listed (R1,000–R1,500/month) is not a hosting fee — that's the operating and maintenance component, which I'll cover in Question 4 below.

DEPLOYMENT AND TRAINING

I'll tighten these figures in the formal proposal and present firm numbers rather than ranges. To give you a sense of what's covered:

The plumbing work (physically installing each meter) is Ryan's domain — that's not included in these costs. My portion covers the technology side of getting 588 devices online and working:
Configuring and pairing each IoT node to its meter and to the LoRaWAN network (588 devices)
Placing and commissioning the 4 LoRaWAN gateways (positioning, signal testing, network configuration)
Provisioning every device on the network server (unique security keys per meter)
Floor-by-floor signal validation and data verification (confirming each meter is reporting correctly)
Integration testing with CLTM (end-to-end data flow from meter to your tenant management system)

On training, there are two audiences:

For Sumir — a hands-on technical walkthrough covering the platform administration, the network server, how to check device status, basic troubleshooting (gateway offline, meter not reporting), and backup verification. Estimated 3–4 hours.

For yourself and building management — a dashboard and reporting session covering how to read consumption data, generate reports, understand alerts (what a leak alert looks like, what action to take), and how the billing data feeds into CLTM. Estimated 2–3 hours.

A good portion of this happens naturally during the pilot — Sumir will learn the system through the commissioning process, and you'll see the dashboards in action during the evaluation period. The formal training is really a structured handover to make sure nothing falls through the cracks.

ANNUAL OPERATING COST (R12,000–R18,000)

This covers three items:

Remote platform monitoring — R500/month. I monitor the system remotely on an ongoing basis: server health, gateway connectivity, meter reporting status, backup completion, and alert response. If a gateway goes offline, a batch of meters stops reporting, or disk space is running low, I pick it up and investigate before it becomes a problem for you. This is proactive — the goal is that you never have to think about the infrastructure.

Off-site cloud backup — R100–R200/month. The system backs up daily to a South African-hosted cloud service. This is the disaster recovery layer — if anything happens to the on-site server or NAS (hardware failure, theft, fire), the complete system can be restored from the cloud. This is a small but important running cost, and it's a real cost for the storage service.

Maintenance reserve — R400–R800/month. This covers ongoing upkeep: operating system security patches, software updates, firmware updates for IoT nodes, minor configuration changes, and ad-hoc technical support when Sumir or your team has a question. Without a retainer like this, every phone call or small request becomes a separate billable job at the hourly rate. The reserve smooths that out and keeps things simple.

You have options here depending on how much responsibility Mosaic Group wants to take on:

Option A: Full support (as quoted) — R1,000–R1,500/month. I handle monitoring, patching, backups, troubleshooting, and ad-hoc support. Your team keeps the power on and the network running. This is my recommendation, particularly for the first 12 months while the system is new.

Option B: Backup only — R100–R200/month. Off-site cloud backup continues, but Mosaic Group handles everything else: server monitoring, security patching, troubleshooting, and updates. This would require Sumir to be comfortable managing a Linux server running containerised applications — which is a step beyond managing network switches and access points. This could be a good option after 6–12 months once Sumir has had time with the system.

Option C: Fully self-managed — R0/month. No off-site backup, no remote support. Local NAS backup only (which is included in the infrastructure capital costs). Full responsibility sits with Mosaic Group. Any support requests would be billed at the standard hourly rate. I would not recommend this for a system protecting a R2 million investment — the off-site backup alone is worth the R100–R200/month.

For context, a third-party SaaS platform providing equivalent monitoring and support would typically charge R5,000–R15,000/month — and you wouldn't own the software.

My recommendation is Option A for the first year, with the option to step down to Option B once Sumir is comfortable and the system has been running stably. We can revisit this at the end of the pilot or after the full rollout — by then you'll have a clear picture of what's involved and can make an informed decision - ultimately its your call.

I hope that gives you what you need. Happy to jump on a call if you'd like to talk through any of this further — or if you'd like me to walk Sumir through the technical side separately, I'm happy to do that too.

Kind regards,
Jason

---

## Internal Notes

### Key Points in This Email
- **No meter limit, no licensing fees** — strong IP ownership message, multi-site is configuration not development
- **Hardware scaling caveat added** — honest note that bigger deployments may need bigger servers
- **Cloud hosting option clarified** — included for reference, not a cost she'd incur with on-prem
- **On-prem responsibilities made transparent** — power, connectivity, security, hardware failure
- **Deployment costs explained** — tech commissioning not plumbing, will tighten in proposal with firm numbers
- **Training broken into two audiences** — Sumir (technical, 3-4 hrs) and Tanya/management (dashboard, 2-3 hrs)
- **Operating costs: three options presented** — full support, backup only, fully self-managed — with clear recommendation
- **SaaS comparison** reinforces value of custom approach
- **Offered call** and offered separate technical walkthrough with Sumir
- **"Ultimately its your call"** — respectful, non-pushy close on operating cost options
