# FERVOai Agent Workflow Guide 🔥

This directory contains **comprehensive custom VS Code Copilot agents** for the FERVOai project. Each agent follows a **5-phase engineering workflow** (~400-700 lines each) for consistent, high-quality outputs.

## Available Agents

| Agent | File | Lines | Primary Tools | Description |
|-------|------|-------|---------------|-------------|
| **Planner 🗺️** | `planner.agent.md` | ~500 | Read + Todo | Strategic planning, task breakdown, agent coordination |
| **Backend Architect 🔥** | `backend-architect.agent.md` | ~600 | Edit + Terminal | Supabase, Edge Functions, PostgreSQL, API design |
| **Frontend Craftsman 🔥** | `frontend-craftsman.agent.md` | ~450 | Edit + Terminal | React, TailwindCSS, accessibility, FERVOai components |
| **DevOps Guardian 🔥** | `devops-guardian.agent.md` | ~600 | Edit + Terminal | CI/CD, GitHub Actions, Vercel, Docker, monitoring |
| **QA & Security Analyst 🔥** | `qa-security-analyst.agent.md` | ~700 | Edit + Terminal | Testing, OWASP Top 10, Vitest, Playwright |
| **FERVOai Brand Expert 🔥** | `fervoai-brand-expert.agent.md` | ~650 | Read-only | Brand voice, visual identity, UX writing |

## 5-Phase Engineering Workflow

Each agent includes comprehensive documentation following this structure:

1. **Phase 1: Design & Analysis** — Requirements gathering, context assessment, risk analysis
2. **Phase 2: Implementation** — Code patterns, templates, FERVOai-specific standards
3. **Phase 3: Verification & Testing** — Test commands, verification checklists, quality gates
4. **Phase 4: Documentation & Deliverables** — Templates, report formats, handoff procedures
5. **Phase 5: Bug Detection & Logging** — Bug report templates, common issues, prevention strategies

## Workflow Diagrams

### 🆕 New Feature Workflow

```
                    ┌──────────────────┐
                    │   Planner 🗺️    │
                    │  "Plan feature"  │
                    └────────┬─────────┘
                             │
              ┌──────────────┼──────────────┐
              ▼              ▼              ▼
     ┌────────────┐  ┌─────────────┐  ┌──────────────┐
     │  Backend   │  │  Frontend   │  │    DevOps    │
     │ Architect  │  │ Craftsman   │  │   Guardian   │
     │    🔥      │  │     🔥      │  │      🔥      │
     └─────┬──────┘  └──────┬──────┘  └──────┬───────┘
           │                │                │
           └────────────────┼────────────────┘
                            ▼
                   ┌────────────────┐
                   │  QA & Security │
                   │   Analyst 🔥   │
                   └────────┬───────┘
                            │
                            ▼
                   ┌────────────────┐
                   │ DevOps Guardian│
                   │   (Deploy) 🔥  │
                   └────────────────┘
```

### 🎨 Landing Page Workflow

```
┌─────────────────────────────────────────────────────────────┐
│ 1. PLAN                                                     │
│    Agent: Planner 🗺️                                       │
│    Prompt: "Plan a landing page for [product]"              │
│    Output: Implementation plan with sections, copy needs    │
└─────────────────────┬───────────────────────────────────────┘
                      │ [Handoff: Start Frontend]
                      ▼
┌─────────────────────────────────────────────────────────────┐
│ 2. BUILD                                                    │
│    Agent: Frontend Craftsman 🔥                             │
│    Prompt: "Build the landing page per the plan above"      │
│    Output: Working React/TailwindCSS code                   │
│    ✓ Uses FERVOai colors                                    │
│    ✓ Dark theme                                             │
│    ✓ Responsive                                             │
│    ✓ Placeholder copy (to be reviewed)                      │
└─────────────────────┬───────────────────────────────────────┘
                      │ [Handoff: Review Brand & Copy]
                      ▼
┌─────────────────────────────────────────────────────────────┐
│ 3. BRAND REVIEW                                             │
│    Agent: FERVOai Brand Expert 🔥                           │
│    Prompt: "Review for brand compliance"                    │
│    Output: Feedback report + rewritten copy                 │
│    🔍 Headlines: "Is it LOUD enough?"                       │
│    🔍 Body: "Does it sound corporate?"                      │
│    🔍 Colors: "Using the right palette?"                    │
└─────────────────────┬───────────────────────────────────────┘
                      │ [Handoff: Implement Fixes]
                      ▼
┌─────────────────────────────────────────────────────────────┐
│ 4. APPLY FIXES                                              │
│    Agent: Frontend Craftsman 🔥                             │
│    Prompt: "Apply brand expert's recommendations"           │
│    Output: Updated code with brand-compliant copy           │
└─────────────────────┬───────────────────────────────────────┘
                      │ [Handoff: Security & A11y Review]
                      ▼
┌─────────────────────────────────────────────────────────────┐
│ 5. QA REVIEW                                                │
│    Agent: QA & Security Analyst 🔥                          │
│    Prompt: "Test accessibility and security"                │
│    Output: Test results, any issues found                   │
└─────────────────────┬───────────────────────────────────────┘
                      │ [Handoff: All Clear - Deploy]
                      ▼
┌─────────────────────────────────────────────────────────────┐
│ 6. DEPLOY                                                   │
│    Agent: DevOps Guardian 🔥                                │
│    Prompt: "Deploy the landing page"                        │
│    Output: Deployment complete, live URL                    │
└─────────────────────────────────────────────────────────────┘
```

