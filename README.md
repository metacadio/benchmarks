# MetaCAD Benchmarks

Reproducible validation cases for [MetaCAD.io](https://metacad.io) — the browser-native maritime CAD.

This repository is an **open-core validation manifest**: each subdirectory specifies a reference vessel, input conditions, and the numerical output range MetaCAD's WASM core produces. The core itself stays closed-source (compiled WASM under proprietary license), but every claim made on metacad.io about Rust speed, IMO benchmark match, or class-society reference traceability is anchored here so anyone — surveyor, journalist, investor, regulator — can verify it.

If a claim isn't in this repo, treat it as marketing.

---

## Cases (v0.1)

| Case | Reference | What we claim |
|---|---|---|
| [`onrt-capsize-mc/`](onrt-capsize-mc/) | ONR Tumblehome (DTMB 5613, 841 panels) | P(capsize) = 63 % ± 5 % at Hs = 4 m beam seas, 30 sims, 3-DOF, 1200 timesteps each |
| [`kvlcc2-stability/`](kvlcc2-stability/) | KVLCC2 (KRISO Very Large Crude Carrier 2, IMO/ITTC standard hull) | GZ-curve match within ±0.02 m at 5°/10°/15°/20°/30°/40° heel for design draft |
| [`cii-bulk-fleet/`](cii-bulk-fleet/) | 1,000 synthetic bulkers from MEPC.354(78) reference fleet | Wall-clock < 800 ms for full CII attained-rating + grade across the fleet on a 2023 laptop |

Each case directory contains:

- `README.md` — what the case proves and how to reproduce
- `inputs.json` — exact inputs (vessel parameters, sea state, sample size, RNG seed)
- `expected.md` — expected output range, with traceability to public reference data

## Versioning

Numbers are tagged. `v0.1` was captured 2026-04-26 against MetaCAD WASM core build `180bd98b`. When the core changes, expected outputs are re-captured in a new tag and the changelog notes any drift.

## License

[Apache-2.0](LICENSE). The fixtures and expected outputs are free to redistribute.
The closed-source WASM core that produces these numbers is **not** part of this repo.

## Reproducing

Today (v0.1): open metacad.io, load the named vessel preset, run the named module with the inputs in `inputs.json`, compare against `expected.md`. Visual reproduction.

Coming v0.2: `cargo run --release --example <case-name>` against a published `vesselcalc-validate` crate that exposes only the verification harness, not the core.

## Contact

- **Issues** — open one on this repo if numbers don't match
- **Email** — mmotorin@gmail.com
- **Live product** — [metacad.io](https://metacad.io)
