# Claude — Build Healynx Doctor & Admin Desktop App

You are building the **doctor and clinic admin desktop web app** for Healynx. Primary canvas: **1440px**. Minimum: **1024px**. This is a desktop-first professional dashboard for clinical use.

**Tech stack (fixed — no additions):**

- Next.js 14 (App Router) + React 18 + TypeScript
- Tailwind CSS 3.x with CSS custom properties
- Shadcn UI
- TanStack Query 5.x, Zustand 4.x, Recharts 2.x, Lucide React
- React Hook Form + Zod
- `fetch` only. `Intl.DateTimeFormat`.

---

## 1. Design Tokens

Same token system as patient app. Copy these into `app/globals.css` under `:root`:

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

**Dark mode (doctor default):** Copy these under `[data-theme="dark"]`:

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

Doctor dashboard defaults to dark mode on first load. Light mode available via toggle in Settings. Replace shadows with `border border-nv-neutral-700` in dark mode.

Typography: same as patient (Plus Jakarta Sans headings, Inter body). Type scale identical.

---

## 2. Layout

Desktop (1024px+): 12-column grid, 24px gutters, max-width 1280px centred.

**Navigation:** Persistent sidebar (256px wide, collapsible to 64px icons-only). Background: `neutral-050` light / `neutral-900` dark.

Sidebar structure:

- Top: Healynx logo + name
- Nav items (48px height each, icon 20px + label body-sm): Dashboard 🏠, Patients 👥, Alerts ⚠ (with count badge), AI Chatbot 🤖, Emergency Access 🔒, Reports 📊, Settings ⚙
- Bottom: Doctor avatar + name + role + [Logout]
- Active: `primary-050` bg, `primary-600` text, 3px left `primary-600` border

**Top bar:** 56px height. Global search (Ctrl+K), notification bell with badge, user menu, settings gear.

---

## 3. File Structure to Create

