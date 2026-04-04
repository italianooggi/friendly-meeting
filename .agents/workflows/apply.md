---
description: End-to-end workflow to generate a tailored CV for a specific job application. This is the main recurring workflow.
---

# Apply Workflow

This is the **core workflow** of the Agente CV system. It takes a job description and produces a tailored, ATS-optimized CV.

## Prerequisites
- The user has a specific job description (URL or text)
- Ideally a candidate Brain exists in `context/candidates/<name>/`

## Steps

### Step 0: Identify the Candidate
// turbo
**This step is mandatory.** Before doing anything else, establish who this CV is for.

Check if the user already stated a candidate name in their message. If not, ask:

> _"¿Para qué candidato trabajamos? (nombre o carpeta)"_

**Resolution logic:**

| Situation | Action |
|---|---|
| User names a candidate (e.g. "Gustavo") | Load Brain from `context/candidates/gustavo/` — announce: _"✅ Trabajando con el perfil de **Gustavo**"_ |
| User provides a folder path | Load Brain from that path |
| No candidate named + no Brain exists | Activate **INVENT mode**: announce _"⚠️ Sin candidato definido — trabajaré con un perfil ficticio. El CV generado será un EJEMPLO y no representará datos reales."_ Use `context/candidate/` templates as base structure. |

> 🔒 **Privacy rule**: The agent must NEVER mix data between candidates. If switching candidates mid-session, explicitly confirm the switch before proceeding.

**From this point forward**, every reference to "the candidate Brain" means the resolved path from this step.

### Step 1: Receive Job Description
// turbo
Ask the user for the job description (if not already provided). Accept:
- A URL to the job posting, OR
- Raw text of the job description

If URL: use `read_url_content` to fetch the page content.

Display context header at the start of every response from here on:
```
👤 Candidato: [Name or FICTICIO]
📁 Brain: [resolved path]
📋 Posición: [Job title @ Company — updated as soon as known]
```

### Step 2: Evaluate Candidate Data Quality (Skill: `candidate-quality`)
Read the skill file at `skills/candidate-quality/SKILL.md` and execute it against the **resolved Brain path from Step 0**:
- Score across 4 dimensions (profile, experiences, skills, stories)
- Assign a quality tier: **S / A / B / C / INVENT**
- Save the quality report to `output/[candidate-name]/quality_report.md`
- Present the tier to the user with a summary

**Gate logic:**
- **Tier S or A** → Proceed to Step 3 (full pipeline)
- **Tier B** → Proceed with ⚠️ warning. Recommend enriching weak areas first, but allow the user to continue.
- **Tier C** → Show specific gaps. Recommend running `/onboarding` interview for the weak pillars. Allow proceeding only if user explicitly acknowledges limitations.
- **Tier INVENT** or **INVENT mode (no candidate)** → Offer to generate an **example CV** for the target position — all content marked as fictional with the disclaimer: _"⚠️ Este CV es un EJEMPLO generado por el sistema. No representa datos reales de ningún candidato."_

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
- Cross-reference `output/job_analysis.md` with the **resolved Brain path from Step 0**
- Generate a match report at `output/[candidate-name]/match_report.md`
- Present to the user:
  - Match score (X/10)
  - Key matched skills
  - Notable gaps (if any)
  - Recommended angle for the CV
  - **Candidate name + quality tier** reminder

**User checkpoint**: Ask the user if they want to adjust the angle or add context before proceeding.

### Step 5: Tailor the Resume (Skill: `resume-tailor`)
Read the skill file at `skills/resume-tailor/SKILL.md` and execute it:
- Write the tailored CV draft based on the match report
- Apply guardrail level from quality tier (S/A = standard, B = warned, C = maximum strict)
- If INVENT mode: all content must be clearly fictional, name the candidate "[CANDIDATE NAME]" as placeholder
- Save to `output/[candidate-name]/cv_draft.md`
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
The validated CV draft is the final product.

Present to the user:
- **Header**: `👤 [Candidate Name] → [Role] @ [Company]`
- **Data Quality Tier** badge (S/A/B/C) — or ⚠️ EJEMPLO if INVENT mode
- The complete CV in Markdown
- The validation score and keyword coverage
- Any honest gap disclosures
- If INVENT mode: prominent disclaimer — _"Este CV es un ejemplo ficticio generado sin datos reales de candidato"_
- If Tier B or C: explicit note about what could be improved with more data

### Step 8: Archive (Optional)
If the user wants to keep a record:
- Copy the output files to `output/archive/[candidate-name]/[company]-[date]/`
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
- **Step 0 is mandatory** — no CV generation starts without knowing the candidate or explicitly activating INVENT mode
- The entire pipeline runs conversationally — the agent executes each skill in sequence
- **Quality tier is the first gate** — it determines guardrail strictness for the entire run
- User checkpoints exist at Step 0, Step 4, and Step 6 for human oversight
- Output files are saved per-candidate: `output/[candidate-name]/`
- **INVENT mode** activates automatically when no candidate Brain exists — generates clearly labeled fictional CVs
- The workflow is designed to complete in a **single conversation session**
- 🔒 Never mix data between candidates — if the user switches candidate mid-session, confirm explicitly
