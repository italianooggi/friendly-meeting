# ATS Best Practices & Formatting Rules

> Reference document derived from state-of-the-art ATS research (April 2026).
> This file is consumed by the `resume-tailor` and `ats-validator` skills.

## The 3 Gates Every CV Must Pass

1. **Parseable** — The ATS can extract your name, experience, dates, and skills correctly
2. **Rankeable** — The matching engine (keyword + semantic) considers you relevant
3. **Convincing** — A human scanning in seconds finds clear evidence of fit

## Formatting Rules (Non-Negotiable)

### Structure
- ✅ **Single column layout** — no sidebars, no multi-column designs
- ✅ **Standard section headings**: "Experience", "Education", "Skills", "Certifications", "Languages"
- ✅ **Reverse chronological order** (most recent first)
- ✅ Contact info in the **body** of the document (NOT in header/footer)
- ✅ Dates in **MM/YYYY** format consistently
- ✅ Standard fonts: Arial, Calibri, Times New Roman, or similar

### Avoid
- ❌ Tables, text boxes, or floating elements
- ❌ Icons replacing text labels (🔧 instead of "Skills:")
- ❌ Skill bars or visual progress indicators (use levels: "Advanced", "Intermediate", or CEFR: B2, C1)
- ❌ Headers and footers (many parsers skip them)
- ❌ Images, logos, photos (unless explicitly required by market convention)
- ❌ Colored backgrounds or complex styling
- ❌ Hidden text, white text, or prompt injection attempts

### File Format
| Format | Reliability | When to Use |
|---|---|---|
| DOCX | High | Default for most ATS portals |
| PDF (text-based, exported from editor) | Medium-High | When portal allows/recommends it |
| PDF (scanned/image) | Very Low | NEVER — ATS can't read it |

### File Naming
Use: `FirstName_LastName_TargetRole.pdf` (e.g., `Miguel_Duran_Frontend_Architect.pdf`)

### Size
Keep under 2 MB. Some parsers have lower thresholds.

---

## Keyword Strategy

### Three Types of ATS Filtering
1. **Binary filters** (dealbreakers): location, work permit, availability, killer questions → Can't be fixed by CV content alone
2. **Keyword matching**: exact term matching against job requirements → Requires explicit vocabulary
3. **Semantic matching** (hybrid, 2026+): embedding-based similarity + keyword relevance → Rewards contextual use of keywords in real achievements

### Keyword Bank Construction
For every job application, build a keyword bank with:
- **Exact terms** from the job description
- **Acronym + full expansion**: "CI/CD (Continuous Integration / Continuous Delivery)"
- **Regional variants**: "CV" vs "resume", "optimisation" vs "optimization"
- **Title synonyms**: "Product Owner" ↔ "Product Manager" (only if honestly applicable)
- **Tool versions**: "React" AND "React.js" AND "ReactJS" (use the version the company uses)

### Keyword Placement (Natural, Not Stuffed)
- In the **Professional Summary**: 2-3 key terms
- In **bullet points**: embedded in real achievements
- In **Skills section**: explicit listing
- NEVER: repeated artificially, added without supporting evidence, or hidden in white text

---

## Bullet Point Formulas

Three proven frameworks (choose the best fit per bullet):

### PAR (Project/Problem – Action – Result)
> **[Action verb] + [what you did] + [tool/method] + [measurable impact]**
>
> "Automaticé la conciliación diaria con Python y SQL, reduciendo el tiempo de cierre de 6h a 45min y disminuyendo errores un 30%."

### CAR (Challenge – Action – Result)
> **[Challenge context] → [what you did] → [outcome]**
>
> "Ante un backlog creciente de incidencias, rediseñé el triage y métricas de SLA; bajamos el tiempo medio de resolución un 25% en 8 semanas."

### XYZ (Google/Federal style)
> **Logré [X] (medido por [Y]) haciendo [Z]**
>
> "Logré reducir costes cloud (medido por € mensuales) renegociando reservas y aplicando políticas de autoscaling."

### Metric Categories (Pick 1-3 per bullet)
- **Growth**: revenue, MRR, pipeline, conversion, retention, ARPU
- **Efficiency**: cycle reduction, automation, throughput, cost per transaction
- **Quality**: defect rate, incidents, compliance, error reduction
- **Scale**: users, data volume, requests/second, regions/countries
- **Experience**: NPS, CSAT, response time, churn

---

## Market-Specific Rules

### US (United States)
- No photo, no age, no marital status, no nationality
- Federal jobs (USAJOBS): require MM/YYYY dates, detailed descriptions, PDF ≤5MB
- "Resume" not "CV" (unless academic)

### UK (United Kingdom)
- No photo (anonymized hiring increasingly common)
- No age, date of birth, marital status, or nationality
- Use "CV" not "resume"
- "Achievements" with facts and figures emphasized

### EU / Spain
- Europass format accepted but not mandatory
- Adapt language to job posting language
- SEPE recommends adapting CV per position, avoiding unexplained abbreviations
- PDF or XML export for portal compatibility

### LATAM
- Photo still common in some markets (Argentina) but trending toward "CV ciego"
- Keep short (1-2 pages)
- Professional email mandatory
- Mexico pushing toward anonymous CVs in some public sectors

---

## Validation Checklist (Pre-Submission)

1. **Plain text test**: Copy-paste CV into Notepad — is the reading order logical?
2. **Auto-fill check**: If the portal parses your CV to fill fields, verify all data is correct
3. **Instruction compliance**: Accepted formats? Size limits? Required documents?
4. **Parser test** (optional): Run through a parsing tool trial to check field extraction
5. **Human scan test**: Can a recruiter understand your fit in 6 seconds?

---

## Anti-Hallucination Guardrails

When using AI to generate CV content:
- ✅ Only rewrite **existing evidence** — never invent achievements
- ✅ Every metric must be traceable to the candidate's actual data
- ✅ Keywords must only be added if the candidate genuinely has that skill
- ❌ Never add keywords from the JD that the candidate doesn't possess
- ❌ Never inflate scope, scale, or impact beyond what really happened
- ❌ Never use hidden text or prompt injection tricks
