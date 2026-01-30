# Site Assessment Report & Meter Requirements Email

**Date:** 2026-01-30
**Reference:** MGW-COR-202601-004
**Attachment:** MGW-Site-Assessment-Report-2026-01-30.pdf

---

## Email 1: Jason van Wyk → Bradley Cassani, Garth Le Roux

**From:** Jason van Wyk <jason@precept.co.za>
**To:** Bradley Cassani <bradley.cassani@precisionmeters.co.za>
**CC:** Garth Le Roux <garth@precisionmeters.co.za>
**Date:** 2026-01-30
**Subject:** Mosaic Group — City Life Site Assessment Report & Meter Requirements
**Attachment:** MGW-Site-Assessment-Report-2026-01-30.pdf

---

Hi Bradley & Garth,

As promised, please find attached my site assessment report for Mosaic Group City Life Building (477 Anton Lembede Street, Durban). The report covers the building profile, water infrastructure, meter requirements, and my recommended solution architecture — including a detailed rationale for why I'm recommending a private LoRaWAN network and your ultrasonic meter range across the board (both unit and bulk).

In summary: 576 x 15mm unit meters + 12 x 50mm bulk meters = 588 total. Phased rollout, pilot first.

I have a few questions that I couldn't resolve from the datasheets alone:

1. Is the LoRaWAN radio standard in the 15mm ultrasonic, or is it an optional module / separate SKU?
2. On the 32–50mm spec sheet, the third column is labelled "40mm" but the flow rates are doubled and the body is shorter (255mm). Is that actually the 50mm variant?
3. Could you provide indicative pricing at these volumes?
   - 15mm ultrasonic (LoRaWAN): 6 (pilot), 48, 100, 576
   - 50mm ultrasonic (LoRaWAN): 1 (pilot), 12
4. Is there a ChirpStack device profile or payload codec available for the W1? (I run a private ChirpStack network.) - else happy to run with my own node.
5. What is the default uplink interval, and can it be configured via NFC?
6. We may need tail ends for the meters - I noted that your meters are 15mm and Ryan the plumber says his apartments have a 16mm intake - (not sure inner or outer dimension difference).

One commercial question: How should we handle the supply chain? I don't need to mark up the meters — my scope is the telemetry platform and integration. I'm not VAT registered, so if I were to purchase and resell, the client effectively pays 15% more than if you invoice them directly. Would it make more sense for Precision to supply the meters directly to Mosaic Group, with Precept handling the technology scope separately? Open to whatever works best from your side.

The client is reviewing pricing in the first week of February, so I'd appreciate any guidance you can provide before then.

Kind regards,

Jason van Wyk
Director | Odoo Specialist
Precept Systems (Pty) Ltd
Mobile: +27 83 288 9052
Email: jason@precept.co.za

---

## Questions Summary

| # | Question | Status |
|---|----------|--------|
| 1 | LoRaWAN radio — standard or optional in 15mm? | Awaiting reply |
| 2 | 32-50mm spec sheet 3rd column — is it 50mm? | Awaiting reply |
| 3 | Volume pricing (15mm and 50mm at pilot + scale quantities) | Awaiting reply |
| 4 | ChirpStack device profile / payload codec | Awaiting reply |
| 5 | Default uplink interval / NFC configurability | Awaiting reply |
| 6 | Tail ends — 15mm meter to 16mm pipe compatibility | Awaiting reply |
| 7 | Supply chain — direct to client or via Precept? (VAT implications) | Awaiting reply |
