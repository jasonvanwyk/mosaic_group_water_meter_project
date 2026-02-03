# Garth Le Roux — Reply to Site Assessment & Meter Questions

**Date:** 2026-02-03, 11:31
**Reference:** MGW-COR-202602-002
**Type:** Email (inbound reply to MGW-COR-202601-004)

---

## Email: Garth Le Roux → Jason van Wyk

**From:** Garth Le Roux <garth@precisionmeters.co.za>
**To:** Jason van Wyk <jason@precept.co.za>
**Date:** 2026-02-03, 11:31
**Subject:** Re: Mosaic Group — City Life Building Site Assessment & Meter Requirements

Garth replied inline ("comments in red") to all 7 questions from the original email (MGW-COR-202601-004, sent 2026-01-30).

---

## Questions & Answers

| # | Question | Answer |
|---|----------|--------|
| 1 | LoRaWAN radio — standard or optional in 15mm? | **Standard** across all ultrasonic meter sizes. Wireless M-BUS also available as alternative. |
| 2 | 32-50mm spec sheet 3rd column — is it actually the 50mm? | **Yes.** Garth raised it with the relevant people. The second "40mm" on the brochure is the 50mm variant. |
| 3 | Volume pricing (15mm and 50mm) | **15mm ultrasonic (LoRaWAN): R2,100/meter ex VAT.** **50mm ultrasonic (LoRaWAN): R9,360/meter ex VAT.** Flat pricing — no volume tiers quoted. |
| 4 | ChirpStack device profile / payload codec | No device profile or codec supplied. They provide **APP & DEV keys** for meters to be programmed on our LoRaWAN system. Codec/profile to be developed by Jason. |
| 5 | Default uplink interval / NFC configurability | **Default: 8 hours.** Can be reconfigured prior to dispatch (not in-field NFC). **Minimum: 4 hours** — but affects battery life. |
| 6 | Tail ends — 15mm meter to 16mm pipe | **Tail pieces can be supplied with the meters** to keep connections on 15mm. |
| 7 | Supply chain — direct to client or via Precept? | **Direct to client preferred.** Garth will provide pricing to Tanya directly (linked to VAT claiming). Mosaic Group procures meters from Precision; Precept handles the platform/integration separately. Either way works for Precision. |

---

## Pricing Summary

| Item | Qty | Unit Price (ex VAT) | Total (ex VAT) |
|------|-----|---------------------|----------------|
| 15mm ultrasonic (LoRaWAN) — unit meters | 576 | R2,100 | R1,209,600 |
| 50mm ultrasonic (LoRaWAN) — bulk meters | 12 | R9,360 | R112,320 |
| **Total meters** | **588** | | **R1,321,920** |

### Pilot Phase

| Item | Qty | Unit Price (ex VAT) | Total (ex VAT) |
|------|-----|---------------------|----------------|
| 15mm ultrasonic (LoRaWAN) — pilot units | 6 | R2,100 | R12,600 |
| 50mm ultrasonic (LoRaWAN) — pilot bulk | 1 | R9,360 | R9,360 |
| **Pilot total** | **7** | | **R21,960** |

---

## Key Technical Notes

- **LoRaWAN is standard** — no need for separate SKU or add-on module
- **No ChirpStack codec provided** — Jason must develop payload decoder (APP + DEV keys supplied per meter)
- **Uplink interval trade-off** — 8hr default preserves battery; 4hr minimum available but shortens battery life
- **Tail pieces included** — resolves 15mm-to-16mm pipe compatibility concern
- **Spec sheet error confirmed** — 50mm data was mislabelled as 40mm on the brochure

## Commercial Arrangement

- **Precision Meters → Mosaic Group** (direct meter supply, Tanya invoiced directly)
- **Precept Systems → Mosaic Group** (telemetry platform, integration, installation support)
- Avoids 15% VAT markup (Precept is not VAT registered)

---

## Jason's Reply

**Date:** 2026-02-03, 11:56
**Subject:** Re: Mosaic Group — City Life Building Site Assessment & Meter Requirements

Brief acknowledgement: "Many thanks Garth - well received and will revert soonest"

---

## Action Items

- [ ] Develop ChirpStack payload codec for Precision Meters ultrasonic range
- [ ] Determine optimal uplink interval (4hr vs 8hr) based on battery life vs monitoring needs
- [ ] Build business case using confirmed pricing
- [ ] Coordinate with Garth re: direct pricing to Tanya
- [ ] Prepare proposal (Precept scope: platform + integration only, meters separate)
