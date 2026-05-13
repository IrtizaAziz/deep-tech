# Claude — Build Healynx Patient Mobile App

You are building the **patient-facing mobile web app** for Healynx, a preventive healthcare platform. This is a responsive web app (PWA) — NOT a native mobile app. It runs in the browser. Primary canvas: **375px**, minimum **320px**.

**Tech stack (fixed — no additions):**
- Next.js 14 (App Router) + React 18 + TypeScript
- Tailwind CSS 3.x with CSS custom properties for all design tokens
- Radix UI: Dialog, Select, Tabs, Switch, Checkbox, RadioGroup, Tooltip, Popover
- TanStack Query 5.x, Zustand 4.x, Recharts 2.x, Lucide React
- React Hook Form + Zod
- `fetch` only (no axios). `Intl.DateTimeFormat` (no date-fns/dayjs).

---

## 1. Design Tokens — Copy into `app/globals.css`

```css
@tailwind base;
@tailwind components;
@tailwind utilities;

:root {
  /* Primary — Deep Teal */
  --nv-primary-900: #0D3B3B; --nv-primary-800: #125C5C; --nv-primary-700: #1A7A7A;
  --nv-primary-600: #1E8C8C; --nv-primary-500: #26A6A6; --nv-primary-400: #3DBEBE;
  --nv-primary-300: #6ED4D4; --nv-primary-200: #A3E5E5; --nv-primary-100: #D4F3F3; --nv-primary-050: #EDFAFA;
  /* Secondary — Soft Blue (AI content) */
  --nv-secondary-700: #1E4D8C; --nv-secondary-600: #2563B8; --nv-secondary-500: #3B82D9;
  --nv-secondary-300: #7EB3F0; --nv-secondary-100: #DCE9FA; --nv-secondary-050: #F0F5FD;
  /* Accent — Warm Amber (alerts) */
  --nv-accent-700: #8C5C1E; --nv-accent-600: #D98C2E; --nv-accent-500: #EDA83E;
  --nv-accent-300: #F4CD84; --nv-accent-100: #FCF0DA; --nv-accent-050: #FEF9F2;
  /* Semantic */
  --nv-success-600: #2D8C4A; --nv-success-100: #D9F2E2; --nv-success-050: #F0FAF4;
  --nv-danger-700: #8C1A1A; --nv-danger-600: #D92D2D; --nv-danger-500: #E84A4A;
  --nv-danger-100: #FCE0E0; --nv-danger-050: #FEF5F5;
  --nv-info-600: #3B82D9; --nv-info-100: #DCE9FA;
  /* Neutrals */
  --nv-neutral-900: #111827; --nv-neutral-800: #1F2937; --nv-neutral-700: #374151;
  --nv-neutral-600: #4B5563; --nv-neutral-500: #6B7280; --nv-neutral-400: #9CA3AF;
  --nv-neutral-300: #D1D5DB; --nv-neutral-200: #E5E7EB; --nv-neutral-150: #EEF0F3;
  --nv-neutral-100: #F3F4F6; --nv-neutral-050: #F9FAFB; --nv-neutral-000: #FFFFFF;
  /* Shadows */
  --nv-shadow-sm: 0 1px 3px 0 rgba(0,0,0,0.08);
  --nv-shadow-md: 0 4px 6px -1px rgba(0,0,0,0.08);
  --nv-shadow-lg: 0 10px 15px -3px rgba(0,0,0,0.08);
  --nv-shadow-focus: 0 0 0 3px rgba(30,140,140,0.25);
}

[data-theme="dark"] {
  --nv-neutral-900: #F9FAFB; --nv-neutral-800: #F3F4F6; --nv-neutral-700: #E5E7EB;
  --nv-neutral-600: #D1D5DB; --nv-neutral-500: #9CA3AF; --nv-neutral-400: #6B7280;
  --nv-neutral-300: #4B5563; --nv-neutral-200: #374151; --nv-neutral-150: #283040;
  --nv-neutral-100: #1F2937; --nv-neutral-050: #111827; --nv-neutral-000: #1F2937;
}
```

Map tokens into `tailwind.config.js` `theme.extend.colors` so `bg-nv-primary-600` works.

---

## 2. Typography

Self-host these fonts via `@font-face` in `globals.css` with `font-display: swap`:

- **Headings:** Plus Jakarta Sans (700 Bold, 600 SemiBold)
- **Body:** Inter (400 Regular, 600 SemiBold)

