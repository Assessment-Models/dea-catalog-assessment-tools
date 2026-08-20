# CR-AM-01 — Decision Points and Rationale Index

> Authoritative reference for every material decision embedded in CR-AM-01 ("OpenDEA Assessment Metamodel Evolution"). Use this when implementing or reviewing any phase of the 4-release roadmap.
>
> This document is a **digest** of `../change-requests/CR-AM-01.md` and `CR-AM-01-companion-rationale.md`. The CR is the spec; this index is the navigation layer that points implementers at the *why*, the *decisions*, and the *consequences* of every section.
>
> **Land-as-authored rule:** When a section below quotes the CR verbatim, the quote is authoritative. When this document adds synthesis (e.g. "Implementation consequence"), the synthesis is advisory and explicitly marked.

---

## 0. Status snapshot

| Field | Value |
|-------|-------|
| CR ID | `CR-AM-01` (full: `DEA-CR-Assessment-001`) |
| Status | **Accepted** (this PR is the landing of the authoritative reference; implementation phases begin in subsequent PRs) |
| Priority | High |
| Scope | `Assessment-Models` organisation |
| Primary repo | `dea-catalog-assessment-tools` |
| Related repo | `dea-catalog-maturity-models` |
| Roadmap | 4 releases × 8 phases (see §32 / §22 of the CR) |
| Implementation strategy | Incremental, backward-compatible evolution — additive migration only (see §46) |
| First concrete milestone | Assessment Metamodel v1 + Assessment Result Model v1 + Compatibility Model v1 + Technology Assessment pilot migration (see §56) |

---

## 1. Decision points index (keyed to CR sections)

The table below makes every material decision in CR-AM-01 navigable. Each row points at the CR section, captures the decision in one sentence, and names the consequence for implementers.

