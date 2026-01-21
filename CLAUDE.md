# CLAUDE.md – AI Development Rules for WAV ERP OS

This document defines **strict rules** for AI-assisted development in this repository.
All contributions (human or AI) must follow these guidelines to prevent hallucination, over-engineering, and architectural drift.

---

## 🎯 Purpose of This File

This file ensures that:
- AI assistants work **repo-first**, not assumption-first
- Features are implemented as **vertical slices** (backend → frontend)
- Business logic is **never invented** without explicit specification
- Code remains **minimal, clear, and traceable**

If something is unclear, **ask first**. Never assume or invent.

---

## 🚫 Forbidden Actions

**DO NOT:**
- Invent database schemas, entities, or fields
- Create user roles or permissions not explicitly defined
- Implement features not requested or documented
- Add "nice to have" functionality without explicit approval
- Over-engineer solutions (no premature abstractions)
- Skip backend implementation to "quickly build UI"
- Add libraries or dependencies without justification
- Create placeholder/mock data in production code
- Rename or restructure existing code without explicit request
- Add comments explaining obvious code (code should be self-documenting)
- Implement generic solutions when specific ones are requested

---

## ✅ Required Actions

**ALWAYS:**
- Read existing code before making changes
- Implement backend-first (Supabase schema, RLS, then frontend)
- Respect defined user roles and permissions
- Follow the project lifecycle states defined in documentation
- Use TypeScript strictly (no `any` types without justification)
- Test authentication and authorization for new features
- Maintain data traceability (who did what, when)
- Ask for clarification when requirements are ambiguous
- Implement the minimum viable solution first
- Use existing patterns and conventions from the codebase

---

## 🏗️ Architecture Rules

### Backend (Supabase)
- **Schema first**: all entities must be defined in Supabase migrations
- **RLS policies**: enforce permissions at database level, never trust frontend
- **Audit trails**: track creation, modification, and state changes
- **Foreign keys**: maintain referential integrity
- **Enums**: use PostgreSQL enums for constrained values (project states, roles, etc.)

### Frontend (Next.js)
- **App Router**: use Next.js 13+ app directory structure
- **Server Components**: default to server components, use client components only when needed
- **Type safety**: import and use types generated from Supabase schema
- **UI library**: use shadcn/ui components consistently
- **Permissions**: UI reflects backend permissions but never enforces them
- **Error handling**: handle errors gracefully with user-friendly messages

### Data Flow
1. User action (UI)
2. API route or Server Action
3. Supabase query with RLS enforced
4. Response to user with appropriate feedback

Never bypass this flow.

---

## 👥 User Roles & Permissions

Roles are defined in the backend and enforced via RLS.

Current roles (as per README):
- **Admin**: full system access
- **Finance**: view financial data, manage payments (cannot edit budgets)
- **Producer / Freelance**: manage assigned projects, collaborate on costs
- **External Providers**: no direct system access (managed by internal users)

**Never create new roles without explicit approval.**

---

## 🔄 Project Lifecycle States

Projects follow defined states (from README):
1. Requirement / Pitch
2. Proposal
3. Budget Closed
4. Sold
5. In Execution
6. Financial Closure
7. Archived

**Rules:**
- State transitions must be explicit and validated
- Each state may unlock/restrict capabilities
- Cannot skip states without business justification
- State history must be auditable

**Never add new states without specification.**

---

## 🧩 Core Entities (Inferred from README)

Based on the README, the system manages:
- **Projects**: central entity with states and lifecycle
- **Budgets**: linked to projects, with cost traceability
- **Providers**: people and companies (hybrid model)
- **Costs**: budget line items with provider associations
- **Payments**: financial transactions
- **Users**: system users with role-based access

**Schema details must be explicitly defined before implementation.**

---

## 📋 Development Workflow

### For New Features:
1. **Understand**: read requirements and existing code
2. **Ask**: clarify ambiguities before implementing
3. **Design**: plan backend schema and RLS policies
4. **Implement Backend**:
   - Create/modify Supabase migrations
   - Define RLS policies
   - Test with different user roles
5. **Implement Frontend**:
   - Create/modify UI components
   - Connect to backend via Supabase client
   - Handle loading and error states
6. **Test**: verify role-based access and data flow
7. **Document**: update relevant documentation if needed

### For Bug Fixes:
1. **Reproduce**: understand the issue
2. **Locate**: find the root cause
3. **Fix**: make minimal, targeted changes
4. **Verify**: ensure fix works and doesn't break other functionality

### For Refactoring:
1. **Justify**: explain why refactoring is needed
2. **Scope**: define clear boundaries
3. **Test**: ensure behavior remains unchanged
4. **Commit**: use clear commit messages

---

## 🎨 Code Style

### TypeScript
- Use strict mode
- Avoid `any` (use `unknown` if type is truly unknown)
- Define interfaces for data structures
- Use Supabase-generated types where possible

### React/Next.js
- Functional components only
- Use hooks appropriately
- Prefer server components over client components
- Keep components focused and single-purpose
- Use descriptive component and variable names

### Database
- Snake_case for table and column names
- Use meaningful foreign key names
- Add indexes for frequently queried columns
- Use transactions for multi-step operations

### File Organization
```
/app                    # Next.js app router
  /auth                # Authentication pages
  /dashboard           # Main application pages
  /api                 # API routes
/components            # React components
  /ui                  # shadcn/ui components
  /shared              # Shared components
/lib                   # Utility functions
  /supabase           # Supabase client and helpers
  /utils              # General utilities
/types                # TypeScript type definitions
/supabase             # Supabase configuration
  /migrations         # Database migrations
/public               # Static assets
```

---

## 🧪 Testing Guidelines

- Test authentication flows for each role
- Verify RLS policies prevent unauthorized access
- Test state transitions and business rules
- Validate form inputs and error handling
- Test with realistic data scenarios

---

## 📦 Dependencies

**Before adding a new dependency:**
1. Check if existing dependencies can solve the problem
2. Verify the package is actively maintained
3. Consider bundle size impact
4. Justify the addition

**Approved stack (from README):**
- Next.js (App Router)
- TypeScript
- Supabase
- shadcn/ui
- Tailwind CSS

---

## 🚀 Implementation Priorities

As stated in README:
1. Authentication & roles
2. Project management core
3. Budgeting engine
4. Provider database
5. Basic financial tracking

**Focus on MVP first. Polish later.**

---

## 💬 Communication Guidelines

When working on this codebase:
- Ask specific questions when unclear
- Explain technical decisions when they deviate from the norm
- Provide context for significant changes
- Use clear, descriptive commit messages
- Reference related issues or documentation

---

## 🎓 Guiding Principles

From the README:
- **Backend-first**: data and permissions define reality
- **Single source of truth** (Supabase)
- **Low cognitive load** over feature quantity
- **Automation over repetition**
- **Modular but integrated architecture**
- **Traceability over shortcuts**
- **MVP first, polish later**

> **Does this make the operation clearer, lighter, and more controllable?**

If the answer is no, don't implement it.

---

## 📚 Key Documentation

- `README.md` - Project overview and philosophy
- This file (`CLAUDE.md`) - Development rules
- Supabase migrations - Database schema source of truth
- Figma designs - UX flow reference (not business rules)

---

## ⚠️ Final Reminder

**This is not a generic ERP.**

It is built for event production and BTL agencies with specific workflows:
- Non-linear projects
- Variable costs
- Hybrid providers (people and companies)
- External financing
- Strict traceability requirements

**Never generalize or add features from generic ERP systems without explicit requirements.**

When in doubt, ask. Never assume.