Type scale (apply as Tailwind classes or `@apply`):
| Use | Size | Weight | Line |
|---|---|---|---|
| Page titles | 32px | 700 | 1.2 |
| Section headings | 24px | 700 | 1.3 |
| Card titles | 20px | 600 | 1.3 |
| Subsections | 18px | 600 | 1.35 |
| Body large (onboarding) | 18px | 400 | 1.6 |
| Body default | 16px | 400 | 1.6 |
| Body small (metadata) | 14px | 400 | 1.5 |
| Caption (timestamps) | 12px | 400 | 1.5 |
| Overline (badges) | 11px | 600 | 1.4, uppercase, letter-spacing 1px |

---

## 3. Layout

Mobile 320–767px: 4-column grid, 16px gutters and margins. All cards full-width, single-column vertical scroll. 16px gaps between cards.

Navigation: Fixed bottom TabBar (56px height + safe-area padding). White bg, 0.5px `neutral-200` top border. 4 tabs: Home 🏠, Supplements 💊, Records 📁, Profile 👤. Active: `primary-600`. Inactive: `neutral-500`. Icons 24px, labels caption 12px.

---

## 4. File Structure to Create

```
app/
├── globals.css
├── layout.tsx
├── page.tsx                            → redirect to /login
├── login/page.tsx
├── register/
│   ├── page.tsx                        → email/phone/password
│   ├── verify/page.tsx                 → OTP
│   ├── consent/page.tsx                → PDPA consent
│   ├── profile/page.tsx                → health profile setup
│   └── onboarding/page.tsx             → first survey + tutorial
├── (patient)/
│   ├── layout.tsx                      → top bar + TabBar + content area
│   ├── dashboard/page.tsx              → S01
│   ├── survey/
│   │   ├── page.tsx                    → S02 (multi-step)
│   │   └── [assessmentId]/page.tsx     → S03 (results)
│   ├── records/
│   │   ├── page.tsx                    → S04 (vault)
│   │   └── [recordId]/
│   │       ├── page.tsx                → record detail
│   │       └── share/page.tsx          → S05 (share config)
│   └── profile/page.tsx
components/
├── ui/
│   ├── button.tsx
│   ├── input.tsx
│   ├── card.tsx
│   ├── bottom-sheet.tsx
│   ├── modal.tsx
│   ├── toast.tsx
│   ├── badge.tsx
│   ├── tab-bar.tsx
│   ├── wellness-gauge.tsx
│   ├── supplement-card.tsx
│   ├── interaction-alert.tsx
│   ├── recommendation-list.tsx
│   ├── ai-summary-card.tsx
│   ├── progress-indicator.tsx
│   ├── file-upload.tsx
│   ├── fab.tsx
│   ├── empty-state.tsx
│   ├── search-bar.tsx
│   ├── avatar.tsx
│   └── loading.tsx
└── survey/
    ├── step-supplements.tsx
    ├── step-health.tsx
    ├── step-lifestyle.tsx
    └── step-review.tsx
lib/
├── api-client.ts
├── cn.ts                               → `export const cn = (...c: any[]) => c.filter(Boolean).join(' ')`
└── validators.ts                       → Zod schemas
stores/
├── auth-store.ts
└── survey-draft-store.ts
hooks/
├── use-dashboard.ts
├── use-survey.ts
└── use-records.ts
mock/
└── data.ts
```

---

## 5. Components — Build in This Order

Each component accepts a TypeScript interface. All colours, spacing, radii use Tailwind classes mapped to tokens. Never hardcode hex or px values.

### 5.1 Button

```tsx
type Variant = 'primary' | 'secondary' | 'danger' | 'danger-secondary' | 'ghost';
type Size = 'sm' | 'md' | 'lg';
interface ButtonProps {
  variant?: Variant; size?: Size; loading?: boolean; fullWidth?: boolean;
  icon?: React.ReactNode; iconPosition?: 'leading' | 'trailing';
}
```

Classes by variant: primary=`bg-nv-primary-600 text-white`, secondary=`border-1.5 border-nv-primary-600 text-nv-primary-600`, danger=`bg-nv-danger-600 text-white`, danger-secondary=`border-1.5 border-nv-danger-600 text-nv-danger-600`, ghost=`text-nv-neutral-600`.

Sizes: sm=`h-8 px-3 text-sm`, md=`h-10 px-4 text-base` (default), lg=`h-12 px-6 text-lg`.

