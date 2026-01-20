# CLAUDE.MD — Development Rules for WAV ERP OS

This document governs AI-assisted development for the **WAV ERP OS** project.
All contributors (human or AI) must follow these rules strictly.

---

## 🎯 Project Context

**WAV ERP OS** is a modular ERP/SaaS system for event production and BTL agencies.
It prioritizes clarity, control, and scalability without sacrificing creative flexibility.

### Core Purpose
- Single source of truth for projects, budgets, providers, and finances
- Reduce cognitive load through automation and clear structure
- Handle non-linear workflows, variable costs, hybrid providers, and strict traceability

### Current Phase
**MVP in active development**

Focus areas:
- Authentication & role-based access control
- Project management core (lifecycle states)
- Budgeting engine with cost traceability
- Provider database
- Basic financial tracking

---

## 🧠 Guiding Principles

Every implementation decision must align with these principles:

1. **Backend-first**: Data and permissions define reality
2. **Single source of truth**: Supabase is the authority
3. **Low cognitive load** over feature quantity
4. **Automation over repetition**
5. **Modular but integrated** architecture
6. **Traceability over shortcuts**
7. **MVP first, polish later**

**Golden Rule:**
> If a feature increases complexity without reducing friction, it is not implemented.

---

## 🛠️ Tech Stack

### Backend
- **Supabase**
  - PostgreSQL database
  - Row-level security (RLS) for all permissions
  - Authentication with role-based access
  - Storage for documents/files
  - Audit trails via triggers

### Frontend
- **Next.js (App Router)** — React framework
- **TypeScript** — strict mode enabled
- **shadcn/ui** — component library
- **Tailwind CSS** — styling
- **Zod** — schema validation
- **React Hook Form** — form handling

### Development
- **ESLint + Prettier** — code quality
- **Husky** — pre-commit hooks
- **AI-assisted coding** — Claude Code

---

## 📐 Architecture Rules

### 1. Repository-First Development
- **Read before writing**: Always examine existing code before making changes
- **No hallucination**: Never invent entities, tables, or logic not defined in the repository
- **Respect structure**: Follow existing patterns for files, folders, and naming

### 2. Backend Authority
- **RLS is mandatory**: All data access must go through Supabase RLS policies
- **Never bypass permissions** in the frontend
- **Frontend reflects backend state**: UI adapts to what the user is allowed to see/do
- **No business logic in the frontend**: Validation and rules live in the database
- **Business rules and permissions live in the database**: UI-level validation is allowed for UX, but never trusted for security

### 3. Vertical Slice Implementation
When implementing features:
- Start with database schema (tables, RLS policies, functions)
- Add API layer (server actions or API routes)
- Build minimal UI that consumes the API
- Test the full flow before adding polish

**Do not:**
- Build UI before backend is ready
- Create components without a clear use case
- Add abstraction layers prematurely

### 4. Role-Based Access Control
User roles (enforced via RLS):
- **Admin**: Full access and control
- **Finance**: Payment and financial visibility (no budget editing)
- **Producer/Freelance**: Project execution and cost collaboration
- **External Providers**: No system access (managed internally)

**Every database query must respect user roles.**

### 5. Project Lifecycle States
Projects move through explicit states:
1. Requirement / Pitch
2. Proposal
3. Budget Closed
4. Sold
5. In Execution
6. Financial Closure
7. Archived

**State transitions must be validated** in the database via triggers or functions.
**UI must adapt** to the current project state.

---

## 🚫 Strict Prohibitions

### Never Do This
- ❌ Create tables or columns not explicitly requested
- ❌ Implement features "because they might be useful later"
- ❌ Add authentication bypass or admin backdoors
- ❌ Use `any` type in TypeScript without justification
- ❌ Store sensitive data in localStorage or cookies
- ❌ Implement client-side-only validation for security-critical data
- ❌ Create generic abstractions for single-use cases
- ❌ Add comments explaining obvious code (code should be self-explanatory)
  - ✅ Exception: Complex RLS policies, triggers, or business rules must be commented
- ❌ Copy-paste code without understanding it
- ❌ Commit commented-out code or console.logs to main branches

### Required Practices
- ✅ Use TypeScript strict mode
- ✅ Validate all inputs with Zod schemas
- ✅ Write RLS policies for every table
- ✅ Use server actions for mutations
- ✅ Follow existing naming conventions
- ✅ Test RLS policies before implementing UI
- ✅ Handle loading and error states in UI
- ✅ Use proper HTTP status codes and error messages
- ✅ Keep components focused and single-purpose

---

## 📁 Project Structure

```
/app                    → Next.js App Router pages
  /(auth)              → Authentication routes
  /(dashboard)         → Main app routes (protected)
  /api                 → API routes (if needed)
/components            → React components
  /ui                  → shadcn/ui components
  /forms               → Form components
  /layouts             → Layout components
/lib                   → Utility functions and shared logic
  /supabase            → Supabase client and helpers
  /validations         → Zod schemas
/types                 → TypeScript type definitions
/hooks                 → Custom React hooks
/public                → Static assets
/supabase              → Supabase migrations and seed data
  /migrations          → Database migrations
  /seed                → Seed data
```

