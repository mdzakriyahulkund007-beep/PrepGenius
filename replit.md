# TalentAI — AI-Driven ATS Resume Intelligence & Mock Interview System

## Overview

A full-stack web application that analyzes resumes for ATS compatibility, matches them against job descriptions, and runs AI-powered mock interviews with real-time evaluation.

## Stack

- **Monorepo tool**: pnpm workspaces
- **Node.js version**: 24
- **Package manager**: pnpm
- **TypeScript version**: 5.9
- **API framework**: Express 5
- **Database**: PostgreSQL + Drizzle ORM
- **Validation**: Zod (`zod/v4`), `drizzle-zod`
- **API codegen**: Orval (from OpenAPI spec)
- **Build**: esbuild (ESM bundle)
- **Auth**: Replit Auth (OpenID Connect + PKCE)
- **Frontend**: React + Vite + Tailwind CSS + Shadcn/ui + Recharts

## Structure

```text
artifacts-monorepo/
├── artifacts/
│   ├── api-server/         # Express API server (auth, resume, interview, dashboard)
│   └── ats-app/            # React + Vite frontend (served at /)
├── lib/                    # Shared libraries
│   ├── api-spec/           # OpenAPI spec + Orval codegen config
│   ├── api-client-react/   # Generated React Query hooks
│   ├── api-zod/            # Generated Zod schemas from OpenAPI
│   ├── replit-auth-web/    # Replit Auth browser hook (useAuth)
│   └── db/                 # Drizzle ORM schema + DB connection
├── scripts/                # Utility scripts
├── pnpm-workspace.yaml
├── tsconfig.base.json
├── tsconfig.json
└── package.json
```

## Features

### Resume Processing
- PDF and DOCX upload via drag-and-drop
- Text extraction and NLP-based skill detection
- Technical skills (Python, React, SQL, etc.) and soft skills extraction
- Resume section detection (Education, Experience, Skills, Projects, etc.)
- Formatting analysis and feedback

### ATS Evaluation
- Overall ATS score (0-100%)
- Keyword matching score
- Formatting score
- Section presence checklist
- Optimization suggestions and missing keywords

### Job Matching
- Paste custom job description or select from 8 presets (SWE Intern, Data Analyst, etc.)
- Semantic + keyword-based match score
- Skill gap analysis (present vs missing skills)
- Top role recommendations

### Mock Interviews
- Text-based AI interview with adaptive questioning
- Video interview with webcam + Web Speech API
- 7 questions per session (behavioral, technical, situational, role-specific)
- Real-time answer evaluation with score, strengths, improvements
- Final report generation

### Dashboard & Reporting
- Stats overview (resumes uploaded, avg ATS score, interviews, avg interview score)
- Recent activity tables
- Skills radar chart (Recharts)
- Interview history and PDF report download

### Authentication
- Replit Auth (OIDC) — sign in with your Replit account
- Persistent sessions stored in PostgreSQL
- User profile in top bar

## Database Tables

- `users` — authenticated user profiles (Replit Auth)
- `sessions` — session storage (Replit Auth)
- `resumes` — uploaded resume metadata + analysis results
- `job_matches` — job match results per resume
- `interview_sessions` — interview sessions with Q&A and scores

## API Routes

All routes under `/api`:
- `GET /api/auth/user` — get current auth state
- `GET /api/login` — initiate OIDC login
- `GET /api/logout` — logout
- `GET /api/resumes` — list user resumes
- `POST /api/resumes` — upload + analyze resume (multipart/form-data)
- `GET /api/resumes/:id` — get resume analysis
- `DELETE /api/resumes/:id` — delete resume
- `GET /api/resumes/:id/ats-score` — get ATS evaluation
- `POST /api/resumes/:id/job-match` — match against job description
- `GET /api/job-presets` — list job description presets
- `GET /api/interviews` — list interview sessions
- `POST /api/interviews` — start new interview
- `GET /api/interviews/:id` — get interview session
- `POST /api/interviews/:id/answer` — submit answer + get next question
- `POST /api/interviews/:id/end` — end interview + generate report
- `GET /api/dashboard` — get dashboard data

## Dev Commands

```bash
# Run frontend
pnpm --filter @workspace/ats-app run dev

# Run API server
pnpm --filter @workspace/api-server run dev

# Push DB schema changes
pnpm --filter @workspace/db run push

# Regenerate API client + Zod schemas after spec changes
pnpm --filter @workspace/api-spec run codegen
```
