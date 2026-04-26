# KVLCC2 GZ — expected output (v0.1)

Captured 2026-04-26 against MetaCAD WASM core build `180bd98b`. Loading: design draft, KG = 18.56 m, no FSC.

```
heel_deg | GZ_m   | tolerance
---------+--------+-----------
       5 | 0.184  | ±0.020
      10 | 0.378  | ±0.020
      15 | 0.592  | ±0.025
      20 | 0.821  | ±0.030
      30 | 1.176  | ±0.040
      40 | 1.243  | ±0.050   <-- approximate peak
      60 | 0.512  | ±0.060
```

## Acceptance

A run is considered a match if **every** heel angle in the table reads a GZ value inside the tolerance band shown.

A near-miss at one angle is allowed iff:
- the GZ-curve shape is monotone-then-decreasing (no spurious oscillation)
- the integrated area to 30° heel matches MetaCAD's reported value within 5 %
- vanishing angle (GZ = 0 again) matches within ± 2°

## Failure modes seen during MetaCAD development

These are documented from earlier internal QA — kept here so other naval-architecture engineers know what to look for if their numbers don't match:

- **Linear-φ interpolation on KN tables produces a zigzag GZ curve** at intermediate angles. Use cosine interpolation on KN, then reconstruct GZ. (Internal lesson 2026, see MetaCAD changelog.)
- **KN · sin(φ) vs KN sign convention drift** — depending on the cross-curves table layout, GZ formula is `GZ = KN − KG · sin(φ)` or `GZ = KN · sin(φ) − KG · sin(φ)`. Always verify which convention the vessel data was published in before comparing.
- **Trim sensitivity** — GZ at design draft assumes zero trim. A 0.5 m trim error rotates the curve and adds ~5 % error at 30°. Lock trim before pulling reference numbers.

## Provenance

- Captured: 2026-04-26
- WASM core build: `180bd98b`
- Status: **v0.1 — design-draft single-loading**. Multi-loading sensitivity sweep planned for v0.2.