| # | CR § | Decision | Consequence / what this forces |
|---|------|----------|--------------------------------|
| D1 | §1, §6 | The architecture separates **what** is assessed (Capability), **where/when** (Scenario), **how** (Assessment Model + Instrument), **what is observed** (Measure / Evidence), **what was produced** (Assessment Result), **how progression is interpreted** (Maturity Model), **how comparability is established** (Benchmark Model), and **how results are presented** (Assessment View). | Every concept becomes a first-class metamodel entity. Implementations must not collapse any two of these — each is independently reusable. |
| D2 | §3.1, §13, §34, AC-03, AC-04 | `maturity_target` becomes **optional**. Maturity Model is one *interpretation* of an Assessment Result, not the only output of an Assessment. | Phase 4 decouples maturity. Existing instruments with `maturity_target` are unaffected (backward-compat alias). New instruments can omit it. One result can be interpreted by multiple maturity models. |
| D3 | §3.2, §8, AC-02 | **Capability** becomes a first-class, independently referenceable model. One capability (e.g. `dea:capability-api-management`) can participate in many assessments. | Phase 5 introduces `dea-catalog-capability-models` (created only when content justifies it — §18, §32). Until then, capabilities may live as IDs referenced from assessment models. |
| D4 | §3.3, §9, §50, §51 | **Scenario** is the missing ingredient for defensible benchmarking. The benchmark unit is `Org × Capability × Scenario × AssessmentModel × MeasurementPeriod`, not `Org × Score`. | Phase 6 introduces scenarios selectively. "Benchmark what is sufficiently controlled; diagnose what is contextually variable" (§51) is the principle. |
| D5 | §3.4, §15, §23, AC-06, AC-07 | **Assessment Result** becomes a persistent, first-class object that stores the exact versions of every model it references. Subsequent model changes do not mutate historical results. | Phase 7 (`dea assess result` etc.) — results carry their lineage. CI must verify "Historical model references resolve" (§40, AC-07). |
| D6 | §3.5, §25, §26, §27, §28 | Semantic versioning alone is insufficient. Each model version declares its **compatibility** explicitly: `backward_compatible`, `scoring_compatible`, `result_compatible`, `benchmark_compatible`, `maturity_compatible`. | Version bumps are no longer inferred from semver. CI generates a compatibility matrix (§27). Benchmark eligibility is *declared*, not computed (§28). |
| D7 | §10, §11, §14 | Measurement is separated from scoring. Measures are observable characteristics with units and rules; ScoringModels are pluggable aggregation/normalisation rules. | The current 4-point scoring model becomes the **default** `ScoringModel`, not the metamodel. Quantitative, qualitative, telemetry-driven, evidence-based scoring all become expressible. |
| D8 | §16, AC-09 | The Enterprise Heatmap is a **view** over Assessment Results, not an Assessment Type. | Phase 4 + §50. The same result dataset produces Enterprise / Capability / Scenario / Maturity / Benchmark / Trend / Diagnostic views. |
| D9 | §17, §41 | A **controlled relationship vocabulary** replaces unrestricted `relationship_type` strings. | Minimum vocabulary: `assesses`, `implements`, `conforms-to`, `uses`, `measures`, `requires`, `supports`, `evaluates`, `occurs-in`, `produces`, `contains`, `interpreted-by`, `eligible-for`, `benchmarks`, `supersedes`, `derived-from`, `compatible-with`. Defined in the metamodel. |
| D10 | §19, §20, AC-01 | `instrument.schema.json` is **not** replaced. New schemas (`assessment-model.schema.json`, `assessment-result.schema.json`, etc.) are added alongside. Existing instruments keep validating. | Compatibility layer reads `maturity_target` as shorthand for `interpretation.maturity_models[*]`, and `produces-score-for` as alias for `interpreted-by`. Both supported during transition. |
| D11 | §22, §24, §23 | Stable IDs + exact component versions (`a:capability-api-management@1.2.0`). Lineage is recorded per version. | Models are composed from versioned components. New versions do not mutate historical references. |
| D12 | §42, §43 | Lifecycle states: `draft → experimental → alpha → beta → stable → deprecated → retired`. **Retired ≠ deleted.** | Definition required to interpret historical results must be retained after retirement (extends current schema's `draft/alpha/beta/stable/deprecated`). |
| D13 | §48 | The existing four-domain taxonomy (Modernization / Technology / Operations / Services Delivery) is **organisational metadata only**, not the upper-level ontology. | Domain ≠ Capability, Domain ≠ Assessment Type. Future assessments (e.g. "Autonomous Operations") may cross the four buckets. |
| D14 | §49, §50, §51 | A formal **Assessment Type taxonomy** is introduced: `enterprise-health`, `capability-assessment`, `maturity-assessment`, `diagnostic`, `baseline`, `readiness`, `scenario`, `comparative`, `benchmark`, `compliance`. An assessment may declare multiple purposes. | Replaces the implicit "assessment = maturity score" framing. |
| D15 | §32, §56 | First concrete milestone is **Technology Assessment pilot migration** because it can demonstrate Enterprise Health → Capability Assessment → Scenario-Based Benchmark in one domain. | The pilot proves the metamodel before the other three domains are migrated. |
| D16 | §54 | Priority tiers: P0 — Required (AssessmentModel, AssessmentResult, Capability ref, Version lineage, Compatibility, Backward compat); P1 — High Value (Scenario, Measure, Evidence, ScoringModel); P2 — Strategic (BenchmarkModel, Population, Normalisation); P3 — Advanced (Automated, Telemetry, AI evidence, Predictive, Continuous). | Sequencing: do not start P2 work until P0 is stable. |

---

## 2. Consequence table — what each implementable artefact must preserve

| Artefact | What it MUST continue to do | Reference |
|----------|-----------------------------|-----------|
| `instrument.schema.json` | Validate every existing instrument without modification | §20, AC-01 |
| `maturity_target` field | Continue to be valid; treated as shorthand for `interpretation.maturity_models` | §20 |
| `produces-score-for` relationship | Continue to be valid; alias for `interpreted-by` | §20 |
| Existing four domain repos | Continue to validate against `instrument.schema.json` | §17, AC-01 |
| Historical assessment results | Remain valid against the exact model versions that produced them | §23, AC-07 |
| `maturity-models/v1-alpha/*.yaml` files | Remain valid; maturity becomes optional interpretation, not removed | §13, D2 |
| Existing scoring bands (0–25 / 26–50 / etc.) | Continue as the default maturity scoring convention | §12 |

---

## 3. Implementation phasing (mirror of CR §32 / §22 with operational notes)

| Phase | What ships | Pre-condition |
|------|-----------|---------------|
| 1 — Metamodel Foundation | `dea-metamodel` repo: UML + JSON Schema + YAML examples + relationship vocabulary + versioning rules | None — this PR lands the reference; Phase 1 work begins in next PR |
| 2 — Schema Compatibility | `assessment-model.schema.json`, `assessment-result.schema.json`, `capability-ref.schema.json`, `scenario-ref.schema.json`, `measure.schema.json`, `evidence.schema.json`, `compatibility.schema.json` added under `dea-catalog-assessment-tools/schemas/`. `instrument.schema.json` unchanged. Compatibility validator added. | Phase 1 metamodel accepted |
| 3 — Refactor Existing Instruments | Each of the 4 domain instruments gains explicit `assessment_model`, `capabilities`, `scenarios`, `measures`, `evidence`, `scoring_model`, `maturity_models` blocks. No existing result invalidated. | Phase 2 schemas accepted |
| 4 — Decouple Maturity | Conceptual change: `Assessment → Maturity` → `Assessment → Result → Maturity`. `maturity_target` deprecated but supported. | Phase 3 instruments validated |
| 5 — Capability Catalog | `dea-catalog-capability-models` created (only when content justifies per §18). Initial capabilities derived from existing instrument dimensions. | Phase 4 accepted |
| 6 — Scenario Catalog | `dea-catalog-scenarios` created. Selective scenarios where benchmarking value is obvious (Digital Product Launch, Legacy Modernization, API Product Launch, Zero-Touch Operations, Cloud Migration, AI Adoption). | Phase 5 stable |
| 7 — Assessment Results | `dea-cli` extends: `dea assess run`, `dea assess validate`, `dea assess result`, `dea assess compare`, `dea assess benchmark`, `dea assess maturity`, `dea assess history`. | Phase 6 stable |
| 8 — Benchmarking | `BenchmarkModel`, `BenchmarkPopulation`, `PeerGroup`, `ComparabilityRule`, `NormalizationRule`, `BenchmarkResult`. | All of phases 1–7 stable |

**Operational rule:** every phase ships a scorecard. No phase auto-rolls into the next. (User convention, reaffirmed in this thread.)

---

## 4. Architectural principle (canonical quote, §5)

> Assessments define how something is measured; capabilities define what can be assessed; scenarios define the context in which it is assessed; maturity models define progression; benchmark models define comparability; assessment results preserve what was observed.

This principle governs every phase. If a proposed change violates any clause of this sentence, the change is wrong.

---

## 5. Critical insight (canonical quote, §11 of the companion rationale)

> Adding a question may technically be a MINOR change while materially changing the statistical interpretation of the resulting score. The model therefore requires version lineage and comparability metadata, not version numbers alone.

This is the single sharpest justification for D6 (explicit compatibility metadata). Every other design decision in CR-AM-01 follows from this insight.

---

## 6. Parking lot (items outside CR-AM-01's first release)

These items were either explicitly scoped out by the CR or have open decisions that this PR does not resolve:

| Item | Status | Where it gets resolved |
|------|--------|-----------------------|
| PR #1 (`docs/maturity-scoring-v2-proposal` on `dea-catalog-maturity-models`) — Emergent/Structured/Systematic/Adaptive/Self-Optimising rename + non-linear bands + effort coefficients | **Stays open** until CR-AM-01 Release 1 lands | When Release 1 begins, decide: fold into CR-AM-01 as v2 of the maturity-model bands, or close superseded |
| DMM-01 levels (Discrete / Converged / Composable / Cognitive / Autonomous) | **Parked**. Not a maturity ladder. Surfaces as input to a Capability Model under §8. | When `dea-catalog-capability-models` lands in Phase 5 |
| Naming of maturity levels (current CMMI names) | **Open** until PR #1 is resolved | PR #1 closure decision |
| Naming of DMM-01 levels (L4/L5 contrast sharpening) | **Parked** with DMM-01 itself | When the DMM-01 framing reappears in a Capability Model |

---

## 7. Acceptance Criteria (verbatim from §45, for quick reference)

| AC | Statement |
|----|-----------|
| AC-01 | Existing instruments continue to validate without modification |
| AC-02 | A single capability can be referenced by multiple assessment models |
| AC-03 | An assessment model can exist without a maturity model |
| AC-04 | One assessment result can be interpreted using more than one compatible maturity model |
| AC-05 | An assessment execution can reference a scenario |
| AC-06 | An assessment result stores the exact versions of all material assessment components |
| AC-07 | Updating an assessment model does not change previously finalized results |
| AC-08 | Results from incompatible assessment models cannot be silently included in a benchmark population |
| AC-09 | A heatmap can be generated from assessment results without requiring a dedicated heatmap assessment instrument |
| AC-10 | A new capability, measure, question, maturity model or scenario can be added without restructuring existing instruments |

---

## 8. Cross-references

| File | Purpose |
|------|---------|
| `../../change-requests/CR-AM-01.md` | The CR itself (authoritative spec) |
| `../../change-requests/README.md` | CR index for this repo |
| `CR-AM-01-companion-rationale.md` | Companion rationale (full `OpenDEAM_Assessment_tools.md` text) |
| `../../README.md` | Updated with Evolution Roadmap section pointing here |

---

## 9. Glossary of terms introduced or formalised by CR-AM-01

| Term | Meaning in CR-AM-01 context |
|------|------------------------------|
| **Assessment** | The conceptual evaluation activity (purpose, subject, scope, context, model, period, status) |
| **Assessment Model** | The semantic specification of an assessment (purpose, type, subject type, scenarios, capabilities, dimensions, measures, evidence, scoring, interpretation, benchmark compatibility) |
| **Assessment Instrument** | The executable/questionnaire realisation of an Assessment Model (self-assessment, workshop, survey, interview, automated, telemetry-driven, hybrid) |
| **Assessment Execution** | A specific instance of running an instrument |
| **Assessment Result** | The persistent output of an execution: scores, evidence, findings, maturity, benchmark, confidence, provenance |
| **Capability** | What an organisation is able to do, independently of how it is assessed |
| **Scenario** | The contextual conditions under which a capability is evaluated (market, regulation, scale, target outcome) |
| **Measure** | An observable characteristic with a unit and a measurement rule |
| **Evidence Requirement / Evidence / Observation** | The provenance layer supporting a measure |
| **Scoring Model** | Pluggable aggregation/normalisation rules (the current 4-point scale becomes the default) |
| **Maturity Model** | Optional interpretation of a result (was: coupled to Assessment; now: decoupled) |
| **Benchmark Model** | Comparability rules, population, peer group, statistical methodology, confidence |
| **Assessment View** | Projection of results — Enterprise / Capability / Scenario / Maturity / Benchmark / Trend / Diagnostic |