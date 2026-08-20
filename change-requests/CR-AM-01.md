Below is a Change Request (CR) structured so it can be used as the basis for an actual implementation across the DEA Assessment-Models repositories. I have treated the current repositories as an existing product that should be evolved backward-compatibly, rather than redesigned from scratch.

The current dea-catalog-assessment-tools already has several useful foundations: a catalog index, shared JSON Schema, common scoring rubric, methodology, stable instrument IDs, metamodel versioning, and semantic versioning.  The maturity-model catalog is likewise already separated from the instruments and defines reusable five-level maturity models. 

CR — OpenDEA Assessment and Maturity Model Evolution

Change Request ID: DEA-CR-Assessment-001
Title: Establish a Composable Assessment, Capability, Maturity and Benchmarking Model
Status: Proposed
Priority: High
Change Type: Architecture / Metamodel / Schema / Catalog / Governance
Target: OpenDEA Assessment Model Ecosystem
Affected Repositories: Assessment-Models organization
Primary Repository: dea-catalog-assessment-tools
Related Repository: dea-catalog-maturity-models
Implementation Strategy: Incremental, backward-compatible evolution

⸻

1. Executive Summary

OpenDEA currently provides a useful foundation for enterprise architecture assessment through domain-specific assessment instruments and maturity models.

The current architecture, however, implicitly treats an assessment primarily as an instrument that:

1. contains dimensions;
2. contains questions;
3. assigns scores;
4. produces a score against a maturity model.

The existing instrument schema requires maturity_target, constrains instruments to four predefined domains, embeds questions within dimensions, and defines a uniform four-point scoring mechanism. (GitHub⁠) The catalog README similarly describes the assessment as an instrument whose results map to maturity models. (GitHub⁠)

This is adequate for the current assessment use cases, but it becomes restrictive when OpenDEA needs to support:

* enterprise-wide health assessments;
* capability-specific assessments;
* scenario-based assessments;
* maturity assessments;
* diagnostic assessments;
* comparative assessments;
* organization-to-organization benchmarking;
* evidence-based assessments;
* quantitative assessments;
* automated/telemetry-driven assessments;
* longitudinal assessment;
* multiple maturity models over the same capability;
* different assessment instruments over the same capability;
* incremental evolution of assessment models without invalidating historical results.

The proposed change is therefore to establish an explicit OpenDEA Assessment Metamodel and make assessment instruments implementations of that model.

The resulting architecture separates:

What is assessed → Capability

Where/under what circumstances → Scenario

How it is assessed → Assessment Model

How it is executed → Assessment Instrument

What is observed → Measure / Evidence

What was produced → Assessment Result

How progression is interpreted → Maturity Model

How comparable results are established → Benchmark Model

How results are presented → Assessment View

This creates an extensible assessment ecosystem without requiring existing instruments or repositories to be discarded.

⸻

2. Problem Statement

2.1 Current Situation

The current assessment catalog provides:

* an umbrella catalog;
* four domain assessment repositories;
* a shared instrument schema;
* common scoring bands;
* assessment methodology;
* versioning conventions.

The current catalog identifies four assessment domains:

* Architecture Modernization;
* Technology;
* Operations;
* Services Delivery. (GitHub⁠)

The current schema requires each instrument to have:

* id;
* name;
* domain;
* version;
* metamodel_version;
* description;
* maturity_target;
* dimensions.

Questions are embedded under dimensions and use a four-value scoring array. (GitHub⁠)

The maturity catalog independently defines five maturity levels and associates assessment results with those maturity levels. (GitHub⁠)

This creates a workable but relatively linear relationship:

Assessment Instrument
        |
        v
     Questions
        |
        v
      Score
        |
        v
 Maturity Model

⸻

3. Architectural Problem

The current model conflates several concepts that should be independently reusable.

Specifically:

Assessment
Capability
Measurement
Scoring
Maturity
Benchmarking
Result

are not equivalent concepts.

This creates several limitations.

3.1 Assessment is coupled to maturity

The current schema requires:

maturity_target:

This means an assessment inherently expects a maturity model.

That is inappropriate for assessments whose purpose is:

* capability diagnosis;
* baseline establishment;
* scenario evaluation;
* compliance;
* readiness;
* benchmarking;
* comparative analysis.

A capability can be assessed without assigning a maturity level.

⸻

3.2 Capability is not sufficiently independent

A capability should be reusable across multiple assessments.

For example:

API Management Capability
       |
       +-- Enterprise Health Assessment
       +-- Digital Platform Assessment
       +-- API Governance Assessment
       +-- Autonomous Operations Assessment
       +-- B2B2X Assessment

The current instrument-centric structure makes this relationship difficult to express cleanly.

⸻

3.3 Scenario is missing as a first-class construct

Benchmarking requires context.

A score cannot automatically be interpreted as comparable simply because two organizations answered the same questions.

Comparability depends on:

* capability;
* scenario;
* scope;
* assessment method;
* measurement definitions;
* scoring model;
* evidence requirements;
* assessment period;
* population.

Therefore:

Organization + Score

is insufficient as a benchmark unit.

⸻

3.4 Results are not sufficiently modeled

The assessment instrument describes how assessment is performed, but the result should be independently represented.

This is required to support:

* historical results;
* longitudinal comparison;
* auditability;
* provenance;
* evidence;
* confidence;
* benchmark eligibility;
* model-version lineage.

⸻

3.5 Versioning does not fully preserve comparability

The existing semantic versioning policy distinguishes:

* MAJOR — fundamental question-set changes;
* MINOR — new dimension or question;
* PATCH — clarification or corrected phrasing. (GitHub⁠)

This is useful but insufficient.

Adding a question may technically be a MINOR change while materially changing the statistical interpretation of the resulting score.

The model therefore requires version lineage and comparability metadata, not version numbers alone.

⸻

4. Change Objective

Establish an extensible OpenDEA Assessment Metamodel that allows:

1. existing instruments to continue operating;
2. existing maturity models to continue operating;
3. capabilities to become independently reusable;
4. scenarios to become first-class assessment context;
5. assessment methods to become reusable;
6. measures and evidence to become first-class constructs;
7. assessment results to become persistent, versioned objects;
8. maturity models to become optional interpretation mechanisms;
9. benchmarking to become a controlled comparative mechanism;
10. enterprise heatmaps to become views over assessment results rather than assessment types;
11. assessment components to evolve independently;
12. historical results to remain valid against the exact models used to produce them.

⸻

5. Target Architectural Principle

The following principle shall govern the Assessment domain:

Assessments define how something is measured; capabilities define what can be assessed; scenarios define the context in which it is assessed; maturity models define progression; benchmark models define comparability; assessment results preserve what was observed.

This principle shall be reflected in the metamodel, schemas, repositories, documentation and validation rules.

⸻

6. Target Conceptual Architecture

The target architecture shall be:

                         Assessment Context
                                |
                                v
                       +-------------------+
                       | Assessment Model  |
                       +---------+---------+
                                 |
             +-------------------+-------------------+
             |                   |                   |
             v                   v                   v
       Capability Model    Scenario Model      Method Model
             |                   |                   |
             +-------------------+-------------------+
                                 |
                                 v
                          Measurement Model
                                 |
                                 v
                         Evidence / Observation
                                 |
                                 v
                         Assessment Result
                                 |
              +------------------+------------------+
              |                  |                  |
              v                  v                  v
        Maturity Model     Benchmark Model     Findings
              |                  |                  |
              v                  v                  v
        Maturity View     Benchmark View     Diagnostic View
                                 |
                                 v
                         Enterprise Heatmap

⸻

7. Core Metamodel

The OpenDEA Assessment Metamodel shall introduce the following principal concepts.

7.1 Assessment

Represents the conceptual evaluation activity.

Assessment

It identifies:

* purpose;
* subject;
* scope;
* context;
* assessment model;
* assessment period;
* status.

⸻

7.2 Assessment Model

Defines the semantic specification of an assessment.

AssessmentModel

It defines:

* purpose;
* assessment type;
* subject type;
* applicable scenarios;
* capabilities;
* dimensions;
* measures;
* evidence requirements;
* scoring;
* interpretation;
* benchmark compatibility.

⸻

7.3 Assessment Instrument

Defines the executable/questionnaire realization of an Assessment Model.

AssessmentInstrument

An instrument may be:

* self-assessment;
* facilitated workshop;
* survey;
* interview;
* automated assessment;
* telemetry-driven assessment;
* hybrid assessment.

Relationship:

AssessmentInstrument
        |
        +-- implements --> AssessmentModel

⸻

8. Capability Model

Introduce:

Capability
CapabilityArea
CapabilityOutcome
CapabilityDependency
CapabilityMeasure
CapabilityEvidence

A capability describes what an organization or entity is able to do, independent of how that capability is assessed.

Example:

API Management
    |
    +-- API Design
    +-- API Governance
    +-- API Lifecycle Management
    +-- API Security
    +-- API Analytics

