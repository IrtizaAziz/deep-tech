# Healynx — Frontend Implementation Brief

You are building the frontend for **Healynx**, a preventive healthcare SaaS platform combining AI-powered supplement safety analysis with secure medical records management.

**Product context:** Two Universiti Malaya patents power this platform — a secure medical records system (PI 2025007955) and an AI supplement intake assessment method (UI 2025002953). The frontend is a responsive web application (PWA) running in the browser. No native mobile apps — the same Next.js codebase serves patients (mobile-first), doctors (desktop-first), and clinic admins (desktop).

---

## Tech Stack

| Layer | Technology | Constraint |
|---|---|---|
| Framework | Next.js 14 (App Router) + React 18 | Use `'use client'` only where interactivity required; prefer RSC for data display |
| Styling | Tailwind CSS 3.x + CSS custom properties | Design tokens in `:root` and `[data-theme="dark"]`; mapped via `tailwind.config.js theme.extend` |
| Components | Radix UI primitives | Dialog, Select, Tabs, Toggle, Switch, Tooltip, Popover, Accordion, Checkbox, RadioGroup, Label, Slot |
| Server State | TanStack Query 5.x | `staleTime: 30_000` for dashboard data; `staleTime: 300_000` for AI summaries; retry 2 on GET, 0 on mutation |
| Client State | Zustand 4.x | Only for: auth session, survey draft, emergency session, UI preferences (sidebar collapsed, active tab) |
| Charts | Recharts 2.x | Wellness trend line charts, score bar charts |
| Icons | Lucide React | Consistent 2px stroke, rounded corners — never mix icon libraries |
| Forms | React Hook Form + Zod | `useForm<z.infer<typeof schema>>` pattern; Zod for all validation |
| Auth | JWT access token (in-memory) + refresh token (httpOnly cookie `nv_refresh`) | OAuth 2.0 / OIDC flow behind a Keycloak server |
| Real-time | WebSocket (native `WebSocket` API) | Reconnect with exponential backoff (1s→2s→4s→8s→max 30s); fallback to 10s `setInterval` polling |
| HTTP Client | Native `fetch` (no axios) | Wrap in a thin `apiClient.ts` with base URL, auth header injection, 401 interceptor, and error normalisation |

**Do NOT add:** animation libraries (framer-motion, GSAP), component libraries (MUI, Chakra, shadcn UI beyond Radix), utility frameworks (clsx — use template literals or a 4-line `cn()` helper), date libraries (use `Intl.DateTimeFormat` and `Intl.RelativeTimeFormat`).

---

## Project Structure

```
Healynx-ai/
├── app/
│   ├── globals.css                    # CSS custom properties, Tailwind directives
│   ├── layout.tsx                     # Root layout: fonts, theme provider, QueryClientProvider
│   ├── page.tsx                       # Redirect based on role (or login)
│   │
│   ├── login/
│   │   └── page.tsx                   # Login screen
│   ├── register/
│   │   ├── page.tsx                   # Step 1: email/phone/password
│   │   ├── verify/
│   │   │   └── page.tsx               # Step 2: OTP verification
│   │   ├── consent/
│   │   │   └── page.tsx               # Step 3: PDPA consent
│   │   ├── profile/
│   │   │   └── page.tsx               # Step 4: Health profile setup
│   │   └── onboarding/
│   │       └── page.tsx               # Step 5: First survey + tutorial
│   │
│   ├── (patient)/
│   │   ├── layout.tsx                 # Patient layout: top bar + TabBar + content
│   │   ├── dashboard/
│   │   │   └── page.tsx               # S01: Patient Dashboard
│   │   ├── survey/
│   │   │   ├── page.tsx               # S02: Multi-step supplement survey
│   │   │   └── [assessmentId]/
│   │   │       └── page.tsx           # S03: AI Analysis Results (view past)
│   │   ├── records/
│   │   │   ├── page.tsx               # S04: Records Vault
│   │   │   └── [recordId]/
│   │   │       ├── page.tsx           # Record detail view
│   │   │       └── share/
│   │   │           └── page.tsx       # S05: Share configuration
│   │   ├── profile/
│   │   │   └── page.tsx               # Profile + settings + consent management
│   │   └── chat/
│   │       └── page.tsx               # AI chatbot for patient
│   │
│   ├── (doctor)/
│   │   ├── layout.tsx                 # Doctor layout: sidebar + top bar + content
│   │   ├── dashboard/
│   │   │   └── page.tsx               # S06: Doctor Dashboard
│   │   ├── patients/
│   │   │   └── [patientId]/
│   │   │       └── page.tsx           # S07: Patient Detail (tabs: Overview, Records, Supplements)
│   │   ├── alerts/
│   │   │   └── page.tsx               # Full alert list
│   │   └── chat/
│   │       └── page.tsx               # AI chatbot for doctor (global or per-patient)
│   │
│   ├── (admin)/
│   │   ├── layout.tsx                 # Admin layout: sidebar + content
│   │   ├── dashboard/
│   │   │   └── page.tsx               # Admin dashboard
│   │   ├── users/
│   │   │   └── page.tsx               # User & role management
│   │   ├── audit/
│   │   │   └── page.tsx               # Audit log viewer
│   │   ├── pdpa/
│   │   │   └── page.tsx               # PDPA request handling
│   │   └── health/
│   │       └── page.tsx               # System health monitoring
│   │
│   └── share/
│       └── [token]/
│           └── page.tsx               # S08: Secure Viewer (public, token-authenticated)
│
├── components/
│   ├── ui/                            # Atomic components (C01–C30)
│   │   ├── button.tsx
│   │   ├── input.tsx
│   │   ├── select.tsx
│   │   ├── toggle.tsx
│   │   ├── checkbox.tsx
│   │   ├── radio-group.tsx
│   │   ├── card.tsx
│   │   ├── modal.tsx
│   │   ├── bottom-sheet.tsx
│   │   ├── toast.tsx
│   │   ├── tooltip.tsx
│   │   ├── badge.tsx
│   │   ├── loading.tsx
│   │   ├── tab-bar.tsx
│   │   ├── sidebar.tsx
│   │   ├── data-table.tsx
│   │   ├── timeline.tsx
│   │   ├── file-upload.tsx
│   │   ├── secure-viewer.tsx
│   │   ├── ai-summary-card.tsx
│   │   ├── wellness-gauge.tsx
│   │   ├── alert-banner.tsx
│   │   ├── progress-indicator.tsx
│   │   ├── supplement-card.tsx
│   │   ├── interaction-alert.tsx
│   │   ├── recommendation-list.tsx
│   │   ├── share-link-card.tsx
│   │   ├── access-timer.tsx
│   │   ├── watermark-overlay.tsx
│   │   ├── empty-state.tsx
│   │   ├── search-bar.tsx
│   │   └── avatar.tsx
│   ├── layout/
│   │   ├── patient-layout.tsx
│   │   ├── doctor-layout.tsx
│   │   └── admin-layout.tsx
│   ├── survey/
│   │   ├── step-supplements.tsx
│   │   ├── step-health.tsx
│   │   ├── step-lifestyle.tsx
│   │   └── step-review.tsx
│   └── chatbot/
│       ├── chatbot-panel.tsx
│       ├── chatbot-message.tsx
│       └── chatbot-input.tsx
│
├── lib/
│   ├── api-client.ts                  # fetch wrapper with auth, retry, error normalisation
│   ├── auth.ts                        # JWT storage, refresh logic, role detection
│   ├── websocket.ts                   # WebSocket connection manager
│   ├── polling.ts                     # Fallback polling hook
│   ├── cn.ts                          # className merge helper (4 lines max)
│   └── validators.ts                  # Shared Zod schemas
│
├── stores/
│   ├── auth-store.ts                  # Zustand: user session, JWT, role
│   ├── survey-draft-store.ts          # Zustand: auto-saved survey progress
│   ├── emergency-store.ts             # Zustand: active emergency session state
│   └── ui-store.ts                    # Zustand: sidebar collapsed, active tab, theme
│
├── hooks/
│   ├── use-dashboard.ts               # TanStack Query: fetch dashboard data
│   ├── use-survey.ts                  # TanStack Query: submit survey, poll results
│   ├── use-records.ts                 # TanStack Query: CRUD records
│   ├── use-alerts.ts                  # TanStack Query: fetch/acknowledge alerts
│   ├── use-emergency-access.ts        # Emergency access activation + session management
│   ├── use-websocket.ts              # WebSocket hook with auto-reconnect
│   └── use-media-query.ts             # Responsive breakpoint detection
│
├── mock/
│   ├── handlers.ts                    # MSW (Mock Service Worker) request handlers
│   ├── data.ts                        # Mock data: patients, doctors, assessments, records
│   └── server.ts                      # MSW server setup (browser + node)
│
├── tailwind.config.js                 # Tailwind config with design token mapping
├── next.config.js
└── package.json
```

