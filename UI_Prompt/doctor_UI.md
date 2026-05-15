# Claude — Healynx Doctor and Admin Desktop App Prompt

You are designing and building the full Healynx desktop web application for doctors and clinic administrators. This is a production-grade clinical dashboard for desktop-first use, with careful attention to healthcare workflows, safety, clarity, and speed.

Build the product end to end, including every major page, modal, empty state, action, search flow, login, logout, settings, and all visible button behavior. If a button appears in the UI, define what it does, its loading state, its disabled state, its success state, its error state, and what happens after the click. Do not leave any important control undefined.

## Product Goal

Healynx helps doctors review patients, assess supplement and medicine interactions, manage emergency access, inspect AI summaries, review alerts, and share secure records. Admin users manage users, audits, PDPA requests, and system health. The experience must feel clinically serious, efficient, and trustworthy.

The app is desktop-first and optimized for 1440px, with a minimum supported width of 1024px. The layout must remain usable on smaller laptop screens, but this is not a mobile-first app.

## Stack and Implementation Rules

- Next.js 14 App Router
- React 18
- TypeScript
- Tailwind CSS 3 with CSS custom properties
- Shadcn UI primitives
- TanStack Query 5
- Zustand 4
- Recharts 2
- Lucide React icons
- React Hook Form and Zod for forms
- Use `fetch` only for network requests
- Use `Intl.DateTimeFormat` for all date formatting

## Design Direction

The visual system must be modern clinical software, not consumer wellness UI. It should feel precise, calm, and operational.

Use the Healynx token system consistently across the app. Doctor mode defaults to dark theme on first load. Light theme is available from Settings. Preserve strong contrast, restrained color use, and obvious hierarchy. Do not rely on generic stock dashboard styling.

Use Plus Jakarta Sans for headings and Inter for body text. Keep the same type scale across the whole app.

When dark mode is active, replace soft shadows with clear borders where appropriate. Surfaces should use visible separation, such as `border border-nv-neutral-700`, instead of depending on heavy shadows.

Use this exact Healynx colour system in `app/globals.css`.

Light mode (`:root`):

```css
:root {
  --nv-primary-900: #0d3b3b;
  --nv-primary-800: #125c5c;
  --nv-primary-700: #1a7a7a;
  --nv-primary-600: #1e8c8c;
  --nv-primary-500: #26a6a6;
  --nv-primary-400: #3dbebe;
  --nv-primary-300: #6ed4d4;
  --nv-primary-200: #a3e5e5;
  --nv-primary-100: #d4f3f3;
  --nv-primary-050: #edfafa;
  --nv-secondary-700: #1e4d8c;
  --nv-secondary-600: #2563b8;
  --nv-secondary-500: #3b82d9;
  --nv-secondary-300: #7eb3f0;
  --nv-secondary-100: #dce9fa;
  --nv-secondary-050: #f0f5fd;
  --nv-accent-700: #8c5c1e;
  --nv-accent-600: #d98c2e;
  --nv-accent-500: #eda83e;
  --nv-accent-100: #fcf0da;
  --nv-accent-050: #fef9f2;
  --nv-success-600: #2d8c4a;
  --nv-success-100: #d9f2e2;
  --nv-danger-700: #8c1a1a;
  --nv-danger-600: #d92d2d;
  --nv-danger-500: #e84a4a;
  --nv-danger-100: #fce0e0;
  --nv-danger-050: #fef5f5;
  --nv-info-600: #3b82d9;
  --nv-info-100: #dce9fa;
  --nv-neutral-900: #111827;
  --nv-neutral-800: #1f2937;
  --nv-neutral-750: #283040;
  --nv-neutral-700: #374151;
  --nv-neutral-600: #4b5563;
  --nv-neutral-500: #6b7280;
  --nv-neutral-400: #9ca3af;
  --nv-neutral-300: #d1d5db;
  --nv-neutral-200: #e5e7eb;
  --nv-neutral-150: #eef0f3;
  --nv-neutral-100: #f3f4f6;
  --nv-neutral-050: #f9fafb;
  --nv-neutral-000: #ffffff;
  --nv-shadow-sm: 0 1px 3px rgba(0, 0, 0, 0.08);
  --nv-shadow-md: 0 4px 6px -1px rgba(0, 0, 0, 0.08);
  --nv-shadow-lg: 0 10px 15px -3px rgba(0, 0, 0, 0.08);
  --nv-shadow-focus: 0 0 0 3px rgba(30, 140, 140, 0.25);
  --nv-shadow-focus-danger: 0 0 0 3px rgba(217, 45, 45, 0.25);
}
```

