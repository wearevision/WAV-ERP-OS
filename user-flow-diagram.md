# WAV ERP OS - User Flow Diagram
**Version:** 1.0.0
**Date:** 2026-01-20
**Purpose:** Navigation and logic flow (NOT UI design)

---

## 🔐 Authentication Flow

### 1. Login Screen
- **Role:** Unauthenticated
- **Project State:** N/A
- **Allowed Actions:**
  - Enter email and password
  - Submit login
  - Request password reset
  - Navigate to sign up (if enabled)
- **Next Screens:**
  - Dashboard (based on user role after successful auth)
  - Password Reset Screen
  - Sign Up Screen

### 2. Password Reset Screen
- **Role:** Unauthenticated
- **Project State:** N/A
- **Allowed Actions:**
  - Enter email
  - Request reset link
  - Return to login
- **Next Screens:**
  - Login Screen
  - Email Sent Confirmation

### 3. Sign Up Screen (Optional - MVP may skip)
- **Role:** Unauthenticated
- **Project State:** N/A
- **Allowed Actions:**
  - Enter user details
  - Submit registration
  - Return to login
- **Next Screens:**
  - Email Verification Screen
  - Login Screen

---

## 🏠 Main Navigation

### 4. Dashboard
- **Role:** Admin, Finance, Producer/Freelance
- **Project State:** N/A
- **Allowed Actions:**
  - View role-specific metrics summary
  - Access navigation menu
  - View recent activity
  - Quick access to active projects
- **Next Screens:**
  - Projects List
  - Providers List (Admin, Producer)
  - Financial Overview (Admin, Finance)
  - User Profile
  - Notifications Center

---

## 📊 Projects Module

### 5. Projects List
- **Role:** Admin, Finance, Producer/Freelance
- **Project State:** All states (filtered by permissions)
- **Allowed Actions:**
  - View projects table (filtered by role permissions)
  - Search/filter projects
  - Sort by columns (name, state, date, budget, client)
  - Create new project (Admin, Producer only)
  - View project details (click row)
  - Export projects list (Admin, Finance)
- **Next Screens:**
  - Project Detail View
  - Create Project Screen (Admin, Producer)
  - Dashboard

### 6. Create Project Screen
- **Role:** Admin, Producer
- **Project State:** N/A (creating new)
- **Allowed Actions:**
  - Enter project basic info (name, client, description)
  - Set initial state (Requirement/Pitch)
  - Assign team members
  - Set timeline
  - Save as draft
  - Submit project
  - Cancel and return
- **Next Screens:**
  - Project Detail View (after creation)
  - Projects List (cancel)

### 7. Project Detail View
- **Role:** Admin, Finance, Producer/Freelance
- **Project State:** Any (view adapts based on state)
- **Allowed Actions:**
  - View project information
  - View current state
  - Navigate to tabs:
    - Overview
    - Budget
    - Timeline
    - Team
    - Documents
    - Financial
    - Activity Log
  - Edit project info (Admin, assigned Producer)
  - Change project state (role + state dependent)
  - Archive project (Admin only)
- **Next Screens:**
  - Project Overview Tab
  - Project Budget Tab
  - Project Timeline Tab
  - Project Team Tab
  - Project Documents Tab
  - Project Financial Tab
  - Project Activity Log Tab
  - Edit Project Screen
  - Projects List

---

## 📊 Project Detail Tabs

### 8. Project Overview Tab
- **Role:** Admin, Finance, Producer/Freelance
- **Project State:** Any
- **Allowed Actions:**
  - View project summary
  - View key metrics
  - View client information
  - View project status
  - Edit basic info (if permitted by role)
- **Next Screens:**
  - Edit Project Screen (if permitted)
  - Other project tabs

### 9. Project Budget Tab
- **Role:** Admin, Finance (view only), Producer (edit based on state)
- **Project State:** Any (edit permissions vary by state)
- **Allowed Actions:**
  - View budget breakdown
  - View cost categories
  - View line items
  - Add/edit line items (Admin, Producer - if state allows)
  - Link providers to costs
  - View budget vs. actual
  - Export budget
  - Lock budget (Admin only, moves to "Budget Closed" state)
- **Next Screens:**
  - Add Budget Line Item Modal
  - Edit Budget Line Item Modal
  - Link Provider Modal
  - Budget Export Screen

### 10. Project Timeline Tab
- **Role:** Admin, Producer/Freelance
- **Project State:** Proposal onwards
- **Allowed Actions:**
  - View project timeline
  - View milestones
  - Add milestone (Admin, assigned Producer)
  - Edit milestone (Admin, assigned Producer)
  - Mark milestone complete
  - View dependencies
- **Next Screens:**
  - Add Milestone Modal
  - Edit Milestone Modal

### 11. Project Team Tab
- **Role:** Admin, Producer (assigned to project)
- **Project State:** Any
- **Allowed Actions:**
  - View team members
  - View roles and responsibilities
  - Add team member (Admin only)
  - Remove team member (Admin only)
  - Change team member role (Admin only)