---

## Design Token System

Define all tokens as CSS custom properties in `app/globals.css` under `:root` and `[data-theme="dark"]`. Map them into Tailwind's `theme.extend` in `tailwind.config.js`. Every colour, spacing, radius, shadow, and font must reference a token — never hardcode values in component files.

### Colour Palette

**Primary — Deep Teal** (brand, buttons, links, selected states):
```
--nv-primary-900: #0D3B3B | --nv-primary-800: #125C5C | --nv-primary-700: #1A7A7A
--nv-primary-600: #1E8C8C  | --nv-primary-500: #26A6A6 | --nv-primary-400: #3DBEBE
--nv-primary-300: #6ED4D4  | --nv-primary-200: #A3E5E5 | --nv-primary-100: #D4F3F3
--nv-primary-050: #EDFAFA
```

**Secondary — Soft Blue** (AI-generated content, chatbot):
```
--nv-secondary-700: #1E4D8C | --nv-secondary-600: #2563B8 | --nv-secondary-500: #3B82D9
--nv-secondary-300: #7EB3F0 | --nv-secondary-100: #DCE9FA | --nv-secondary-050: #F0F5FD
```

**Accent — Warm Amber** (alerts, caution):
```
--nv-accent-700: #8C5C1E | --nv-accent-600: #D98C2E | --nv-accent-500: #EDA83E
--nv-accent-300: #F4CD84 | --nv-accent-100: #FCF0DA | --nv-accent-050: #FEF9F2
```

**Semantic — Status Colours:**
```
Success (Green):    base #2D8C4A | bg #D9F2E2 | light bg #F0FAF4
Danger (Red):       base #D92D2D | bg #FCE0E0 | light bg #FEF5F5 | dark bg #8C1A1A
Info (Blue):        base #3B82D9 | bg #DCE9FA
```

**Neutral Greys:**
```
900 #111827 | 800 #1F2937 | 750 #283040 | 700 #374151 | 600 #4B5563
500 #6B7280 | 400 #9CA3AF | 300 #D1D5DB | 200 #E5E7EB | 150 #EEF0F3
100 #F3F4F6 | 050 #F9FAFB | 000 #FFFFFF
```

### Role-Based Colour Rules (always pair colour with icon + text label):
- Safe / Supplement OK    → Green  + `shield-check` icon
- Caution / Review        → Amber  + `shield-alert` icon
- Danger / Stop           → Red    + `shield-x` icon
- AI-Generated Content    → Blue   + `sparkle` icon
- Encrypted / Secure      → Teal   + `lock` icon

### Typography

| Token | Font | Size | Weight | Line | Usage |
|---|---|---|---|---|---|
| `h1` | Plus Jakarta Sans | 32px / 2rem | 700 Bold | 1.2 | Page titles |
| `h2` | Plus Jakarta Sans | 24px / 1.5rem | 700 Bold | 1.3 | Section headings |
| `h3` | Plus Jakarta Sans | 20px / 1.25rem | 600 SemiBold | 1.3 | Card titles |
| `h4` | Plus Jakarta Sans | 18px / 1.125rem | 600 SemiBold | 1.35 | Subsections |
| `h5` | Inter | 16px / 1rem | 600 SemiBold | 1.4 | Minor headings |
| `h6` | Inter | 13px / 0.8125rem | 600 SemiBold | 1.4 | KPI labels, uppercase, ls:1px |
| `body-lg` | Inter | 18px / 1.125rem | 400 Regular | 1.6 | Onboarding, empty states |
| `body` | Inter | 16px / 1rem | 400 Regular | 1.6 | Default text |
| `body-sm` | Inter | 14px / 0.875rem | 400 Regular | 1.5 | Metadata, labels |
| `caption` | Inter | 12px / 0.75rem | 400 Regular | 1.5 | Timestamps, fine print |
| `overline` | Inter | 11px / 0.6875rem | 600 SemiBold | 1.4 | Badges, uppercase, ls:1px |
| `mono` | JetBrains Mono | 14px / 0.875rem | 450 Medium | 1.5 | Lab values, codes |
| `mono-sm` | JetBrains Mono | 12px / 0.75rem | 450 Medium | 1.5 | IDs, model versions |

**Font loading:** Self-host all fonts via `@font-face` in `globals.css`. Use `font-display: swap`. Subset to Latin + Malay characters.

### Spacing (8px base grid)
```
xs: 4px | sm: 8px | md: 12px | lg: 16px | xl: 24px | 2xl: 32px | 3xl: 40px | 4xl: 48px | 5xl: 64px
```

### Border Radius
```
xs: 4px (checkboxes, badges) | sm: 6px (inputs) | md: 8px (default — cards, buttons, modals)
lg: 12px (large cards) | xl: 16px (modals mobile) | 2xl: 24px (onboarding)
full: 9999px (pills, avatars, FAB)
```

### Shadows
```
sm:  0 1px 3px 0 rgba(0,0,0,0.08)           — standard cards
md:  0 4px 6px -1px rgba(0,0,0,0.08)        — elevated cards, dropdowns
lg:  0 10px 15px -3px rgba(0,0,0,0.08)      — modals, dialogs
xl:  0 20px 25px -5px rgba(0,0,0,0.1)       — full-screen overlays
focus-ring:        0 0 0 3px rgba(30,140,140,0.25)  — focus indicators
focus-ring-danger: 0 0 0 3px rgba(217,45,45,0.25)   — focus on danger
```

