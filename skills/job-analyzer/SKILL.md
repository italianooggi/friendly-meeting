---
name: job-analyzer
description: Analyzes a job description (URL or raw text) and extracts structured requirements including hard skills, soft skills, keywords, experience level, and company context.
---

# Job Analyzer Skill

## Purpose
Parse and analyze a job description to extract a structured breakdown of what the position requires. This is always the **first step** in the CV generation pipeline.

## Input
- A job description provided as:
  - Raw text pasted by the user
  - A URL to a job posting (use `read_url_content` or browser tools to fetch it)

## Process

### Step 1: Extract Raw Content
If the input is a URL, fetch the page content and extract the job description text, discarding navigation, ads, and boilerplate.

### Step 2: Structured Analysis
Analyze the job description and extract the following categories:

```markdown
## Job Analysis Report

### Position Overview
- **Job Title**: [extracted title]
- **Company**: [company name]
- **Location**: [location / remote status]
- **Seniority Level**: [Junior / Mid / Senior / Lead / Principal]
- **Employment Type**: [Full-time / Part-time / Contract]

### Hard Skills Required
<!-- Technical skills explicitly mentioned -->
- [Skill 1] — [Required / Preferred / Nice-to-have]
- [Skill 2] — [Required / Preferred / Nice-to-have]

### Soft Skills Required
<!-- Leadership, communication, etc. -->
- [Skill 1]
- [Skill 2]

### Key Responsibilities
<!-- Main duties of the role -->
- [Responsibility 1]
- [Responsibility 2]

### ATS Keywords
<!-- Critical keywords that MUST appear in the CV for ATS matching -->
- [keyword1]
- [keyword2]
- [keyword3]

### Experience Requirements
- **Minimum Years**: [N years]
- **Domain Experience**: [industry/domain if specified]
- **Education**: [degree requirements if any]

### Culture & Values Signals
<!-- What the company seems to value based on language used -->
- [Signal 1]
- [Signal 2]
```

### Step 3: Build the Keyword Bank
For each extracted keyword, build a variant map. This is critical for downstream matching and ATS optimization.

```markdown
### Keyword Bank (with variants)
| Primary Term | Acronym | Expansion | Regional Variant |
|---|---|---|---|
| CI/CD | CI/CD | Continuous Integration / Continuous Delivery | — |
| React | React | React.js, ReactJS | — |
| PM | PM | Project Management | — |
| optimisation | — | — | optimization (US) |
```

**Rules for the keyword bank:**
- Use the **exact form** the company uses as the primary term
- Include acronym + full expansion for every abbreviation
- Note regional spelling variants (UK vs US English)
- Include tool version specifics if mentioned (e.g., "Python 3.x", "Node 18+")

### Step 4: Detect Binary Filters (Dealbreakers)
Identify any hard filters that could cause automatic rejection regardless of CV quality:
- **Location requirements**: on-site, hybrid, specific city/country
- **Work authorization**: visa sponsorship mentioned/excluded
- **Availability**: immediate start, specific date
- **Killer questions**: certifications, clearances, licenses that are non-negotiable
- **Language requirements**: minimum level specified

```markdown
### Binary Filters Detected
| Filter | Value | Dealbreaker? |
|---|---|---|
| Location | Barcelona (Hybrid) | ⚠️ Check candidate location |
| Work Permit | EU citizens only | ⚠️ Verify eligibility |
| Language | English C1 minimum | Check CEFR level |
| Availability | Immediate | Check notice period |
```

### Step 5: Detect Market Context
Identify which market conventions apply (see `context/ats_rules.md` for full rules):
- **Language of posting** → CV should match this language
- **Market** (US/UK/EU/LATAM) → affects photo, personal data, format conventions
- **Industry** → regulated industries may have specific requirements

### Step 6: Save Output
Save the analysis report to `output/job_analysis.md` for consumption by downstream skills.

## Reference
Always consult `context/ats_rules.md` for the latest ATS formatting and keyword strategy rules.

## Output
A structured markdown file (`output/job_analysis.md`) containing all extracted requirements, the keyword bank with variants, binary filters, and market context.

## Tips for the Agent
- Pay special attention to **hidden requirements** — skills mentioned in the description body but not in a formal "requirements" list
- Distinguish between **required** vs **nice-to-have** skills
- Extract the **exact phrasing** used for keywords (e.g., "React.js" vs "React" vs "ReactJS" — use the version the company uses)
- Note any **deal-breakers** (e.g., "must have X certification", "must be located in Y")
- **Binary filters come FIRST** — if the candidate can't pass location/visa/language filters, flag it immediately before doing any CV work
- Look for **verb patterns** in responsibilities — these hint at the company's expectations (e.g., "own" = they want autonomy, "collaborate" = team-oriented)
- **Salary range** if mentioned — useful for candidate's decision-making