The same capability may participate in multiple assessments.

⸻

9. Scenario Model

Introduce:

Scenario
ScenarioContext
ScenarioOutcome
ScenarioConstraint
ScenarioMeasure

A scenario defines the contextual conditions under which capability is evaluated.

Example:

Scenario:
B2B API Product Launch
Context:
- multi-market
- regulated environment
- external ecosystem
- high transaction volume
Expected outcomes:
- time-to-market
- reuse
- security
- reliability
- customer adoption

The scenario becomes a critical mechanism for meaningful benchmarking.

⸻

10. Measure Model

Introduce:

Measure
Metric
MeasurementRule
MeasurementUnit
Threshold

A measure represents an observable characteristic.

Examples:

Automation Rate
Time To Market
Defect Rate
API Reuse Rate
Deployment Frequency
Incident Recovery Time

Measurements shall not be inherently tied to questionnaire scoring.

This permits:

Questionnaire
       |
       v
Observation
       |
       v
Measure

and:

Telemetry
       |
       v
Observation
       |
       v
Measure

to produce equivalent types of assessment evidence.

⸻

11. Evidence Model

Introduce:

EvidenceRequirement
Evidence
Observation
EvidenceSource
EvidenceProvenance
Confidence

Evidence shall support:

* source;
* timestamp;
* assessor;
* provenance;
* confidence;
* observation;
* supporting artifact.

This establishes the basis for evidence-based and AI-assisted assessment.

⸻

12. Scoring Model

Scoring shall be separated from the assessment instrument.

Introduce:

ScoringModel
ScoringRule
ScoreBand
AggregationRule
NormalizationRule

The current 0–3 question scoring model should remain supported as a default scoring model, rather than becoming a mandatory property of the metamodel.

The existing 0–25 / 26–50 / 51–75 / 76–90 / 91–100 bands may continue as the default maturity scoring convention. (GitHub⁠)

Future models may support:

* weighted scores;
* normalized measures;
* thresholds;
* categorical results;
* quantitative metrics;
* statistical models;
* composite indicators.

⸻

13. Maturity Model

The existing maturity model catalog should remain a separate reusable model family.

The current maturity repository already establishes five progression levels:

1. Ad Hoc;
2. Defined;
3. Managed;
4. Quantitatively Managed;
5. Optimising. (GitHub⁠)

The relationship should evolve from:

Assessment
    |
    +-- produces-score-for --> Maturity

toward:

AssessmentResult
    |
    +-- interpreted-by --> MaturityModel

This allows the same assessment result to be interpreted without forcing every assessment to have a maturity model.

⸻

14. Benchmark Model

Introduce:

BenchmarkModel
BenchmarkPopulation
PeerGroup
ComparabilityRule
NormalizationRule
BenchmarkResult

A benchmark model defines:

* who may participate;
* what is being compared;
* required assessment model;
* scenario;
* capability;
* measures;
* normalization;
* eligibility;
* statistical methodology;
* confidence requirements.

The fundamental benchmark unit should be:

Organization
× Capability
× Scenario
× AssessmentModel
× MeasurementPeriod

rather than:

Organization
× Score

⸻

15. Assessment Result

Introduce AssessmentResult as a first-class concept.

AssessmentResult:
  id:
  assessment_model:
  assessment_version:
  subject:
  scope:
  scenario:
  assessment_period:
  assessor:
  method:
  observations:
  evidence:
  measures:
  scores:
  findings:
  maturity:
  benchmark:
  confidence:
  provenance:
  status:

The result shall preserve the exact model versions used to generate it.

⸻

16. Assessment Views

Enterprise heatmaps shall not become a separate assessment type.

Instead:

AssessmentResult
       |
       +-- Enterprise View
       +-- Capability View
       +-- Scenario View
       +-- Maturity View
       +-- Benchmark View
       +-- Trend View
       +-- Diagnostic View

The enterprise heatmap becomes a projection of aggregated assessment results.

This allows one assessment dataset to support:

* executive heatmap;
* capability radar;
* maturity profile;
* benchmark position;
* trend analysis;
* gap analysis.

⸻

17. Required Relationship Model

The metamodel shall support at least these relationships:

