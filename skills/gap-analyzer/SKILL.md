---
name: gap-analyzer
description: Evaluates one or more job offers against the candidate's Brain, ranks them by fit, and generates a specific GAP report with actionable recommendations for each position.
---

# GAP Analyzer Skill

## Purpose
Answer the question: **"Given these job offers, which one fits me best — and what exactly am I missing for each?"**

This skill produces:
1. A **ranked list** of job offers sorted from best to worst fit
2. A **GAP score** per position (points of gap vs. profile)
3. **Specific, actionable recommendations** to close each gap
4. A **"quick win" suggestion**: which gap items are achievable in <30 days

## Input
- `context/candidate/` — Candidate Brain (profile, experiences, skills, life_stories)
- `output/quality_report.md` — Candidate quality tier (run `candidate-quality` first if missing)
- **One or more job descriptions** — as URLs or pasted text (provided by user)

## Process

### Step 1: Load Candidate Profile
Read all files in `context/candidate/` to build a complete picture of the candidate's:
- Hard skills with proficiency levels
- Soft skills with evidence
- Years and types of experience
- Education and certifications
- Industry exposure
- Current quality tier (from `output/quality_report.md` or re-evaluate)

### Step 2: Analyze Each Job Offer
For each job offer provided, extract:
- Required hard skills (list each one)
- Required soft skills
- Minimum years of experience
- Education requirements
- Industry/domain requirements
- Seniority level
- Binary filters (location, language, visa)
- Any "nice-to-have" extras

Use the same process as the `job-analyzer` skill.

### Step 3: Score Each Offer

For each job offer, calculate a **Fit Score** out of 100:

| Dimension | Weight | Scoring Logic |
|---|---|---|
| Hard Skills Match | 35 pts | (Matched skills / Total required skills) × 35 |
| Experience Match | 25 pts | Relevance of past roles to responsibilities |
| Seniority Alignment | 15 pts | Does candidate level match expected level? |
| Education Match | 10 pts | Degree, certifications, domain |
| Soft Skills Match | 10 pts | Leadership, communication, collaboration |
| Differentiators | 5 pts | Unique strengths that exceed requirements |

**GAP Score = 100 - Fit Score**

For each skill/requirement that is NOT matched, classify the gap:

| Gap Type | Description |
|---|---|
| 🔴 **Hard Gap** | Skill is absent and critical. Blocking. |
| 🟠 **Soft Gap** | Skill is weak or partially present. Improvable. |
| 🟡 **Adjacent Gap** | Candidate has a related skill that could transfer with framing. |
| 🟢 **No Gap** | Candidate fully meets the requirement. |

### Step 4: Generate Rankings

Sort all analyzed offers by **Fit Score (descending)**. Present as:

```markdown
## 🏆 Job Fit Ranking

| # | Company | Role | Fit Score | GAP Score | Recommendation |
|---|---|---|---|---|---|
| 1 | [Company] | [Role] | 82/100 | 18 pts | Apply now |
| 2 | [Company] | [Role] | 65/100 | 35 pts | Apply after quick wins |
| 3 | [Company] | [Role] | 41/100 | 59 pts | Requires significant upskilling |
```

### Step 5: Generate Per-Job GAP Report

For each job offer in the ranking:

```markdown
## GAP Report: [Role] @ [Company]

### Fit Score: [X]/100 | GAP: [Y] points

### ✅ Matched Requirements
- [Requirement]: [Candidate evidence] — Source: [experiences.md / skills.md / etc.]

### 🔴 Hard Gaps (Blocking — must address)
- [Missing skill/requirement]
  → **How to close**: [Specific action] — Effort: [time estimate]

### 🟠 Soft Gaps (Important — address soon)
- [Weak skill/requirement]
  → **How to close**: [Specific action] — Effort: [time estimate]

### 🟡 Adjacent Strengths (Can be re-framed)
- [Candidate skill] → maps to [Job requirement] via [explanation of connection]

### ⚡ Quick Wins (Achievable in <30 days)
- [ ] [Specific action] — e.g., "Completa el módulo de ChromaDB en LangChain docs — 4h"
- [ ] [Specific action] — e.g., "Añade un proyecto con PyTorch a tu GitHub — 8h"
- [ ] [Specific action] — e.g., "Obtén la certificación X — 2 días"

### 💡 CV Angle for This Role
[How to frame the CV if applying — what to emphasize, what to downplay]

### Verdict
[Apply now / Apply after quick wins / Requires significant investment / Do not apply]
```

### Step 6: Save Report
Save the full ranking + GAP reports to `output/gap_analysis.md`.

Present a summary to the user, then offer to:
- Run `/apply` for the top-ranked offer immediately
- Show detailed GAP report for a specific offer
- Start a quick-win plan for a specific offer

---

## Gap-Closing Recommendation Logic

When generating "How to close" recommendations, use this priority order:

1. **Certifications** — if a certification can prove the skill (e.g., "Google Cloud Professional Data Engineer")
2. **Portfolio projects** — if a personal project would demonstrate the skill
3. **Re-framing existing experience** — if adjacent skills can be connected
4. **Courses/resources** — if learning is required (be specific: platform + course name)
5. **Community/visibility** — if open source or networking would help

Always provide **specific, actionable steps** — never generic advice like "learn more about X".

---

## Example Output Pattern

```
Se han detectado 30 puntos de GAP con esta posición.

Para acortar ese GAP deberías:
1. 🔴 Añadir ChromaDB o Pinecone a un proyecto real (o documentar uso de vectores en tus
   pipelines actuales de GCP) — 6-8 horas de trabajo
2. 🟠 Incluir PyTorch en tu skill set: completa el tutorial oficial + un mini-proyecto
   de fine-tuning — 2-3 días
3. 🟡 Re-encuadrar tus filtros de Kalman como experiencia en "optimización de modelos
   con datos ruidosos" — es adjacent directo a cuantización/destilación
4. ⚡ Quick win: Sube un repositorio público con tu pipeline N8N+LangChain — demuestra
   agentes de IA en producción sin necesidad de nueva formación
```

---

## When to Run This Skill
- When the user has **2+ job offers** and needs to prioritize
- When the user wants to know **what to learn next** to improve competitiveness
- As a **career planning tool** — "¿Qué me falta para llegar a X rol en 6 meses?"
- Before running `/apply` — to validate if the effort is worth it

## Tips for the Agent
- The ranking is only as good as the job analysis — be thorough in extracting requirements
- "Adjacent Gaps" are powerful: the candidate may be stronger than they think
- Quick Wins should be **genuinely quick** — if it takes >2 weeks, it's not a quick win
- Be honest about Hard Gaps — don't oversell a misfit to make the user feel good
- The GAP score should reflect reality: a 60-point GAP on a role means "significant investment needed"
