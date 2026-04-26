# CII Bulk Fleet — expected output (v0.1)

Captured 2026-04-26. Engineering target — full fleet generator ships v0.2.

## Wall-clock budget

```
1,000 vessels × (CII attained + grade A-E lookup + Z-trajectory comparison)
─────────────────────────────────────────────────────────────────────────
target:  < 800 ms   (2023 mid-range laptop, single thread WASM)
ceiling: < 1,500 ms (any hardware with WebGPU adapter)
```

## Grade distribution (shape)

The synthetic fleet is generated to roughly track MEPC.354(78) reference-line behaviour for the 2026 Z-trajectory point. The expected grade split:

```
A: ~5 %    B: ~20 %    C: ~45 %    D: ~22 %    E: ~8 %     ±5 pp per grade
```

Expected grade *order* is C > B > D > E > A (a typical aging-fleet shape under tightening Z-trajectory). If a run produces e.g. 60 % A or 40 % E, the inputs were not generated from this case's seed.

## Fleet-average CII attained (year = 2026)

```
3.6 ± 0.4 g CO₂ / dwt-nm
```

This sits at the mid-band of MEPC.354's dd-vector for bulkers in the < 279 k DWT class evaluated against the 2026 Z-trajectory point.

## Bit-exactness

Re-running with the same RNG seed (`0xC11`) **must** reproduce identical:

- The 1,000 vessels generated (DWT, distance, fuel mix per vessel)
- The CII attained value per vessel to 1 e-6 g CO₂/dwt-nm
- The grade letter assigned per vessel

If any of these vary across runs on the same machine, MetaCAD has a numerical-determinism regression. Please [open an issue](https://github.com/metacadio/benchmarks/issues).

## Acceptance for v0.1

This case is **v0.1: target-published, generator-pending**. A user cannot yet reproduce the wall-clock number end-to-end from this repository alone, because the fleet generator is not yet published.

**v0.2 (next release) ships:**

- `gen_fleet.py` — < 50 lines, regenerates the 1,000 vessels from the seed
- `expected_grades.csv` — the per-vessel grade for every vessel (1,000 rows)
- `harness.html` — a static HTML page that loads the CSV into the metacad.io fleet endpoint and reports actual wall-clock

Until then, the < 800 ms target is the engineering budget MetaCAD's emissions module is held to internally, not yet a community-reproducible fact.

## Provenance

- Captured: 2026-04-26
- WASM core build: `180bd98b`
- Reference framework: IMO MEPC.336/337/354/400
- Status: **v0.1 — engineering claim, generator pending v0.2**
