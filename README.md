# WAV ERP OS

**WAV ERP OS** is a modular ERP / SaaS operating system designed specifically for **event production and BTL agencies**.  
It centralizes projects, budgets, providers, and financial operations while reducing cognitive load through clear structure and automation.

This is **not a generic ERP**.  
It is built around real-world agency workflows: non-linear projects, variable costs, hybrid providers (people and companies), external financing, and strict traceability requirements.

---

## 🎯 Purpose

WAV ERP OS exists to solve operational chaos in creative agencies by providing:

- A **single source of truth** for all projects
- Clear project states and transitions
- Structured budgeting with full cost traceability
- Provider history and cost reuse
- Financial visibility (payments, margins, commissions)
- Automation that reduces manual work and mental overhead

The system prioritizes **clarity, control, and scalability** without sacrificing creative flexibility.

---

## 🧠 Core Principles

- **Backend-first**: data and permissions define reality
- **Single source of truth** (Supabase)
- **Low cognitive load** over feature quantity
- **Automation over repetition**
- **Modular but integrated architecture**
- **Traceability over shortcuts**
- **MVP first, polish later**

If a feature increases complexity without reducing friction, it is not implemented.

---

## 🧩 Architecture Overview

- **Backend**: Supabase  
  - Authentication  
  - Role-based access control (RLS)  
  - Relational database  
  - Storage and auditability  

- **Frontend**: Next.js + TypeScript  
  - Minimal, functional UI first  
  - Role-aware navigation and guards  
  - UI reflects backend permissions (never overrides them)

- **Design & Flow**: Figma  
  - Defines screen flows and UX intent  
  - Does not define business rules

- **AI-Assisted Development**: Claude Code  
  - Used as an implementation accelerator  
  - Governed by strict repository rules (`CLAUDE.md`)

---

## 👥 User Roles (High Level)

- **Admin**: full access and control
- **Finance**: payments, financial visibility (no budget editing)
- **Producer / Freelance**: project execution and cost collaboration
- **External Providers**: no system access (managed internally)

Visibility and permissions are enforced **at the backend level**.

---

## 🔄 Project Lifecycle

Projects move through explicit states, such as:

1. Requirement / Pitch  
2. Proposal  
3. Budget Closed  
4. Sold  
5. In Execution  
6. Financial Closure  
7. Archived  

Each state unlocks or restricts system capabilities.

---

## 🚧 Project Status

**Status:** MVP in active development  

Current focus:
- Authentication & roles
- Project management core
- Budgeting engine
- Provider database
- Basic financial tracking

UI refinement, advanced automation, reporting, and AI features will follow in later phases.

---

## 🛠️ Tech Stack

- **Next.js (App Router)**
- **TypeScript**
- **Supabase**
- **shadcn/ui + Tailwind**
- **AI-assisted coding with Claude Code**

---

## 📁 Repository Rules

This repository is governed by strict implementation rules to prevent hallucination, over-engineering, and architectural drift.

See:
- `CLAUDE.md` → AI and development rules
- Internal documentation for business logic and architecture

Claude (or any contributor) must:
- Work repo-first
- Respect defined roles and flows
- Avoid inventing logic or entities
- Implement features as vertical slices

---

## 🧭 Guiding Question

> **Does this make the operation clearer, lighter, and more controllable?**

If the answer is no, it does not belong in WAV ERP OS.

---

## 📌 License

License to be defined.  
All rights reserved until explicitly stated otherwise.

---

## ✉️ Contact

This project is developed as a private operating system for agency workflows.  
For collaboration or inquiries, contact the repository owner.
