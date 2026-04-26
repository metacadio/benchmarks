# Contributing

This repository is **fixtures-only** — its job is to anchor every numerical claim MetaCAD makes on metacad.io. Contributions follow a tighter pattern than typical open-source repos.

## What we accept

| Welcome | Not the right repo |
|---|---|
| Reporting a mismatch between MetaCAD and these expected outputs | Feature requests for the metacad.io product |
| Adding a new reference case (vessel + conditions + expected output) | Bug reports against the closed-source WASM core |
| Tightening a tolerance band when published reference data justifies it | Marketing copy edits |
| Improving reproducibility instructions | Generic "make tests pass" PRs |

For bug reports against the live product, use the [contact form on metacad.io](https://metacad.io) or email `mmotorin@gmail.com`.

## Adding a new case

1. Create `<case-id>/` at the repo root
2. Drop in three required files:
   - `README.md` — what the case proves, vessel + conditions, how to reproduce, provenance
   - `inputs.json` — must include `case_id`, `captured_at`, `wasm_core_build` keys; CI rejects it otherwise
   - `expected.md` — expected output range with explicit tolerance per metric, plus failure-mode notes if you noticed any during development
3. Run the CI check locally if you can: `python -c "import json; json.load(open('<case-id>/inputs.json'))"`
4. Open a PR — the `validate` workflow will run automatically and must pass before merge.

## Reporting a mismatch

Open a [Validation Mismatch issue](https://github.com/metacadio/benchmarks/issues/new/choose). Include:

- Which case you ran (`onrt-capsize-mc` / `kvlcc2-stability` / etc.)
- Hardware (laptop year, OS, browser + version)
- WebGPU adapter (if relevant — Chrome `chrome://gpu/`)
- Exact numbers you observed
- The inputs.json content you fed in (whole file, not just diff)

If the mismatch involves NDA-covered data (real DCS records, charter party numbers), email `mmotorin@gmail.com` instead.

## Provenance discipline

Every expected number must carry traceability:

- A class-society or IMO reference document (e.g. MEPC.354(78), MSC.1/Circ.1627)
- Or a peer-reviewed publication
- Or a model-test report with public availability

"MetaCAD says so" is **not** acceptable as the sole source for an expected value, except in transitional v0.1 captures explicitly labelled `Status: v0.1 — single-machine claim`.

---

## License

By contributing, you agree your changes are licensed under [Apache-2.0](LICENSE) — the same terms as the rest of this repo.
