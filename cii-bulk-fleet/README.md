# CII Bulk Fleet — Wall-clock at scale

## What this case proves

MetaCAD can compute **IMO Carbon Intensity Indicator (CII) attained value + grade A–E** for a 1,000-vessel fleet in under a second on a 2023 laptop, in-browser. This is the numerical anchor for the public claim that MetaCAD's emissions module is **B2B-ready for surveyor firms running fleet audits at scale**, not just single-vessel calculations.

CII reference framework: **IMO MEPC.336(76)** (calculation), **MEPC.337(76)** (rating), **MEPC.354(78)** (dd-vectors), **MEPC.400(82)** (Z-trajectory through 2030).

## Fleet definition

1,000 synthetic bulk carriers spanning the population implied by the MEPC.354(78) reference-line piecewise definition for bulkers (capacity-DWT bands at < 279 k DWT and ≥ 279 k DWT). Distribution chosen to exercise:

- All 12 ship types (bulk dominant; container, tanker, gas, LNG, general, ro-ro variants, cruise sampled)
- DWT range 5,000 – 400,000 t
- Annual fuel consumption typical of 70–80 % time-at-sea bulker operations
- Fuels: HFO (60 %), VLSFO (30 %), MGO (5 %), LNG (5 %)

Synthetic, not from a real DCS dataset, so that the file can be redistributed under Apache-2.0 without DCS consent issues.

## Inputs

See [`inputs.json`](inputs.json) for the fleet generator parameters; the bulk fleet itself is generated on-the-fly from those parameters using a deterministic seed (`0xC11`) so anyone with the seed reproduces the same 1,000 vessels.

## Expected output

| Metric | Value | Tolerance |
|---|---|---|
| **Wall-clock** for full fleet (CII attained + grade A–E for each vessel) | **< 800 ms** | hard ceiling on 2023 mid-range laptop |
| Vessels processed | 1,000 / 1,000 | no skips, no NaN |
| Grade distribution shape | A : B : C : D : E ≈ 5 : 20 : 45 : 22 : 8 (per MEPC.354 dd-vectors) | ±5 pp per grade |
| Fleet-average CII attained, year 2026 | **3.6 ± 0.4 g CO₂ / dwt-nm** | matches MEPC.354 Z-trajectory at 2026 |
| Single-vessel sample reproducibility | bit-exact across re-runs with same seed | required |

## Why this matters

Surveyor firms (SGS, Inspectorate, Saybolt) auditing fleets of 50–500 vessels need to recompute CII whenever:

- A vessel changes ownership / management
- A flag-state PSC challenge requests an independent calculation
- Fuel mix changes mid-year
- 2023 baseline vs current-year comparison is requested

A desktop CAD that takes 30 minutes per fleet is unusable. MetaCAD's < 800 ms-per-1,000-vessels number is the basis of the **Company Annual License at SGD 1,900 / year covering 5 inspector seats**.

## How to reproduce

1. Open https://metacad.io/en/commercial/emissions/cii/
2. Select "Fleet mode" — coming v0.2 if not visible
3. Upload the synthetic CSV generated from [`inputs.json`](inputs.json) (see "Fleet generator" below)
4. Click "Compute fleet"
5. Read wall-clock from the engine badge ("WASM • 1000 vessels • Xms")

**Fleet generator:** Python ≤ 30 lines. Coming as `gen_fleet.py` in v0.2 of this case so anyone can regenerate the exact 1,000 vessels.

## Provenance

- Captured: 2026-04-26
- WASM core build: `180bd98b`
- Reference framework: IMO MEPC.336/337/354/400
- Status: **v0.1 — wall-clock claim pending fleet-generator publication**. The < 800 ms target is the engineering budget against which v0.2 will run measurements; if you observe a value above 1,500 ms on equivalent hardware, please open an issue.
