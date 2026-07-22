# Assessment Scoring Rubric

Uniform across all four assessment domains. This rubric defines how raw instrument scores map to maturity bands.

---

## Scoring Method

Each instrument has:
- **4–6 dimensions** per domain
- **3–8 questions** per dimension
- **4-point scale** per question: `[0, 1, 2, 3]`

Where each score means:

| Score | Meaning |
|-------|---------|
| **0** | Not started / no awareness |
| **1** | Partially addressed / informal practice |
| **2** | Formally defined / documented |
| **3** | Fully implemented / consistently practiced |

**Total score formula:**

```
score = sum(all question scores) / (number of questions × 3) × 100
```

Result: a number 0–100.

---

## Maturity Bands

| Score | Band | Visual | Recommended Action |
|-------|------|--------|---------------------|
| 0–25 | **Ad Hoc** | red | Strategy + foundation work — establish baseline, secure sponsorship |
| 26–50 | **Defined** | orange | Tooling + rollout — codify practices, train teams |
| 51–75 | **Managed** | yellow | Operationalisation + governance — automate, measure, enforce |
| 76–90 | **Quantitatively Managed** | teal | Metrics + optimisation — predictive, data-driven improvements |
| 91–100 | **Optimising** | green | Continuous improvement — innovation, mentoring, contribution |

---

## Dimension Weighting

For instruments where dimensions are not equally weighted, the `weight` field on each dimension scales its contribution to the total. Default is equal weighting.

Example (technology assessment):
- `stack-fitness` — weight 0.30
- `technical-debt` — weight 0.25
- `lifecycle-management` — weight 0.20
- `skill-coverage` — weight 0.15
- `tooling-integration` — weight 0.10

---

## Per-Question Evidence

Each question in the instrument YAML has an `evidence` field describing what to look for. When scoring:

1. Read the question
2. Read the evidence list
3. Score 0–3 based on how many evidence items are present
4. Cite at least one piece of evidence per score of 2 or 3

This makes scores defensible in a stakeholder review.

---

## Aggregating Multiple Domain Scores

For the `dea:maturity-ea-capability` overall score:

```
ea_capability = (modernization × 0.25) + (technology × 0.25) +
                (operations × 0.25) + (services_delivery × 0.25)
```

(Adjust weights per organisation.)

---

## Reporting Cadence

| Score Change | Recommended Action |
|--------------|---------------------|
| Score dropped > 10 points | Trigger root-cause review |
| Score flat for 2 quarters | Re-validate instrument questions still apply |
| Score increased > 15 points | Promote learnings — capture as case study |
| All four domains at Optimising | Framework is self-sustaining — focus on community contribution |