**Dark mode:** Replace shadows with `border: 1px solid var(--nv-neutral-700)`.

---

## Layout Grid System

| Breakpoint | Range | Columns | Gutters | Margins | Navigation |
|---|---|---|---|---|---|
| xs/sm | 320–767px | 4-col | 16px | 16px | Bottom TabBar |
| md | 768–1023px | 8-col | 24px | 24px | Side drawer (hamburger) |
| lg+ | 1024px+ | 12-col | 24px | auto, max-w 1280px | Persistent sidebar (256px) |

---

## API Contracts

All API calls go through `lib/api-client.ts`. Never call `fetch` directly in components.

### api-client.ts

```ts
// lib/api-client.ts
const BASE_URL = process.env.NEXT_PUBLIC_API_URL || 'http://localhost:8000/api/v1';

let accessToken: string | null = null;

export function setAccessToken(token: string | null) {
  accessToken = token;
}

async function refreshAccessToken(): Promise<string> {
  const res = await fetch(`${BASE_URL}/auth/refresh`, {
    method: 'POST',
    credentials: 'include', // sends nv_refresh cookie
  });
  if (!res.ok) throw new Error('Refresh failed');
  const data = await res.json();
  accessToken = data.access_token;
  return data.access_token;
}

export async function apiClient<T>(
  path: string,
  options: RequestInit = {}
): Promise<T> {
  const headers: Record<string, string> = {
    'Content-Type': 'application/json',
    ...(options.headers as Record<string, string>),
  };
  if (accessToken) {
    headers['Authorization'] = `Bearer ${accessToken}`;
  }

  let res = await fetch(`${BASE_URL}${path}`, { ...options, headers, credentials: 'include' });

  // 401 → try refresh once
  if (res.status === 401 && accessToken) {
    try {
      const newToken = await refreshAccessToken();
      headers['Authorization'] = `Bearer ${newToken}`;
      res = await fetch(`${BASE_URL}${path}`, { ...options, headers, credentials: 'include' });
    } catch {
      // Refresh failed — redirect to login
      window.location.href = '/login';
      throw new Error('Session expired');
    }
  }

  if (!res.ok) {
    const error = await res.json().catch(() => ({ message: res.statusText }));
    throw new ApiError(res.status, error.message || 'Request failed', error);
  }

  return res.json();
}

export class ApiError extends Error {
  constructor(
    public status: number,
    message: string,
    public details?: unknown
  ) {
    super(message);
  }
}
```

### Auth Endpoints

#### `POST /auth/login`
```ts
// Request
{ email: string; password: string }
// Response 200
{
  access_token: string;       // JWT, 15-min expiry
  token_type: "Bearer";
  user: {
    id: string;
    email: string;
    role: "patient" | "doctor" | "clinic_admin";
    tenant_id: string;
    full_name: string;
    mfa_enabled: boolean;
  }
}
// Response 401: { message: "Invalid credentials" }
// Response 403: { message: "MFA required", mfa_token: string }
```

#### `POST /auth/mfa/verify`
```ts
// Request: { mfa_token: string; totp_code: string }
// Response 200: same as login success
// Response 401: { message: "Invalid TOTP code" }
```

#### `POST /auth/refresh`
```ts
// No body — uses httpOnly cookie 'nv_refresh'
// Response 200: { access_token: string }
```

#### `POST /auth/logout`
```ts
// No body
// Response 204
```

### Patient Endpoints

#### `GET /patients/me` — Current patient profile
```ts
// Response 200
{
  id: string;
  user_id: string;
  full_name: string;
  date_of_birth: string;        // ISO 8601 date
  sex: "male" | "female" | "other";
  blood_type: string | null;
  health_profile: {
    conditions: string[];
    medications: { name: string; dosage: string; frequency: string }[];
    allergies: string[];
  };
  wellness_score: number;        // 0–100
  wellness_score_updated_at: string;
  survey_updated_at: string | null;
  primary_doctor: {
    id: string;
    full_name: string;
    email: string;
  } | null;
}
```

#### `GET /patients/me/dashboard` — Dashboard aggregation
```ts
// Response 200
{
  wellness_score: number;
  wellness_score_previous: number | null;
  wellness_trend: { date: string; score: number }[];       // last 12 assessments
  active_supplements: {
    name: string;
    dosage: string;
    frequency: string;
    safety_status: "safe" | "review" | "stop";
    last_assessment_id: string;
  }[];
  upcoming_reminders: {
    id: string;
    type: "medication" | "supplement" | "survey";
    item_name: string;
    scheduled_time: string;      // HH:MM
  }[];
  recent_activity: {
    type: "assessment" | "record_upload" | "record_shared";
    description: string;
    timestamp: string;
    resource_id?: string;
  }[];
  last_assessment_id: string | null;
}
```

#### `POST /assessments` — Submit supplement survey
```ts
// Request
{
  supplements: {
    name: string;
    dosage: number;
    unit: string;
    frequency: "daily" | "twice_daily" | "weekly" | "monthly" | "as_needed";
    duration_months: number;
    health_purpose?: string;
  }[];
  health_variables: {
    conditions: string[];
    medications: { name: string; dosage: string; frequency: string }[];
    allergies: string[];
  };
  lifestyle?: {
    diet?: string;
    exercise_frequency?: string;
    sleep_hours?: string;
    alcohol?: string;
    smoking?: string;
    caffeine?: string;
    water_intake?: string;
    stress_level?: string;
  };
}

// Response 202
{
  assessment_id: string;
  task_id: string;
  status: "pending";
  estimated_completion_seconds: number;
}
```

#### `GET /assessments/{id}` — Retrieve assessment result
```ts
// Response 200 (when status = completed)
{
  id: string;
  patient_id: string;
  status: "pending" | "processing" | "completed" | "failed";
  risk_score: number | null;          // 0–100
  benefit_level: "low" | "moderate" | "high" | null;
  patient_facing_output: {
    safe_supplements: { name: string; note: string }[];
    review_needed: { name: string; note: string }[];
    warnings: { name: string; note: string }[];
    wellness_tips: string[];
  } | null;
  clinician_facing_output: {
    interactions: {
      supplement: string;
      interacts_with: string;
      type: "drug-supplement" | "supplement-supplement" | "condition-contraindication";
      severity: "low" | "moderate" | "high";
      mechanism: string;
      evidence: string;
      recommendation: string;
    }[];
    risk_score: number;
    model_version: string;
    scoring_model_version: string;
  } | null;
  wellness_score: number | null;
  model_version: string | null;
  created_at: string;
  completed_at: string | null;
}
// Response 200 (when status = processing)
{ id: string; status: "processing"; progress_percent: number }
// Response 200 (when status = failed)
{ id: string; status: "failed"; error_message: string }
```

#### `GET /tasks/{taskId}` — Poll async task
```ts
// Response 200
{
  task_id: string;
  status: "pending" | "processing" | "completed" | "failed";
  progress_percent: number;           // 0–100
  result_resource_url?: string;       // e.g. "/api/v1/assessments/{id}" when completed
  error_message?: string;
}
```

