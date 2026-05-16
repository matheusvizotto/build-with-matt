---
name: saas-spec
description: Interview-driven SaaS specification generator. Asks targeted questions one at a time to understand the product, then produces a complete PRD + technical spec ready for development. Use when someone says "quero criar um SaaS", "me ajuda a especificar", "cria uma spec", or "vamos planejar o produto".
---

# SaaS Spec Generator

Interview the user to fully understand their SaaS idea, then produce a complete Product Requirements Document (PRD) + Technical Specification in one structured output.

---

## Phase 1 — Discovery Interview

Ask questions **one at a time**. Never batch multiple questions. Wait for each answer before moving to the next.

Start with:

> **Qual é a ideia do seu SaaS? Me conta em poucas palavras o que ele faz e para quem.**

Then cover these areas in order, adapting the question naturally to what the user already told you. Skip any area where the answer is already obvious from a previous response.

### Area 1 — Problem & Market
- What specific problem does it solve?
- Who has this problem? (role, industry, company size)
- How are they solving it today? What's broken about that?
- Is there a paying market? Any signal of willingness to pay?

### Area 2 — Product & Features
- What are the 3 core things the product must do at launch?
- What makes it different from existing tools?
- What is explicitly OUT of scope for v1?
- Is there a workflow or process this product replaces entirely?

### Area 3 — Users & Access
- Who are the user types? (e.g. admin, end user, viewer)
- Is it multi-tenant? Does one account have multiple users?
- Any specific onboarding or trial flow in mind?

### Area 4 — Business Model
- How will it charge? (subscription, per seat, usage-based, one-time)
- What pricing tiers, if any?
- What's the target MRR / revenue goal for year 1?

### Area 5 — Technical Preferences
- Any stack preference or constraint? (Next.js, Python, etc.)
- Where does it need to run? (Cloud: Vercel, AWS, self-hosted)
- Any integrations required at launch? (Stripe, Slack, CRM, etc.)
- Any specific auth requirements? (SSO, magic link, social login)
- Existing codebase to integrate with, or greenfield?

### Area 6 — Timeline & Team
- What's the target launch date or deadline?
- Who's building it? (solo, small team, agency)
- Any budget constraints on infrastructure?

---

## Phase 2 — Build the Spec

After all questions are answered (or the user says "pode gerar"), build the full document silently. Output everything in one structured markdown block.

### Document Structure

```markdown
# [Product Name] — Product & Technical Specification
**Version:** 1.0  
**Date:** YYYY-MM-DD  
**Status:** Draft

---

## 1. Executive Summary
[2-3 sentences: what it is, who it's for, what problem it solves]

---

## 2. Problem Statement
### The Problem
[Clear articulation of the pain point]

### Current Alternatives & Why They Fail
[How users solve this today and what's broken]

### Opportunity
[Why now, why this approach]

---

## 3. Target Users

| Persona | Role | Pain Point | Goal |
|---|---|---|---|
| [Name] | [Title/type] | [What frustrates them] | [What they want to achieve] |

---

## 4. Product Goals & Success Metrics

| Goal | Metric | Target | Timeline |
|---|---|---|---|
| [Business goal] | [KPI] | [Number] | [When] |

---

## 5. Core Features — MVP Scope

### In Scope (v1)

For each feature:

#### [Feature Name]
**What it does:** [1 sentence]  
**User story:** As a [user], I want to [action] so that [benefit].  
**Acceptance criteria:**
- [ ] [Testable criterion 1]
- [ ] [Testable criterion 2]

### Out of Scope (v1)
- [Feature explicitly deferred]
- [Feature explicitly deferred]

---

## 6. User Flows

### [Core Flow Name] (e.g. Onboarding)
1. User arrives at [entry point]
2. [Step]
3. [Step]
4. [End state]

[Repeat for each critical flow]

---

## 7. Technical Specification

### Stack
| Layer | Technology | Rationale |
|---|---|---|
| Frontend | [e.g. Next.js 15, TypeScript] | [why] |
| Backend | [e.g. Next.js API routes / FastAPI] | [why] |
| Database | [e.g. PostgreSQL via Supabase] | [why] |
| Auth | [e.g. NextAuth / Clerk] | [why] |
| Hosting | [e.g. Vercel] | [why] |
| Payments | [e.g. Stripe] | [why] |
| Email | [e.g. Resend] | [why] |

### Data Models

#### [Entity Name]
```
[EntityName] {
  id: uuid (PK)
  [field]: [type] — [description]
  created_at: timestamp
  updated_at: timestamp
}
```

[Repeat for each core entity]

### Key Relationships
- [Entity A] belongs to [Entity B] (via [field])
- [Entity A] has many [Entity C]

### API Endpoints (Core)

| Method | Endpoint | Auth | Description |
|---|---|---|---|
| POST | /api/auth/login | Public | User authentication |
| GET | /api/[resource] | Required | [Description] |
| POST | /api/[resource] | Required | [Description] |
| PUT | /api/[resource]/:id | Required | [Description] |
| DELETE | /api/[resource]/:id | Required + ownership | [Description] |

### Authentication & Authorization
- Auth method: [e.g. JWT / session-based]
- User roles: [list roles and permissions]
- Multi-tenancy: [how tenant isolation works]

### Infrastructure & Deployment
- Hosting: [platform + region]
- CI/CD: [e.g. GitHub Actions → Vercel]
- Environments: dev / staging / production
- Secrets management: [e.g. Vercel env vars]

### Integrations
| Service | Purpose | Priority |
|---|---|---|
| [Stripe] | [Payments] | [Must-have] |

---

## 8. Business Model

### Pricing
| Plan | Price | What's Included |
|---|---|---|
| [Free / Starter] | $0 | [features] |
| [Pro] | $X/mo | [features] |
| [Business] | $X/mo | [features] |

### Revenue Targets
- Month 3: [X MRR]
- Month 12: [X MRR]

---

## 9. Timeline & Milestones

| Milestone | What's Done | Target Date |
|---|---|---|
| MVP Build | [Core features live] | [Date] |
| Private Beta | [10 users testing] | [Date] |
| Public Launch | [Open to all] | [Date] |

---

## 10. Risks & Open Questions

### Risks
| Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|
| [e.g. Low adoption] | Medium | High | [Beta program, waitlist] |

### Open Questions
- [ ] [Decision not yet made]
- [ ] [Dependency not yet confirmed]

---

## 11. Development Checklist (Phase 1)

### Setup
- [ ] Repo created, CI/CD configured
- [ ] Environments: dev, staging, production
- [ ] Auth system implemented
- [ ] Database schema created and migrated

### Core Features
- [ ] [Feature 1] — backend + frontend
- [ ] [Feature 2] — backend + frontend

### Pre-Launch
- [ ] Payments integrated and tested
- [ ] Error monitoring configured (e.g. Sentry)
- [ ] Basic analytics set up
- [ ] Landing page live
- [ ] Onboarding flow tested end-to-end
```

---

## Rules

- One question at a time — never batch
- Adapt tone to what the user gives you (brief answer = brief follow-up, detailed = go deeper)
- If the user skips a question, use a sensible default and note it in the spec as an assumption
- After generating the spec, ask: "Quer que eu salve isso como arquivo ou ajuste alguma seção?"
- If they want to save: write to `./[product-name]-spec.md` in the current directory
