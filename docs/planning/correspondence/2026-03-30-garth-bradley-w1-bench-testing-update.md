# Email: W1 Bench Testing Update & Meter Architecture Discussion

**Reference:** MGW-COR-202603-001
**Date:** 2026-03-30
**From:** Jason van Wyk (jason@precept.co.za)
**To:** Garth Le Roux (garth@precisionmeters.co.za)
**CC:** Bradley Cassani (bradley.cassani@precisionmeters.co.za)
**Subject:** Mosaic Group — QALCOSONIC W1 Bench Testing Update & Meter Architecture Discussion
**Status:** SENT (2026-03-30)

---

## Summary

Update to Garth and Bradley on findings from bench testing the QALCOSONIC W1 purchased via RFQ P00044.

### Key Points

1. **W1 is a sealed unit** — integrated LoRaWAN radio, sealed battery, factory-set uplink interval (8hr default, 4hr minimum)
2. **Only 3–6 readings per day** — not sufficient for real-time monitoring platform as proposed to Tanya
3. **NFC/optical polling not viable** — would drain sealed battery and require physical reader at every meter
4. **The platform proposed to Tanya requires near-real-time data** — live dashboards, rapid leak detection, burst alerts
5. **Original architecture assumed pulse output** — external RAK3172 nodes controlling the LoRaWAN parameters

### Questions to Garth & Bradley

1. Does Precision offer (or can source) an ultrasonic meter with **electronic pulse output** in 15mm and 50mm?
2. Is there a variant **without integrated radio** (or at lower cost without it)?
3. Does Axioma make a "dumb" version of the W1 — ultrasonic measurement only, pulse/wired output?

### Two Paths Presented

| Path | Meter | Platform | Product |
|------|-------|----------|---------|
| **A: Open architecture (preferred)** | Ultrasonic + pulse output | Full Precept real-time monitoring | As proposed to Tanya |
| **B: Axioma ecosystem** | W1 as-is, 4-8hr reads | AMR-style billing platform | Different, simpler project |

### Tone

- Constructive, not critical of the W1
- Reaffirmed commitment to Precision Meters
- Offered to discuss on a call

---

## Internal Notes

- This is the first correspondence since 23 Feb (5-week gap)
- Tanya has not replied since 23 Feb either — her response may depend on this meter question being resolved
- The RFQ P00044 pulse output question was never formally answered by Garth
- If Precision can't supply a pulse-output meter, Jason may need to explore other ultrasonic meter suppliers — which conflicts with the Precision exclusivity commitment made to Garth on 4 Feb
- That tension is deliberately not raised in this email — one step at a time
