# 🧠 Agente CV

> An agent-based system for generating and optimizing CVs tailored to specific job positions. It analyzes job descriptions, matches them with your experience, and rewrites your resume to maximize alignment and ATS performance. The system combines profile extraction, job matching, and iterative refinement to produce high-quality, role-specific CVs.

## Architecture

This project follows a **KISS (Keep It Simple)** philosophy with a modular, filesystem-driven design inspired by agentic skill ecosystems.

```
Agente-CV/
├── context/
│   └── candidate/          # 🧠 The "Brain" — your persistent knowledge base
│       ├── profile.md       # Personal info, summary, preferences
│       ├── experiences.md   # Work history with metrics and achievements
│       ├── skills.md        # Technical and soft skills inventory
│       └── life_stories.md  # Impact stories (STAR method), awards, highlights
│
├── skills/                  # 🛠️ Modular agent skills
│   ├── job-analyzer/        # Parses job descriptions, extracts requirements
│   ├── profile-matcher/     # Matches candidate context to job requirements
│   ├── resume-tailor/       # Rewrites and optimizes resume content
│   └── ats-validator/       # Validates keyword coverage and ATS compliance
│
├── .agents/
│   └── workflows/           # 📋 Step-by-step orchestration flows
│       ├── onboarding.md    # Initial candidate profile setup
│       └── apply.md         # End-to-end job application flow
│
├── output/                  # 📄 Generated CVs and reports
└── templates/               # 📐 LaTeX/formatting templates
```

## How It Works

### 1. Onboarding (One-time setup)
Feed your professional history into the `context/candidate/` directory. The system stores everything in plain Markdown — portable, readable, and version-controllable.

### 2. Apply to a Job
Provide a job description (URL or text). The system:
1. **Analyzes** the job requirements (hard skills, soft skills, keywords)
2. **Matches** your context to find the most relevant experiences and stories
3. **Tailors** your resume content with optimized bullet points
4. **Validates** ATS keyword coverage before final output

### 3. Output
A polished, ATS-optimized CV ready to submit.

## Tech Stack
- **Context Storage**: Plain Markdown (Obsidian-compatible)
- **Agent Skills**: Python scripts, modular and composable
- **Orchestration**: Workflow definitions in Markdown (agent-driven)
- **Validation**: Pydantic models for structured output verification

## Getting Started

```bash
# 1. Clone the repository
git clone https://github.com/yourusername/Agente-CV.git

# 2. Populate your context
# Edit the files in context/candidate/ with your information

# 3. Run the apply workflow
# Provide a job description and let the agent do the rest
```

## License
MIT
