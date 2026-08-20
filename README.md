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

> **Future evolution (per [CR-AM-01](change-requests/CR-AM-01.md)):** versioning will be extended with explicit compatibility metadata (`backward_compatible`, `scoring_compatible`, `result_compatible`, `benchmark_compatible`, `maturity_compatible`). SemVer alone is insufficient — adding a question can change the statistical interpretation of a score. See CR-AM-01 §25 / §26 for the full policy.

---

## Evolution Roadmap

This catalog is governed by an explicit, phased evolution roadmap defined in [CR-AM-01](change-requests/CR-AM-01.md): *"Establish a Composable Assessment, Capability, Maturity and Benchmarking Model"*.

The roadmap is **architectural evolution, not repository rewrite**. Existing instruments continue to validate throughout. New schemas are added alongside `instrument.schema.json`, never as replacements (CR-AM-01 §20 / AC-01).

### Architectural principle (canonical)

> Assessments define how something is measured; capabilities define what can be assessed; scenarios define the context in which it is assessed; maturity models define progression; benchmark models define comparability; assessment results preserve what was observed. — CR-AM-01 §5

### Phasing (4 releases, 8 phases)

| Phase | What ships |
|-------|-----------|
| 1 — Metamodel Foundation | `dea-metamodel` repo: UML + JSON Schema + YAML examples + controlled relationship vocabulary |
| 2 — Schema Compatibility | New schemas added under `schemas/` (`assessment-model.schema.json`, `assessment-result.schema.json`, `capability-ref.schema.json`, `scenario-ref.schema.json`, `measure.schema.json`, `evidence.schema.json`, `compatibility.schema.json`); `instrument.schema.json` unchanged |
| 3 — Refactor Existing Instruments | Each of the 4 domain instruments gains explicit `assessment_model`, `capabilities`, `scenarios`, `measures`, `evidence`, `scoring_model`, `maturity_models` blocks. **No existing result invalidated.** |
| 4 — Decouple Maturity | `maturity_target` becomes an optional interpretation reference, not a required coupling. Backward-compat alias retained. |
| 5 — Capability Catalog | `dea-catalog-capability-models` created (only when content justifies per CR §18) |
| 6 — Scenario Catalog | `dea-catalog-scenarios` created selectively where benchmarking value is obvious |
| 7 — Assessment Results | `dea-cli` extends: `dea assess run / validate / result / compare / benchmark / maturity / history` |
| 8 — Benchmarking | `BenchmarkModel`, `BenchmarkPopulation`, `ComparabilityRule`, `NormalizationRule`, `BenchmarkResult` |

**Operational rule:** every phase ships a scorecard. No phase auto-rolls into the next.

### Decision-point index

For the material decisions embedded in CR-AM-01 (D1–D16), the consequence table for each implementable artefact, and the parking lot, see [`docs/rationale/CR-AM-01-decision-points.md`](docs/rationale/CR-AM-01-decision-points.md).

### Companion rationale

The explanatory companion to the CR — covering the same architectural intent from a different angle (the user-authored "OpenDEAM_Assessment_tools.md") — lives at [`docs/rationale/CR-AM-01-companion-rationale.md`](docs/rationale/CR-AM-01-companion-rationale.md).

### CR rationale table

| CR | Why | Consequence |
|----|-----|-------------|
| [CR-AM-01](change-requests/CR-AM-01.md) | Current architecture couples Assessment → Score → Maturity, which prevents capability reuse, scenario-based benchmarking, longitudinal result analysis, and incremental evolution without invalidating historical scores. | Establishes an explicit Assessment Metamodel with Capability, Scenario, Measure, Evidence, ScoringModel, MaturityModel, BenchmarkModel, AssessmentView as independent, reusable, versioned entities. Backward-compatible migration. Technology Assessment is the first pilot. |

### What is **not** changing in this PR

- No new schemas are added.
- `instrument.schema.json` is untouched.
- No existing instrument, score, or maturity mapping is invalidated.
- No new repository is created (Capability/Scenario/Benchmark catalogs arrive in later phases, only when content justifies them).
- PR #1 on the maturity-models repo (`docs/maturity-scoring-v2-proposal` — rename / non-linear bands / effort coefficients) **stays open** until CR-AM-01 Release 1 lands. The two CRs converge there; this PR does not pre-empt that decision.

### Parking lot — superseded / parked items

- **DMM-01 levels** (Discrete / Converged / Composable / Cognitive / Autonomous) — proposed earlier in this thread as a separate maturity ladder. **Parked, not deleted, not promoted.** Under CR-AM-01 §8 the same conceptual content (enterprise + ecosystem as a single organism, progression across architectural conditions) is better modelled as a **Capability Model** than as a maturity ladder. DMM-01 will resurface as input to `dea-catalog-capability-models` when Phase 5 lands.
- **Maturity-band renames** (Ad Hoc → Emergent etc.) — tracked in PR #1 on the maturity-models repo. Resolved in CR-AM-01 Release 1.

---

## License

MIT — see `LICENSE`.