# CLAUDE.md - AI Development Rules for WAV ERP OS

> **Critical**: This file governs all AI-assisted development on WAV ERP OS.
> Read it fully before making any changes. These rules prevent hallucination, over-engineering, and architectural drift.

---

## 🎯 Project Context

**WAV ERP OS** is a modular ERP/SaaS operating system designed specifically for **event production and BTL agencies**.

### What This Is
- A specialized ERP for creative agencies with non-linear projects
- Single source of truth for projects, budgets, providers, and finances
- Backend-first system with strict data governance
- MVP-focused: clarity and control over feature quantity

### What This Is NOT
- Not a generic ERP or CRM
- Not a feature-complete product (yet)
- Not a playground for experimental patterns
- Not a place to showcase every possible abstraction

---

## 🧠 Core Development Principles

These principles override all other considerations:

1. **Backend-First**: Data model and permissions define reality. UI reflects them, never overrides.
2. **Single Source of Truth**: Supabase is the only source of truth. No redundant state.
3. **Low Cognitive Load**: Simplicity > cleverness. Explicit > implicit.
4. **Automation Over Repetition**: Reduce manual work, not add configuration options.
5. **Traceability**: Every change must be auditable. No shortcuts.
6. **MVP First**: Ship functional before beautiful. Iteration beats perfection.
7. **Repo-First**: Only implement what exists in specs/docs. Never invent.

**Guiding Question:**
> *"Does this make the operation clearer, lighter, and more controllable?"*
> If no → don't implement it.

---

## 🛠️ Tech Stack

### Backend
- **Supabase** (PostgreSQL + Auth + RLS + Storage)
- Database-first design with strong typing
- Row Level Security (RLS) for all access control
- Edge Functions for complex operations

### Frontend
- **Next.js 14+** (App Router only)
- **TypeScript** (strict mode)
- **React Server Components** where possible
- **shadcn/ui** + **Tailwind CSS** for UI
- Minimal client-side state

### Development
- **pnpm** for package management (when initialized)
- **ESLint** + **Prettier** for code quality
- **Claude Code** as implementation accelerator

---

## 📐 Architecture Rules

### Database & Backend

1. **Schema First**
   - All tables must be defined in Supabase first
   - Never assume entities exist - verify in database schema
   - Use proper foreign keys and constraints
   - Add created_at, updated_at to all tables

2. **Row Level Security (RLS)**
   - Every table must have RLS policies
   - Permissions are enforced at DB level, not app level
   - Roles: admin, finance, producer, freelance
   - UI visibility mirrors backend permissions

3. **Relationships**
   - Use proper relational design (normalized)
   - Avoid JSON blobs unless truly dynamic
   - Cascade deletes must be intentional
   - Use views for complex queries

### Frontend & UI

1. **File Organization**
   ```
   app/
   ├── (auth)/          # Auth-protected routes
   ├── (public)/        # Public routes
   ├── api/             # API routes
   └── layout.tsx       # Root layout

   components/
   ├── ui/              # shadcn components (don't edit)
   ├── features/        # Feature-specific components
   └── shared/          # Reusable business components

   lib/
   ├── supabase/        # Supabase client & types
   ├── utils/           # Helper functions
   └── hooks/           # Custom React hooks
   ```

2. **Component Rules**
   - Server Components by default
   - Client Components only when needed (use "use client")
   - One component per file
   - Props typed with TypeScript interfaces
   - No prop drilling > 2 levels (use context or composition)

3. **Data Fetching**
   - Server Components: fetch directly from Supabase
   - Client Components: use hooks or SWR/React Query
   - No waterfalls: parallel queries when possible
   - Cache strategically with Next.js cache controls

### Code Quality

1. **TypeScript**
   - Strict mode enabled
   - No `any` types (use `unknown` if needed)
   - Define interfaces for all data structures
   - Generate types from Supabase schema

2. **Naming Conventions**
   - Components: PascalCase (`ProjectCard.tsx`)
   - Functions: camelCase (`calculateBudget`)
   - Constants: UPPER_SNAKE_CASE (`MAX_FILE_SIZE`)
   - Files: kebab-case or PascalCase (be consistent)
   - Database: snake_case (`project_budgets`)

3. **Error Handling**
   - Always handle async errors
   - User-facing error messages (not stack traces)
   - Log errors for debugging
   - Graceful degradation where possible

---

## 🚫 What NOT to Do

### Never Invent
- ❌ Don't create entities or fields not in specs
- ❌ Don't add features "for future use"
- ❌ Don't assume business logic - ask or verify
- ❌ Don't create abstractions prematurely

### Never Over-Engineer
- ❌ Don't add configuration for single-use cases
- ❌ Don't create helpers/utils for one-time operations
- ❌ Don't add feature flags without explicit requirement
- ❌ Don't design for hypothetical future requirements

### Never Bypass Security
- ❌ Don't disable RLS policies
- ❌ Don't use service role client in frontend
- ❌ Don't trust client-side validation alone
- ❌ Don't expose sensitive data in API responses

### Never Compromise Traceability
- ❌ Don't delete data (soft delete instead)
- ❌ Don't modify records without audit trail
- ❌ Don't skip validation for "admin shortcuts"

---

## ✅ How to Implement Features

### 1. Understand First
- Read existing code before writing new code
- Verify database schema exists
- Check role permissions requirements
- Identify integration points

### 2. Vertical Slices
Implement features as complete vertical slices:
- Database schema/migrations
- Backend logic/API routes
- Frontend UI components
- Type definitions
- Basic validation

### 3. Implementation Order
1. **Database**: Create/verify tables, RLS policies
2. **Types**: Generate/update TypeScript types
3. **Backend**: API routes or server actions
4. **Frontend**: UI components
5. **Integration**: Connect frontend to backend
6. **Validation**: Add error handling

