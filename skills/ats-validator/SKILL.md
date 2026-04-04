---
name: ats-validator
description: Validates a tailored CV draft against the job requirements to ensure optimal ATS keyword coverage, proper formatting, and anti-hallucination compliance.
---

# ATS Validator Skill

## Purpose
Act as the **quality gate** before final output. Validate that the tailored CV meets ATS optimization standards, contains required keywords, and does not contain hallucinated content.

## Input
- `output/cv_draft.md` — The tailored CV draft (produced by `resume-tailor`)
- `output/job_analysis.md` — The original job analysis (for keyword checklist AND keyword bank)
- `output/match_report.md` — The match report (for gap awareness and honest gap disclosure)
- `context/candidate/` — The source-of-truth context files
- `context/ats_rules.md` — **MUST READ** for validation criteria reference

## Process

### Step 1: Plain-Text Parsing Test (NEW — from ATS research)
Simulate what an ATS parser would see:
1. Strip all Markdown formatting from `cv_draft.md` — treat it as raw text
2. Verify the **reading order is logical**: Name → Summary → Skills → Experience → Education
3. Verify dates appear in correct chronological order (most recent first)
4. Verify no critical data is "lost" (contact info, company names, dates)
5. If the order is broken → **automatic FAIL** (parsing will break in real ATS)

### Step 2: Keyword Coverage Check (Enhanced with Keyword Bank)
For each keyword in `job_analysis.md → Keyword Bank`:
1. Search the CV draft for the **primary term** (exact match)
2. If not found, search for **any variant** (acronym, expansion, regional variant)
3. Check that keywords appear **in context** (within achievement bullets), not just in a skills list
4. Calculate coverage: `(found keywords / total required keywords) × 100`
5. **Target: 60-80% keyword coverage**
6. Flag missing critical keywords separately from nice-to-have keywords

**Anti-stuffing check**: Flag if any keyword appears more than 3 times — ATS penalizes unnatural repetition

### Step 3: Structural Validation (Enhanced per ATS research)
Verify against `context/ats_rules.md` formatting rules:
- [ ] **Single column layout** — no sidebars, tables, or multi-column elements
- [ ] **Standard section headings** (Experience, Education, Skills, Certifications, Languages)
- [ ] **Standard date formats** (MM/YYYY consistently)
- [ ] **Contact info in body** (NOT in header/footer — parsers often skip these)
- [ ] **No icons replacing text** (no 📧 instead of "Email")
- [ ] **No skill bars or visual indicators** (use text levels: "Advanced", "C1", etc.)
- [ ] **No hidden text** or white-on-white keywords
- [ ] **Standard fonts** referenced (Arial, Calibri, Times New Roman)
- [ ] **Length appropriate** (1-2 pages for most roles)
- [ ] **File size** would be under 2 MB when exported

### Step 4: Market Compliance Check
Based on `job_analysis.md → Market Context`:
- [ ] **US**: No photo, no age, no personal data included
- [ ] **UK**: No photo, no DOB, no marital status
- [ ] **EU/Spain**: Language matches job posting language
- [ ] **LATAM**: Appropriate for target country conventions
- [ ] **Language consistency** — no mixed-language headings unless deliberate

### Step 5: Content Quality Check
- [ ] Every bullet point starts with an action verb
- [ ] Every bullet uses PAR, CAR, or XYZ formula structure
- [ ] At least **60%** of bullet points contain quantified metrics
- [ ] No buzzword-only bullets (e.g., "team player passionate about innovation")
- [ ] Job title matches or is close to the target position
- [ ] Professional summary mentions the target role/industry
- [ ] Skills section lists 12-18 items grouped by category
- [ ] Acronyms are expanded on first use ("CI/CD (Continuous Integration / Continuous Delivery)")

### Step 6: Anti-Hallucination Audit
For **each claim** in the CV:
1. Trace it back to a specific source in `context/candidate/`
2. Verify numbers/metrics match the original — **no inflation**
3. Verify no keywords from the JD were added that the candidate doesn't actually possess
4. Flag any claim that cannot be sourced as ⚠️ UNVERIFIED
5. Verify the **spirit and scale** of rephrased achievements match the original

### Step 5: Generate Validation Report

```markdown
## ATS Validation Report

### Overall Score: [X/100]

### Keyword Coverage: [Y%]
| Keyword | Found | Location in CV |
|---|---|---|
| React | ✅ | Experience §1, Skills |
| TypeScript | ✅ | Experience §1, Skills |
| [Missing keyword] | ❌ | — |

### Structure Check
- [✅/❌] Clear section headers
- [✅/❌] Standard date formats
- [✅/❌] ATS-friendly formatting
- [✅/❌] Contact info at top
- [✅/❌] Appropriate length

### Content Quality
- Action verb compliance: [X]%
- Quantified bullets: [X]%
- Buzzword-free: [Yes/No]

### Hallucination Check
- [✅] All claims verified against source context
- [⚠️] Unverified claims: [list if any]

### Recommendations
1. [Actionable fix 1]
2. [Actionable fix 2]
3. [Actionable fix 3]
```

### Step 8: Scoring Formula
Calculate the overall score as a weighted sum:

| Component | Weight | Score Range |
|---|---|---|
| Parsing Test (Step 1) | 20% | 0 or 100 (pass/fail) |
| Keyword Coverage (Step 2) | 30% | 0-100 based on % |
| Structure (Step 3) | 15% | % of checks passed |
| Market Compliance (Step 4) | 10% | % of checks passed |
| Content Quality (Step 5) | 15% | % of checks passed |
| Hallucination Audit (Step 6) | 10% | 0 if any unverified; 100 if clean |

**Overall = Σ (weight × component score)**

### Step 9: Decision
- **Score ≥ 75**: ✅ PASS — CV is ready for final formatting
- **Score 50-74**: ⚠️ NEEDS REVISION — Send back to `resume-tailor` with specific recommendations
- **Score < 50**: ❌ FAIL — Significant rework needed
- **Parsing Test = 0**: ❌ AUTOMATIC FAIL — regardless of other scores

## Output
A validation report saved to `output/validation_report.md`.

## Tips for the Agent
- Be **strict but fair**: the goal is to catch real problems, not nitpick style
- **Missing keywords are the highest priority** — they directly affect ATS pass rates
- If a keyword is missing but the candidate genuinely doesn't have that skill, note it as an **honest gap** rather than forcing it in
- Check for **keyword stuffing** too — ATS systems can penalize unnatural repetition
