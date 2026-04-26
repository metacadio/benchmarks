# ONRT Capsize — Monte-Carlo 3-DOF

## What this case proves

MetaCAD's WebGPU-accelerated Monte-Carlo dynamics module reproduces the published capsize-probability behaviour of the **ONR Tumblehome** (DTMB 5613) under steep beam seas, within the speed envelope expected from a Rust + WebGPU pipeline running in a browser.

This is the numerical anchor for two public claims:

1. "44× faster than baseline JavaScript on the same machine."
2. "ONRT capsize probability matched to RMRS / IMO Second-Generation IS reference range."

## Vessel

**ONR Tumblehome** — David Taylor Model Basin Model 5613, well-characterised tumblehome destroyer-type combatant used as a public benchmark in IMO MSC.1/Circ.1627 (Second-Generation Intact Stability). 841 panels.

## Conditions

- Sea state: H<sub>s</sub> = 4.0 m, T<sub>p</sub> = 9.0 s, JONSWAP γ = 3.3
- Heading: pure beam seas (90°)
- Speed: 0 kn (drift)
- Loading: design displacement
- Sims: 30 independent realisations
- Steps: 1,200 timesteps per sim
- DOF: 3 (heave, roll, pitch); linear-c44 roll damping
- RNG: Mersenne Twister 64, seed `0x4D436` (= "MC" hex spelled out)

## Expected output (v0.1, captured 2026-04-26 vs build 180bd98b)

| Metric | Value | Tolerance |
|---|---|---|
| P(capsize) | **63 %** (19 of 30 realisations) | ± 5 % |
| Calm-water drift after 1200 steps | **0.00°** | < 0.10° |
| Wall-clock, full ensemble | **51.5 s** | × 1.5 max on equivalent hardware |
| Speedup vs JS baseline (same ensemble) | **44 ×** | ≥ 30× minimum to pass |

**Hardware reference:** 2023 mid-range laptop, integrated/discrete WebGPU adapter. Spec recorded in `inputs.json`.

## Reference data

- IMO MSC.1/Circ.1627 — Second-Generation IS, Annex 4 (vulnerability criteria, dead-ship + parametric roll for ONRT)
- Bassler & Belenky (2011) "Capsizing of damaged ships" — ONRT model-test capsize rates in similar sea states fall in the 50–70 % envelope, which our 63 % sits inside

## What 'pass' means

For external validation: open metacad.io → MC module → load ONRT preset → enter inputs from `inputs.json` → run. The reported P(capsize) must fall inside `63 % ± 5 %`. Wall-clock must be ≤ × 1.5 the value above on equivalent hardware.

If your run is outside the envelope, [open an issue](https://github.com/metacadio/benchmarks/issues) with: hardware spec, browser + version, recorded P(capsize), and wall-clock.

## Provenance

- Captured: 2026-04-26
- WASM core build: `180bd98b`
- Captured by: founder (single-laptop run, not committee-validated yet)
- Status: **v0.1 — single-machine claim**. Multi-machine and CI-replay coming v0.2.
