# dea-catalog-assessment-tools

Enterprise Architecture Assessment Instruments — scoring rubrics, gap analysis, target state design across architecture modernization, technology, operations, and services delivery.

---

## Overview

This is the **umbrella index** for the assessment catalog. The actual instruments live in four sibling repositories (one per domain) under the [Assessment-Models](https://github.com/Assessment-Models) organisation:

| Domain | Repository | Focus |
|--------|-----------|-------|
| Architecture Modernization | [dea-assessment-modernization](https://github.com/Assessment-Models/dea-assessment-modernization) | Legacy migration, strangler-fig adoption, decomposition strategy |
| Technology | [dea-assessment-technology](https://github.com/Assessment-Models/dea-assessment-technology) | Stack fitness, technical debt, lifecycle, skill coverage |
| Operations | [dea-assessment-operations](https://github.com/Assessment-Models/dea-assessment-operations) | Incident response, observability, deployment, SLOs |
| Services Delivery | [dea-assessment-services-delivery](https://github.com/Assessment-Models/dea-assessment-services-delivery) | Time-to-market, defect rates, customer outcomes |

This repo holds:
- The **catalog index** (`assessments/index.yaml`) — registry of all assessment instruments and their versions
- **Shared schemas** (`schemas/instrument.schema.json`) — uniform format all instruments must follow
- **Shared scoring rubric** (`assessments/scoring-rubric.md`) — bands and dimension framework
- **Methodology** — how to run a self-assessment vs a facilitated workshop

---

## When to use each instrument

| Situation | Recommended instrument |
|-----------|------------------------|
| Starting an architecture modernisation programme | `dea-assessment-modernization` |
| Reviewing technology stack health | `dea-assessment-technology` |
| Incident or reliability concerns | `dea-assessment-operations` |
| Delivery slipping, customer outcomes flat | `dea-assessment-services-delivery` |

For a holistic EA health check, run all four and combine into the `dea:maturity-ea-capability` score from [dea-catalog-maturity-models](https://github.com/Assessment-Models/dea-catalog-maturity-models).

---

## Catalog Schema

Every assessment instrument file (`assessments/<domain>/v1-alpha/instrument.yaml`) must follow `schemas/instrument.schema.json`:

```yaml
id:                  dea:assessment-<domain>
name:                <Human name>
domain:              <domain>
version:             "1.0.0-alpha"
metamodel_version:   "^0.1.0"
description:         <one-line summary>
maturity_target:     dea:maturity-<domain>   # which maturity model this scores

dimensions:          # 4-6 dimensions, each 0-25
  - id:               <dimension>
    name:             <human name>
    weight:           <0-1>
    questions:
      - id:           q1
        text:         <question>
        scoring:      [0, 1, 2, 3]    # 4-point scale
        evidence:     <what to look for>

total_questions:      <integer>
duration_minutes:     <integer>          # typical self-assessment time
facilitator_required: <bool>

relationships:
  - source_id: dea:assessment-<domain>
    target_id: dea:maturity-<domain>
    relationship_type: produces-score-for
```

---

## Scoring Bands

Uniform across all instruments — see `assessments/scoring-rubric.md`:

| Score | Band | Action |
|-------|------|--------|
| 0–25 | Ad Hoc | Strategy + foundation work |
| 26–50 | Defined | Tooling + rollout |
| 51–75 | Managed | Operationalisation + governance |
| 76–90 | Quantitatively Managed | Metrics + optimisation |
| 91–100 | Optimising | Continuous improvement |

---

## Related Catalogs

- [dea-catalog-maturity-models](https://github.com/Assessment-Models/dea-catalog-maturity-models) — produces maturity levels from assessment scores
- [dea-metamodel](https://github.com/technehub-labs/dea-metamodel) — entity definitions
- [dea-cli](https://github.com/technehub-labs/dea-cli) — runs `dea assess <domain>` against an instrument

---

## Versioning

- **MAJOR** — instrument question set fundamentally changes
- **MINOR** — new dimension or question added
- **PATCH** — clarifications, corrected phrasing

---

## License

MIT — see `LICENSE`.