```
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

---

## 4. Components — Build in This Order

### 4.1 Sidebar

```tsx
interface SidebarProps {
  role: "doctor" | "admin";
  collapsed: boolean;
  onToggle: () => void;
}
```

Doctor nav: Dashboard, Patients, Alerts (with count badge), AI Chatbot, Emergency Access, Reports, Settings.
Admin nav: Dashboard, Users, Audit Logs, PDPA Requests, Clinic Settings, System Health, Billing.

Width: 256px → 64px on collapse (icons only, no labels). Transition: 200ms. Use `next/link` for nav items.

### 4.2 Data Table

```tsx
interface Column<T> {
  key: string;
  header: string;
  sortable?: boolean;
  render: (row: T) => React.ReactNode;
}
interface DataTableProps<T> {
  columns: Column<T>[];
  data: T[];
  onRowClick?: (row: T) => void;
  pagination?: {
    page: number;
    total: number;
    perPage: number;
    onPageChange: (p: number) => void;
  };
}
```

Header: `neutral-100` bg, 13px semibold uppercase. Sortable: ▲▼ icons. Row: 52px height, 0.5px bottom border `neutral-200`. Hover: `neutral-050`. Selected: `primary-050`. Pagination: "← 1 2 3 →" + per-page selector.

On mobile (<1024px): collapse to card list — each row becomes a stacked card.

### 4.3 Timeline

Vertical line 2px `neutral-200`. Dots 12px (filled `primary-600` for recent, outlined for older). Cards indented 24px. "Show older entries" expander.

### 4.4 Alert Banner

Variants: info=`bg-nv-info-100 border-l-4 border-nv-info-600`, warning=`bg-nv-accent-100 border-l-4 border-nv-accent-600`, critical=`bg-nv-danger-050 border-l-4 border-nv-danger-600`. Dismissible unless emergency.

### 4.5 Emergency Banner

```tsx
interface EmergencyBannerProps {
  patientName: string;
  reason: string;
  remainingSeconds: number;
  onEnd: () => void;
}
```

Full-width sticky top (z-50). `bg-nv-danger-700 text-white`. Content: 🔴 "EMERGENCY ACCESS ACTIVE — ALL ACTIONS LOGGED" + ⏱ countdown timer (monospace) + patient name + reason + [End Emergency Access] button. Timer decrements every second. Non-dismissible. On expiry or end: banner disappears, toast.

### 4.6 Access Timer Bar

4px height. Track: `neutral-200`. Fill: `primary-600` (>24h), `accent-600` (<24h), `danger-600` (<1h). Text above: body-sm + mono time. Emergency: always `danger-600`.

### 4.7 Secure Viewer

Fullscreen minimal-chrome page. Dark surround (`neutral-900`). Document area: white, centred, max 900px, padding 32px.

Watermark: diagonal repeating text `{recipient} | {datetime} | {ip}` at 12% opacity, `neutral-600`, rotated -20°, server-rendered as overlay.

Anti-copy: `user-select: none`, `onContextMenu` prevented, Ctrl+C/S/P blocked via keydown handler, print CSS `body{display:none}`.

Access timer bar at top. "View-only" warning text. [Request Extension] link. 15-min inactivity overlay. Emergency mode: red-tinted watermark + "EMERGENCY ACCESS" prefix.

### 4.8 Chatbot Panel

```tsx
interface ChatbotPanelProps {
  patientId?: string;
  isOpen: boolean;
  onClose: () => void;
}
```

Side panel, right 30% width, resizable. Header: 🤖 + "AI Clinical Assistant" + close. Messages: user (right, `primary-600` bg, white text) / AI (left, `secondary-050` bg, dark text). AI messages: sparkle icon + answer + source citations (body-sm, `neutral-500`) + disclaimer "AI-generated. Verify clinically." + model version.

Input: text field + send button. Suggested queries below input.

### 4.9 AI Summary Card

Same rules as patient app. ALL AI content must use this. Header: sparkle + "AI-GENERATED" overline + [Flag Error]. Body: summary text. Footer: confidence bar (4px, green≥80%, amber 60-79%, red<60%) + model version (mono, 12px, 60% opacity).

### 4.10 Wellness Gauge — Compact

Used in table cells. 36px number, mini arc below, trend arrow. No qualitative label. Props: `{ score: number; previousScore?: number; compact: true }`.

### 4.11 Modal

Centred, max 560px (narrow) / 720px (wide). Backdrop: `rgba(0,0,0,0.5)` with `backdrop-blur-sm`. Close: ✕, Escape, backdrop click. Focus trapped.

**Emergency Access variant:** 2px `danger-600` border, `danger-050` header, non-dismissible backdrop. Quick-reason buttons + free-text justification. [Cancel] + [Confirm Emergency] (danger).

### 4.12 Button / Input / Badge / Empty State

Same specs as patient app. Desktop touch targets: 40×40px minimum.

---

## 5. Screens — Build in This Order

### S06 — Doctor Dashboard

**File:** `app/(doctor)/dashboard/page.tsx`

Layout: Sidebar (left 256px) + Top bar + Content area.

Top row: KPI cards (3 columns) — Total Patients, Pending Reviews, Active Alerts (breakdown high/amber/low).

Second row: Priority Alerts (8 columns, sorted by severity, red pulse on new) + Quick Actions (4 columns — [+ New Patient], [Emergency Access], [View All Alerts]).

Third row: Patient List (full-width DataTable). Columns: Patient (name), Score (WellnessGauge compact), Alerts (count badge coloured by highest severity), Last Visit, AI Summary (one-line sparkle snippet). Sortable, filterable, searchable, paginated (50/page).

```tsx
"use client";
import { useDoctorDashboard } from "@/hooks/use-doctor-dashboard";
import { DataTable } from "@/components/ui/data-table";
import { WellnessGauge } from "@/components/ui/wellness-gauge";
import { Card } from "@/components/ui/card";
import { Badge } from "@/components/ui/badge";
import { Skeleton } from "@/components/ui/loading";