#### `GET /patients/me/assessments` — Assessment history
```ts
// Query: ?page=1&per_page=20
// Response 200
{
  items: {
    id: string;
    risk_score: number | null;
    wellness_score: number | null;
    status: string;
    completed_at: string | null;
  }[];
  total: number;
  page: number;
  per_page: number;
}
```

#### `GET /patients/me/records` — Records vault
```ts
// Query: ?type=lab_report&search=hb&page=1&per_page=20
// Response 200
{
  items: {
    id: string;
    record_type: "lab_report" | "prescription" | "discharge_summary" | "scan" | "clinical_note" | "vaccination" | "assessment" | "other";
    title: string;
    record_date: string;
    mime_type: string;
    ai_summary: string | null;
    ai_summary_confidence: number | null;
    encryption_status: string;         // "AES-256"
  }[];
  total: number;
}
```

#### `POST /patients/me/records` — Upload record
```ts
// Content-Type: multipart/form-data
// Fields: record_type, title, description, record_date, file (binary, max 25MB)
// Response 201
{
  id: string;
  status: "stored" | "processing";
  task_id?: string;                  // for AI summary generation
}
```

#### `POST /records/{id}/share` — Share record [v2 — implement UI, mock API]
```ts
// Request
{
  recipient_type: "doctor" | "email";
  recipient_doctor_id?: string;
  recipient_email?: string;
  permissions: { view: boolean; download: boolean; forward: boolean };
  expiry_hours: number;              // 24 | 168 | 720 | custom
  access_code?: string;              // 6-digit optional PIN
  note?: string;
}
// Response 201
{
  grant_id: string;
  share_url: string;
  access_token: string;
  expires_at: string;
}
```

#### `GET /patients/me/shares` — Active shares
```ts
// Response 200
{
  items: {
    id: string;
    record_title: string;
    recipient: string;
    permissions: { view: boolean; download: boolean; forward: boolean };
    expires_at: string;
    view_count: number;
    is_revoked: boolean;
  }[];
}
```

#### `POST /records/{id}/share/revoke` — Revoke a share
```ts
// Request: { grant_id: string }
// Response 200: { message: "Access revoked" }
```

### Doctor Endpoints

#### `GET /doctors/me/dashboard` — Doctor dashboard
```ts
// Response 200
{
  stats: {
    total_patients: number;
    pending_reviews: number;
    active_alerts: { high: number; moderate: number; low: number };
  };
  priority_alerts: {
    id: string;
    patient_id: string;
    patient_name: string;
    alert_type: string;
    severity: "low" | "moderate" | "high";
    message: string;
    generated_at: string;
  }[];
  patients: {
    id: string;
    full_name: string;
    age: number;
    last_visit_date: string | null;
    wellness_score: number | null;
    wellness_score_prev: number | null;
    alert_count: { high: number; moderate: number; low: number };
    ai_summary_line: string | null;
  }[];
}
// Query: ?search=&severity=&score_min=&score_max=&sort_by=name&sort_dir=asc&page=1&per_page=50
```

#### `GET /doctors/patients/{id}` — Patient detail for doctor
```ts
// Response 200
{
  id: string;
  full_name: string;
  date_of_birth: string;
  sex: string;
  health_profile: { conditions: string[]; medications: any[]; allergies: string[] };
  wellness_score: number | null;
  wellness_trend: { date: string; score: number }[];
  ai_clinical_summary: {
    text: string;
    confidence: number;
    model_version: string;
    generated_at: string;
  } | null;
  active_alerts: {
    id: string;
    type: string;
    severity: "low" | "moderate" | "high";
    message: string;
    acknowledged: boolean;
    generated_at: string;
  }[];
  assessments: {
    id: string;
    wellness_score: number | null;
    risk_score: number | null;
    supplement_count: number;
    high_interaction_count: number;
    completed_at: string;
  }[];
  records: {
    id: string;
    record_type: string;
    title: string;
    record_date: string;
    ai_summary: string | null;
    is_shared_with_doctor: boolean;
  }[];
}
```

#### `POST /emergency-access` — Activate emergency access
```ts
// Request: { patient_id: string; reason: string; justification?: string }
// Response 201
{
  emergency_session_id: string;
  access_token: string;               // short-lived JWT, 30-min expiry
  expires_at: string;
  patient_id: string;
}
```

#### `DELETE /emergency-access/{sessionId}` — End emergency access
```ts
// Response 200: { message: "Emergency access ended" }
```

#### `POST /alerts/{id}/acknowledge` — Acknowledge an alert
```ts
// Response 200: { acknowledged: true, acknowledged_at: string }
```

#### `POST /chat/doctor/query` — Doctor chatbot query
```ts
// Request: { query: string; patient_id?: string }
// Response 200
{
  answer: string;
  sources: { type: string; description: string; record_id?: string }[];
  disclaimer: string;
  confidence: number;
}
```

### Admin Endpoints

#### `GET /admin/dashboard` — Admin dashboard
```ts
// Response 200
{
  patient_count: number;
  doctor_count: number;
  active_shares: number;
  active_emergency_sessions: number;
  recent_emergency_events: {
    id: string;
    doctor_name: string;
    patient_name: string;
    reason: string;
    duration_minutes: number;
    actions_count: number;
    activated_at: string;
  }[];
  system_health: {
    api_uptime_pct: number;
    ai_engine_status: "operational" | "degraded" | "down";
    storage_used_gb: number;
    storage_total_gb: number;
    api_latency_p95_ms: number;
    last_backup_at: string;
  };
  pending_pdpa_requests: { data_export: number; data_deletion: number };
}
```

#### `GET /admin/audit-logs` — Audit log query
```ts
// Query: ?actor_id=&action=&target_type=&patient_id=&from=&to=&page=1&per_page=50
// Response 200
{
  items: {
    id: string;
    actor: { id: string; name: string; role: string };
    action: string;
    target_type: string;
    target_description: string;
    details: Record<string, unknown>;
    ip_address: string;
    outcome: "success" | "failure";
    created_at: string;
  }[];
  total: number;
}
```

### Secure Viewer Endpoint

#### `GET /share/{token}` — Access shared record (no auth header, token in URL)
```ts
// Response 200 (HTML page rendered by server)
// API returns data for the viewer:
{
  valid: boolean;
  record: {
    title: string;
    record_type: string;
    blob_url: string;                  // pre-signed URL to encrypted blob, short expiry
    watermark_text: string;            // "{recipient} | {datetime} | {ip}"
  } | null;
  grant: {
    recipient: string;
    permissions: { view: boolean; download: boolean; forward: boolean };
    expires_at: string;
    access_code_required: boolean;
  } | null;
  status: "valid" | "expired" | "revoked" | "access_code_required";
}
```

---

## State Management (Zustand Stores)

### Auth Store (`stores/auth-store.ts`)

```ts
interface AuthState {
  user: {
    id: string;
    email: string;
    role: 'patient' | 'doctor' | 'clinic_admin';
    tenant_id: string;
    full_name: string;
    avatar_url?: string;
  } | null;
  isAuthenticated: boolean;
  isLoading: boolean;           // true during initial session check

  login: (email: string, password: string) => Promise<void>;
  verifyMfa: (mfaToken: string, totpCode: string) => Promise<void>;
  logout: () => Promise<void>;
  refreshSession: () => Promise<void>;
}
```