Source	Relationship	Target
Assessment	conforms-to	AssessmentModel
AssessmentModel	assesses	Capability
AssessmentModel	applicable-to	Scenario
AssessmentModel	uses	AssessmentMethod
AssessmentModel	measures	Measure
AssessmentModel	requires	EvidenceRequirement
AssessmentModel	uses	ScoringModel
AssessmentInstrument	implements	AssessmentModel
AssessmentExecution	executes	AssessmentInstrument
AssessmentExecution	assesses	Subject
AssessmentExecution	occurs-in	Scenario
AssessmentExecution	produces	AssessmentResult
AssessmentResult	contains	Observation
Observation	supported-by	Evidence
Observation	measures	Measure
AssessmentResult	interpreted-by	MaturityModel
AssessmentResult	eligible-for	BenchmarkModel
BenchmarkModel	compares	AssessmentResult
AssessmentResult	produces	Finding
AssessmentResult	represented-by	AssessmentView

⸻

18. Target Information Model

The core information model should become:

+-------------------+
| AssessmentModel   |
+-------------------+
| id                |
| version           |
| purpose           |
| assessmentType    |
| subjectType       |
| status            |
+---------+---------+
          |
          | assesses
          v
+-------------------+
| Capability        |
+-------------------+
| id                |
| version           |
| name              |
| outcome           |
+-------------------+
          |
          | evaluated-in
          v
+-------------------+
| Scenario          |
+-------------------+
| id                |
| version           |
| context           |
| outcome           |
+-------------------+
          |
          | measures
          v
+-------------------+
| Measure           |
+-------------------+
| id                |
| unit              |
| definition        |
| measurementRule   |
+-------------------+
          |
          | produces
          v
+-------------------+
| AssessmentResult  |
+-------------------+
| id                |
| modelVersion      |
| subject           |
| period            |
| confidence        |
+---------+---------+
          |
          +------------------+
          |                  |
          v                  v
+-------------------+  +-------------------+
| MaturityModel     |  | BenchmarkModel    |
+-------------------+  +-------------------+

⸻

19. Schema Evolution

The current instrument.schema.json shall not be immediately replaced.

Instead, introduce a new schema family.

Recommended structure:

schemas/
├── instrument.schema.json
├── assessment-model.schema.json
├── assessment-result.schema.json
├── capability-ref.schema.json
├── scenario-ref.schema.json
├── measure.schema.json
├── evidence.schema.json
├── scoring-model.schema.json
├── benchmark-model.schema.json
├── compatibility.schema.json
└── common.schema.json

The existing instrument schema remains supported during transition.

⸻

20. Backward Compatibility

Existing instruments shall continue to validate without modification.

For existing instruments:

maturity_target:
  dea:maturity-technology

shall remain supported.

It shall be interpreted as shorthand for:

interpretation:
  maturity_models:
    - id: dea:maturity-technology

Existing:

relationships:
  - relationship_type: produces-score-for

shall remain valid.

The new canonical relationship becomes:

relationships:
  - relationship_type: interpreted-by

A compatibility layer shall support both during the transition period.

⸻

21. New Assessment Model Schema

A target instrument may evolve toward:

id: dea:assessment-api-capability
name: API Capability Assessment
version: 1.0.0
metamodel_version: 1.0.0
type: assessment-model
purpose:
  - capability-assessment
  - diagnostic
subject_type: enterprise
capabilities:
  - id: dea:capability-api-management
    version: 1.0.0
scenarios:
  - id: dea:scenario-b2b-api-product
    version: 1.0.0
dimensions:
  - id: governance
    name: Governance
    weight: 0.25
measures:
  - id: api-reuse-rate
evidence_requirements:
  - id: api-governance-policy
  - id: api-lifecycle-process
scoring_model:
  id: dea:scoring-four-point
  version: 1.0.0
maturity_models:
  - id: dea:maturity-api-management
    version: 1.0.0

The key change is that maturity becomes optional.

⸻

22. Component Versioning

All reusable assessment components shall have stable identifiers.

For example:

dea:capability-api-management
dea:scenario-b2b-api-product
dea:measure-api-reuse-rate
dea:scoring-four-point
dea:maturity-api-management

Versions shall be separately addressable:

dea:capability-api-management@1.2.0

An assessment model shall reference exact versions.

Example:

components:
  capabilities:
    - dea:capability-api-management@1.2.0
  scenarios:
    - dea:scenario-b2b-api-product@1.0.0
  scoring:
    - dea:scoring-four-point@1.0.0
  maturity:
    - dea:maturity-api-management@2.0.0

⸻

23. Immutable Historical Results

Once an assessment result has been finalized, the underlying model references shall be immutable.

Example:

