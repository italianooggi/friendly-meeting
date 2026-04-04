---
name: candidate-quality
description: Evaluates the depth and completeness of the candidate's Brain (context/candidate/) and assigns a quality tier (S/A/B/C/INVENT) that determines what the system can reliably produce.
---

# Candidate Quality Evaluator Skill

## Purpose
Before generating any CV, **assess the quality of the candidate's input data**. The quality tier directly determines what the system can promise and what guardrails apply.

> "A CV is only as good as the raw material behind it."

## Why This Matters
- Prevents the system from producing weak CVs that damage the candidate's chances
- Sets honest expectations about output quality
- Gates the pipeline: low-quality input → the system asks for more data instead of guessing
- Directly reinforces anti-hallucination: **you can't write what you don't know**

## Input
- `context/candidate/profile.md`
- `context/candidate/experiences.md`
- `context/candidate/skills.md`
- `context/candidate/life_stories.md`

## Process

### Step 1: Score Each Brain File

Evaluate each file against its criteria and assign points:

#### profile.md (max 20 points)
| Criteria | Points | What to Check |
|---|---|---|
| Contact info complete | 3 | Name, location, email, phone, LinkedIn |
| Professional summary exists | 3 | 2-3 sentence summary with identity |
| Professional identity tags | 3 | Titles, awards, certifications listed |
| Languages with CEFR levels | 3 | At least native language + one more with level |
| Target market/role defined | 4 | From Pillar 3 of interview: what they want next |
| Preferences documented | 4 | What to emphasize, what to exclude, work model |

#### experiences.md (max 35 points)
| Criteria | Points | What to Check |
|---|---|---|
| At least 2 verified positions | 5 | Company, title, dates (MM/YYYY) confirmed |
| At least 3 positions with full history | 5 | Complete career timeline proportional to experience |
| Achievements have metrics (≥60%) | 8 | Numbers, percentages, time savings, user counts |
| Achievements use PAR/CAR/XYZ format | 5 | Structured as Action + Context + Result |
| Technologies listed per role | 4 | Specific tools, not just "programming" |
| Career gaps explained (if any) | 4 | Honest framing of any timeline breaks |
| Recency: most recent role is detailed | 4 | Current/last role has 4+ detailed bullets |

#### skills.md (max 20 points)
| Criteria | Points | What to Check |
|---|---|---|
| Hard skills listed (≥10) | 5 | Technical skills with categories |
| Proficiency levels assigned | 4 | Not just listed, but rated (Advanced/Intermediate/etc.) |
| Soft skills with evidence | 4 | Not just "leadership" — "Led team of 8 for 2 years" |
| Education complete | 4 | Degree, institution, year, honors if any |
| Certifications with dates | 3 | Issuer and year, not just name |

#### life_stories.md (max 25 points)
| Criteria | Points | What to Check |
|---|---|---|
| At least 3 STAR stories | 8 | Situation + Task + Action + Result, fully formed |
| Stories have quantified results | 6 | Every story includes at least one metric |
| Stories cover different competencies | 5 | Not all about the same thing (leadership + technical + impact + innovation) |
| Personal differentiators present | 3 | Side projects, community, content creation, passions |
| Defining moments / career pivots | 3 | Unique narrative elements that make the candidate memorable |

---

### Step 2: Calculate Total Score and Assign Tier

**Total possible: 100 points**

| Tier | Score Range | Label | Meaning |
|---|---|---|---|
| **S** | 85-100 | 🏆 Datos Excelentes | Brain completo, rico en métricas y narrativas. El sistema puede generar CVs de alta competitividad con máxima personalización. |
| **A** | 70-84 | ✅ Datos Buenos | Brain sólido con algunas áreas que podrían enriquecerse. El sistema genera CVs competitivos, puede haber bullets sin métrica que se resuelven con "proxy metrics". |
| **B** | 50-69 | ⚠️ Datos Aceptables | Brain funcional pero con gaps notables. El sistema genera un CV correcto pero no diferenciador. Se recomienda enriquecer antes de aplicar a roles altamente competitivos. |
| **C** | 25-49 | 🔶 Datos Insuficientes | Brain demasiado fino. El sistema puede generar un borrador básico pero el resultado será genérico. Se requiere completar el onboarding antes de aplicar. |
| **INVENT** | 0-24 | 🔴 Modo Ejemplo | Datos insuficientes para representar al candidato real. El sistema solo puede generar **CVs de ejemplo/plantilla** para un puesto dado, usando datos ficticios. Estos CVs deben marcarse claramente como NO REALES. |