### File Naming Conventions
- **Components**: PascalCase (e.g., `ProjectCard.tsx`)
- **Utilities**: camelCase (e.g., `formatCurrency.ts`)
- **Types**: PascalCase (e.g., `Project.ts`)
- **Hooks**: camelCase with `use` prefix (e.g., `useProject.ts`)
- **Server actions**: camelCase (e.g., `createProject.ts`)

---

## 🔒 Security Requirements

### Authentication
- Use Supabase Auth exclusively
- Store JWT in httpOnly cookies via Supabase middleware
- Validate sessions on every protected route
- Implement proper sign-out flow

### Authorization
- RLS policies must cover all CRUD operations
- Test policies with different user roles
- Never trust client-side role checks
- Log authorization failures for audit

### Data Validation
- Validate all inputs at the API boundary with Zod
- Sanitize user inputs to prevent XSS
- Use parameterized queries (Supabase handles this)
- Never interpolate user data into SQL strings

### Sensitive Data
- Store secrets in environment variables
- Never commit `.env` files
- Use Supabase vault for sensitive configurations
- Encrypt PII if required by regulation

---

## 🧪 Testing Strategy

### MVP Phase (Current)
- **Manual testing** of critical paths
- **RLS policy verification** via Supabase SQL editor
- **Type safety** via TypeScript compilation

### Future Phases
- Unit tests for utilities and validation logic
- Integration tests for server actions
- E2E tests for critical user flows

**Do not spend time on tests during MVP unless explicitly requested.**

---

## 💬 Communication Style

When implementing features:
1. **Confirm understanding** of the request
2. **Ask clarifying questions** before starting
3. **Explain your approach** briefly
4. **Show what you're doing** as you work
5. **Report completion** with next steps

### Asking Questions
Prefer specific questions over assumptions:
- ✅ "Should project states be editable by Producers, or Admin-only?"
- ❌ "I'll assume Producers can edit states."

### Reporting Work
- Show file changes with context
- Mention side effects (new dependencies, migrations, etc.)
- Highlight anything that needs manual testing
- Suggest next logical steps

---

## 🚀 Implementation Workflow

### When Starting New Work
1. Read existing code in the affected area
2. Check if database schema needs changes
3. Verify user role requirements
4. Ask clarifying questions if anything is unclear
5. Create database changes first (migrations)
6. Implement backend logic (RLS, functions, actions)
7. Build minimal UI
8. Test the full vertical slice

### When Modifying Existing Code
1. Read the current implementation fully
2. Understand why it was built that way
3. Identify side effects of your changes
4. Update related code (types, validations, etc.)
5. Maintain consistency with existing patterns

### When Stuck
- Don't guess or implement workarounds
- Ask for clarification or guidance
- Suggest alternatives with trade-offs
- Explain what you've tried and why it didn't work

---

## 📋 Code Quality Checklist

Before considering any work "done":

- [ ] TypeScript compiles with no errors
- [ ] No `any` types without justification
- [ ] All inputs validated with Zod schemas
- [ ] RLS policies tested for target user roles
- [ ] UI handles loading and error states
- [ ] No hardcoded values that should be configurable
- [ ] Consistent naming with rest of codebase
- [ ] No commented-out code or debug logs
- [ ] Dependencies added to `package.json` if needed
- [ ] Environment variables documented if added

---

## 🎨 UI/UX Guidelines