Assessment Result AR-2026-001
Assessment Model:
dea:assessment-api-capability@1.4.0
Capability:
dea:capability-api-management@1.2.0
Scenario:
dea:scenario-b2b-api-product@1.0.0
Scoring:
dea:scoring-four-point@1.0.0
Maturity:
dea:maturity-api-management@2.0.0

Subsequent model changes shall not mutate this historical result.

⸻

24. Model Lineage

Every new model version shall support:

lineage:
  previous_version: dea:assessment-api-capability@1.3.0
  change_type: minor
  supersedes:
    - dea:assessment-api-capability@1.3.0

For component changes:

lineage:
  changed_components:
    - id: q-governance-03
      previous_version: 1.0.0
      new_version: 2.0.0

⸻

25. Comparability Contract

Every model version shall declare its comparability properties.

Example:

compatibility:
  backward_compatible: true
  scoring_compatible: true
  result_compatible: true
  benchmark_compatible: false
  maturity_compatible: true

This creates a distinction between:

Technically compatible

A result can still be processed.

Scoring compatible

Scores have the same interpretation.

Benchmark compatible

Results may legitimately participate in cross-organization benchmarking.

Maturity compatible

Results can be mapped to the same maturity interpretation.

These properties must not be inferred solely from semantic version numbers.

⸻

26. Versioning Policy

The existing semantic-versioning policy should be expanded.

PATCH

Use for:

* typo;
* grammar;
* non-semantic clarification;
* additional explanatory evidence;
* metadata correction.

No score interpretation may change.

⸻

MINOR

Use for:

* additional optional evidence;
* additional optional measure;
* new optional assessment question;
* new optional dimension;
* additive capability mapping.

Existing result interpretation must remain valid.

⸻

MAJOR

Use when:

* scoring changes;
* weighting changes;
* question meaning changes;
* dimension meaning changes;
* mandatory evidence changes;
* maturity interpretation changes;
* comparability changes;
* normalization changes.

A major version must never silently reinterpret historical results.

⸻

27. Assessment Compatibility Matrix

CI shall generate a compatibility matrix.

Example:

Model	Previous	Scoring	Maturity	Benchmark
v1.0	—	—	—	—
v1.1	v1.0	Yes	Yes	Yes
v1.2	v1.1	Yes	Yes	No
v2.0	v1.2	No	No	No

This makes model evolution transparent.

⸻

28. Benchmark Eligibility

A result shall be benchmarkable only if:

AssessmentModel compatible
AND
Scenario compatible
AND
Capability compatible
AND
Measure compatible
AND
Scoring compatible
AND
Evidence requirements compatible
AND
Population criteria satisfied

Otherwise:

benchmark_status = NOT_COMPARABLE

rather than silently including the result.

⸻

29. Enterprise Heatmap Model

Enterprise heatmaps should aggregate normalized capability results.

Example:

Enterprise
   |
   +-- Strategy Capability       72
   +-- Customer Capability       64
   +-- Operations Capability     81
   +-- Technology Capability     76
   +-- Data Capability           58

The heatmap is therefore a derived analytical view.

It should not imply that the values are automatically benchmarkable across organizations.

The UI should distinguish:

Enterprise Health

from:

Peer Benchmark

⸻

30. Benchmark View

Benchmarking should present:

Organization Score
Peer Median
Peer Quartile
Percentile
Sample Size
Confidence
Scenario
Assessment Version
Measurement Period

Example:

API Management Capability
Organization:       78
Peer Median:        67
Percentile:         82nd
Sample Size:        43
Scenario:           B2B API Product
Assessment Version: 2.1.0
Confidence:         High

This is substantially more meaningful than showing an isolated maturity score.

⸻

31. Repository Evolution

The existing repository structure should remain intact.

Current structure:

Assessment-Models/
├── dea-catalog-assessment-tools
├── dea-catalog-maturity-models
├── dea-assessment-modernization
├── dea-assessment-technology
├── dea-assessment-operations
└── dea-assessment-services-delivery

The assessment catalog currently serves as an umbrella index over the four domain repositories and contains the shared schema and scoring methodology. (GitHub⁠)

The recommended evolution is:

Assessment-Models/
│
├── dea-metamodel
│
├── dea-catalog-assessment-tools
│
├── dea-catalog-capability-models
│
├── dea-catalog-scenario-models
│
├── dea-catalog-maturity-models
│
├── dea-catalog-benchmark-models
│
├── dea-assessment-modernization
├── dea-assessment-technology
├── dea-assessment-operations
└── dea-assessment-services-delivery

However, the new repositories should be introduced only when there is enough content to justify them.