### 🔧 Backend Feature Workflow

```
┌─────────────────────────────────────────────────────────────┐
│ 1. PLAN                                                     │
│    Agent: Planner 🗺️                                       │
│    Prompt: "Plan a new AI service endpoint"                 │
└─────────────────────┬───────────────────────────────────────┘
                      │ [Handoff: Start Backend]
                      ▼
┌─────────────────────────────────────────────────────────────┐
│ 2. BUILD BACKEND                                            │
│    Agent: Backend Architect 🔥                              │
│    Tasks:                                                   │
│    - Create Edge Function                                   │
│    - Update database schema                                 │
│    - Configure credit costs                                 │
└─────────────────────┬───────────────────────────────────────┘
                      │ [Handoff: Security Review]
                      ▼
┌─────────────────────────────────────────────────────────────┐
│ 3. SECURITY AUDIT                                           │
│    Agent: QA & Security Analyst 🔥                          │
│    Checks:                                                  │
│    - Authentication required?                               │
│    - Input validation?                                      │
│    - SQL injection safe?                                    │
│    - Credit check before AI call?                           │
└─────────────────────┬───────────────────────────────────────┘
                      │ [Handoff: Fix Backend Issues] or [Deploy]
                      ▼
┌─────────────────────────────────────────────────────────────┐
│ 4. FRONTEND INTEGRATION                                     │
│    Agent: Frontend Craftsman 🔥                             │
│    Tasks:                                                   │
│    - Add API client method                                  │
│    - Create UI for new service                              │
│    - Handle loading/error states                            │
└─────────────────────┬───────────────────────────────────────┘
                      │ [Handoff: Deploy Changes]
                      ▼
┌─────────────────────────────────────────────────────────────┐
│ 5. DEPLOY                                                   │
│    Agent: DevOps Guardian 🔥                                │
│    Tasks:                                                   │
│    - Deploy Edge Function                                   │
│    - Run database migration                                 │
│    - Set environment variables                              │
└─────────────────────────────────────────────────────────────┘
```

## Quick Reference: When to Use Each Agent

| Task | Start With | Then |
|------|------------|------|
| New feature | Planner → | Backend/Frontend |
| Build UI component | Frontend Craftsman → | Brand Expert |
| Write copy/headlines | Brand Expert | → Frontend Craftsman |
| Create API endpoint | Backend Architect → | QA Analyst |
| Fix security issue | QA Analyst → | Backend/Frontend |
| Deploy to production | DevOps Guardian | — |
| Review for launch | QA Analyst → | DevOps Guardian |

## Handoff Best Practices

1. **Let the agent finish** before clicking handoff
2. **Context carries forward** — the next agent sees the conversation
3. **Be specific** in follow-up prompts
4. **Loop back** if fixes create new issues

## Agent Capabilities Summary

| Agent | Can Edit Code | Can Run Terminal | Primary Output |
|-------|--------------|------------------|----------------|
| Planner | ❌ | ❌ | Plans, roadmaps |
| Frontend Craftsman | ✅ | ✅ | React/CSS code |
| Backend Architect | ✅ | ✅ | Edge Functions, SQL |
| DevOps Guardian | ✅ | ✅ | CI/CD, deployment |
| QA & Security | ✅ | ✅ | Tests, reports |
| Brand Expert | ❌ | ❌ | Copy, feedback |

---

*Part of the FERVOai multiverse. Let's f***ing GO.* 🔥