### Design Philosophy
- **Functional over beautiful** in MVP phase
- Clear labels and intuitive flows
- Minimal cognitive load (don't make users think)
- Consistent spacing and typography (use Tailwind utilities)

### Component Standards
- Use shadcn/ui components as base
- Compose complex UIs from simple components
- Keep components under 200 lines when possible
- Extract logic to hooks or utilities

### Forms
- Use React Hook Form + Zod
- Show validation errors inline
- Disable submit during loading
- Provide success/error feedback
- Clear form after successful submission

### Tables & Lists
- Show loading skeletons
- Handle empty states
- Implement pagination if dataset can grow large
- Make actions obvious (buttons, not just icons)

---

## 🎨 Figma Contract

Figma designs serve as UX intent and visual guidance, not technical specification.

### What Figma Defines
- Screen flows and navigation patterns
- Visual design and component layout
- User interaction patterns
- Content hierarchy and information architecture

### What Figma Does NOT Define
- **Permissions**: Determined by RLS policies in the database
- **Business rules**: Enforced by database constraints, triggers, and functions
- **State transitions**: Validated by backend logic
- **Data validation**: Handled by Zod schemas and database constraints
- **Security controls**: Implemented via Supabase RLS

### Conflict Resolution
**If a Figma flow conflicts with backend rules, backend wins.**

Examples:
- Figma shows "Edit Budget" button → Backend RLS denies Finance role → Button is hidden/disabled in UI
- Figma shows linear workflow → Backend allows non-linear state transitions → UI adapts to backend reality
- Figma shows all project data → User has limited permissions → UI displays only permitted data

### Implementation Guidelines
1. Start with backend (schema, RLS, business rules)
2. Reference Figma for UX patterns and visual design
3. Adapt UI to reflect actual user permissions and data access
4. Never implement Figma flows that bypass backend security

**Remember:** Figma is a design tool, not a security specification. Always verify permissions and business rules against the database schema and RLS policies.

---

## 🗄️ Database Guidelines

### Schema Design
- Use clear, descriptive table and column names
- Add foreign keys with proper constraints
- Use enums for fixed value sets
- Add indexes for frequently queried columns
- Include `created_at` and `updated_at` timestamps

### Migrations
- Create migrations for all schema changes
- Write reversible migrations when possible
- Test migrations on local database first
- Never modify existing migrations (create new ones)

### RLS Policies
- Name policies clearly: `{table}_{action}_{role}`
- Document complex policy logic with comments (required for non-obvious business rules)
- Test with different user roles
- Prefer simple policies over clever ones

### Triggers & Functions
- Use PostgreSQL functions for complex business logic
- Create triggers for audit trails
- Keep functions focused and single-purpose
- Document parameters and return types with SQL comments
- Add inline comments for complex business logic

---

## 📦 Dependencies

### Adding New Dependencies
Before adding a new package:
1. Check if existing dependencies can solve the problem
2. Verify package is actively maintained
3. Check bundle size impact
4. Prefer libraries that are:
   - Well-documented
   - TypeScript-native
   - Widely used in Next.js ecosystem

### Approved Libraries
- **UI**: shadcn/ui, Radix UI, Lucide icons
- **Forms**: React Hook Form, Zod
- **Data fetching**: Supabase client
- **Date handling**: date-fns
- **Utilities**: clsx, tailwind-merge

**Ask before adding packages outside this list.**

---

## 🐛 Error Handling

### Backend
- Return structured errors with proper status codes
- Log errors for debugging (but don't expose internals to client)
- Use meaningful error messages
- Handle edge cases explicitly

### Frontend
- Show user-friendly error messages
- Provide recovery actions when possible
- Log errors to console in development
- Use error boundaries for component crashes

### Common Patterns
```typescript
// Server action error handling
try {
  const result = await someOperation()
  return { success: true, data: result }
} catch (error) {
  console.error('Operation failed:', error)
  return { success: false, error: 'User-friendly message' }
}

// Frontend error handling
if (!result.success) {
  toast.error(result.error)
  return
}
```

---

## 🔄 Git Workflow

### Commits
- Write clear, descriptive commit messages
- Use conventional commits format: `type(scope): message`
  - `feat`: New feature
  - `fix`: Bug fix
  - `refactor`: Code restructuring
  - `docs`: Documentation changes
  - `chore`: Maintenance tasks

### Branches
- Work on feature branches: `feature/description`
- Keep branches short-lived
- Merge to main via pull requests
- Delete branches after merging

### Pull Requests
- Describe what changed and why
- List any breaking changes
- Mention related issues
- Request review if working in a team

---

## ✅ Definition of Done

A feature is complete when:
1. Database schema is finalized and migrated
2. RLS policies are in place and tested
3. Backend logic (server actions) is implemented
4. UI is functional and handles all states
5. TypeScript compiles with no errors
6. Manual testing passes for happy path and error cases
7. Code follows project conventions
8. No debug code or commented-out sections remain

**Do not:**
- Consider work done if only UI is built
- Skip error handling "for now"
- Leave TODO comments without tracking them
- Merge code that doesn't compile

---

## 🎓 Learning Resources

If you need to understand something better:

- **Next.js App Router**: https://nextjs.org/docs
- **Supabase**: https://supabase.com/docs
- **RLS Policies**: https://supabase.com/docs/guides/auth/row-level-security
- **shadcn/ui**: https://ui.shadcn.com
- **Zod**: https://zod.dev
- **React Hook Form**: https://react-hook-form.com

---

## 🚨 When in Doubt

**Always:**
- Ask clarifying questions
- Read existing code first
- Follow established patterns
- Prioritize simplicity
- Respect the backend-first principle

**Never:**
- Guess at requirements
- Invent features
- Override permissions
- Ignore errors
- Rush to "just make it work"

---

## 📞 Final Notes

This is a **real operating system** for **real agencies** with **real financial consequences**.

- Mistakes in permissions could expose confidential data
- Bugs in budgeting could cause financial errors
- Poor UX increases cognitive load on already busy teams

**Take your time. Ask questions. Build it right.**

If this file conflicts with user instructions, ask for clarification before proceeding.

---

**Last Updated:** 2026-01-20
**Version:** 1.0.0