Dark mode (`[data-theme="dark"]`):

```css
[data-theme="dark"] {
  --nv-neutral-900: #f9fafb;
  --nv-neutral-800: #f3f4f6;
  --nv-neutral-700: #d1d5db;
  --nv-neutral-600: #9ca3af;
  --nv-neutral-500: #6b7280;
  --nv-neutral-400: #4b5563;
  --nv-neutral-300: #374151;
  --nv-neutral-200: #1f2937;
  --nv-neutral-150: #1f2937;
  --nv-neutral-100: #111827;
  --nv-neutral-050: #0f172a;
  --nv-neutral-000: #1f2937;
}
```

## Global App Behavior

The app has two major areas: doctor and admin.

- Doctor area contains the clinical workflow: dashboard, patients, alerts, chatbot, emergency access, reports, settings.
- Admin area contains operational management: dashboard, users, audit logs, PDPA requests, clinic settings, system health, billing.

Both areas should share the same core components and design language, but have role-specific navigation and page content.

Authenticated screens should also surface a subtle security cue where relevant, such as a data residency badge, encryption indicator, or secure-session status, so the clinical environment feels trusted and compliant.

Every screen must support the following global behaviors where relevant:

- Search via a global search field and Ctrl+K keyboard shortcut
- Notifications bell with badge and dropdown
- User menu with profile, settings, theme toggle, and logout
- Loading skeletons for all async data
- Empty states with helpful action buttons
- Error states with retry actions
- Confirmation dialogs for destructive actions
- Accessible keyboard navigation
- Clear focus states
- Helpful toast notifications for success, error, and warning events

## Authentication and Session Flow

Create a full authentication experience.

### Login

Build a clean login page for doctors and admins with email, password, remember me, and optional clinic selection if needed. Include:

- Email field with validation
- Password field with show/hide toggle
- MFA / TOTP step when enabled on the account
- Sign in button
- Forgot password link
- Optional SSO or magic link placeholder if the product supports it later
- Inline error messages for invalid credentials, network failure, locked account, expired password, and unauthorized role
- Loading state on submit
- Success transition into the correct role-based area

After login, route doctor users to the doctor dashboard and admin users to the admin dashboard. If MFA is enabled, the flow must pause on a verification step before routing.

### Logout

Logout must be available from the user menu and sidebar bottom section. It should:

- Ask for confirmation if the user has an active emergency session
- Clear session state, local UI state, and any sensitive cached data
- Redirect to the login screen
- Show a logged-out confirmation toast

### Session Expiry

If a session expires, show a modal or full-page lock screen prompting the user to sign in again. Preserve unsaved work only if it is safe to do so.

## Shared Layout Structure

### Doctor Layout

Use a persistent left sidebar and a top bar.

Sidebar rules:

- 256px wide by default
- Collapsible to 64px icon-only mode
- Top section contains Healynx logo and app name
- Middle section contains the primary navigation
- Bottom section contains doctor avatar, name, role, and logout button
- Active item uses primary background, primary text, and a left border accent
- Collapsed state keeps icons only and shows tooltips on hover

Doctor navigation items:

- Dashboard
- Patients
- Alerts with count badge
- AI Chatbot
- Emergency Access
- Reports
- Settings

Top bar rules:

- 56px height
- Global search field with Ctrl+K hint
- Notification bell with unread count badge
- Settings gear shortcut
- User menu with account actions

### Admin Layout

Use a similar sidebar and top bar structure, but with admin navigation items:

- Dashboard
- Users
- Audit Logs
- PDPA Requests
- Clinic Settings
- System Health
- Billing

## Core UI Components

Build these as reusable components first, then use them across the app.

### Sidebar

The sidebar must support role-based item sets, collapse/expand, active states, badges, hover labels, and tooltips in collapsed mode. It must use `next/link` for navigation.

