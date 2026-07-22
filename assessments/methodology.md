# Assessment Methodology

How to run an assessment. Two modes: **self-assessment** and **facilitated workshop**. Choose based on stakes and available time.

---

## Self-Assessment

**When to use:**
- Initial baseline scan
- Annual health check
- Pre-merge gate for new initiatives

**Process:**
1. Identify the domain(s) to assess
2. Pull the instrument YAML from the relevant `dea-assessment-*` repo
3. Assign one person per dimension (4–6 people for a full four-domain sweep)
4. Each person scores their dimension independently
5. Aggregate scores — discuss differences of > 1 point per question
6. Produce a final score and band
7. File the result in your architecture registry

**Duration:** 2–4 hours total

**Output:** One-page summary per domain with scores and top 3 actions

---

## Facilitated Workshop

**When to use:**
- Strategy review or transformation kick-off
- Significant organisational change
- Board-level reporting

**Process:**
1. Engage an Assessment Models facilitator (or trained internal facilitator)
2. Stakeholder pre-read — instrument dimensions shared 1 week before
3. Workshop — 3–4 hours, structured:
   - **Per dimension (45 min):** Facilitator presents questions, participants score live, evidence cited
   - **Cross-cut discussion (45 min):** Where do domains reinforce or contradict?
   - **Action prioritisation (30 min):** Top 5 actions by impact / effort
4. Report — published within 5 working days
5. Follow-up — quarterly re-assessment

**Duration:** 3–4 hour workshop + 5 day report turnaround

**Output:** Detailed report with scores, evidence, prioritised action plan

---

## Choosing Between Modes

| Factor | Self-Assessment | Facilitated |
|--------|-----------------|-------------|
| Stakes | Low–medium | High |
| Participants | 1 per dimension | 8–15 cross-functional |
| Time | 2–4 hours | 3–4 hour workshop |
| Output depth | One-pager | Detailed report |
| Cost | Free | Paid (engagement) |

---

## Combining with Maturity Models

Always pair assessment scores with maturity-level analysis:

1. Run the assessment
2. Map score to maturity band
3. Read the maturity model's `characteristics`, `exit_criteria`, and `evidence` for that band
4. Identify gaps between current state and band exit criteria
5. Define top 3 actions to close gaps
6. Schedule next assessment to track progress

The maturity model gives the *meaning* behind the score.

---

## Common Pitfalls

1. **Scoring aspirationally.** People score where they want to be, not where they are. Counter: require evidence citations for scores of 2+.
2. **Skipping dimensions.** Marking a dimension N/A when it's inconvenient. Counter: every dimension must be answered.
3. **Single-respondent bias.** One person scores everything. Counter: assign dimensions to whoever owns that area.
4. **No follow-up.** Assessment is run, report is filed, nothing changes. Counter: each assessment must produce a tracked action plan with owners and dates.

---

## Related Documents

- `scoring-rubric.md` — how scores map to bands
- `schemas/instrument.schema.json` — required structure of each instrument
- [dea-catalog-maturity-models](https://github.com/Assessment-Models/dea-catalog-maturity-models) — the maturity model registry