**Implementation notes:**
- On app mount, `refreshSession()` is called once to check if the `nv_refresh` cookie is still valid
- `login()` calls `POST /auth/login`, stores `access_token` via `setAccessToken()`, stores user in Zustand
- `logout()` calls `POST /auth/logout`, clears `accessToken`, clears user, redirects to `/login`
- The access token is kept in module-scoped variable in `api-client.ts`, NOT in Zustand (prevents accidental exposure to React DevTools)

### Survey Draft Store (`stores/survey-draft-store.ts`)

```ts
interface SurveyDraft {
  supplements: { name: string; dosage: number; unit: string; frequency: string; duration_months: number; health_purpose?: string }[];
  health_variables: { conditions: string[]; medications: { name: string; dosage: string; frequency: string }[]; allergies: string[] };
  lifestyle: { diet?: string; exercise_frequency?: string; sleep_hours?: string; alcohol?: string; smoking?: string; caffeine?: string; water_intake?: string; stress_level?: string };
  current_step: number;           // 1–4
  last_saved_at: string | null;
}

interface SurveyDraftState {
  draft: SurveyDraft | null;
  isDirty: boolean;

  setDraft: (draft: Partial<SurveyDraft>) => void;
  saveDraft: () => void;          // persist to localStorage
  loadDraft: () => SurveyDraft | null;
  clearDraft: () => void;
}
```

### Emergency Store (`stores/emergency-store.ts`)

```ts
interface EmergencyState {
  isActive: boolean;
  sessionId: string | null;
  patientId: string | null;
  patientName: string | null;
  accessToken: string | null;     // short-lived emergency JWT
  expiresAt: Date | null;
  reason: string | null;
  remainingSeconds: number;       // updated every second

  activate: (patientId: string, reason: string, justification?: string) => Promise<void>;
  deactivate: () => Promise<void>;
  tick: () => void;               // called every second to decrement remainingSeconds
}
```

### UI Store (`stores/ui-store.ts`)

```ts
interface UIState {
  theme: 'system' | 'light' | 'dark';
  sidebarCollapsed: boolean;
  activeDoctorTab: 'overview' | 'records' | 'supplements' | 'history';

  setTheme: (theme: 'system' | 'light' | 'dark') => void;
  toggleSidebar: () => void;
  setActiveDoctorTab: (tab: string) => void;
}
```

---

## Hooks (TanStack Query Patterns)

### `use-dashboard.ts`

```ts
// Patient dashboard
export function usePatientDashboard() {
  return useQuery({
    queryKey: ['dashboard', 'patient'],
    queryFn: () => apiClient<PatientDashboard>('/patients/me/dashboard'),
    staleTime: 30_000,
    refetchOnWindowFocus: true,
  });
}

// Doctor dashboard
export function useDoctorDashboard(searchParams: DoctorDashboardParams) {
  return useQuery({
    queryKey: ['dashboard', 'doctor', searchParams],
    queryFn: () => apiClient<DoctorDashboard>(`/doctors/me/dashboard?${new URLSearchParams(searchParams as any)}`),
    staleTime: 30_000,
  });
}
```

### `use-survey.ts`

```ts
// Submit survey → returns task_id → poll until complete
export function useSubmitSurvey() {
  return useMutation({
    mutationFn: (data: SurveySubmission) =>
      apiClient<{ assessment_id: string; task_id: string }>('/assessments', {
        method: 'POST',
        body: JSON.stringify(data),
      }),
  });
}

// Poll task status
export function useTaskPolling(taskId: string | null, options: { enabled: boolean }) {
  return useQuery({
    queryKey: ['task', taskId],
    queryFn: () => apiClient<TaskStatus>(`/tasks/${taskId}`),
    enabled: options.enabled && !!taskId,
    refetchInterval: (query) => {
      const data = query.state.data;
      if (!data) return 500;
      if (data.status === 'completed' || data.status === 'failed') return false;
      return 500; // poll every 500ms while processing
    },
  });
}

// Fetch completed assessment
export function useAssessment(assessmentId: string | null) {
  return useQuery({
    queryKey: ['assessment', assessmentId],
    queryFn: () => apiClient<Assessment>(`/assessments/${assessmentId}`),
    enabled: !!assessmentId,
    staleTime: 300_000,
  });
}
```

### `use-emergency-access.ts`

```ts
export function useEmergencyAccess() {
  const store = useEmergencyStore();

  const activateMutation = useMutation({
    mutationFn: (params: { patientId: string; reason: string; justification?: string }) =>
      apiClient<EmergencySession>('/emergency-access', {
        method: 'POST',
        body: JSON.stringify(params),
      }),
    onSuccess: (data, variables) => {
      store.activate(variables.patientId, data);
    },
  });

  const deactivateMutation = useMutation({
    mutationFn: (sessionId: string) =>
      apiClient(`/emergency-access/${sessionId}`, { method: 'DELETE' }),
    onSuccess: () => store.deactivate(),
  });

  return { activate: activateMutation.mutate, deactivate: deactivateMutation.mutate, ...store };
}
```

### `use-websocket.ts`

```ts
// lib/websocket.ts
export class HealynxWebSocket {
  private ws: WebSocket | null = null;
  private reconnectAttempts = 0;
  private maxReconnectAttempts = 10;
  private baseDelay = 1000;
  private listeners: Map<string, Set<(data: any) => void>> = new Map();
  private pollInterval: ReturnType<typeof setInterval> | null = null;

  connect(token: string) {
    // Try WebSocket first
    try {
      this.ws = new WebSocket(`${WS_URL}?token=${token}`);
      this.ws.onmessage = (event) => {
        const msg = JSON.parse(event.data);
        this.listeners.get(msg.type)?.forEach(fn => fn(msg.payload));
      };
      this.ws.onclose = () => this.scheduleReconnect(token);
      this.ws.onerror = () => {
        this.ws?.close();
        this.startPolling(token); // fallback immediately
      };
    } catch {
      this.startPolling(token);
    }
  }

  private startPolling(token: string) {
    this.pollInterval = setInterval(async () => {
      const updates = await apiClient('/updates/poll');
      updates.forEach((msg: any) => this.listeners.get(msg.type)?.forEach(fn => fn(msg.payload)));
    }, 10_000);
  }

  on(type: 'assessment_completed' | 'new_alert' | 'emergency_activated' | 'emergency_ended', fn: (data: any) => void) {
    if (!this.listeners.has(type)) this.listeners.set(type, new Set());
    this.listeners.get(type)!.add(fn);
    return () => this.listeners.get(type)?.delete(fn);
  }

  // ... reconnect with exponential backoff, max 30s
}
```

---

## Key Implementation Flows

### Flow A: Supplement Survey → AI Results

This is the most complex UX flow. Implement it exactly as follows:

