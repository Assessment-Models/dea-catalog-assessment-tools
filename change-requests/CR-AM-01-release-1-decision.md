# CR-AM-01 Release 1 — Decision Record

Change Request ID: DEA-CR-Assessment-001-R1
Title: CR-AM-01 Release 1 — Legacy Cluster Disposition and Maturity Band Scheme
Status: Decided (2026-10-27) — D-1: Plan B (freeze legacy cluster); D-2: close superseded; D-3: instruments active; D-4: manual view now, generated view when fan-out lands
Decision Owner: Assessment-Models governance + technehub-labs/dea-metamodel owners
Primary Repository: dea-catalog-assessment-tools
Affected Repositories: dea-catalog-maturity-models, dea-assessment-{modernization,technology,operations,services-delivery}, assessment-registry, assessment-ci
Umbrella CR: CR-AM-01 — OpenDEA Assessment Metamodel Evolution (`change-requests/CR-AM-01.md`)
Related: CR-AM-11 — Federated Assessment Model Ecosystem; CR-AM-09 — v2 maturity scale architecture

⸻

## 1. Purpose

CR-AM-01 defines an 8-phase, multi-release roadmap. Several items were explicitly
defered to "Release 1" as a decision point. The ecosystem integration work
(Phases 1–4 of the integration plan) has now landed, which makes these decisions
the remaining blockers for a coherent end-state. This document records each
deferred decision, the options, and a recommendation, so governance can decide
in one pass.

## 2. Current state (as of this decision)

- The four assessment instruments are federated: contract handshake
  (`model.yaml`, `opendea-assessment-metamodel` v1.0), Level 1/2 CI, released
  and tagged `v1.1.0-alpha`.
- The registry indexes 7 assets with immutable references; Level 3 ecosystem CI
  validates registry↔manifest coherence on every PR.
- The instrument records carry `status: candidate-for-migration` pending this
  decision.
- `dea-catalog-maturity-models` is marked legacy (MIGRATION.md); its models use
  the v1-alpha linear bands and are not consumers of the federated contract
  suite.
- PR #1 on `dea-catalog-maturity-models` (`docs/maturity-scoring-v2-proposal`)
  remains open pending this decision.

## 3. Decision D-1 — Disposition of the v1-alpha legacy cluster

**Question:** Port the v1-alpha maturity models and instruments to v2-canonical
(Plan A), or freeze them as legacy with declared supersession (Plan B)?

### Option A — Port to v2-canonical (fold v2 in)

- Port the five maturity models in `dea-catalog-maturity-models` to the v2
  non-linear band scheme (Emergent / Structured / Systematic / Adaptive /
  Self-Optimising, per CR-AM-09 §17), each into its own `maturity-model-*`
  repository per the org taxonomy.
- Update each instrument's `maturity_target` to the ported model.

**Pros:** the org's user-facing assets score against a live, contract-conformant
scheme; the legacy coexistence period ends; instruments' `candidate-for-migration`
status resolves to `active`.

**Cons:** re-scoring is *breaking* — v1 linear bands and v2 non-linear bands
produce different maturity levels for the same raw scores, so historical
results are not comparable; requires 5 model ports + 4 instrument updates;
consumes the open PR #1 proposal as v2 band input.

**Cost estimate:** 5 model extractions/normalizations + registration records +
4 instrument retargets. The migration machinery (IDENTIFY → CLASSIFY → EXTRACT
→ NORMALIZE → VALIDATE → REGISTER → RELEASE) is already documented in
CR-AM-11 §26 Phase 6.

### Option B — Freeze as legacy (supersede)

- Keep the v1-alpha models frozen as reference content; register with
  `superseded_by` pointing at the canonical v2 maturity architecture in
  `technehub-labs/dea-metamodel` (already the routed destination per
  MIGRATION.md).
- Instruments keep scoring against the legacy scheme, declared explicitly as
  such; new v2-canonical maturity models are authored independently in
  `maturity-model-*` repos (the digital-transformation model is the pattern).

**Pros:** no breaking change; historical results stay interpretable; zero
porting cost; fully reversible (a later Release can still port).

