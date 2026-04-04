---
name: profile-matcher
description: Matches the candidate's context (experiences, skills, life stories) against analyzed job requirements to find the most relevant content for the CV.
---

# Profile Matcher Skill

## Purpose
Cross-reference the job analysis output with the candidate's knowledge base to identify the **most relevant** experiences, skills, and stories to include in the tailored CV.

## Input
- `output/job_analysis.md` — The structured job analysis (produced by `job-analyzer`)
- `context/candidate/` — The full candidate knowledge base:
  - `profile.md` — Summary, identity, preferences
  - `experiences.md` — Work history
  - `skills.md` — Skills inventory
  - `life_stories.md` — Impact narratives

## Process

### Step 1: Load Context
Read all files in `context/candidate/` to build a complete picture of the candidate.

### Step 2: Match Hard Skills
For each **Hard Skill Required** from the job analysis:
1. Search `skills.md` for a direct match
2. If found → mark as ✅ MATCH with evidence
3. If not found → mark as ❌ GAP
4. If partially related → mark as ⚠️ ADJACENT (explain the connection)

### Step 3: Match Experiences
For each **Key Responsibility** from the job analysis:
1. Search `experiences.md` for relevant work experience
2. Rank experiences by relevance to this specific job
3. Select the **top 3-5 most relevant** bullet points/achievements

### Step 4: Select Impact Stories
For each matched experience, check `life_stories.md` for:
1. STAR-formatted stories that demonstrate the required competencies
2. Stories with **quantified results** that align with the job's domain
3. Prioritize stories that demonstrate the **seniority level** required

### Step 5: Gap Analysis
Identify what the candidate is **missing** relative to the job requirements:
- Skills not present in the inventory
- Experience gaps
- Potential concerns (e.g., industry mismatch)

### Step 6: Generate Match Report

```markdown
## Profile Match Report

### Match Score: [X/10]

### Matched Skills
| Job Requirement | Candidate Evidence | Source |
|---|---|---|
| React | 5+ years React experience at Adevinta | experiences.md |
| ... | ... | ... |

### Skill Gaps
- [Gap 1] — Suggested mitigation strategy
- [Gap 2] — Suggested mitigation strategy

### Selected Experiences (Ranked)
1. [Experience title] — Relevance: [High/Medium] — Why: [explanation]
2. ...

### Selected Stories
1. [Story title] — Maps to: [requirement] — Key metric: [number]
2. ...

### Recommended Summary Angle
[Brief description of how to angle the professional summary for this specific job]
```

## Output
A structured match report saved to `output/match_report.md`.

## Tips for the Agent
- **Prefer depth over breadth**: 3 highly relevant experiences > 7 loosely related ones
- **Respect candidate preferences**: Check `profile.md` → Preferences section
- **Adjacent skills matter**: If the job asks for Vue.js and the candidate knows React, that's an ADJACENT match worth mentioning
- **Quantified results win**: Always prefer experiences/stories that include numbers, percentages, or metrics
- **Recency bias**: More recent experiences should be weighted higher unless the job specifically values longevity
