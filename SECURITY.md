# Security policy

This repository contains **fixtures and expected-output manifests only** — no executable code beyond the CI validation script. There is no security boundary to attack inside this repo.

If you discover a security issue in the **closed-source MetaCAD WASM core** that produces the numbers verified here:

- **Email** — mmotorin@gmail.com — with subject `[SECURITY] MetaCAD core <short title>`
- **Telegram** — [@fxadmins](https://t.me/fxadmins) — for a faster response
- Please **do not open a public issue** for security-sensitive reports

We commit to:

- Acknowledging the report within **72 hours**
- Providing a fix or mitigation plan within **30 days** for confirmed issues
- Crediting reporters by name (or anonymously, on request) in the public changelog

---

## Numerical-correctness reports

Numerical correctness is also a kind of safety in maritime software. If a benchmark produces numbers inside this repo's tolerances but **disagrees with class-society or IMO reference data**, please report it — open a regular issue with the [`numerical-discrepancy`](https://github.com/metacadio/benchmarks/issues/new/choose) template (or email if the discrepancy involves real DCS / charter data covered by NDA).

A capsize-probability error or a GZ-curve sign flip in safety-critical use is treated with the same seriousness as a security vulnerability.