States: disabled=`opacity-40 cursor-not-allowed`, loading=spinner replaces text, focus=`shadow-[var(--nv-shadow-focus)]`. Full-width buttons on mobile (`w-full` below 768px). Icon gap: 8px.

### 5.2 Input

Height: 44px (h-11). Border: 1.5px `neutral-300` (border-nv-neutral-300). Focus: 2px `primary-600` + focus shadow. Error: 2px `danger-600` + `danger-050` bg. Label above in body-sm. Error message below in `danger-600` 14px.

Types: text, email, password (eye toggle icon), search (magnifying glass leading + X clear trailing), textarea (auto-resize 3-10 rows).

### 5.3 Card

Variants: standard=`bg-white border border-nv-neutral-200 shadow-sm`, elevated=`shadow-md`, ai-content=`bg-nv-secondary-050 border border-nv-secondary-300`, alert-amber=`bg-nv-accent-100`, alert-red=`bg-nv-danger-100`. Padding: 16px (p-4). Rounded: 8px (rounded-lg). Header row: icon + title (h5) + optional action button.

### 5.4 Bottom Sheet

Trigger: tap. Slides up from bottom, max 90vh. Drag handle: 32×4px grey bar at top (touch drag to dismiss). Backdrop: `rgba(0,0,0,0.5)`. Use Radix Dialog with custom animation. 300ms slide-up ease-out.

### 5.5 Toast

Variants: success=`bg-nv-success-050 border-l-4 border-nv-success-600 text-nv-success-600`, error=`bg-nv-danger-050 border-l-4 border-nv-danger-600 text-nv-danger-600`, warning=`bg-nv-accent-050 border-l-4 border-nv-accent-600`, info=`bg-nv-info-100 border-l-4 border-nv-info-600`.

Position: bottom-center above TabBar. Icon + message + close. 14px. Auto-dismiss: 5s success/info, 10s warning, persist error. Max 3 stacked.

### 5.6 Badge

Colours: green, amber, red, blue, grey, teal. Small rounded pill: 11px overline font, uppercase. Count badge: red circle 18px, white text, positioned top-right of parent. "99+" for >99.

### 5.7 TabBar (Bottom Navigation)

Fixed bottom, 56px height + `pb-safe` (env safe-area). White bg, border-t-0.5 `neutral-200`. 4 tabs: Home, Supplements, Records, Profile. Each: icon 24px + label 12px. Active: `primary-600`. Inactive: `neutral-500`. Use `next/link` for navigation.

### 5.8 Wellness Gauge

Semi-circular radial gauge (use Recharts or SVG). Score number: 48px Plus Jakarta Sans Bold. Arc colour gradient: 0–40 red, 41–60 amber, 61–80 teal, 81–100 green. Trend arrow: ▲ green (up) or ▼ red (down) + delta number. Qualitative label: "Needs Attention"(0-40), "Fair"(41-60), "Good"(61-80), "Excellent"(81-100).

Props: `{ score: number; previousScore?: number; animated?: boolean }`

Animation: arc draws 0→target (800ms ease-out), number counts up. Skip animations if `prefers-reduced-motion`.

### 5.9 Supplement Card

Per-supplement display. Colour-coded: green card=`bg-nv-success-100 border-nv-success-600/20`, amber=`bg-nv-accent-100 border-nv-accent-600/20`, red=`bg-nv-danger-100 border-nv-danger-600/25`.

Content: supplement name (h5), dosage/frequency (body-sm), safety icon (shield-check/shield-alert/shield-x 20px) + status badge, explanation text (body-sm), evidence line (caption), expandable "Learn More" section.

### 5.10 Interaction Alert Row

Left border: 3px `neutral-400` (low), 3px `accent-600` (moderate), 3px `danger-600` (high). Content: supplement + drug names (h5), severity badge, mechanism description (body-sm), recommendation (body-sm), evidence source (caption).

### 5.11 Recommendation List

Bulleted list. Each row: icon 20px + body text. Icons: ✓ green (continue), ⚡ amber (modify), ✕ red (stop), 💡 blue (consider). 16px row gap.

### 5.12 AI Summary Card

**Mandatory for ALL AI content.**

`bg-nv-secondary-050 border border-nv-secondary-300`. Header: `Sparkles` icon (16px, `secondary-600`) + "AI-GENERATED" overline (11px uppercase, `secondary-600`). Body: summary text (body). Confidence bar: 4px height, `neutral-200` track, fill green ≥80%, amber 60-79%, red <60%. Footer: model version (caption, mono, 60% opacity) + [Flag Inaccuracy] link.