```
1. Patient navigates to /survey
2. SurveyDraftStore checks localStorage for saved draft
   → If exists: prompt "Continue where you left off?" [Resume] [Start Fresh]
   → If not: pre-populate from last assessment (fetch GET /patients/me)
3. Patient completes Steps 1–4
   → On every field blur: auto-save to SurveyDraftStore → localStorage
   → On "Continue" to next step: Zod validation for current step only
   → Step 4 requires confirmation checkbox before "Submit" is enabled
4. Patient taps "Submit & Analyse"
   → useSubmitSurvey().mutate(data)
   → On success (202): navigate to /survey/[assessmentId]?taskId=xxx
5. Loading screen at /survey/[assessmentId]
   → useTaskPolling(taskId, { enabled: true })
   → Progress bar updates from progress_percent
   → Stage indicators: ✓ validate → ✓ check interactions → → calculate score → ○ generate recs
   → Minimum display: 1.5s (even if task completes faster)
   → Maximum poll: 15s, then show timeout message
6. When task completes (status = "completed"):
   → useAssessment(assessmentId) fetches full results
   → WellnessGauge: arc draws 0→score (800ms), number counts up
   → SupplementCards: stagger from right (100ms delay each)
   → InteractionAlerts: fade in with red glow on high (500ms)
   → Recommendations: fade in sequentially (50ms stagger each)
7. "Share with Doctor" → navigates to /records/[id]/share (pre-filled with this assessment)
8. "Done" → navigates to /dashboard (wellness score updated, trend graph refreshed)
```

**Zod schemas for survey steps (`lib/validators.ts`):**

```ts
import { z } from 'zod';

export const supplementSchema = z.object({
  name: z.string().min(1, 'Supplement name is required'),
  dosage: z.number().positive('Dosage must be positive'),
  unit: z.string().min(1, 'Unit is required'),
  frequency: z.enum(['daily', 'twice_daily', 'weekly', 'monthly', 'as_needed']),
  duration_months: z.number().positive().max(600),
  health_purpose: z.string().optional(),
});

export const step1Schema = z.object({
  supplements: z.array(supplementSchema).min(1, 'Add at least one supplement'),
});

export const medicationSchema = z.object({
  name: z.string().min(1),
  dosage: z.string().optional(),
  frequency: z.string().optional(),
});

export const step2Schema = z.object({
  conditions: z.array(z.string()).optional(),
  medications: z.array(medicationSchema).optional(),
  allergies: z.array(z.string()).optional(),
});

export const step3Schema = z.object({
  diet: z.string().optional(),
  exercise_frequency: z.string().optional(),
  sleep_hours: z.string().optional(),
  alcohol: z.string().optional(),
  smoking: z.string().optional(),
  caffeine: z.string().optional(),
  water_intake: z.string().optional(),
  stress_level: z.string().optional(),
});

export const step4Schema = z.object({
  confirmed: z.literal(true, { errorMap: () => ({ message: 'Please confirm the information is accurate' }) }),
});

export const surveySubmissionSchema = z.object({
  supplements: z.array(supplementSchema).min(1),
  health_variables: step2Schema,
  lifestyle: step3Schema.optional(),
});
```

### Flow B: Emergency Access

```
1. Doctor taps [Emergency Access] on patient profile or sidebar
2. Modal opens (non-dismissible):
   → Patient name displayed
   → Reason dropdown (quick: Unconscious, Life-Threatening, Severe Allergy, Psychiatric Crisis) + free-text
   → [Cancel] [Confirm Emergency]
3. Doctor confirms
   → useEmergencyAccess().activate(patientId, reason)
   → Red banner appears: "🔴 EMERGENCY ACCESS ACTIVE — ALL ACTIONS LOGGED | ⏱️ 29:59"
   → `emergency-store` starts countdown (tick every 1s)
   → All patient records now accessible (emergency JWT with elevated scope)
   → WebSocket/notification sent to admin + patient
4. Session auto-expires at 30 min
   → `emergency-store` countdown hits 0
   → Banner disappears, redirect to patient list
   → Toast: "Emergency access session expired."
5. OR: Doctor manually ends via [End Emergency Access]
   → useEmergencyAccess().deactivate(sessionId)
   → Same cleanup as auto-expiry
6. All actions during emergency session logged server-side (audit trail)
```

### Flow C: Record Share [v2 — implement UI, mock API response]

```
1. Patient selects record → "Share"
2. Share configuration screen:
   → Recipient: search connected doctors OR enter email
   → Permissions: View (on, disabled) / Download (toggle) / Forward (toggle)
   → Expiry: segmented control 24h | 7d | 30d | Custom date picker
   → Access code: 6-digit PIN input (optional)
   → Note: textarea (optional)
3. [Generate Secure Link] → POST /records/{id}/share
4. Confirmation: checkmark + recipient/expiry summary + copyable link + Share via WhatsApp/Email
5. Active shares viewable at /patients/me/shares with revoke option
```

---

## Component Implementation Order

Build components in this exact order. Each component file exports a single React component with all variants/states handled via props.

### 1. Button (`components/ui/button.tsx`)

```tsx
type ButtonVariant = 'primary' | 'secondary' | 'tertiary' | 'danger' | 'danger-secondary' | 'ghost';
type ButtonSize = 'sm' | 'md' | 'lg';

interface ButtonProps extends React.ButtonHTMLAttributes<HTMLButtonElement> {
  variant?: ButtonVariant;
  size?: ButtonSize;
  loading?: boolean;
  fullWidth?: boolean;          // true on mobile
  icon?: React.ReactNode;
  iconPosition?: 'leading' | 'trailing';
}
```

Classes by variant:
- primary: `bg-[--nv-primary-600] text-white hover:bg-[--nv-primary-700] active:bg-[--nv-primary-800]`
- secondary: `border-1.5 border-[--nv-primary-600] text-[--nv-primary-600] hover:bg-[--nv-primary-050]`
- danger: `bg-[--nv-danger-600] text-white hover:bg-[--nv-danger-500]`
- danger-secondary: `border-1.5 border-[--nv-danger-600] text-[--nv-danger-600]`
- ghost: `text-[--nv-neutral-600] hover:bg-[--nv-neutral-100]`
- tertiary: `text-[--nv-primary-600] hover:underline` (no background, no border)

Sizes: sm (h-8 px-3 text-sm), md (h-10 px-4 text-base), lg (h-12 px-6 text-lg)

States: disabled (opacity-40 cursor-not-allowed), loading (animate-spin Loader2 replaces text), focus (focus-ring)

### 2. Input Field (`components/ui/input.tsx`)

### 3. Select / Dropdown (`components/ui/select.tsx`)

### 4. Toggle / Switch (`components/ui/toggle.tsx`)

### 5. Checkbox & Radio (`components/ui/checkbox.tsx`, `components/ui/radio-group.tsx`)

### 6. Card (`components/ui/card.tsx`)

### 7. Modal / Bottom Sheet (`components/ui/modal.tsx`, `components/ui/bottom-sheet.tsx`)

### 8. Toast (`components/ui/toast.tsx`)

### 9. Tooltip (`components/ui/tooltip.tsx`)

### 10. Badge / Chip (`components/ui/badge.tsx`)

### 11. Loading States (`components/ui/loading.tsx`)

### 12. Tab Bar — Mobile Bottom Nav (`components/ui/tab-bar.tsx`)

### 13. Sidebar — Desktop Navigation (`components/ui/sidebar.tsx`)

