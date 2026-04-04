---
description: End-to-end workflow to generate a tailored CV for a specific job application. This is the main recurring workflow.
---

# Apply Workflow

This is the **core workflow** of the Agente CV system. It takes a job description and produces a tailored, ATS-optimized CV.

## Prerequisites
- The candidate's Brain (`context/candidate/`) is populated (run `onboarding` first if not)
- The user has a specific job description (URL or text)

## Steps

### Step 1: Receive Job Description
// turbo
Ask the user for the job description. Accept:
- A URL to the job posting, OR
- Raw text of the job description

If URL: use `read_url_content` to fetch the page content.

### Step 2: Evaluate Candidate Data Quality (Skill: `candidate-quality`)
Read the skill file at `skills/candidate-quality/SKILL.md` and execute it:
- Score the candidate's Brain across 4 dimensions (profile, experiences, skills, stories)
- Assign a quality tier: **S / A / B / C / INVENT**
- Save the quality report to `output/quality_report.md`
- Present the tier to the user with a summary

**Gate logic:**
- **Tier S or A** → Proceed to Step 3 (full pipeline)
- **Tier B** → Proceed with ⚠️ warning. Recommend enriching weak areas first, but allow the user to continue.
- **Tier C** → Show specific gaps. Recommend running `/onboarding` interview for the weak pillars. Allow proceeding only if user explicitly acknowledges limitations.
- **Tier INVENT** → Cannot generate a real CV. Offer to generate an **example CV** for the target position instead (all content marked as fictional). Redirect to `/onboarding`.

### Step 3: Analyze the Job (Skill: `job-analyzer`)
Read the skill file at `skills/job-analyzer/SKILL.md` and execute it:
- Extract all structured requirements from the job description
- Build the keyword bank with variants
- Detect binary filters (location, visa, language)
- Save the analysis to `output/job_analysis.md`
- Present a quick summary to the user:
  - Position title and company
  - Top 5 required skills
  - Seniority level
  - ⚠️ Any binary filter warnings

### Step 4: Match Candidate Profile (Skill: `profile-matcher`)
Read the skill file at `skills/profile-matcher/SKILL.md` and execute it:
- Cross-reference `output/job_analysis.md` with `context/candidate/`
- Generate a match report at `output/match_report.md`
- Present to the user:
  - Match score (X/10)
  - Key matched skills
  - Notable gaps (if any)
  - Recommended angle for the CV
  - **Data quality tier** reminder (from Step 2)

**User checkpoint**: Ask the user if they want to adjust the angle or add context before proceeding.

### Step 5: Tailor the Resume (Skill: `resume-tailor`)
Read the skill file at `skills/resume-tailor/SKILL.md` and execute it:
- Write the tailored CV draft based on the match report
- Apply guardrail level from quality tier (S/A = standard, B = warned, C = maximum strict)
- Save to `output/cv_draft.md`
- Present the draft to the user for review

### Step 6: Validate ATS Compliance (Skill: `ats-validator`)
Read the skill file at `skills/ats-validator/SKILL.md` and execute it:
- Run keyword coverage, structure, and hallucination checks
- Save report to `output/validation_report.md`
- Present the score and any issues

**Decision point:**
- **Score ≥ 75**: Proceed to final output
- **Score < 75**: Return to Step 5 with the validator's recommendations and iterate

### Step 7: Final Output
The validated CV draft at `output/cv_draft.md` is the final product.

Present to the user:
- **Data Quality Tier** badge (S/A/B/C)
- The complete CV in Markdown
- The validation score and keyword coverage
- Any honest gap disclosures
- If Tier B or C: explicit note about what could be improved with more data

### Step 8: Archive (Optional)
If the user wants to keep a record:
- Copy the output files to `output/archive/[company-name]-[date]/`
- This preserves the quality report, analysis, match report, and final CV for future reference

## Pipeline Diagram

```
                    ┌──────────────────┐
┌─────────────┐    │ Quality Evaluator │
│  Job Input  │───▶│ S/A → proceed    │
│ (URL/Text)  │    │ B → warn         │
└─────────────┘    │ C → restrict     │
                    │ INVENT → example │
                    └────────┬─────────┘
                             │
                    ┌────────▼────────┐    ┌─────────────────┐
                    │  Job Analyzer   │───▶│ Profile Matcher │
                    │ + Keyword Bank  │    │ + Match Score   │
                    └─────────────────┘    └────────┬────────┘
                                                    │
                    ┌──────────────┐       ┌────────▼────────┐
                    │ATS Validator │◀──────│ Resume Tailor   │
                    │ + Scoring    │       │ + PAR/CAR/XYZ   │
                    └──────┬───────┘       └─────────────────┘
                           │
                    ┌──────▼───────┐
                    │  ≥ 75? PASS  │──▶ Final CV [Tier Badge]
                    │  < 75? LOOP  │──▶ Back to Tailor
                    └──────────────┘
```

## Notes
- The entire pipeline runs conversationally — the agent executes each skill in sequence
- **Quality tier is the first gate** — it determines guardrail strictness for the entire run
- User checkpoints exist at Step 2, Step 4, and Step 6 for human oversight
- Each run overwrites the `output/` files (use archiving in Step 8 to preserve)
- The workflow is designed to complete in a **single conversation session**
- **INVENT mode** is for demo/example only — it generates fictional CVs clearly labeled as such