export default function DoctorDashboard() {
  const { data, isLoading } = useDoctorDashboard();

  const columns = [
    {
      key: "name",
      header: "Patient",
      render: (r) => <span className="font-semibold">{r.full_name}</span>,
    },
    {
      key: "score",
      header: "Score",
      render: (r) => (
        <WellnessGauge
          score={r.wellness_score}
          previousScore={r.wellness_score_prev}
          compact
        />
      ),
    },
    {
      key: "alerts",
      header: "Alerts",
      render: (r) => {
        if (r.alert_count.high > 0)
          return <Badge color="red">{r.alert_count.high} high</Badge>;
        if (r.alert_count.moderate > 0)
          return <Badge color="amber">{r.alert_count.moderate} moderate</Badge>;
        return <Badge color="green">0</Badge>;
      },
    },
    {
      key: "last_visit",
      header: "Last Visit",
      render: (r) => r.last_visit_date ?? "—",
    },
    {
      key: "summary",
      header: "AI Summary",
      render: (r) => (
        <span className="text-sm text-nv-neutral-600">
          ✦ {r.ai_summary_line}
        </span>
      ),
    },
  ];

  return (
    <div className="flex flex-col gap-6 p-6">
      {/* KPI Row */}
      <div className="grid grid-cols-3 gap-6">{/* KPI cards */}</div>
      {/* Alerts + Actions Row */}
      <div className="grid grid-cols-12 gap-6">
        <div className="col-span-8">{/* Priority alerts */}</div>
        <div className="col-span-4">{/* Quick actions */}</div>
      </div>
      {/* Patient List */}
      <Card>
        <DataTable
          columns={columns}
          data={data?.patients ?? []}
          onRowClick={(row) => router.push(`/patients/${row.id}`)}
          pagination={{
            page: 1,
            total: 247,
            perPage: 50,
            onPageChange: () => {},
          }}
        />
      </Card>
    </div>
  );
}
```

**States:** Loading (skeleton rows in table), No alerts (green "All patients current" message), Emergency active (red EmergencyBanner across top, all records accessible).

### S07 — Patient Detail View

**File:** `app/(doctor)/patients/[patientId]/page.tsx`

Patient header bar: name, demographics, NRIC (masked), conditions, primary doctor, WellnessGauge compact. [Emergency Access] danger-secondary button.

Tab navigation (Radix Tabs): Overview, Records, Supplements, History.

**Overview tab:** AI Summary Card (8 columns) + Active Alerts panel (4 columns). Each alert: severity icon + description + [Acknowledge] button. High alerts require acknowledgement.

**Records tab:** Record cards with AI summaries, share status, upload button.

**Supplements tab:** Timeline of assessments with trend sparkline.

**AI Chatbot button:** Opens ChatbotPanel (right side panel).

```tsx
'use client';
import { usePatientDetail } from '@/hooks/use-patient-detail';
import { Tabs, TabsContent, TabsList, TabsTrigger } from '@/components/ui/tabs';
import { AISummaryCard } from '@/components/ui/ai-summary-card';
import { AlertBanner } from '@/components/ui/alert-banner';
import { Timeline } from '@/components/ui/timeline';
import { Button } from '@/components/ui/button';
import { useEmergencyStore } from '@/stores/emergency-store';

