# CR-AM-06 — First-Class Component Registry (Maturity-Components)

## Status

- **State:** Proposed — awaiting review.
- **Series tag:** `CR-AM` (Assessment Metamodel series, continuing from CR-AM-01/CR-AM-11).
- **Primary repo:** new `Assessment-Models/maturity-components` (taxonomy class: Reusable components, per GOVERNANCE.md).
- **Consumers:** `maturity-model-digital-transformation` (imports four CR-AM-10 components today by value-copy); any future model declaring component reuse.
- **Predecessors:** CR-AM-10 (component registry semantics: import/extend/override kinds); CR-AM-11 (§20 component references, §19 explicit-reuse rule; taxonomy reserves the `maturity-components` class); GOVERNANCE.md (legacy-naming exception closes new repos to taxonomy naming — this would be the first taxonomy-named repo, per M-2 of the advancement review).
- **Origin:** Advancement review M-1 — the machinery enforces explicit reuse, but the components being reused exist only as value-copies inside one model repo; nothing resolvable exists at the class level.

## Primary objective

Establish `maturity-components` as the first independently versioned **reusable-component repository**, register its component packages as first-class assets in `assessment-registry`, and convert `maturity-model-digital-transformation`'s four value-copied components into resolvable references — proving the full component-reuse chain end-to-end.

## Why now

- CR-AM-11 §20 defined component references (id, version, immutable reference, kind) and CR-AM-10 defined reuse semantics — but no component asset is registered, so the reference chain (model → component package → canonical source) is unresolvable beyond one repo's self-copies.
- GOVERNANCE.md's taxonomy reserves the `maturity-components` class; it is the only unpopulated class the current content justifies (four CR-AM-10 components already exist as content).
- M-2 of the advancement review: no repo has ever been created under the new taxonomy naming. This CR creates the first one, exercising the CONTRIBUTING.md onboarding path on a fresh case rather than a pre-existing legacy repo.

## Scope

In scope:

- New repository `Assessment-Models/maturity-components` with the standard model-repo structure (CR-AM-11 §22): `model.yaml` handshake, `VERSION`, Level 1 validation, Level 2 reusable workflow.
- Component packages: one YAML per component under `components/`, carrying the CR-AM-10 kind semantics at the *package* level (`import`/`extend`/`override` remain declared at the consuming model's reference site, per CR-AM-10 §4).
- First content: the four CR-AM-10 components (`digital-culture`, `technology-adoption`, `operations-modernization`, `services-delivery`), extracted from their current value-copies with provenance notes, pinned to the OpenDEA Component Registry source (`technehub-labs/dea-metamodel` at its published reference).
- Registration: one registry record for the repository's component release(s); `maturity-model-digital-transformation`'s registry record gains `components` entries pointing at the registered component assets (upgrade from the current self-referential pins).
- Ecosystem CI: `validate_ecosystem.py` already checks component references' immutability; this CR extends the check to resolve component ids against registered component records (currently informational if unregistered).

Out of scope (future CRs or existing machinery):

- CR-AM-10's full override/extension machinery (the consuming model declares kinds; the package repo is content-neutral).
- Component versioning policy beyond SemVer + tags (inherit CR-AM-11 §12).
- A component *marketplace* / search UI.

## Design constraints

1. **Value-copy is not forbidden** — small consumers may keep copies; but registered component assets must exist so the reference chain is resolvable. The digital-transformation model is converted as the reference consumer.
2. **No scoring or scale semantics in components** (CR-AM-11 §19): component packages contain dimensions/criteria/level-expectations content only.
3. **Standard structure:** the repo follows the CONTRIBUTING.md walkthrough exactly — this is deliberately the first test of self-service onboarding (M-2).
4. **Immutable pins throughout:** component packages pin their canonical source; consumers pin component releases.

## Execution checklist

| # | Action | Repo |
|---|--------|------|
| 1 | Land this CR doc | dea-catalog-assessment-tools (CR-AM series home) |
| 2 | Create `maturity-components` repo (taxonomy-named, standard structure) | new repo |
| 3 | Extract + land the four CR-AM-10 components with provenance | maturity-components |
| 4 | Tag `v1.0.0`; register in assessment-registry | assessment-registry |
| 5 | Convert digital-transformation's component references to the registered assets (kind-preserving); re-tag | maturity-model-digital-transformation |
| 6 | Extend `validate_ecosystem.py`: component ids resolve to registered component records | assessment-ci |
| 7 | Update CONTRIBUTING.md worked example with the component path | .github |

## Impact summary

Populates the last content-justified taxonomy class, converts the ecosystem's only component reuse from value-copy to a resolvable registered chain, and creates the first taxonomy-named repository — validating the naming rule and the self-service onboarding path in one move. No breaking change: consumers pin by tag; the digital-transformation conversion is a reference update, not a semantic change.