- **Next Screens:**
  - Add Team Member Modal
  - Edit Team Member Modal

### 12. Project Documents Tab
- **Role:** Admin, Finance, Producer/Freelance
- **Project State:** Any
- **Allowed Actions:**
  - View uploaded documents
  - Upload document (Admin, assigned Producer)
  - Download document
  - Delete document (Admin, document uploader)
  - Filter by document type
- **Next Screens:**
  - Upload Document Modal
  - Document Preview

### 13. Project Financial Tab
- **Role:** Admin, Finance
- **Project State:** Sold onwards (when financial tracking begins)
- **Allowed Actions:**
  - View invoices
  - View payments
  - View payment status
  - Record payment (Finance only)
  - View financial summary
  - Generate financial report
  - Reconcile budget vs. actual
  - Close financials (Admin only, moves to "Financial Closure" state)
- **Next Screens:**
  - Record Payment Modal
  - View Invoice Detail
  - Financial Report Screen

### 14. Project Activity Log Tab
- **Role:** Admin, Finance, Producer/Freelance
- **Project State:** Any
- **Allowed Actions:**
  - View all project activities
  - Filter by activity type
  - Filter by user
  - Filter by date range
  - Export activity log
- **Next Screens:**
  - Activity Detail Modal (if needed)

---

## 💼 Providers Module

### 15. Providers List
- **Role:** Admin, Producer/Freelance
- **Project State:** N/A
- **Allowed Actions:**
  - View providers table
  - Search/filter providers
  - Sort by columns
  - Create new provider (Admin only)
  - View provider details
  - Export providers list (Admin)
- **Next Screens:**
  - Provider Detail View
  - Create Provider Screen (Admin)
  - Dashboard

### 16. Create Provider Screen
- **Role:** Admin
- **Project State:** N/A
- **Allowed Actions:**
  - Enter provider information (name, type, contact)
  - Set provider category
  - Add payment terms
  - Add documents/certifications
  - Save provider
  - Cancel and return
- **Next Screens:**
  - Provider Detail View (after creation)
  - Providers List (cancel)

### 17. Provider Detail View
- **Role:** Admin, Producer/Freelance
- **Project State:** N/A
- **Allowed Actions:**
  - View provider information
  - View provider type (internal/external/hybrid)
  - View contact details
  - View payment terms
  - View linked projects
  - View transaction history
  - Edit provider info (Admin only)
  - Archive provider (Admin only)
- **Next Screens:**
  - Edit Provider Screen (Admin)
  - Linked Project Detail
  - Providers List

---

## 💰 Financial Module

### 18. Financial Overview
- **Role:** Admin, Finance
- **Project State:** N/A (aggregated view)
- **Allowed Actions:**
  - View financial dashboard
  - View revenue summary
  - View expenses summary
  - View pending payments
  - View profit/loss by project
  - Filter by date range
  - Filter by project state
  - Export financial data
- **Next Screens:**
  - Project Financial Tab (click project)
  - Payment Processing Screen
  - Financial Reports Screen
  - Dashboard

### 19. Payment Processing Screen
- **Role:** Finance
- **Project State:** N/A (can process payments for multiple projects)
- **Allowed Actions:**
  - View pending payments
  - Record payment
  - Upload payment proof
  - Mark payment as complete
  - Link payment to invoice
  - Link payment to provider
  - Cancel/return
- **Next Screens:**
  - Financial Overview
  - Project Financial Tab

### 20. Financial Reports Screen
- **Role:** Admin, Finance
- **Project State:** N/A
- **Allowed Actions:**
  - Select report type
  - Configure date range
  - Select projects to include
  - Generate report
  - Export report (PDF, Excel)
  - Schedule recurring reports (Admin only)
- **Next Screens:**
  - Report Preview
  - Financial Overview

---

## 👤 User Management (Admin Only)

### 21. Users List
- **Role:** Admin
- **Project State:** N/A
- **Allowed Actions:**
  - View all users
  - Search/filter users
  - View user roles
  - Create new user
  - Edit user
  - Deactivate user
  - View user activity
- **Next Screens:**
  - Create User Screen
  - Edit User Screen
  - User Activity Screen
  - Dashboard

### 22. Create User Screen
- **Role:** Admin
- **Project State:** N/A
- **Allowed Actions:**
  - Enter user details
  - Assign role (Admin/Finance/Producer/Freelance)
  - Set permissions
  - Send invitation email
  - Cancel and return
- **Next Screens:**
  - Users List
  - User Detail View (after creation)

### 23. Edit User Screen
- **Role:** Admin
- **Project State:** N/A
- **Allowed Actions:**
  - Edit user information
  - Change user role
  - Modify permissions
  - Reset password
  - Deactivate account
  - Save changes
  - Cancel and return
- **Next Screens:**
  - Users List
  - User Detail View

---

## ⚙️ Settings & Profile

### 24. User Profile
- **Role:** All authenticated users
- **Project State:** N/A
- **Allowed Actions:**
  - View own profile
  - Edit personal information
  - Change password
  - Update notification preferences
  - View assigned projects
  - Sign out