### Data Table

Build a flexible data table with:

- Column definitions with custom renderers
- Sortable headers
- Row hover states
- Selected row state
- Pagination controls
- Per-page selector
- Mobile fallback into stacked cards on smaller widths

Provide row click support, optional actions column, loading skeleton rows, and empty table messaging.

### Timeline

Build a vertical timeline with line, dots, indented cards, and a show-more expander for older entries. Use it for assessments, audit history, and clinical events.

### Alert Banner

Support info, warning, and critical variants. It must be dismissible unless the alert is tied to an emergency state. Include icon, severity text, message, and optional action button.

### Emergency Banner

This is a high-priority sticky banner shown whenever emergency access is active. It must show:

- Emergency state message
- Countdown timer
- Patient name
- Reason for access
- End Emergency Access button

The banner is non-dismissible and should appear across every screen while the emergency session is active. When the timer reaches zero or the doctor ends the session, it should disappear and show a toast.

### Access Timer Bar

Create a compact progress bar that represents access time remaining. It must change color based on urgency and include a readable time label.

### Secure Viewer

This is a minimal chrome document viewer for shared records. It must include:

- Dark surround
- Centered white document area
- Watermark overlay with recipient, time, and IP metadata
- View-only warning
- Request Extension link
- Inactivity timeout overlay
- Anti-copy protection
- Revoked and expired states
- Access PIN flow when required

### Chatbot Panel

Create a right-side slide-over panel for AI clinical assistant chat. It must include:

- Header with title and close button
- Scrollable message history
- User and AI message styling differences
- Source citations under AI messages
- AI disclaimer
- Model version label
- Input box with send button
- Suggested prompt chips

### AI Summary Card

All AI-generated clinical summaries must use this standardized card. It must include:

- AI-generated label
- Flag error action
- Summary body
- Confidence indicator bar
- Model version metadata

### Wellness Gauge

Use a compact version for table cells and a richer version for detail pages. The compact variant should show score, trend direction, and a small arc or progress indicator.

### Modal

Build a general modal component with size variants, backdrop blur, close button, Escape support, backdrop click support, and focus trapping.

Include a special Emergency Access modal variant that is non-dismissible, visually emphasized, and includes reason selection, free-text justification, Cancel, and Confirm Emergency actions.

### Basic UI Primitives

Create reusable button, input, badge, card, and empty-state components consistent with the patient app design system.

## Doctor Screens

### Doctor Dashboard

This is the primary landing page after doctor login.

Include these sections:

1. KPI cards for total patients, pending reviews, and active alerts
2. Priority alerts panel sorted by severity, showing newest and most urgent items first
3. Quick actions panel with New Patient, Emergency Access, and View All Alerts buttons
4. Patient list table with wellness scores, alert counts, last visit dates, and AI summary snippets

Doctor dashboard controls and behaviors:

- Search patients from the top bar search field
- Click a patient row to open patient detail
- Click New Patient to open a create patient modal or page
- Click Emergency Access to begin the emergency flow
- Click View All Alerts to open the alerts page
- Sorting on score, last visit, and alert severity
- Pagination and per-page selection
- Empty state for no alerts and no patients
- Loading skeletons for cards and table rows

The patient table must include row-level actions when needed, such as open profile, review alert, or start emergency access. The AI summary snippet should be short and clinically clear.

### Patient Detail

This is the most detailed clinical page and must support tabbed navigation for Overview, Records, Supplements, and History.

The header must show:

- Patient name
- Demographics
- Masked NRIC or equivalent identifier
- Primary doctor
- Relevant conditions
- Current wellness score
- Emergency Access button

Overview tab:

- AI Summary Card
- Active alerts panel
- Acknowledge button for each alert
- High severity alerts require explicit acknowledgement
- Additional clinical context sections as needed

- Medication Conflict Acknowledgement: For alerts identified as medication or drug–drug conflicts, include an inline "Acknowledge" action that opens a compact modal. The modal must: show the conflict summary, auto-fill the clinician identity, provide a required `clinicianNotes` textarea for High-severity items, and include `Confirm acknowledgement` and `Cancel` buttons. On confirm the UI calls `POST /medication-conflicts/{conflictId}/acknowledge` with `{ clinicianId, clinicianNotes }`, appends an immutable audit log entry (actor, timestamp, IP), and updates the alert state to `Acknowledged by clinician` showing the clinician note and timestamp in the patient's record. The acknowledge flow should be auditable and surfaced in the Alerts page and the patient's History tab.