**AI content missing this card = blocking bug.**

### 5.13 Progress Indicator (Multi-Step)

4 circles (24px) connected by 2px lines. Completed: filled `primary-600` + white check. Current: `primary-600` border + white fill + bold number. Upcoming: `neutral-300` border + grey number. Labels below: caption (hide on mobile).

### 5.14 File Upload Zone

Default: dashed 2px `neutral-300` border, centred icon + "Tap to select. PDF, JPEG, PNG, TIFF (Max 25MB)" in body-sm.
Uploading: progress bar (6px, `neutral-200` track, `primary-600` fill) + "Encrypting...".
Complete: ✓ checkmark + "AI summary generating..." text.

### 5.15 FAB (Floating Action Button)

56×56px circle, `primary-600` bg, white + icon (24px), `shadow-md`. Position: `fixed bottom-24 right-6` (above TabBar). On tap: expands to action sheet (Take Photo, Library, Browse Files).

### 5.16 Empty State

Centred: line-art illustration placeholder (h-40 w-40, `neutral-200` bg circle) + title (h3) + description (body, `neutral-600`) + CTA button.

### 5.17 Search Bar

Search icon leading (20px, `neutral-500`), clear ✕ trailing (appears when text entered). Height: 44px. 300ms debounce.

### 5.18 Avatar

32px circle. Photo (`img`, rounded-full) or initials fallback (`primary-600` bg, white text, first char of first+last name).

### 5.19 Loading States

Spinner: `Loader2` from Lucide, `animate-spin`. Sizes: 16, 20, 24, 32, 48px. Skeleton: `neutral-200` rounded rect with shimmer animation (linear gradient sweep, 1.5s infinite). Progress bar: 6px height, `neutral-200` track, `primary-600` fill, `rounded-full`.

---

## 6. Screens — Build in This Order

### S01 — Patient Dashboard

**File:** `app/(patient)/dashboard/page.tsx`

Features a greeting header, a WellnessGauge card (animated on mount), an AI Insight card (AI Summary Card style), a Supplement Tracker card with SupplementCard entries, an Upcoming Reminders card, a Recent Activity timeline, and a fixed TabBar.

**Data source:** `GET /patients/me/dashboard` → type `PatientDashboard` (see mock data below).

**States:**
- Loading: skeleton cards (4 card placeholders with shimmer)
- Empty (first-time): wellness card replaced with "Complete your first supplement survey" CTA, AI insight hidden, supplement tracker hidden
- Error: inline error card "Couldn't load dashboard" + [Retry]

```tsx
// app/(patient)/dashboard/page.tsx
'use client';
import { usePatientDashboard } from '@/hooks/use-dashboard';
import { WellnessGauge } from '@/components/ui/wellness-gauge';
import { AISummaryCard } from '@/components/ui/ai-summary-card';
import { SupplementCard } from '@/components/ui/supplement-card';
import { Card } from '@/components/ui/card';
import { Button } from '@/components/ui/button';
import { Skeleton } from '@/components/ui/loading';

export default function PatientDashboard() {
  const { data, isLoading, isError, refetch } = usePatientDashboard();
  if (isLoading) return <DashboardSkeleton />;
  if (isError) return <ErrorCard onRetry={refetch} />;
  if (!data?.last_assessment_id) return <EmptyDashboard />;

  return (
    <div className="flex flex-col gap-4 px-4 pb-24">
      {/* Greeting */}
      <div className="pt-4">
        <h5 className="text-nv-neutral-600">Good morning, {data.full_name}</h5>
      </div>
      {/* Wellness Gauge */}
      <Card variant="elevated">
        <WellnessGauge score={data.wellness_score} previousScore={data.wellness_score_previous} animated />
      </Card>
      {/* AI Insight */}
      {data.ai_insight && (
        <AISummaryCard summary={data.ai_insight} confidence={data.ai_insight_confidence} modelVersion={data.ai_model_version} />
      )}
      {/* Supplement Tracker */}
      <Card>
        <h3 className="mb-3">Supplement Tracker</h3>
        {data.active_supplements.map(s => <SupplementCard key={s.name} {...s} />)}
        <Button variant="primary" fullWidth className="mt-3" href="/survey">Update Supplement Intake</Button>
      </Card>
      {/* Reminders + Activity — standard Card components */}
    </div>
  );
}
```

