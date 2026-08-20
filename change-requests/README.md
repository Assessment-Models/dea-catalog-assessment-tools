# Change Requests — `dea-catalog-assessment-tools`

This index tracks every Change Request (CR) that has landed in this repository.

CRs follow the **land-as-authored** convention: the file in `change-requests/` is byte-identical to the originating attachment (md5-verified). Each CR is the authoritative spec; supplementary rationale lives in `docs/rationale/`.

| CR | Title | Status | Landed | Source PR |
|----|-------|--------|--------|-----------|
| [CR-AM-01](CR-AM-01.md) | OpenDEA Assessment Metamodel Evolution — Establish a Composable Assessment, Capability, Maturity and Benchmarking Model | Accepted | 2026-08-20 | this PR |

---

## CR-AM-01 quick links

| Document | Purpose |
|----------|---------|
| [`CR-AM-01.md`](CR-AM-01.md) | The CR itself (authoritative spec, 56 sections) |
| [`../docs/rationale/CR-AM-01-companion-rationale.md`](../docs/rationale/CR-AM-01-companion-rationale.md) | Companion rationale (the original explanatory document) |
| [`../docs/rationale/CR-AM-01-decision-points.md`](../docs/rationale/CR-AM-01-decision-points.md) | Decision-point index, phased roadmap, parking lot, ACs, glossary |

---

## Convention notes

- **Naming**: `CR-<series>-NN.md` where `<series>` is a short tag (e.g. `AM` for Assessment Metamodel, `MM` for a future specific maturity-model CR). Specific CRs that descend from CR-AM-01 will use their own compliant numbering.
- **Status values**: `Proposed` → `Accepted` → `Implemented` → `Superseded`. `Accepted` means the CR has landed as authoritative reference; `Implemented` is reserved for CRs whose phases are fully complete.
- **Backward compatibility**: CRs in this repo MUST NOT invalidate existing instruments. New schemas are added alongside `instrument.schema.json`, never as replacements (see CR-AM-01 §20 / §33).