export default function PatientDetail({ params }: { params: { patientId: string } }) {
  const { data } = usePatientDetail(params.patientId);
  const emergency = useEmergencyStore();

  return (
    <div className="flex flex-col gap-6 p-6">
      {emergency.isActive && <EmergencyBanner patientName={emergency.patientName} reason={emergency.reason} remainingSeconds={emergency.remainingSeconds} onEnd={emergency.deactivate} />}
      {/* Patient header */}
      <div className="flex items-center justify-between">
        <div>
          <h2>{data?.full_name}</h2>
          <p className="text-sm text-nv-neutral-600">{data?.health_profile?.conditions?.join(', ')}</p>
        </div>
        <WellnessGauge score={data?.wellness_score ?? 0} compact />
        <Button variant="danger-secondary" onClick={() => /* open emergency modal */}>Emergency Access</Button>
      </div>
      <Tabs defaultValue="overview">
        <TabsList>
          <TabsTrigger value="overview">Overview</TabsTrigger>
          <TabsTrigger value="records">Records</TabsTrigger>
          <TabsTrigger value="supplements">Supplements</TabsTrigger>
        </TabsList>
        <TabsContent value="overview">
          <div className="grid grid-cols-12 gap-6">
            <div className="col-span-8">
              <AISummaryCard summary={data?.ai_clinical_summary?.text} confidence={data?.ai_clinical_summary?.confidence} modelVersion={data?.ai_clinical_summary?.model_version} />
            </div>
            <div className="col-span-4">
              {data?.active_alerts?.map(a => <AlertBanner key={a.id} variant={a.severity === 'high' ? 'critical' : 'warning'} message={a.message} />)}
            </div>
          </div>
          <Timeline items={data?.assessments} />
        </TabsContent>
      </Tabs>
    </div>
  );
}
```

### Emergency Access Flow (Modal → Banner → Expiry)

1. Doctor taps [Emergency Access] → Emergency Modal opens (non-dismissible)
2. Doctor selects/enters reason → [Confirm Emergency]
3. `POST /emergency-access` → receive `emergency_session_id` + `access_token` (30-min JWT)
4. Zustand `emergency-store`: set `isActive=true`, start `setInterval` counting down from 1800s
5. EmergencyBanner renders at top of every screen
6. On expiry (remainingSeconds=0) OR doctor taps [End Emergency Access]: `DELETE /emergency-access/{id}`, banner disappears, redirect to patient list

```ts
// stores/emergency-store.ts
interface EmergencyState {
  isActive: boolean;
  sessionId: string | null;
  patientId: string | null;
  patientName: string | null;
  reason: string | null;
  remainingSeconds: number;
  activate: (data: {
    sessionId: string;
    patientId: string;
    patientName: string;
    reason: string;
  }) => void;
  deactivate: () => void;
  tick: () => void;
}
```

### S08 — Secure Viewer

**File:** `app/share/[token]/page.tsx`

No sidebar, no top bar — minimal chrome. Dark surround. Persistent AccessTimerBar. Watermark overlay. Document content rendered as image/canvas (not downloadable). Anti-copy enforced. "View-only" warning. [Request Extension] link.

**States:**

- Valid: document visible, timer counting
- Expired: "Access expired. Contact owner." + [Request Extension]
- Revoked: "Access revoked by owner."
- Inactivity timeout: overlay "Session timed out. Reload."
- Access code required: PIN input (3 attempts max)

### Admin Screens

**Admin Dashboard (`app/(admin)/dashboard/page.tsx`):** KPI cards (patients, doctors, shares, emergency sessions). Recent emergency events table. System health panel (uptime, AI engine status, storage, latency). Pending PDPA requests count.

**User Management (`app/(admin)/users/page.tsx`):** DataTable of users (name, role, status, actions). [+ Add Doctor] opens modal: name, email, role, permissions checkboxes. [Send Invitation] button.

**Audit Logs (`app/(admin)/audit/page.tsx`):** Filter bar (actor, action, target type, date range). DataTable of audit entries. Click row → expand detail (actor, action, target, details JSON, IP, user agent, outcome). [Export CSV].

**PDPA Requests (`app/(admin)/pdpa/page.tsx`):** Tabs: Pending, In Progress, Completed. Data export requests: [Process Export] button. Data deletion requests: [Confirm Identity] → [Delete] with confirmation.

**System Health (`app/(admin)/health/page.tsx`):** Service status grid (API, Auth, AI Engine, OCR, DB, Redis, Storage — each with 🟢/🟡/🔴 indicator). AI engine metrics (assessments processed, avg time, p95, errors). Recent incidents log.

---

## 6. Mock Data — Copy into `mock/doctor-data.ts`

```ts
export const MOCK_DOCTOR_DASHBOARD = {
  stats: {
    total_patients: 247,
    pending_reviews: 12,
    active_alerts: { high: 3, moderate: 8, low: 156 },
  },
  priority_alerts: [
    {
      id: "alt-1",
      patient_id: "pat-001",
      patient_name: "Rajan a/l Kumar",
      alert_type: "high_severity_interaction",
      severity: "high" as const,
      message: "Jamu Herbal + Metformin: Hepatotoxic risk. STOP immediately.",
      generated_at: "2026-09-16T10:30:03+08:00",
    },
    {
      id: "alt-2",
      patient_id: "pat-003",
      patient_name: "Ahmad bin Ismail",
      alert_type: "high_severity_interaction",
      severity: "high" as const,
      message: "Supplement X + Warfarin: Bleeding risk.",
      generated_at: "2026-09-12T14:00:00+08:00",
    },
    {
      id: "alt-3",
      patient_id: "pat-002",
      patient_name: "Priya d/o Ravi",
      alert_type: "wellness_score_drop",
      severity: "moderate" as const,
      message: "Wellness score dropped 18 points.",
      generated_at: "2026-09-14T09:00:00+08:00",
    },
  ],
  patients: [
    {
      id: "pat-001",
      full_name: "Rajan a/l Kumar",
      age: 58,
      last_visit_date: "2026-09-15",
      wellness_score: 72,
      wellness_score_prev: 58,
      alert_count: { high: 2, moderate: 0, low: 0 },
      ai_summary_line: "DM2, HTN, 3 supplements — 1 high-risk interaction",
    },
    {
      id: "pat-002",
      full_name: "Priya d/o Ravi",
      age: 34,
      last_visit_date: "2026-09-14",
      wellness_score: 85,
      wellness_score_prev: null,
      alert_count: { high: 0, moderate: 0, low: 0 },
      ai_summary_line: "Hypothyroid — stable on levothyroxine",
    },
    {
      id: "pat-003",
      full_name: "Ahmad bin Ismail",
      age: 62,
      last_visit_date: "2026-09-12",
      wellness_score: 45,
      wellness_score_prev: 63,
      alert_count: { high: 1, moderate: 0, low: 0 },
      ai_summary_line: "CKD Stage 3, DM2 — declining renal function",
    },
  ],
};