### 4. Quality Checks
- [ ] TypeScript compiles with no errors
- [ ] RLS policies tested
- [ ] Error states handled
- [ ] User feedback for async operations
- [ ] Responsive design (mobile-friendly)

---

## 🔐 User Roles & Permissions

### Role Hierarchy
1. **Admin**: Full access to everything
2. **Finance**: View all, manage payments (no budget editing)
3. **Producer**: Manage assigned projects, submit costs
4. **Freelance**: View assigned projects, submit time/costs

### Permission Enforcement
- Backend: RLS policies on all tables
- Frontend: Conditional rendering based on `auth.user.role`
- API: Verify role before operations
- Never trust client for authorization

---

## 📊 Project Lifecycle States

Projects flow through these states:

1. **Requirement / Pitch** - Initial concept
2. **Proposal** - Formal proposal stage
3. **Budget Closed** - Budget locked
4. **Sold** - Client approved
5. **In Execution** - Active production
6. **Financial Closure** - Reconciliation
7. **Archived** - Completed

**Rules:**
- State transitions must be explicit
- Some actions only available in certain states
- State changes are audited
- No backward transitions without admin approval

---

## 💰 Budget & Cost System

### Key Concepts
- **Budget Lines**: Individual line items in project budget
- **Actual Costs**: Real costs incurred during execution
- **Providers**: People or companies providing services
- **Cost History**: Reusable cost database from past projects

### Rules
- Budgets locked after "Budget Closed" state
- Actual costs always linked to budget lines
- Provider costs are tracked for future estimation
- All costs have proof of payment/invoice
- Commission calculation is automated

---

## 🧪 Testing Strategy

### Current Phase (MVP)
- Manual testing for critical paths
- TypeScript for type safety
- Database constraints for data integrity

### Future Phases
- Unit tests for business logic
- Integration tests for API routes
- E2E tests for critical workflows

**Don't write tests yet unless explicitly requested.**

---

## 📝 Commit & PR Guidelines

### Commit Messages
```
Format: <type>: <description>

Types:
- feat: New feature
- fix: Bug fix
- refactor: Code refactoring
- chore: Tooling/config changes
- docs: Documentation only

Examples:
- feat: add project budget creation form
- fix: correct RLS policy for finance role
- refactor: extract budget calculation to helper
```

### Pull Requests
- Clear description of what and why
- Reference any related issues
- Test the change locally first
- Small, focused PRs preferred

---

## 🤖 Working with Claude Code

### When Asking for Implementation
1. Provide context: which feature, which user role
2. Specify state/permission requirements
3. Indicate if this affects existing data
4. Mention if UI/UX design exists (Figma)

### What Claude Should Do
- ✅ Read existing code before writing
- ✅ Verify database schema exists
- ✅ Implement as vertical slice
- ✅ Follow existing patterns
- ✅ Ask when unclear
- ✅ Use TypeScript strictly
- ✅ Handle errors gracefully

### What Claude Should NOT Do
- ❌ Invent entities or business rules
- ❌ Add features not requested
- ❌ Refactor unrelated code
- ❌ Add premature abstractions
- ❌ Bypass security policies
- ❌ Write tests (unless requested)

---

## 📚 Key Files & Locations

### Configuration
- `CLAUDE.md` - This file (development rules)
- `README.md` - Project overview
- `.gitignore` - Git ignore rules
- `.env.local` - Environment variables (not in repo)

### When Initialized
- `package.json` - Dependencies
- `next.config.js` - Next.js configuration
- `tailwind.config.js` - Tailwind configuration
- `tsconfig.json` - TypeScript configuration

### Database
- Supabase dashboard for schema
- Migrations in project (when set up)
- Types generated from schema

---

## 🎓 Learning Resources

### Next.js App Router
- Server Components by default
- Client Components with "use client"
- Server Actions for mutations
- Route handlers for APIs

### Supabase
- Row Level Security (RLS)
- Real-time subscriptions
- Storage for files
- Edge Functions

### shadcn/ui
- Copy components, don't npm install
- Customize in `components/ui/`
- Built on Radix UI primitives

---

## 🚦 Decision Framework

When implementing anything, ask:

1. **Does it exist in specs/docs?**
   - No → Don't implement, ask first

2. **Does it add complexity?**
   - Yes → Is it worth the cognitive load?

3. **Does it break a principle?**
   - Yes → Don't do it

4. **Can it be simpler?**
   - Yes → Make it simpler

5. **Is it traceable/auditable?**
   - No → Add traceability

6. **Does it respect roles/permissions?**
   - No → Fix the security model

---

## ⚡ Quick Reference

### Starting a New Feature
1. Check if database schema exists
2. Verify role permissions needed
3. Read related existing code
4. Implement backend first
5. Then implement frontend
6. Test the vertical slice

### Modifying Existing Feature
1. Read current implementation fully
2. Understand current data flow
3. Check for dependent features
4. Make minimal changes needed
5. Verify no regressions

### When Stuck
1. Check database schema
2. Review role permissions
3. Look for similar patterns in codebase
4. Ask for clarification
5. Don't guess or invent

---

## 📞 Contact & Governance

This file is maintained by the project owner.

**For Claude Code:**
- Treat this as source of truth
- When in doubt, ask
- Never override these rules
- Suggest improvements via PR

**For Developers:**
- Read before contributing
- Follow or update these rules
- Consistency > personal preference

---

## 📌 Version

**Version:** 1.0
**Last Updated:** 2026-01-22
**Status:** Active - MVP Development Phase

---

**Remember:** This is not about limiting creativity—it's about channeling it into solving real problems for agency operations, not creating technical complexity for its own sake.

Code with **purpose, clarity, and control**.