Records tab:

- List of record cards
- Record title, type, date, share status
- AI summary for each record where relevant
- Upload button
- View, share, download, or revoke actions depending on permissions
- Share action opens a secure share configuration modal with recipient search or email input, view-only default permissions, optional access code, expiry options, and a generate-link action
- Revoke action asks for confirmation and logs the change to audit history

Supplements tab:

- Timeline of assessments or supplement-related evaluations
- Trend indicators
- Interaction risk highlights

History tab:

- Chronological clinical activity
- Visits, assessments, changes in wellness score, and key interventions

Patient detail actions:

- Open chatbot panel from a visible AI Chatbot button
- Open emergency access modal
- Acknowledge alerts
- Navigate between tabs with accessible tab controls
- Show loading and empty states for each tab

### Alerts Page

This page shows the full alert list.

Include:

- Filters by severity, type, status, date, and patient
- Search within alerts
- Sort by newest, severity, and unresolved status
- Dismiss, acknowledge, or open patient actions
- Critical alert emphasis with visual treatment and strong copy
- Badge counts for high, moderate, and low alerts

### AI Chatbot Page

This is a full-page version of the chatbot with larger chat history and richer context.

Include:

- Patient context header when patient-linked
- Suggested query prompts
- Citation links or source references
- Clear disclaimer that output is AI-generated and must be clinically verified
- Copy response button where appropriate
- Retry on failed generation

### Reports Page

If the app includes a doctor reports page, it must support:

- Date range filters
- Patient filters
- Export buttons for PDF and CSV if allowed
- Summary cards for caseload, alert trends, and review completion
- Charts for overall wellness and alert volume

### Settings Page

The doctor settings page must include all user-facing settings:

- Theme toggle for dark and light mode
- Notification preferences
- Search preferences
- Language or locale settings if supported
- Profile details
- Password change form
- Session management and active device review
- Emergency access defaults if relevant

Every form must have validation, save, cancel, and loading states.

## Emergency Access Flow

Emergency access is a critical workflow and must be treated as a first-class feature.

Flow:

1. Doctor clicks Emergency Access
2. A non-dismissible modal opens
3. Doctor selects or enters a reason and justification
4. Doctor confirms emergency access
5. The app activates an emergency session in global state
6. A sticky emergency banner appears across all screens
7. Countdown starts immediately
8. On expiry or manual end, the session is closed and the UI updates globally

Emergency access behaviors:

- Include a visible timer
- Log all actions during the session
- Use red styling and highly visible warnings
- Require confirmation before ending the session early
- Redirect the user to the appropriate safe screen after ending
- Emergency access modal must include predefined quick-reason options such as unconscious patient, life-threatening emergency, severe allergic reaction, and acute psychiatric crisis, plus a free-text justification field
- Confirm Emergency button stays disabled until a reason is entered
- Emergency activation must trigger audit logging and notification behavior for the patient and clinic admin
- Emergency sessions expire automatically after 30 minutes unless ended earlier

## Secure Share Viewer

This viewer must be designed for shared documents and record access with minimal UI chrome.

States to support:

- Valid and active
- Expired
- Revoked
- Inactivity timeout
- Access code required

The viewer must:

- Hide navigation chrome
- Prevent copying and printing where appropriate
- Show access timer at the top
- Show view-only warnings
- Display a watermark overlay tied to viewer identity and session metadata
- Offer Request Extension when access is near expiry or expired

### Share Record Modal

When a doctor or patient shares a record, use a secure share configuration modal or drawer with:

- Recipient search for connected doctors or email input
- View permission on by default and locked
- Download and forward permissions off by default
- Expiry choices for 24h, 7d, 30d, or custom
- Optional 6-digit access code
- Note field for context
- Generate Secure Link primary action
- Success state that replaces the form with a copyable link, share-by-email or share-by-WhatsApp actions, and a Done button

## Admin Screens

### Admin Dashboard