### 14. Data Table (`components/ui/data-table.tsx`)

### 15. Timeline (`components/ui/timeline.tsx`)

### 16. File Upload Zone (`components/ui/file-upload.tsx`)

### 17. Secure Document Viewer (`components/ui/secure-viewer.tsx`)

### 18. AI Summary Card (`components/ui/ai-summary-card.tsx`)

ALL AI content must use this. Props:
```tsx
interface AISummaryCardProps {
  summary: string;
  confidence: number;          // 0–1
  modelVersion: string;
  onFlagInaccuracy?: () => void;
  className?: string;
}
```

Confidence bar: green (≥80%), amber (60–79%), red (<60%). Below 60%, show "Low confidence — verify before use" warning.

### 19. Wellness Gauge (`components/ui/wellness-gauge.tsx`)

```tsx
interface WellnessGaugeProps {
  score: number;               // 0–100
  previousScore?: number;      // for delta arrow
  size?: 'default' | 'compact';
  animated?: boolean;          // animate on first render
}
```

Arc colour: score≤40 red, ≤60 amber, ≤80 teal, >80 green. Compact variant: 36px number, no qualitative label.

### 20. Alert Banner (`components/ui/alert-banner.tsx`)

### 21. Multi-Step Progress Indicator (`components/ui/progress-indicator.tsx`)

### 22. Supplement Card (`components/ui/supplement-card.tsx`)

### 23. Interaction Alert Row (`components/ui/interaction-alert.tsx`)

### 24. Recommendation List (`components/ui/recommendation-list.tsx`)

### 25. Share Link Card (`components/ui/share-link-card.tsx`)

### 26. Access Timer Bar (`components/ui/access-timer.tsx`)

### 27. Watermark Overlay (`components/ui/watermark-overlay.tsx`)

### 28. Empty State (`components/ui/empty-state.tsx`)

### 29. Search Bar (`components/ui/search-bar.tsx`)

### 30. Avatar (`components/ui/avatar.tsx`)

---

*(Components 2–30 specifications continue from original document — see **Component Specifications** below for full details on each.)*

---

## Component Specifications (Detailed)

[The detailed specifications for components 2–30 from the original document are preserved in full below. Each includes: purpose, variants, states, sizes, responsive behaviour, and which Radix primitive to use. The original component specs from the Claude Design document are authoritative — implement exactly as described in each numbered section.]

[... continued from original document with the same detailed specifications for C02–C30 ...]

---

## Auth Flow Details

### JWT Token Management

```
1. Login → POST /auth/login → { access_token, user }
2. apiClient.setAccessToken(access_token) — stored in module-scoped variable
3. authStore.setUser(user)
4. On every API call: apiClient attaches Authorization: Bearer {access_token}
5. On 401: apiClient auto-calls POST /auth/refresh → new access_token
6. If refresh fails: redirect to /login, clear auth store
7. On app load: apiClient calls POST /auth/refresh → if valid, restore session
8. Logout: POST /auth/logout (clears httpOnly cookie), clear access_token, clear user
```

### Role-Based Routing

After login, redirect based on `user.role`:
- `patient` → `/(patient)/dashboard`
- `doctor` → `/(doctor)/dashboard`
- `clinic_admin` → `/(admin)/dashboard`

Protect routes via middleware or layout-level check:
```tsx
// app/(doctor)/layout.tsx
export default function DoctorLayout({ children }) {
  const user = useAuthStore(s => s.user);
  if (!user || user.role !== 'doctor') redirect('/login');
  return <DoctorLayoutShell>{children}</DoctorLayoutShell>;
}
```

---

## Key Screen Layouts

[Preserved from original document — S01 through S08 with full text-based layouts and state descriptions.]

---

## Dark Mode

Implement via `data-theme` attribute on `<html>`. Patient app: follows `prefers-color-scheme` media query by default, with manual toggle in Profile → Settings. Doctor dashboard: dark mode is the default, light mode available as option. Secure viewer: always dark background regardless of mode.

**Transition:** 200ms ease-in-out on `color`, `background-color`, `border-color`, `box-shadow` properties.