---

### Step 3: Generate Quality Report

```markdown
## Candidate Data Quality Report

### Overall Tier: [S/A/B/C/INVENT] — [Score]/100
[One-line description of what this means]

### Score Breakdown
| Area | Score | Max | Status |
|---|---|---|---|
| Profile | [X] | 20 | [🟢/🟡/🔴] |
| Experiences | [X] | 35 | [🟢/🟡/🔴] |
| Skills | [X] | 20 | [🟢/🟡/🔴] |
| Life Stories | [X] | 25 | [🟢/🟡/🔴] |

### Strengths
- [What's well-documented]
- [What's particularly rich]

### Critical Gaps (blocking higher tier)
- [Missing item 1] — Impact: [why it matters for CV quality]
- [Missing item 2] — Impact: [why it matters]

### Recommendations to Level Up
1. [Specific action to improve score] — Estimated effort: [X min]
2. [Specific action] — Estimated effort: [X min]
3. [Specific action] — Estimated effort: [X min]

### Pipeline Permission
- [✅/❌] Can generate tailored CV for job applications
- [✅/❌] Can generate competitive CV for senior roles
- [✅/❌] Can run full ATS optimization
- [✅/❌] Anti-hallucination guardrails: [STRICT / RELAXED / BLOCKED]
```

### Step 4: Save Output
Save the quality report to `output/quality_report.md`.

---

## Tier Behavior Rules (How Each Tier Affects the Pipeline)

### Tier S (85-100) — Full Power
- Pipeline runs without restrictions
- All bullet formulas available
- Deep personalization possible
- Summary can be highly strategic

### Tier A (70-84) — Standard Operation
- Pipeline runs normally
- May use "proxy metrics" where exact numbers are missing
- Flag any bullet that required estimation

### Tier B (50-69) — Proceed with Warnings
- Pipeline runs but shows ⚠️ warnings at each step
- Resume-tailor must flag weak sections
- ATS validator relaxes metric density requirement (50% instead of 60%)
- Suggest running `/onboarding` interview for gap areas BEFORE applying

### Tier C (25-49) — Restricted Mode
- Pipeline runs ONLY after user explicitly acknowledges quality limitations
- Output CV is labeled as "DRAFT — REQUIRES ENRICHMENT"
- System actively recommends specific interview questions from `candidate-interview` skill to fill gaps
- Anti-hallucination guardrails are **MAXIMUM STRICT**

### Tier INVENT (0-24) — Example Mode Only
- **Cannot generate a real CV** — insufficient candidate data
- Can generate a **template/example CV** for a given job description
- All generated content is marked as fictional: `<!-- EXAMPLE ONLY — NOT REAL CANDIDATE DATA -->`
- Useful for: showing the user what a good CV looks like for a specific role
- Required disclosure: "Este CV es un ejemplo generado por el sistema. No representa datos reales del candidato."
- The user must run `/onboarding` to provide real data before generating a real CV

---

## When to Run This Skill
1. **After onboarding** — to validate the Brain is ready
2. **Before every `/apply` run** — to set expectations and guardrails
3. **After adding new data** — to track improvement
4. **On demand** — when the user asks "¿está listo mi perfil?"

## Tips for the Agent
- Be honest but constructive — the goal is to **help the candidate improve**, not to discourage
- The biggest quality multiplier is **metrics**: a single experience with numbers is worth more than three without
- INVENT mode is valuable — use it to show the candidate what their CV COULD look like if they provide real data
- Quality tier should be **prominently displayed** whenever the system generates output
