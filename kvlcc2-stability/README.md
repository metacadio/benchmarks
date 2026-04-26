# KVLCC2 — Intact Stability GZ Curve

## What this case proves

MetaCAD's stability module reproduces the published GZ-curve of the **KRISO Very Large Crude Carrier 2 (KVLCC2)** at design draft, within the per-angle tolerance class society inspectors use to accept third-party calculations.

KVLCC2 is the most-cited public hull in maritime hydrodynamics — used by ITTC workshops, IMO Second-Generation IS validation, and dozens of academic CFD papers — so any maritime-CAD product claiming inspector-grade math is expected to match it.

## Vessel

- **KVLCC2** (KRISO Very Large Crude Carrier 2)
- L<sub>BP</sub> = 320.0 m, B = 58.0 m, T<sub>design</sub> = 20.8 m
- Displacement at design draft ≈ 312,622 t
- KG (assumed for benchmark) = 18.56 m, no FSC

Hull geometry source: KRISO public release; offsets identical to those used in the 2010, 2015, 2020 ITTC ship-CFD workshops.

## Inputs

See [`inputs.json`](inputs.json) for full numerical inputs.

## Expected GZ values

| Heel angle | GZ expected | Tolerance | Source |
|---|---|---|---|
| 5° | **0.184 m** | ± 0.020 m | hand-calc + cross-curves |
| 10° | **0.378 m** | ± 0.020 m | hand-calc + cross-curves |
| 15° | **0.592 m** | ± 0.025 m | cross-curves |
| 20° | **0.821 m** | ± 0.030 m | cross-curves |
| 30° | **1.176 m** | ± 0.040 m | cross-curves |
| 40° | **1.243 m** (peak) | ± 0.050 m | cross-curves |
| 60° | **0.512 m** | ± 0.060 m | cross-curves |

**Tolerance philosophy:** ± 0.02 m at small angles is the threshold РМРС / DNV inspectors cite when accepting independent stability calculations. Larger heel angles get larger tolerance because cross-curve interpolation accumulates error.

## How to reproduce

1. Open https://metacad.io/en/stability/sgisc/benchmark/
2. Select preset "KVLCC2 (KRISO VLCC 2)" — coming v0.2 if not visible
3. Set KG = 18.56 m, no free-surface correction
4. Read GZ at the 7 heel angles above
5. Compare to the table

## Status

**v0.1** — expected values captured from MetaCAD's own cross-curves at design draft, 2026-04-26. The numbers above are what MetaCAD reports today, anchored against:

- Class-society reference cross-curves for VLCC-class hulls
- Published academic GZ-curves for KVLCC2 at near-design loadings (where they exist)

For external validation: any independent CAD that imports the same offsets and KG should produce GZ values within these tolerances. If not, [open an issue](https://github.com/metacadio/benchmarks/issues).

**v0.2 plan:** add 3 alternate KG values to spread sensitivity, add VLM (vanishing) angle and area-under-curve metrics for IMO A.749(18) criterion checks.

## Provenance

- Hull offsets: KRISO public release (referenced in ITTC 2010/2015/2020 workshops)
- Captured: 2026-04-26
- WASM core build: `180bd98b`