### S02 — Supplement Survey (Multi-Step)

**File:** `app/(patient)/survey/page.tsx`

Four steps. Use `ProgressIndicator` at top. Auto-save to localStorage via `survey-draft-store` on every field blur. Pre-populate from prior assessment data. TabBar hidden — step indicator visible instead.

**Step 1 (`step-supplements.tsx`):** List current supplements (name, dosage, frequency — pre-populated from prior). [+ Add Another] opens bottom sheet with form fields. Supplements stored in React Hook Form array field.

**Step 2 (`step-health.tsx`):** Searchable multi-select for conditions, medication list (name + dosage + frequency), allergy list. Pre-populated from patient health profile.

**Step 3 (`step-lifestyle.tsx`):** 8 dropdown selects (diet, exercise, sleep, alcohol, smoking, caffeine, water, stress). All optional with "Prefer not to say".

**Step 4 (`step-review.tsx`):** Summary of steps 1–3 in read-only cards. Required checkbox: "I confirm this information is accurate." [Submit & Analyse] button disabled until checked.

**Zod schemas (`lib/validators.ts`):**
```ts
export const supplementSchema = z.object({
  name: z.string().min(1), dosage: z.number().positive(),
  unit: z.string().min(1), frequency: z.enum(['daily','twice_daily','weekly','monthly','as_needed']),
  duration_months: z.number().positive().max(600), health_purpose: z.string().optional(),
});
export const step1Schema = z.object({ supplements: z.array(supplementSchema).min(1, 'Add at least one supplement') });
```

**On submit:** Call `POST /assessments` → receive `task_id` → navigate to `/survey/[assessmentId]?taskId=xxx`.

### S03 — AI Analysis Loading + Results

**Loading state:** Brain icon pulsing, progress bar (6px, updates from polling), 5 stage indicators (validate → interactions → scoring → recommendations → complete). Poll `GET /tasks/{taskId}` every 500ms. Min display 1.5s. Max 15s.

**Results screen:** When task completed, fetch `GET /assessments/{id}`. Display: WellnessGauge (animated), Supplement Cards (3 — stagger in from right 100ms each), Interaction Alerts (fade + red glow pulse on high), Recommendation List (fade sequentially), [Share with Doctor] button, [Done] button.

**State: No risks** → Interactions section shows "✅ No drug-supplement interactions detected" with green tone.

### S04 — Records Vault

**File:** `app/(patient)/records/page.tsx`

SearchBar at top. Filter chips: All, Lab Reports, Prescriptions, Scans, Assessments. Record cards: icon (type-based), title, record type badge, date, AI summary snippet with sparkle icon, [➤] detail link. FAB bottom-right. Empty state: illustration + "Upload your first record" CTA.

### S05 — Record Share

**File:** `app/(patient)/records/[recordId]/share/page.tsx`

Document info header. Recipient: search connected doctors or email input. Permissions: View (toggle, on, locked), Download (toggle, off), Forward (toggle, off). Expiry: segmented control 24h|7d|30d|Custom. Access code: 6-digit PIN (optional). Note: textarea. [Generate Secure Link] primary button. Confirmation state: ✓ + share link (copyable) + Share via WhatsApp/Email + Done.

### Auth Screens

**Login (`app/login/page.tsx`):** Centred card max 440px. Logo + tagline. Email + password inputs. [Log In] button. [Forgot password?] + [Create account] links.

**Register (`app/register/page.tsx`):** Full name, email, phone (+60 prefix), password (min 8, 1 upper, 1 digit). [Verify & Continue].

**Register — OTP (`app/register/verify/page.tsx`):** 6-digit code input boxes. [Resend] with 30s cooldown.

**Register — Consent (`app/register/consent/page.tsx`):** "What we collect" / "How we protect" / "Your rights" sections. Required checkbox for data processing consent. Optional checkbox for anonymised research. [Agree & Continue] disabled until required box checked.

**Register — Health Profile (`app/register/profile/page.tsx`):** DOB picker, sex dropdown, conditions (multi-select), medications (name + dosage), allergies. [Save & Continue].

**Onboarding (`app/register/onboarding/page.tsx`):** "Complete your first supplement survey" prompt → link to survey. [Skip for now].

---

## 7. Mock Data — Copy into `mock/data.ts`