- **Next Screens:**
  - Edit Profile Screen
  - Change Password Screen
  - Dashboard
  - Login Screen (after sign out)

### 25. System Settings
- **Role:** Admin
- **Project State:** N/A
- **Allowed Actions:**
  - Configure general settings
  - Manage project states configuration
  - Configure cost categories
  - Set up integrations
  - View audit logs
  - Manage system notifications
- **Next Screens:**
  - General Settings Tab
  - Project Configuration Tab
  - Cost Categories Tab
  - Integrations Tab
  - Audit Logs Tab
  - Dashboard

---

## 🔔 Notifications

### 26. Notifications Center
- **Role:** All authenticated users
- **Project State:** N/A
- **Allowed Actions:**
  - View all notifications
  - Filter by type (project updates, mentions, system)
  - Mark as read
  - Mark all as read
  - Navigate to related entity
  - Delete notification
- **Next Screens:**
  - Related Project Detail
  - Related Provider Detail
  - Dashboard

---

## 📋 State Transition Rules

### Project State Flow (enforced by backend)

**1. Requirement / Pitch**
- **Who can transition:** Admin, assigned Producer
- **Allowed next states:**
  - Proposal (when initial requirements are defined)
  - Archived (if pitch rejected)

**2. Proposal**
- **Who can transition:** Admin, assigned Producer
- **Allowed next states:**
  - Budget Closed (when budget is finalized and locked)
  - Requirement / Pitch (if revisions needed)
  - Archived (if proposal rejected)

**3. Budget Closed**
- **Who can transition:** Admin only
- **Allowed next states:**
  - Sold (when client approves and signs)
  - Proposal (if budget needs revision - requires Admin approval)
  - Archived (if deal falls through)

**4. Sold**
- **Who can transition:** Admin only
- **Allowed next states:**
  - In Execution (when work begins)

**5. In Execution**
- **Who can transition:** Admin, assigned Producer
- **Allowed next states:**
  - Financial Closure (when work is complete and ready for final accounting)

**6. Financial Closure**
- **Who can transition:** Admin, Finance
- **Allowed next states:**
  - Archived (when all payments reconciled and project complete)

**7. Archived**
- **Who can transition:** Admin only
- **Allowed next states:**
  - None (terminal state)
  - Can be "unarchived" only by Admin to previous state if needed

---

## 🎯 Role-Based Screen Access Matrix

### Admin
- ✅ All screens
- ✅ All actions
- ✅ All project states

### Finance
- ✅ Dashboard
- ✅ Projects List (read-only on most fields)
- ✅ Project Detail View (focus on Budget and Financial tabs)
- ✅ Financial Overview
- ✅ Payment Processing
- ✅ Financial Reports
- ✅ User Profile
- ✅ Notifications
- ❌ Create/Edit Projects (except financial data)
- ❌ Providers Create/Edit
- ❌ User Management
- ❌ System Settings

### Producer/Freelance
- ✅ Dashboard
- ✅ Projects List (assigned projects only)
- ✅ Project Detail View (assigned projects)
- ✅ Project Budget (edit if state allows)
- ✅ Project Timeline
- ✅ Project Team (view only)
- ✅ Project Documents
- ✅ Providers List (view only)
- ✅ Provider Detail View (view only)
- ✅ User Profile
- ✅ Notifications
- ❌ Financial Overview
- ❌ Payment Processing
- ❌ Financial Reports (unless Admin grants access)
- ❌ User Management
- ❌ System Settings

---

## 🔒 Permission Notes

**CRITICAL:** All permissions shown above are **enforced by Supabase RLS policies**.

- UI adapts to hide/disable actions based on actual backend permissions
- If RLS denies access, the UI element is hidden or disabled
- Never show buttons/links for actions the user cannot perform
- State transitions are validated server-side via database triggers
- Client-side checks are for UX only (show/hide elements)
- Backend is the single source of truth for all permissions

---

## 📝 Implementation Notes for Design

1. **Start with backend permissions** - Design flows based on what RLS allows
2. **Figma shows intent, not authority** - If design conflicts with permissions, permissions win
3. **Progressive disclosure** - Only show actions/data user can access
4. **State-aware UI** - Buttons/forms adapt based on project state
5. **Clear feedback** - When action is denied, show why (friendly message)
6. **Audit everything** - All state changes and critical actions are logged

---

## 🎨 FigJam Usage

Copy this structure into FigJam as:
- **Sticky notes** for each screen (color-coded by module)
- **Arrows** showing navigation paths
- **Swim lanes** for different user roles
- **Decision diamonds** for state transitions
- **Icons** to indicate permissions (lock = restricted)

Color coding suggestion:
- 🔐 Auth flow: Blue
- 🏠 Main navigation: Gray
- 📊 Projects: Green
- 💼 Providers: Orange
- 💰 Financial: Yellow
- 👤 Users: Purple
- ⚙️ Settings: Red
- 🔔 Notifications: Cyan

---

**End of User Flow Diagram**