The admin dashboard should summarize operational status.

Include:

- KPI cards for patients, doctors, shares, and emergency sessions
- Recent emergency events table
- System health summary panel
- Pending PDPA requests count
- Alerts or incidents requiring admin attention

Buttons on this page should include drill-down links into users, audit logs, PDPA, and health views.

### User Management

This page manages doctors and other users.

Include:

- Search field
- Filters for role, status, and clinic
- Data table of users
- Add Doctor button
- Invitation flow
- Edit user button
- Suspend or deactivate actions with confirmation
- Reset password or resend invitation actions

The Add Doctor modal must include:

- Name
- Email
- Role
- Permissions checkboxes
- Send Invitation button
- Cancel button

### Audit Logs

This page should show who did what, when, where, and with what result.

Include:

- Filters for actor, action, target type, and date range
- Search
- Expandable row details
- JSON or structured detail panel
- IP address and user agent display
- Export CSV and PDF buttons
- Entries should reflect immutable access, sharing, emergency, and alert-acknowledgement events

### PDPA Requests

This page handles privacy and data rights workflows.

Include tabs or segmented views for:

- Pending
- In Progress
- Completed

Support actions such as:

- Process export
- Confirm identity
- Approve deletion
- Reject or hold request
- View request history

Every sensitive action must show a confirmation step and log the outcome.

### Clinic Settings

This page manages clinic-level configuration.

Include settings for:

- Clinic name and branding
- Working hours
- Notification rules
- Emergency workflow defaults
- Document sharing defaults
- Role permissions overview

### System Health

This page must present operational health clearly.

Include:

- Status cards for API, Auth, AI Engine, OCR, DB, Redis, and Storage
- Green, amber, and red status indicators
- AI engine metrics like processed volume, average time, p95, and errors
- Recent incidents list
- Service restart or diagnostics actions if allowed
- Clear indicator for AI engine status, storage latency, and uptime health

### Billing

If the billing page is included, it should show plan status, invoices, usage, payment method, and download actions.

## Form and Interaction Details

Every meaningful button and control should have a defined purpose.

Examples of required button behavior:

- Search button submits the current query or opens advanced filters
- Clear search resets the query and results
- Save persists changes and shows a success toast
- Cancel closes a modal or reverts an unsaved form
- Delete opens a confirmation modal and requires a second confirmation
- Export starts a job and shows progress or completion feedback
- Retry reattempts a failed fetch or failed AI operation
- Close dismisses panels, dialogs, and overlays where allowed
- End Emergency Access immediately ends the session after confirmation

All buttons must have hover, focus, disabled, and loading states. Primary actions must be visually distinct from secondary and destructive actions.

## Search Behavior

Search is a global feature and must be available from the top bar in both doctor and admin areas.

Support:

- Ctrl+K open search
- Search patients by name, ID, condition, alert, or record
- Search users in the admin area
- Search audits and PDPA requests by relevant terms
- Clearable search input
- Debounced result updates where appropriate
- No-results state with helpful next steps

Search should feel fast and intelligent, but it must remain transparent and predictable.

## Notifications

The notification bell must open a panel or dropdown with unread and read items, severity markers, timestamps, and direct links to the relevant page. Unread counts should update visually after viewing or dismissing items.

## Data States

For every page and component, define these states:

- Loading
- Empty
- Error
- Partial data
- Success
- Disabled or read-only when permissions block actions

Do not leave any major page without a graceful empty-state design.

## Accessibility and Safety

Make the application keyboard accessible and screen-reader friendly.

Requirements:

- Clear focus ring and visible focus states
- Semantic buttons and controls
- Escape closes dismissible layers
- Proper labels and helper text for forms
- Accessible tab navigation
- Sufficient contrast in dark mode and light mode

For clinical safety:

- AI-generated content must always be marked as such
- Include visible disclaimers where clinical judgment is required
- Avoid implying that AI output is a final diagnosis
- Highlight critical interactions clearly and urgently

## Responsive Rules

Although this is desktop-first, the layout must gracefully scale down to laptop widths.

- Sidebar may collapse automatically at narrower widths
- Tables should become card stacks on small widths
- Panels should reflow into vertical sections when needed
- Modals must fit within the viewport and remain usable

