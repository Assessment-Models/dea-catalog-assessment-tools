# Contributing to dea-catalog-assessment-tools

This is the umbrella index. The actual instruments live in:

- [dea-assessment-modernization](https://github.com/Assessment-Models/dea-assessment-modernization)
- [dea-assessment-technology](https://github.com/Assessment-Models/dea-assessment-technology)
- [dea-assessment-operations](https://github.com/Assessment-Models/dea-assessment-operations)
- [dea-assessment-services-delivery](https://github.com/Assessment-Models/dea-assessment-services-delivery)

## Changes to this repo

- **Index updates** — adding a new domain repo entry. Open PR with:
  - `assessments/index.yaml` updated with new domain entry
  - README links updated
  - Changelog entry
- **Methodology updates** — edits to `assessments/methodology.md` or `scoring-rubric.md`
- **Schema updates** — changes to `schemas/instrument.schema.json` require a major version bump and must be coordinated with all domain instrument repos

## Changes to domain instruments

File the PR in the relevant domain repo. The CI there will validate the instrument against `instrument.schema.json`.

## Versioning

- **MAJOR** — schema fundamentally changes, all instruments must update
- **MINOR** — new domain repo added
- **PATCH** — methodology, scoring rubric, or doc clarifications

## Code of Conduct

Be respectful. Argue ideas, not people. PR reviews are learning opportunities.