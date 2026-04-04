---
name: resume-tailor
description: Rewrites and optimizes resume content based on the match report, producing a tailored CV draft optimized for the specific job position.
---

# Resume Tailor Skill

## Purpose
Take the match report and produce a **complete, tailored CV draft** in Markdown format. The CV must be optimized for the specific job while remaining 100% truthful to the candidate's real experience.

## Input
- `output/match_report.md` — The profile match report (produced by `profile-matcher`)
- `output/job_analysis.md` — The original job analysis (for keyword bank and market context)
- `context/candidate/profile.md` — For personal info and summary preferences
- `context/ats_rules.md` — **MUST READ FIRST** — Contains formatting rules, bullet formulas, and market-specific conventions

## Process

### Step 1: Craft the Professional Summary
Write a 2-3 sentence professional summary that:
- Opens with years of experience and primary role identity
- Highlights the **most relevant achievement** for this specific job
- Includes 2-3 **ATS keywords** naturally
- Follows the candidate's tone preferences from `profile.md`

### Step 2: Select and Rewrite Experience Bullets
For each selected experience from the match report:
1. Write **3-6 bullet points** per position (more for the most recent/relevant)
2. Each bullet **MUST** use one of the three proven formulas from the research:

**🎯 PAR (Project/Problem – Action – Result):**
> [Action verb] + [what you did] + [tool/method] + [measurable impact]
> _"Automatié la conciliación diaria con Python y SQL, reduciendo el tiempo de cierre de 6h a 45min y disminuyendo errores un 30%."_

**🎯 CAR (Challenge – Action – Result):**
> [Challenge context] → [what you did] → [outcome]
> _"Ante un backlog creciente de incidencias, rediseñé el triage y métricas de SLA; bajamos el tiempo medio de resolución un 25% en 8 semanas."_

**🎯 XYZ (Google/Federal style):**
> Logré [X] (medido por [Y]) haciendo [Z]
> _"Logré reducir costes cloud (medido por € mensuales) renegociando reservas y aplicando políticas de autoscaling."_

3. Naturally embed keywords from the **Keyword Bank** in `job_analysis.md`
4. Use the **exact terminology** the company uses (primary term from the keyword bank)
5. Include acronym + expansion on first mention: "CI/CD (Continuous Integration / Continuous Delivery)"

**Golden Rules for Bullets:**
- Start with strong action verbs: Led, Developed, Implemented, Optimized, Designed, Architected, Mentored, Reduced, Increased, Delivered
- Include numbers: percentages, team sizes, time savings, user counts
- Connect to business impact, not just technical tasks
- Vary the verb — never start two consecutive bullets with the same word
- Use metric categories: Growth, Efficiency, Quality, Scale, Experience (see `context/ats_rules.md`)

### Step 3: Curate Skills Section
From the matched skills:
1. List skills in order of relevance to the job
2. Use the **exact naming** the job description uses
3. Group logically (Languages, Frameworks, Tools, Cloud, etc.)
4. Include adjacent skills with honest framing

### Step 4: Include Education & Awards
- Education: Include degree, university, location, year, and honors
- Awards: Include only if relevant to the job or highly prestigious (GitHub Star, MVP, GDE)

### Step 5: Assemble the CV Draft

```markdown
# [Candidate Name]
[Location] · [LinkedIn] · [Phone] · [Email]

## Professional Summary
[Tailored summary paragraph]

## Professional Experience

### [Company Name] | [Location]
**[Title]** | [Start Date] – [End Date]
- [Bullet 1 — strongest, most relevant achievement]
- [Bullet 2]
- [Bullet 3]
- [Bullet 4 — if warranted]

### [Company Name 2] | [Location]
**[Title]** | [Start Date] – [End Date]
- [Bullet 1]
- [Bullet 2]

## Education
**[University]** | [Location]
[Degree] | [Year]
- [Honors/Distinctions]

## Technical Skills
[Grouped skill list]

## Additional
- [Content creation, languages, certifications — if relevant]
```

### Step 6: Apply Market-Specific Rules
Based on `job_analysis.md → Market Context`:
- **US**: Exclude photo, age, marital status, nationality. Use "resume" language.
- **UK**: Exclude photo, DOB. Use "CV" language. Include facts and figures.
- **EU/Spain**: Match language of job posting. Consider Europass format if requested.
- **LATAM**: Adapt to country conventions (photo norms vary).
- **File naming**: `FirstName_LastName_TargetRole.pdf`

### Step 7: Save Output
Save the tailored CV to `output/cv_draft.md`.

## Output
A complete, ready-to-validate CV draft saved to `output/cv_draft.md`.

## Anti-Hallucination Rules ⚠️
- **NEVER invent** experiences, skills, or metrics not present in the candidate's context
- **NEVER exaggerate** numbers or scope beyond what the source material states
- **NEVER add keywords from the JD that the candidate doesn't genuinely possess**
- If a skill gap exists, **omit it** — do not fabricate proficiency
- Every claim in the CV must be traceable to a file in `context/candidate/`
- When rephrasing, maintain the **spirit and scale** of the original achievement
- Prefer "proxy metrics" over no metrics — but NEVER invent the metric itself

## Reference
Always consult `context/ats_rules.md` for the latest formatting rules, bullet formulas, and market-specific conventions.