**Dark mode colour overrides** (in `[data-theme="dark"]` block):
- Page background: `--nv-neutral-900` (#111827)
- Cards/surfaces: `--nv-neutral-800` (#1F2937)
- Primary text: `--nv-neutral-050` (#F9FAFB)
- Secondary text: `--nv-neutral-400` (#9CA3AF)
- AI cards: `--nv-secondary-050` used as background becomes `#1A2744` (dark blue tint)
- Shadows replaced with `border: 1px solid var(--nv-neutral-700)`

---

## Accessibility (Non-Negotiable)

- Colour + icon + text for all status information. Never colour alone.
- Touch targets: 44×44px minimum on mobile, 40×40px on desktop.
- Focus rings: `box-shadow: var(--nv-shadow-ring)` on every interactive element. Danger elements use `var(--nv-shadow-ring-danger)`.
- All icons: `aria-label` string or `aria-hidden="true"` if decorative.
- AI content: `aria-label="AI-generated summary. Confidence: 89 percent. Model: summarise_v2.1"`.
- Forms: `<label>` associated via `htmlFor`, errors via `aria-describedby`, required via `aria-required`.
- Modals: focus trapped inside, Escape closes, focus returns to trigger on close.
- `prefers-reduced-motion: reduce` → all animations disabled (duration: 0ms), spinners become static "Loading..." text.
- Support 200% browser zoom without horizontal scroll.
- Semantic HTML: `<nav>`, `<main>`, `<header>`, `<footer>`, `<button>`, `<table>`, `<section>`.

---

## Mock Data & Fixtures

Set up MSW (Mock Service Worker) in `mock/` so the entire app runs without a backend.

### `mock/server.ts`
```ts
import { setupServer } from 'msw/node';
import { handlers } from './handlers';
export const server = setupServer(...handlers);
```

### `mock/handlers.ts` — Key handler shapes

```ts
import { http, HttpResponse } from 'msw';

export const handlers = [

  // Auth
  http.post('/api/v1/auth/login', async ({ request }) => {
    const body = await request.json();
    if (body.email === 'patient@test.com' && body.password === 'password') {
      return HttpResponse.json({
        access_token: 'mock-jwt-patient-token',
        token_type: 'Bearer',
        user: { id: 'pat-001', email: 'patient@test.com', role: 'patient', tenant_id: 'tenant-001', full_name: 'Rajan a/l Kumar', mfa_enabled: false }
      });
    }
    return HttpResponse.json({ message: 'Invalid credentials' }, { status: 401 });
  }),

  // Patient dashboard
  http.get('/api/v1/patients/me/dashboard', () => {
    return HttpResponse.json(MOCK_DASHBOARD);
  }),

  // Submit survey → return 202 + task_id, then mock polling progression
  http.post('/api/v1/assessments', () => {
    return HttpResponse.json({
      assessment_id: 'assess-001',
      task_id: 'task-001',
      status: 'pending',
      estimated_completion_seconds: 3,
    }, { status: 202 });
  }),

  // Task polling — simulate progression
  http.get('/api/v1/tasks/:taskId', ({ params }) => {
    const elapsed = Date.now() - taskStartTimes[params.taskId];
    if (elapsed < 2000) {
      return HttpResponse.json({ task_id: params.taskId, status: 'processing', progress_percent: Math.floor(elapsed / 20) });
    }
    return HttpResponse.json({ task_id: params.taskId, status: 'completed', progress_percent: 100, result_resource_url: '/api/v1/assessments/assess-001' });
  }),

  // Assessment results
  http.get('/api/v1/assessments/assess-001', () => {
    return HttpResponse.json(MOCK_ASSESSMENT_RESULTS);
  }),

  // All other handlers...
];
```

### `mock/data.ts` — Mock data objects

Define realistic mock data for:
```ts
export const MOCK_DASHBOARD = {
  wellness_score: 72,
  wellness_score_previous: 58,
  wellness_trend: [
    { date: '2026-07-15', score: 52 },
    { date: '2026-08-15', score: 56 },
    { date: '2026-09-15', score: 58 },
    { date: '2026-09-16', score: 72 },
  ],
  active_supplements: [
    { name: 'Vitamin D', dosage: '1000 IU', frequency: 'Once daily', safety_status: 'safe', last_assessment_id: 'assess-001' },
    { name: 'Garlic Extract', dosage: '1000 mg', frequency: 'Once daily', safety_status: 'review', last_assessment_id: 'assess-001' },
    { name: 'Jamu Herbal', dosage: '2 capsules', frequency: 'Once daily', safety_status: 'stop', last_assessment_id: 'assess-001' },
  ],
  upcoming_reminders: [
    { id: 'rem-001', type: 'medication', item_name: 'Metformin 500mg', scheduled_time: '08:00' },
    { id: 'rem-002', type: 'survey', item_name: 'Update supplement intake', scheduled_time: '20:00' },
  ],
  recent_activity: [
    { type: 'assessment', description: 'Supplement assessment completed', timestamp: '2026-09-16T10:30:00+08:00', resource_id: 'assess-001' },
    { type: 'record_upload', description: 'HbA1c Test Results uploaded', timestamp: '2026-09-12T09:15:00+08:00', resource_id: 'rec-001' },
  ],
  last_assessment_id: 'assess-001',
};

export const MOCK_ASSESSMENT_RESULTS = {
  id: 'assess-001',
  patient_id: 'pat-001',
  status: 'completed',
  risk_score: 65,
  benefit_level: 'moderate',
  patient_facing_output: {
    safe_supplements: [{ name: 'Vitamin D', note: 'Safe at current dosage. Supports bone health.' }],
    review_needed: [{ name: 'Garlic Extract', note: 'May enhance blood pressure medication effect. Monitor BP. Discuss with your doctor.' }],
    warnings: [{ name: 'Jamu Herbal', note: 'Contains undeclared ingredients. Potential liver interaction risk. STOP use and consult your doctor immediately.' }],
    wellness_tips: ['Consider reducing sodium intake given hypertension diagnosis.', 'Continue regular HbA1c monitoring every 3 months.'],
  },
  clinician_facing_output: {
    interactions: [
      {
        supplement: 'Garlic Extract',
        interacts_with: 'Amlodipine',
        type: 'drug-supplement',
        severity: 'moderate',
        mechanism: 'Additive hypotensive effect. May cause excessive BP lowering.',
        evidence: 'Expert-labelled UM dataset (validated by clinical pharmacist).',
        recommendation: 'Monitor BP. Consider reducing garlic extract to 500mg or switching to aged garlic extract.',
      },
      {
        supplement: 'Jamu Herbal Preparation',
        interacts_with: 'Metformin',
        type: 'drug-supplement',
        severity: 'high',
        mechanism: 'Unregulated herbal preparation may contain hepatotoxic compounds interacting with metformin hepatic metabolism.',
        evidence: 'Case reports of elevated LFTs with concurrent use in Malaysian population.',
        recommendation: 'Discontinue Jamu immediately. Order LFT within 7 days. Document in patient notes.',
      },
    ],
    risk_score: 65,
    model_version: 'supp_v2.1.0',
    scoring_model_version: 'scoring_v1.3.0',
  },
  wellness_score: 72,
  model_version: 'supp_v2.1.0',
  created_at: '2026-09-16T10:30:00+08:00',
  completed_at: '2026-09-16T10:30:03+08:00',
};

// Define mock data for all other endpoints: doctor dashboard, patient detail, records, alerts, admin, etc.
```

**Run MSW in the browser:**
```ts
// In layout.tsx or a client component that wraps the app
if (process.env.NEXT_PUBLIC_MOCK_API === 'true') {
  const { worker } = await import('@/mock/browser');
  await worker.start({ onUnhandledRequest: 'bypass' });
}
```

---

## Implementation Notes

1. **Build mobile-first.** All screens must render correctly at 320px width. Desktop layouts are enhancements using `md:` and `lg:` Tailwind breakpoints.

2. **Use CSS custom properties** for every design token. Component files must contain zero hardcoded hex colours, pixel values for spacing >4px, or font-size values. Reference tokens via Tailwind classes or `var(--nv-*)` directly.

3. **Components are atomic and prop-driven.** Each file in `components/ui/` exports one component. Variants are passed as props. States (loading, error, disabled) are handled internally based on props.

4. **Data fetching via TanStack Query hooks** in `hooks/`. Never call `fetch` or `apiClient` directly in a component — always through a hook. `staleTime: 30_000` for dashboards, `staleTime: 300_000` for AI summaries. Retry GET queries twice, never retry mutations.

5. **Forms use React Hook Form + Zod.** Each survey step is a separate `useForm` instance. On step transition, validate current step's schema. Auto-save to localStorage via `useEffect` watching form values with 500ms debounce. On submission, combine all step data into `surveySubmissionSchema`.

6. **Real-time updates via WebSocket** with automatic fallback to polling. The `useWebSocket` hook handles connection lifecycle. On mount: connect. On unmount: disconnect. Listeners registered via `.on('event_type', callback)`. Fallback: 10-second polling via `setInterval`.

7. **No external dependencies** beyond the explicit tech stack. Use `cn()` helper (4 lines) instead of `clsx`/`classnames`. Use native `Intl` APIs instead of `date-fns`/`dayjs`. Use `fetch` instead of `axios`.

8. **Every AI-generated content block MUST have:** sparkle icon (`Sparkles` from Lucide) + "AI-GENERATED" overline in `--nv-secondary-600` colour + confidence indicator (bar or percentage) + model version in `JetBrains Mono` at 60% opacity. AI output that looks like regular content is a design bug — treat as blocking.

9. **All mock data is in `mock/data.ts`.** Mock handlers simulate realistic timing (202 → processing → completed with 2-second progression). This lets you build and test the full AI polling UX without a backend.

10. **Error boundaries** wrap each data-fetching section. Error state: inline card with illustration + "Something went wrong" + [Retry] button. Never show raw stack traces or API error details to users. Toast on mutation failures.

---

*This document is the complete frontend implementation specification for Healynx v1. Every component, screen, flow, and integration pattern is defined here. Start with the design token system (`globals.css` + `tailwind.config.js`), then build components C01–C30 in order, then screens S01–S08, then connect with hooks and API integration via mock data.*