**Cons:** the org's four flagship instruments remain tied to a superseded
scheme until v2 replacements exist; two scoring schemes coexist indefinitely.

### Recommendation — B now, A opportunistically

Freeze the v1-alpha cluster (Option B) and author v2-canonical domain maturity
models as new independent repositories when needed, letting instruments migrate
one at a time as each v2 replacement lands. Rationale: B is reversible and A is
not (porting invalidates historical comparability); the federated machinery
makes incremental, per-domain migration cheap, so the big-bang port buys nothing.
The digital-transformation composite already spans operations/technology/
services-delivery via CR-AM-10 components — it, not the legacy catalog, is the
natural v2 anchor for those domains.

## 4. Decision D-2 — Maturity scoring proposal (PR #1 on dea-catalog-maturity-models)

**Question:** Fold the `maturity-scoring-v2-proposal` (non-linear bands,
renames, per-level `effort_multiplier` coefficients) into Release 1, or close
it superseded?

- If D-1 = A: fold PR #1's band scheme in as the port target.
- If D-1 = B (recommended): close PR #1 as **superseded by** the canonical v2
  maturity architecture (CR-AM-09) — its ideas are already canonical there, so
  the PR no longer pre-empts anything and should not stay open.

**Recommendation:** close superseded, with a comment crediting the proposal as
input to CR-AM-09's v2 bands.

## 5. Decision D-3 — Instrument record status

**Question:** What do the four instruments' `status` values become?

- If D-1 = B (recommended): `candidate-for-migration` → **`active`**. They are
  federated, contract-conformant, released, and their (legacy) scoring target
  is a deliberate, declared choice — not a pending migration. `notes` in each
  record should state the scoring target is the frozen v1-alpha scheme and
  name the v2 migration path per domain.

## 6. Decision D-4 — Catalog index end-state

**Question:** Final fate of `assessments/index.yaml` in this repo — generated
registry view, or frozen legacy pointer?

The index is already demoted to a legacy view with a supersession header, and
CI cross-checks its entries against registry records. Remaining choice:

- **D-4a:** keep the manual view + CI cross-check (current state; zero cost,
  slight duplication risk).
- **D-4b:** generate the view from the registry in `assessment-ci` and open a
  PR here on registry changes (needs the org dispatch credential).

**Recommendation:** D-4a now, D-4b when cross-repo fan-out lands.

## 7. Execution checklist (executed 2026-10-27 — see PRs registry#7, maturity-models#4, and the superseded PR #1 closure recorded in §4)

| # | Action | Repo | Depends on |
|---|--------|------|------------|
| 1 | Record decisions D-1..D-4 in this file; set Status: Decided | dea-catalog-assessment-tools | ✅ done (this update) |
| 2 | Close PR #1 as superseded (with credit comment) | dea-catalog-maturity-models | ✅ no-op — PR #1 was already merged (2026-08-21); supersession recorded with credit in §4 and MIGRATION.md |
| 3 | Flip instrument registry records to `status: active` + scoring-target notes | assessment-registry | ✅ done (PR #7, merged) |
| 4 | Add `superseded_by` to the legacy catalog record per D-1 | assessment-registry | ✅ done (PR #7 notes → CR-AR-05 machine-readable field, PR #8 merged) |
| 5 | Update MIGRATION.md from "decision deferred" to the decided outcome | dea-catalog-maturity-models | ✅ done (PR #4, merged) |
| 6 | (If D-1 = A) execute the §26 migration pipeline per model | new maturity-model-* repos | n/a — D-1 = B |

## 8. Impact summary

No breaking change under the recommended path (B / close / active / D-4a). All
changes are metadata and documentation; every affected artifact is
machine-validated (registry validator, Level 3 ecosystem CI). Historical
assessment results remain interpretable; the ecosystem keeps a single
authoritative index (the registry) and one live scoring anchor per domain going
forward.

⸻

*Companion documents: `docs/rationale/CR-AM-01-companion-rationale.md`,
`docs/rationale/CR-AM-01-decision-points.md`,
[legacy-migration-plan](https://github.com/technehub-labs/dea-metamodel/blob/main/assessment-models/governance/legacy-migration-plan.md).*
