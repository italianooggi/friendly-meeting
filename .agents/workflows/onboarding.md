---
description: One-time setup workflow to populate the candidate's knowledge base (Brain) from scratch or from existing sources. Uses the structured candidate interview protocol.
---

# Onboarding Workflow

This workflow guides the initial setup of the candidate's persistent context — the "Brain" that powers all future CV generation. It uses a structured **4-pillar interview** to extract maximum value.

## Prerequisites
- The repository has been cloned and the directory structure exists
- The candidate has at least one of: existing CV, LinkedIn profile, or is ready to answer questions

## Steps

### Step 0: Identify the Candidate
// turbo
**This step is mandatory.** Before doing anything else, establish who this onboarding is for.

Ask:
> _"¿Cuál es el nombre del candidato que vamos a registrar?"_

**Resolution logic:**

| Situation | Action |
|---|---|
| User provides a name (e.g. "Guadalupe") | Brain will be saved to `context/candidates/guadalupe/` — announce: _"✅ Creando Brain para **Guadalupe**"_ |
| Candidate already exists | Ask: _"Ya existe un Brain para este candidato. ¿Quieres actualizar los datos existentes o crear uno nuevo?"_ |
| No name provided | Ask again — onboarding cannot proceed without a candidate name |

> 🔒 **Privacy rule**: Each candidate has their own isolated folder. Data from one candidate never touches another's Brain.

Create the folder if it doesn't exist: `context/candidates/[name]/`

Display this header at the start of every response from here on:
```
👤 Candidato: [Name]
📁 Brain: context/candidates/[name]/
🎯 Modo: Onboarding
```

### Step 1: Choose Onboarding Mode
The candidate can be onboarded via any combination of:
- **Mode A**: Structured Interview (RECOMMENDED — deepest extraction)
- **Mode B**: Existing CV/Resume (PDF, Word, or text) — quick start
- **Mode C**: LinkedIn profile URL — supplementary data

**Best practice**: Start with Mode B or C to pre-populate, then run Mode A to fill gaps.

### Step 2: Pre-populate from Existing Sources (if available)
If the candidate provides a CV or LinkedIn URL:
1. Extract raw content using `read_url_content` or browser tools
2. Parse into initial drafts of the Brain files
3. Mark any sections that are thin or missing context as `<!-- NEEDS INTERVIEW -->`

### Step 3: Run the Candidate Interview (Skill: `candidate-interview`)
Read `skills/candidate-interview/SKILL.md` and execute the structured interview.

The interview covers **4 pillars**:
1. 🔙 **De Dónde Vengo** (Where I Come From) — education, early career, formative experiences
2. 📍 **Lo Que Soy Ahora** (What I Am Now) — current role, impact, skills, identity
3. 🚀 **Hacia Dónde Voy** (Where I'm Going) — aspirations, target roles, learning path
4. 🌟 **Quién Soy** (Who I Am) — life stories, passions, motivations, differentiators

**User checkpoint**: The interview can be done in multiple sessions. Ask the candidate how much time they have.

### Step 4: Create/Update Profile (`context/candidates/[name]/profile.md`)
From interview answers (mainly Pillars 2, 3, 4):
- [ ] Full name, location, contact info
- [ ] Professional summary (2-3 sentences, adaptable)
- [ ] Professional identity tags (titles, certifications, awards)
- [ ] Languages spoken and CEFR proficiency level
- [ ] Services offered (if freelance/consulting)
- [ ] Target market and job preferences (from Pillar 3)
- [ ] Personal preferences for CV generation (what to emphasize or exclude)
- [ ] Work permit / availability / location preferences

### Step 5: Create/Update Experiences (`context/candidates/[name]/experiences.md`)
From interview answers (mainly Pillars 1 and 2):
- [ ] **Every** past role listed with: company, location, dates (MM/YYYY), title, type
- [ ] 3-6 key achievements per role using **PAR/CAR/XYZ** formulas (with **metrics**)
- [ ] Technologies used per role
- [ ] Mark template placeholders if data is incomplete
- [ ] Note career gaps with honest framing (from Q52)

### Step 6: Create/Update Skills Inventory (`context/candidates/[name]/skills.md`)
From interview answers (mainly Pillar 2, Q20-Q23):
- [ ] Technical skills grouped by category with proficiency levels
- [ ] Soft skills with evidence from interview stories
- [ ] Certifications and awards (with dates and issuers)
- [ ] Education (degree, institution, year, honors)
- [ ] Skills currently being learned (from Pillar 3, Q31-Q33)

### Step 7: Create/Update Life Stories (`context/candidates/[name]/life_stories.md`)
From interview answers (mainly Pillar 4, Q39-Q50):
- [ ] At least 3-4 impact stories in STAR format
- [ ] Each story must include quantified results
- [ ] Include "defining moments" and career pivots
- [ ] Side projects, open source, community work
- [ ] Content creation / public speaking evidence
- [ ] Motivations and passions that could differentiate in a cover letter

### Step 8: Validate Completeness
Review all created files against minimum thresholds:
- [ ] `profile.md` has all required sections
- [ ] `experiences.md` has at least 2 verified positions with metrics
- [ ] `skills.md` covers hard skills, soft skills, and education
- [ ] `life_stories.md` has at least 3 fully-formed STAR stories
- [ ] No `<!-- NEEDS INTERVIEW -->` markers remain
- [ ] Career gaps are noted and explained
- [ ] Target role/market preferences are defined

### Step 9: Save as Baseline
The candidate's Brain is now ready. Confirm to the user:

```
✅ Brain creado para: [Candidate Name]
📁 Ubicación: context/candidates/[name]/
🏆 Calidad estimada: [run candidate-quality for exact score]

Próximo paso: ejecuta /apply con una oferta laboral.
```

## Output
A fully populated `context/candidates/[name]/` directory — isolated, private, never committed to git.

## Notes
- **Step 0 is mandatory** — no files are created until the candidate name is confirmed
- This workflow should be **re-run** whenever the candidate changes jobs, learns a new skill, or achieves something notable
- The interview (Step 3) can be done **partially** — start with what's available, enrich later
- Files are plain Markdown — the candidate can edit them manually at any time
- `context/candidates/` is in `.gitignore` — real candidate data is NEVER pushed to GitHub
- The `candidate-interview` skill has 53 questions organized by pillar — you don't need to ask all of them every time
- 🔒 Each candidate is fully isolated — running onboarding for Guadalupe will never touch Gustavo's files