⸻

32. Recommended Implementation Order

Phase 1 — Metamodel Foundation

Create:

dea-metamodel

Define:

* Assessment;
* AssessmentModel;
* AssessmentInstrument;
* AssessmentExecution;
* AssessmentResult;
* Capability;
* Scenario;
* Measure;
* Evidence;
* ScoringModel;
* MaturityModel;
* BenchmarkModel.

Deliver:

* UML;
* JSON Schema;
* YAML examples;
* relationship vocabulary;
* versioning rules.

⸻

33. Phase 2 — Schema Compatibility

Extend:

dea-catalog-assessment-tools

Add:

schemas/
    assessment-model.schema.json
    assessment-result.schema.json
    capability-ref.schema.json
    scenario-ref.schema.json
    measure.schema.json
    evidence.schema.json
    compatibility.schema.json

Keep:

instrument.schema.json

unchanged initially.

Add a compatibility validator.

⸻

34. Phase 3 — Refactor Existing Instruments

For each existing instrument:

modernization
technology
operations
services-delivery

introduce explicit:

assessment_model:
capabilities:
scenarios:
measures:
evidence:
scoring_model:
maturity_models:

Initially these may be derived from existing fields.

No existing assessment should be invalidated.

⸻

35. Phase 4 — Decouple Maturity

Change the conceptual model from:

Assessment → Maturity

to:

Assessment → Result → Maturity

Maintain:

maturity_target:

as a backward-compatible alias.

Mark it as deprecated once all consumers support the new structure.

⸻

36. Phase 5 — Capability Catalog

Create a reusable capability catalog.

Initial capabilities should be derived from existing assessment dimensions and questions.

For example:

Technology Assessment
        |
        +-- Technology Lifecycle
        +-- Technical Debt
        +-- Platform Fitness
        +-- Skills

These should become reusable capability references rather than remaining permanently embedded inside individual instruments.

⸻

37. Phase 6 — Scenario Catalog

Introduce scenarios selectively.

Do not attempt to model every possible enterprise scenario.

Start with scenarios where benchmarking provides obvious value.

Examples:

Digital Product Launch
Legacy Modernization
API Product Launch
Zero-Touch Operations
Cloud Migration
AI Adoption

⸻

38. Phase 7 — Assessment Results

Extend dea-cli and future tooling to persist structured results.

The current ecosystem already identifies dea-cli as a consumer of the assessment and maturity catalogs. (GitHub⁠)

The CLI should evolve toward commands such as:

dea assess run
dea assess validate
dea assess result
dea assess compare
dea assess benchmark
dea assess maturity
dea assess history

⸻

39. Phase 8 — Benchmarking

Only after the following are stable:

* capability IDs;
* scenario IDs;
* measure IDs;
* assessment model versions;
* result structure;
* compatibility rules.

Then introduce benchmark models.

Benchmarking should be treated as an analytical layer, not as an extension of the questionnaire.

⸻

40. CI/CD Requirements

Every repository shall validate:

Schema

YAML → JSON Schema

References

Referenced capability exists
Referenced scenario exists
Referenced maturity model exists
Referenced scoring model exists

Versioning

Version follows SemVer
Lineage is valid

Compatibility

Declared compatibility matches actual changes

Relationship validity

Relationship types are registered
Source/target types are valid

Result integrity

Historical model references resolve

⸻

41. Relationship Vocabulary

A controlled relationship vocabulary shall replace unrestricted relationship strings where possible.

Minimum vocabulary:

assesses
implements
conforms-to
uses
measures
requires
supports
evaluates
occurs-in
produces
contains
interpreted-by
eligible-for
benchmarks
supersedes
derived-from
compatible-with

Relationship types should be defined in the metamodel.

⸻

42. Governance

Every model shall have:

owner:
steward:
status:
effective_date:
review_date:

Recommended statuses:

draft
experimental
alpha
beta
stable
deprecated
retired

The current assessment schema already supports lifecycle status values including draft, alpha, beta, stable and deprecated. (GitHub⁠)

retired should be added at the metamodel level.

⸻

43. Model Lifecycle

Draft
  |
  v
Experimental
  |
  v
Alpha
  |
  v
Beta
  |
  v
Stable
  |
  v
Deprecated
  |
  v
Retired

Retirement must never delete the definition required to interpret historical assessment results.

⸻

44. Definition of Done

The CR shall be considered implemented when:

* AssessmentModel is defined.
* AssessmentInstrument is separated conceptually from AssessmentModel.
* AssessmentResult is a first-class object.
* Capability is independently referenceable.
* Scenario is independently referenceable.
* Measure is independently referenceable.
* Evidence is independently referenceable.
* ScoringModel is independently referenceable.
* MaturityModel is optional.
* BenchmarkModel is independently referenceable.
* Enterprise heatmaps are derived views.
* Stable IDs exist for reusable components.
* Exact component versions can be referenced.
* Model lineage is recorded.
* Compatibility is explicitly declared.
* Historical results remain immutable.
* Existing instruments continue to validate.
* Existing maturity_target relationships remain supported.
* CI validates cross-repository references.
* CLI can resolve model versions.
* Benchmark eligibility can be determined automatically.
* Documentation describes the new architecture.

⸻

45. Acceptance Criteria

AC-01 — Existing Instrument Compatibility

An existing assessment instrument must continue to validate without requiring immediate modification.

⸻

AC-02 — Capability Reuse

A single capability can be referenced by multiple assessment models.

⸻

AC-03 — Maturity Independence

An assessment model can exist without a maturity model.

⸻

AC-04 — Multiple Maturity Interpretations

One assessment result can be interpreted using more than one compatible maturity model.

⸻

AC-05 — Scenario Context

An assessment execution can reference a scenario.

⸻

AC-06 — Result Persistence

An assessment result stores the exact versions of all material assessment components.

⸻

AC-07 — Historical Integrity

Updating an assessment model does not change previously finalized results.

⸻

AC-08 — Benchmark Protection

Results from incompatible assessment models cannot be silently included in a benchmark population.

⸻

AC-09 — Enterprise Heatmap

A heatmap can be generated from assessment results without requiring a dedicated heatmap assessment instrument.

⸻

AC-10 — Incremental Evolution

A new capability, measure, question, maturity model or scenario can be added without restructuring existing instruments.

⸻

46. Migration Strategy

Migration shall follow:

Existing
  |
  v
Compatible
  |
  v
Mapped
  |
  v
Enhanced
  |
  v
Canonical

Not:

Existing
   |
   X
Delete
   |
   X
Rebuild

The migration process should therefore be additive.

⸻

47. Existing-to-Target Mapping

Current construct	Target construct
Instrument	AssessmentInstrument
Instrument schema	AssessmentInstrument schema
Instrument dimensions	AssessmentModel dimensions
Questions	AssessmentMeasure / AssessmentQuestion
Question evidence	EvidenceRequirement
Scoring array	ScoringRule
maturity_target	Interpretation / MaturityModel reference
Relationships	Metamodel relationship
Domain	Capability / domain classification
Score	AssessmentResult
Maturity score	AssessmentResult interpretation
Scoring rubric	ScoringModel
Catalog index	Model Registry

⸻

48. Important Design Decision

The existing four-domain taxonomy should not become the permanent upper-level ontology of assessment.

The current domains are useful organizational categories:

Modernization
Technology
Operations
Services Delivery

but future OpenDEA assessments may cross those boundaries.

For example:

Autonomous Operations

could include:

* technology;
* operations;
* data;
* governance;
* AI;
* automation;
* organization;
* culture.

Therefore:

Domain ≠ Capability

and:

Domain ≠ Assessment Type

The current domain classification should remain as metadata for catalog organization.

⸻

49. Assessment Type Taxonomy

The metamodel should define assessment purpose separately.

Recommended initial taxonomy:

enterprise-health
capability-assessment
maturity-assessment
diagnostic
baseline
readiness
scenario
comparative
benchmark
compliance

An assessment may support multiple purposes.

Example:

purpose:
  - capability-assessment
  - diagnostic
  - benchmark

⸻

50. Enterprise Assessment vs Capability Assessment

The target model should explicitly support both.

Enterprise Assessment

Subject:
Enterprise
Scope:
Enterprise-wide
Capabilities:
Multiple
Scenario:
Enterprise transformation
Primary Output:
Health heatmap
Benchmark:
Optional / limited

Capability Assessment

Subject:
Enterprise / Business Unit / Product
Scope:
Specific capability
Scenario:
Defined scenario
Primary Output:
Capability score + evidence + gaps
Benchmark:
Strongly supported

This distinction directly addresses the fundamental problem identified in the CR.

⸻

51. Benchmarking Principle

OpenDEA shall adopt:

Benchmark what is sufficiently controlled; diagnose what is contextually variable.

Enterprise heatmaps are primarily diagnostic.

Scenario-based capability assessments can become comparative.

