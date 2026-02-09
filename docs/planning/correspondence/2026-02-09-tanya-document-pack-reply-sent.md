# Email: Reply to Tanya — Pilot Software, Valve Pricing & Commercial Structure

**Reference:** MGW-COR-202602-017
**Date:** 2026-02-09, 21:15
**From:** Jason van Wyk (jason@precept.co.za)
**To:** Tanya Dowley (tanya@mosaicgroup.co.za)
**CC:** Garth Le Roux (garth@precisionmeters.co.za), Bradley Cassani (bradley.cassani@precisionmeters.co.za)
**Subject:** RE: City Life Water Metering — Documents for Review
**In Reply To:** MGW-COR-202602-016 (Tanya's reply, 9 Feb 2026)
**Status:** SENT

---

Dear Tanya,

I'm glad you were seated - the numbers tend to have that effect :)

Thank you for taking the time to go through the documents, and great questions.

SOFTWARE FOR THE PILOT

The pilot would run on the full production platform - the same system, not a stripped-down demo. Everything you'd see at full scale (real-time dashboards, consumption analytics, leak detection alerts, usage reporting) would be live and functional from day one of the pilot. The only thing that changes between the pilot and the full rollout is the number of meters connected - the platform itself is ready at pilot.

The CLTM integration would also be scoped during the pilot - we'd work with Sumir's developer to agree the API specification, so that billing data flows through to your tenant management system as part of the evaluation.

MOTORISED BALL VALVES

The business case does note that valves are excluded but that the system is designed to be valve-ready from the start.

To give you a sense of ballpark costs, here's what the per-valve pricing looks like across the quality tiers for the 16mm unit valves (ex VAT):

- Budget imported (brass): ~R250/valve - Brass body, 12V DC, basic open/close, no position feedback. ~R144,000 for 576 units.
- Mid-range imported (stainless steel): ~R650/valve - Stainless steel, 12V/24V DC, position feedback, proven international brands. ~R374,000 for 576 units.
- Premium SA-sourced (stainless steel): ~R3,500/valve - Stainless steel, multiple voltage options, position feedback, local warranty and support. ~R2,016,000 for 576 units.

The 50mm floor-level valves (12 needed) range from ~R500 to ~R10,000 each depending on the same quality tier.

On top of the valve hardware, there would be additional installation costs (plumbing fittings and electrical connection per unit) in the region of R100,000 - R250,000 for the full 12-floor scope.

My recommendation would be the mid-range tier - stainless steel for potable water safety, position feedback so the system knows each valve's state, and proven brands with a track record in smart metering. I'd want to get formal quotes from a few suppliers to firm up the numbers if valves are something you'd like included.

There is one technical consideration worth mentioning: the smart meters run on long-life batteries (8-10 years, no wiring needed), but motorised valves draw significantly more power and would need a low-voltage power supply on each floor. The building already has power and UPS infrastructure on every floor, so this is straightforward - but it does add a wiring component to the installation that isn't there with meters alone.

We could do this is a phased rollout as well - deploy meters first (monitoring, billing, leak detection) and add valves as a second phase once the platform is up and running. The financial case is already strong on metering alone, and the valve-ready design means nothing gets replaced or reworked when valves are added later. That said, if remote shutoff is a priority for you, we can absolutely include valves in the scope from the outset - I'd just want to get firm pricing and factor in the power infrastructure before quoting.

PILOT COMMERCIAL STRUCTURE

Commercially, I'm happy to structure the pilot in a way that limits your upfront investment risk. For the pilot phase, you would pay for the hardware and installation costs, plus a 20% deposit on the platform development fees and hosting at cost for the duration of the pilot. The remaining development fees would only become payable once the pilot is installed, running, and you've had a chance to evaluate it properly.

To put approximate numbers to it (all ex VAT):

- Smart meters (7) from Precision Meters: ~R22,000 (invoiced by Precision directly)
- IoT hardware (7 nodes + 1 gateway): ~R12,000 - R14,000
- 20% deposit on platform development fees: ~R8,000 - R11,000
- Total pilot outlay: approximately R42,000 - R47,000

The remaining ~R32,000 - R44,000 in development fees would be deferred until after the pilot evaluation.

I'd suggest running the pilot for three months - that gives you and your board real consumption data, working dashboards, and live leak detection to assess before making any further commitment. After the evaluation period, if you're happy to proceed with the full rollout, we'd move to standard milestone-based payments for the remaining floors.

The intention is that you have something tangible and working before the bulk of the investment is on the table. I would formalise this in the proposal.

Happy to discuss/walk though any of this with you, Sumir, or Ryan.

Kind Regards,
Jason

---

## Internal Notes

### Key Points in This Email
- **Full production platform for pilot** — not a demo, same system as Fairfield
- **Valve pricing given as three tiers** — budget R250, mid-range R650, premium R3,500 per DN15 valve
- **Recommended mid-range tier** — SS304, position feedback, proven brands
- **Phased valve approach offered** — meters first, valves later (or valves from the start if she prefers)
- **Pilot commercial structure introduced** — 20% deposit on dev fees, hardware + install upfront, remainder deferred
- **Pilot costed at ~R42K-R47K total** (including meters from Precision)
- **3-month evaluation period** proposed
- **Offered to formalise in proposal**

### Pricing Basis (not in email)

Valve estimates based on research conducted 2026-02-09:

| Tier | DN15 Unit Price | DN50 Unit Price | Source |
|------|-----------------|-----------------|--------|
| Budget import (CWX-15Q brass) | ~R250 | ~R500 | AliExpress / direct China |
| Mid-range import (U.S. Solid SS304) | ~R650 | ~R1,200 | Amazon / U.S. Solid |
| SA-sourced (Pneumatron SS304) | ~R3,500 | ~R10,000 | Pneumatron.co.za |

### Suppliers to Contact for Formal Quotes (if Tanya wants valves)
- **Pneumatron** (SA) — pneumatron.co.za — DN15 + DN50 SS304 12V DC
- **Valvix** (SA) — sales@valvix.co.za — PAV DN15 + POM DN50
- **WET Technologies** (SA) — sales@wet-sa.co.za — KLD20S range
- **Precision Meters** (Bradley/Garth) — ask if they supply or can source valves