export const MOCK_PATIENT_DETAIL = {
  id: "pat-001",
  full_name: "Rajan a/l Kumar",
  date_of_birth: "1968-01-15",
  sex: "male",
  health_profile: {
    conditions: ["Type 2 Diabetes", "Hypertension"],
    medications: [
      { name: "Metformin", dosage: "500mg", frequency: "Twice daily" },
      { name: "Amlodipine", dosage: "5mg", frequency: "Once daily" },
      { name: "Atorvastatin", dosage: "20mg", frequency: "Once daily" },
    ],
    allergies: ["Penicillin"],
  },
  wellness_score: 72,
  wellness_trend: [
    { date: "2026-07-15", score: 52 },
    { date: "2026-08-15", score: 56 },
    { date: "2026-09-15", score: 58 },
    { date: "2026-09-16", score: 72 },
  ],
  ai_clinical_summary: {
    text: "Rajan, 58M. DM2 (2018). HTN (2019). Metformin 500mg BID, Amlodipine 5mg OD, Atorvastatin 20mg OD. HbA1c: 7.2% (Sep 12, improving from 7.8%). 3 supplements: Vitamin D (safe), Garlic Extract (moderate risk with Amlodipine), Jamu Herbal (HIGH RISK — hepatotoxic). Recommend: discontinue Jamu, order LFT, monitor BP. Score: 72 (up 14).",
    confidence: 0.89,
    model_version: "summarise_v2.1.0",
    generated_at: "2026-09-16T10:30:00+08:00",
  },
  active_alerts: [
    {
      id: "alt-1",
      type: "high_severity_interaction",
      severity: "high" as const,
      message: "Jamu Herbal + Metformin: Hepatotoxic risk. STOP. Order LFT.",
      acknowledged: false,
      generated_at: "2026-09-16T10:30:03+08:00",
    },
  ],
  assessments: [
    {
      id: "asmt-001",
      wellness_score: 72,
      risk_score: 65,
      supplement_count: 3,
      high_interaction_count: 1,
      completed_at: "2026-09-16T10:30:03+08:00",
    },
    {
      id: "asmt-000",
      wellness_score: 58,
      risk_score: 45,
      supplement_count: 3,
      high_interaction_count: 0,
      completed_at: "2026-09-15T14:00:00+08:00",
    },
  ],
  records: [
    {
      id: "rec-001",
      record_type: "lab_report",
      title: "HbA1c Test Results",
      record_date: "2026-09-12",
      ai_summary: "HbA1c: 7.2% (improving from 7.8%).",
      is_shared_with_doctor: true,
    },
  ],
};
```

---

## 7. Build Order

1. `globals.css` + `tailwind.config.js` — tokens + dark mode
2. `components/ui/` — Sidebar, DataTable, Timeline, AlertBanner, EmergencyBanner, AccessTimer, SecureViewer, ChatbotPanel (also reuse Card, Button, Input, Badge, EmptyState from patient component spec)
3. `stores/emergency-store.ts` — Zustand store
4. `mock/doctor-data.ts`
5. `hooks/` — useDoctorDashboard, usePatientDetail, useEmergencyAccess
6. Doctor layout: sidebar + top bar
7. S06 Doctor Dashboard
8. S07 Patient Detail (with Emergency Access flow)
9. S08 Secure Viewer
10. Admin screens (Dashboard → Users → Audit → PDPA → Health)

Dark mode: doctor app defaults to dark. Ensure all components render correctly on `[data-theme="dark"]`.