This prevents the assessment system from generating misleading organizational league tables from heterogeneous enterprise health scores.

⸻

52. Expected Benefits

Architectural

* Clear separation of concerns.
* Reusable models.
* Composable assessments.
* Independent evolution.
* Better semantic integrity.

Operational

* More assessment types.
* Automated assessment.
* Evidence-driven assessment.
* Longitudinal tracking.
* Reusable questionnaires.

Analytical

* Capability benchmarking.
* Scenario comparison.
* Maturity progression.
* Trend analysis.
* Enterprise heatmaps.

Governance

* Model lineage.
* Version traceability.
* Historical reproducibility.
* Benchmark eligibility.
* Explicit ownership.

⸻

53. Risks and Mitigations

Risk	Mitigation
Metamodel becomes over-engineered	Implement minimum viable core first
Existing instruments break	Maintain compatibility schema
Too many repositories	Introduce catalogs only when justified
Benchmarking becomes statistically misleading	Require explicit comparability rules
Version proliferation	Use component composition and lineage
Historical results become ambiguous	Persist exact model versions
Capability taxonomy becomes contentious	Treat capabilities as independently governed reference models
Scoring becomes overly complex	Retain current scoring model as default
Migration becomes disruptive	Use additive migration

⸻

54. Recommended Priority

P0 — Required

AssessmentModel
AssessmentResult
Capability reference
Version lineage
Compatibility
Backward compatibility

P1 — High Value

Scenario
Measure
Evidence
ScoringModel

P2 — Strategic

BenchmarkModel
BenchmarkPopulation
Normalization
Comparability analytics

P3 — Advanced

Automated assessment
Telemetry assessment
AI evidence interpretation
Predictive maturity
Continuous assessment

⸻

55. Strategic Outcome

The implementation should transform OpenDEA from an:

Assessment Instrument Catalog

into an:

Assessment Model Ecosystem

with the following semantic stack:

                    OPENDEA ASSESSMENT ECOSYSTEM
                           ┌───────────┐
                           │   View    │
                           └─────▲─────┘
                                 │
                           ┌─────┴─────┐
                           │   Result  │
                           └─────▲─────┘
                                 │
                   ┌─────────────┼─────────────┐
                   │             │             │
             ┌─────┴─────┐ ┌─────┴─────┐ ┌─────┴─────┐
             │ Maturity  │ │ Benchmark │ │ Findings  │
             └───────────┘ └───────────┘ └───────────┘
                                 ▲
                                 │
                       ┌─────────┴─────────┐
                       │ Assessment Model  │
                       └─────────┬─────────┘
                                 │
             ┌───────────────────┼───────────────────┐
             │                   │                   │
       ┌─────┴─────┐       ┌─────┴─────┐       ┌─────┴─────┐
       │ Capability│       │ Scenario  │       │ Measurement│
       └───────────┘       └───────────┘       └───────────┘

This architecture provides the critical property the current ecosystem needs:

Every assessment artifact can evolve independently while remaining composable through stable identities, explicit relationships, version lineage and compatibility rules.

The existing catalogs therefore become clients of the metamodel rather than competing definitions of the assessment architecture.

⸻

56. Final Recommendation

Approve this CR as an architectural evolution rather than a repository rewrite.

The immediate implementation should be limited to establishing the metamodel and compatibility mechanisms. Do not begin by creating large new capability, scenario or benchmark catalogs.

The first milestone should be:

Assessment Metamodel v1
        +
Assessment Result Model v1
        +
Compatibility Model v1
        +
Migration of one existing assessment

The Technology Assessment would be a suitable pilot because it can demonstrate all three required perspectives:

Enterprise Health
        ↓
Capability Assessment
        ↓
Scenario-Based Benchmark

Once that pilot proves the model, the remaining assessment domains can be migrated using the same pattern.

This gives OpenDEA a controlled path from today’s instrument-centric implementation toward a genuinely composable assessment architecture, without invalidating the work already present in the assessment and maturity repositories.

The critical implementation insight is that this CR should not initially change the semantics of the existing four instruments. The current repository is small and clean enough to make it an excellent pilot: it already has a shared schema, catalog, scoring rubric, methodology, versioning, and explicit linkage to the maturity catalog.  The maturity repository likewise already has the reusable-model separation that the new architecture needs. 

I would make the next implementation artifact the actual dea-metamodel PlantUML class model + proposed JSON Schema, because that is the point where this CR becomes directly implementable and we can test the proposed model against the existing YAML rather than merely describing the architecture.