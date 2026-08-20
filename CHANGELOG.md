[Unreleased]
└── pending
    └── CR-AM-01: documented Assessment Metamodel Evolution roadmap (8-phase, 4-release). Added:
        ├── change-requests/CR-AM-01.md (CR verbatim, md5-verified)
        ├── change-requests/README.md (CR index)
        ├── docs/rationale/CR-AM-01-companion-rationale.md (companion rationale)
        └── docs/rationale/CR-AM-01-decision-points.md (decision-point index, parking lot, ACs, glossary)
    NOTE: No schema/code changes. instrument.schema.json untouched. All existing instruments continue to validate.
          Implementation begins in next PRs (Phases 1–8 per CR-AM-01 §32).

1.0.0-alpha
└── initial release
    ├── Umbrella catalog index for 4 domain assessment repositories
    ├── Shared scoring rubric (4-point scale, 0-100 sum, 5 bands)
    ├── Methodology guide (self-assessment vs facilitated workshop)
    ├── JSON Schema for all domain instruments
    └── GitHub Actions CI validates index.yaml and schema