```ts
export const MOCK_PATIENT_DASHBOARD = {
  full_name: 'Rajan a/l Kumar',
  wellness_score: 72,
  wellness_score_previous: 58,
  wellness_trend: [
    { date: '2026-07-15', score: 52 }, { date: '2026-08-15', score: 56 },
    { date: '2026-09-15', score: 58 }, { date: '2026-09-16', score: 72 },
  ],
  active_supplements: [
    { name: 'Vitamin D', dosage: '1000 IU', frequency: 'Once daily', safety_status: 'safe' as const, last_assessment_id: 'asmt-001' },
    { name: 'Garlic Extract', dosage: '1000 mg', frequency: 'Once daily', safety_status: 'review' as const, last_assessment_id: 'asmt-001' },
    { name: 'Jamu Herbal', dosage: '2 capsules', frequency: 'Once daily', safety_status: 'stop' as const, last_assessment_id: 'asmt-001' },
  ],
  upcoming_reminders: [
    { id: 'rem-1', type: 'medication' as const, item_name: 'Metformin 500mg', scheduled_time: '08:00' },
    { id: 'rem-2', type: 'survey' as const, item_name: 'Update supplement intake', scheduled_time: '20:00' },
  ],
  recent_activity: [
    { type: 'assessment' as const, description: 'Supplement assessment completed', timestamp: '2026-09-16T10:30:00+08:00', resource_id: 'asmt-001' },
    { type: 'record_upload' as const, description: 'HbA1c Test Results uploaded', timestamp: '2026-09-12T09:15:00+08:00', resource_id: 'rec-001' },
  ],
  last_assessment_id: 'asmt-001',
  ai_insight: 'Based on your last assessment, consider reducing garlic extract and discussing your jamu herbal use with Dr. Amira.',
  ai_insight_confidence: 0.89,
  ai_model_version: 'summarise_v2.1.0',
};

export const MOCK_ASSESSMENT_RESULTS = {
  id: 'asmt-001',
  status: 'completed' as const,
  risk_score: 65,
  benefit_level: 'moderate' as const,
  patient_facing_output: {
    safe_supplements: [{ name: 'Vitamin D', note: 'Safe at current dosage. Supports bone health.' }],
    review_needed: [{ name: 'Garlic Extract', note: 'May enhance blood pressure medication effect. Monitor BP.' }],
    warnings: [{ name: 'Jamu Herbal', note: 'Contains undeclared ingredients. Potential liver interaction risk. STOP use and consult your doctor immediately.' }],
    wellness_tips: ['Consider reducing sodium intake given hypertension diagnosis.'],
  },
  wellness_score: 72,
  model_version: 'supp_v2.1.0',
  created_at: '2026-09-16T10:30:00+08:00',
  completed_at: '2026-09-16T10:30:03+08:00',
};

export const MOCK_RECORDS = [
  { id: 'rec-001', record_type: 'lab_report', title: 'HbA1c Test Results', record_date: '2026-09-12', mime_type: 'application/pdf', ai_summary: 'HbA1c: 7.2% (improving from 7.8%).', ai_summary_confidence: 0.89, encryption_status: 'AES-256' },
  { id: 'rec-002', record_type: 'clinical_note', title: 'Blood Pressure Log', record_date: '2026-08-28', mime_type: 'application/pdf', ai_summary: 'BP: 128/82. Well-controlled.', ai_summary_confidence: 0.92, encryption_status: 'AES-256' },
];
```

---

## 8. Build Order

1. `globals.css` — all tokens
2. `tailwind.config.js` — map tokens to Tailwind
3. `lib/cn.ts` — classname merge helper (4 lines)
4. `components/ui/` — build Button, Input, Card, Badge, Loading, EmptyState first (used everywhere)
5. Then: BottomSheet, Toast, TabBar, Avatar, SearchBar, FAB
6. Then: WellnessGauge, SupplementCard, InteractionAlert, RecommendationList, AISummaryCard, ProgressIndicator, FileUpload
7. `mock/data.ts` — copy mock objects
8. `hooks/` — usePatientDashboard, useSubmitSurvey, useTaskPolling, useRecords (hardcode to return mock data initially)
9. `app/(patient)/layout.tsx` — TabBar + content area
10. Build screens: Dashboard → Survey → Results → Records → Share
11. Build auth screens
12. Wire TanStack Query last

Every screen must work at 320px width. All colour-coded info must include icon + text (never colour alone). All touch targets 44×44px min. All AI content must use AISummaryCard with sparkle + confidence + model version.