Do not build a mobile app experience. The priority is desktop clarity.

## Content and Tone

The app copy should sound professional, concise, and medically appropriate. Avoid playful language. Use direct operational wording such as Review, Confirm, Acknowledge, Export, End Session, and Request Extension.

## File Structure to Create

```text
app/
├── (doctor)/
│   ├── layout.tsx                     → sidebar + top bar + content
│   ├── dashboard/page.tsx             → S06
│   ├── patients/
│   │   └── [patientId]/page.tsx       → S07
│   ├── alerts/page.tsx                → full alert list
│   └── chat/page.tsx                  → AI chatbot
├── (admin)/
│   ├── layout.tsx                     → sidebar (admin nav) + content
│   ├── dashboard/page.tsx             → admin dashboard
│   ├── users/page.tsx                 → user management
│   ├── audit/page.tsx                 → audit logs
│   ├── pdpa/page.tsx                  → PDPA requests
│   └── health/page.tsx               → system health
├── share/
│   └── [token]/page.tsx              → S08 Secure Viewer
components/
├── ui/
│   ├── sidebar.tsx
│   ├── data-table.tsx
│   ├── timeline.tsx
│   ├── alert-banner.tsx
│   ├── emergency-banner.tsx
│   ├── access-timer.tsx
│   ├── secure-viewer.tsx
│   ├── watermark-overlay.tsx
│   ├── chatbot-panel.tsx
│   ├── ai-summary-card.tsx
│   ├── wellness-gauge.tsx             → compact variant for tables
│   ├── card.tsx
│   ├── button.tsx
│   ├── input.tsx
│   ├── modal.tsx
│   ├── badge.tsx
│   └── empty-state.tsx
stores/
├── emergency-store.ts
hooks/
├── use-doctor-dashboard.ts
├── use-patient-detail.ts
├── use-emergency-access.ts
mock/
└── doctor-data.ts
```

## Components — Build in This Order

Build the core primitives first, then the role-based shells, then the clinical workflow components.

1. Sidebar
2. Data Table
3. Timeline
4. Alert Banner
5. Emergency Banner
6. Access Timer Bar
7. Secure Viewer
8. Chatbot Panel
9. AI Summary Card
10. Wellness Gauge
11. Modal
12. Button / Input / Badge / Empty State

Use the shared primitives across doctor, admin, and secure-share screens. AI content must always render through AI Summary Card. Emergency access and secure viewer logic should be global rather than page-local.

## Mock Data

Use the following mock data shape when wiring the doctor dashboard and patient detail pages:

- Doctor dashboard stats: total patients, pending reviews, active alerts broken down by high / moderate / low
- Priority alerts: patient name, alert type, severity, message, generated at
- Patient list: name, age, last visit date, wellness score, previous score, alert counts, AI summary line
- Patient detail: demographics, health profile, wellness trend, AI clinical summary, active alerts, assessments, records

The patient detail mock should include at minimum:

- Type 2 Diabetes and Hypertension in conditions
- Metformin, Amlodipine, and Atorvastatin in medications
- Penicillin in allergies
- A wellness trend history
- A high-severity alert for Jamu Herbal + Metformin
- At least one record such as an HbA1c lab report

Mock values should be realistic enough to drive the doctor dashboard, alerts, patient profile, and secure share flows without placeholder-only content.

## Build Order

Implement in this order:

1. Global styles, tokens, typography, dark mode, and layout shell
2. Shared components: sidebar, top bar, button, input, badge, card, modal, table, timeline, alert banner, emergency banner, access timer, secure viewer, chatbot panel, AI summary card, empty state, wellness gauge
3. Emergency access store and any supporting global state
4. Mock data and hooks
5. Authentication screens and session handling
6. Doctor dashboard
7. Patient detail experience
8. Alerts page and chatbot page
9. Secure share viewer
10. Admin dashboard, users, audit, PDPA, clinic settings, system health, and billing

## Final Output Expectations

Build the full UI and interaction model as if it is going into a real clinical environment. The result should feel complete, polished, and internally consistent across all pages.

Do not stop after the overview. Implement every page, every button, every modal, every state, and every important workflow that a doctor or admin user would reasonably need.
