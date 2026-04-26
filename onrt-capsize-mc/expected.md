# ONRT Capsize MC — expected output (v0.1)

Captured 2026-04-26 against MetaCAD WASM core build `180bd98b` on a 2023 mid-range laptop with WebGPU enabled. Single-machine result; multi-machine validation pending v0.2.

## Primary metric

```
P(capsize) = 19 / 30 = 63.3 %
```

This sits inside the 50 – 70 % envelope reported in published model-test studies of the ONR Tumblehome under similar beam-sea conditions (Bassler & Belenky 2011 et seq.).

## Wall-clock

| Stage | Time |
|---|---|
| Pipeline init (panel load + WebGPU bind) | 1.8 s |
| 30 realisations × 1200 steps (compute) | 49.7 s |
| **Full ensemble end-to-end** | **51.5 s** |

## Comparison to baseline

| Implementation | Wall-clock for same ensemble | Speedup |
|---|---|---|
| v0 — pure-JS reference (math.js + naive RK4) | 2,252 s (37.5 min) | × 1.0 |
| v1.0 — WASM, static bias, fast-math | 921 s (15 min) | × 2.4 |
| **v1.1 — WASM + WebGPU + dynamic bias** | **51.5 s** | **× 44** |

The 44× headline figure on metacad.io references the v0 → v1.1 jump.

## Calm-water drift sanity check

After 1,200 steps in calm water (Hs = 0), the vessel should not drift more than 0.10° in roll. Observed: **0.00°** (numerical zero to single-precision tolerance). This guards against integrator instability dressed up as 'capsize'.

## How to fail this case

A run is considered a non-match if:

- P(capsize) < 58 % or > 68 % (5 % outside the band)
- Calm-water drift > 0.10°
- Wall-clock > 77.25 s (× 1.5) on equivalent hardware
- Any error or NaN in the output

If you observe a non-match, please [open an issue](https://github.com/metacadio/benchmarks/issues/new) with hardware spec, browser, recorded numbers, and the input JSON you used.

## Provenance

- Source memory: `project_mc_performance.md` (MetaCAD internal)
- Hardware: 2023 mid-range laptop (precise GPU model recorded in next capture)
- Single-run claim, not 95 %-CI bracketed; v0.2 will add 10-run averaging across 3 hardware tiers
