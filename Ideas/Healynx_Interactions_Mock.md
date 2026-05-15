# Healynx — Interactions, Modals & Mock Data

**Covers:** All modals/dialogs, loading states, micro-interactions, toasts, empty states, error states, AI content rules, and full mock API response shapes.

---

## Modals & Dialogs

### Emergency Access Modal

Triggered by: Doctor taps [Emergency Access] on patient profile or sidebar.

```
┌──────────────────────────────────────┐
│  🚨 EMERGENCY ACCESS                 │
│                                      │
│  You are requesting unrestricted     │
│  access to Rajan a/l Kumar's full    │
│  medical records.                    │
│                                      │
│  All actions will be logged and      │
│  audited. Patient and admin will     │
│  be notified.                        │
│                                      │
│  Reason for emergency access:*       │
│  ┌──────────────────────────────────┐│
│  │ Patient unconscious — suspected  ││
│  │ drug interaction. ER admission.  ││
│  └──────────────────────────────────┘│
│                                      │
│  Quick reasons:                      │
│  [Unconscious Patient]               │
│  [Life-Threatening Emergency]        │
│  [Severe Allergic Reaction]          │
│  [Acute Psychiatric Crisis]          │
│  [Other — free text]                 │
│                                      │
│  ┌──────────┐ ┌────────────────────┐ │
│  │ [Cancel]  │ │[Confirm Emergency]│ │  Danger-secondary + Danger
│  └──────────┘ └────────────────────┘ │
└──────────────────────────────────────┘
```

- 2px `danger-600` border, `danger-050` header
- Backdrop non-dismissible (must explicitly Cancel or Confirm)
- Quick reasons fill the text field on tap
- [Confirm Emergency] disabled until reason entered
- Desktop: centred modal 560px. Mobile: full-width bottom sheet.

**After confirmation:** Red banner appears at top of screen. Timer starts at 30:00 counting down. All patient records accessible. Audit logged.

**Auto-expiry:** Banner disappears, redirect to patient list, toast: "Emergency access session expired."

**Manual end:** Doctor taps [End Emergency Access] on banner → immediate deactivation.

### Share Configuration Modal

Triggered by: Patient taps "Share" on any record.

```
┌──────────────────────────────────────┐
│  ← Cancel          Share Record      │
│  ────────────────────────────────────│
│                                      │
│  Sharing: HbA1c Test Results (PDF)   │
│                                      │
│  RECIPIENT                           │
│  ┌──────────────────────────────────┐│
│  │ 🔍 Search connected doctors...   ││
│  └──────────────────────────────────┘│
│  ○ Dr. Amira (Primary Care)          │
│  ○ Dr. Wei (Cardiologist)            │
│  ── or ──                            │
│  ┌──────────────────────────────────┐│
│  │ cardiologist@hospital.com        ││
│  └──────────────────────────────────┘│
│                                      │
│  PERMISSIONS                         │
│  ☑ View document              [●──] │  Toggle on (locked)
│  ☐ Allow download             [──○] │  Toggle off
│  ☐ Allow forward              [──○] │  Toggle off
│                                      │
│  EXPIRY                              │
│  ┌──────┬──────┬──────┬────────────┐ │
│  │ 24h  │ 7d ● │ 30d  │ Custom...  │ │  Segmented control
│  └──────┴──────┴──────┴────────────┘ │
│                                      │
│  ACCESS CODE (optional)              │
│  ┌──┐ ┌──┐ ┌──┐ ┌──┐ ┌──┐ ┌──┐      │
│  │ 1│ │ 2│ │ 3│ │ 4│ │ 5│ │ 6│      │
│  └──┘ └──┘ └──┘ └──┘ └──┘ └──┘      │
│                                      │
│  NOTE TO RECIPIENT (optional)        │
│  ┌──────────────────────────────────┐│
│  │ Please review before our         ││
│  │ appointment next week.           ││
│  └──────────────────────────────────┘│
│                                      │
│  ┌──────────────────────────────────┐│
│  │    [Generate Secure Link]        ││  Primary button
│  └──────────────────────────────────┘│
└──────────────────────────────────────┘
```

**Confirmation state (replaces modal content):**
```
       ✅ Link Created!

Recipient: cardiologist@hospital.com
Expires: September 23, 2026 (7 days)
Permissions: View only

Share link:
┌──────────────────────────────────┐
│ healynx.ai/share/xK9mW7qR2t  [📋]│
└──────────────────────────────────┘

[Share via WhatsApp]  [Share via Email]
                  [Done]
```

### Confirmation Dialog

Used for destructive actions (delete record, revoke share, end emergency access).

```
┌──────────────────────────────────────┐
│  ⚠️ Revoke Access?                   │
│                                      │
│  cardiologist@hospital.com will      │
│  lose access to HbA1c Test Results.  │
│  This cannot be undone.              │
│                                      │
│  ┌──────────┐ ┌────────────────────┐ │
│  │ [Cancel]  │ │[Revoke Access]    │ │  Ghost + Danger
│  └──────────┘ └────────────────────┘ │
└──────────────────────────────────────┘
```

### File Upload Action Sheet (Mobile)

Trigger: FAB tap on Records Vault.

```
                    ┌──────────────────────┐
                    │ 📷  Take Photo        │  48px height each
                    │ 🖼️  Choose from Library│
                    │ 📁  Browse Files       │
                    │ ───────────────────── │
                    │ [Cancel]              │
                    └──────────────────────┘
                             ▲
                           [ + ]
```

Backdrop: semi-transparent overlay. Tap backdrop to dismiss.

### Access Code Prompt (Secure Viewer)

```
┌──────────────────────────────────────┐
│  🔒 Enter Access Code                │
│                                      │
│  The document owner has set an       │
│  access code. Enter it to view.      │
│                                      │
│  ┌──┐ ┌──┐ ┌──┐ ┌──┐ ┌──┐ ┌──┐      │
│  │  │ │  │ │  │ │  │ │  │ │  │      │
│  └──┘ └──┘ └──┘ └──┘ └──┘ └──┘      │
│                                      │
│  2 attempts remaining                │
│                                      │
│  ┌──────────────────────────────────┐│
│  │           [Verify]               ││
│  └──────────────────────────────────┘│
└──────────────────────────────────────┘
```

Max 3 attempts → token invalidated → "Access blocked. Contact the document owner."

### Add Supplement Bottom Sheet (Survey Step 1)

Trigger: [+ Add Another Supplement] on survey.

```
┌──────────────────────────────────────┐
│  ─── (drag handle) ────         [✕] │
│  Add Supplement                      │
│  ────────────────────────────────────│
│                                      │
│  Supplement Name*                    │
│  ┌──────────────────────────────────┐│
│  │ 🔍 Search or type name...        ││
│  └──────────────────────────────────┘│
│                                      │
│  Dosage*                             │
│  ┌──────────┐ ┌────────────────────┐ │
│  │   500    │ │  mg           [▼]  │ │
│  └──────────┘ └────────────────────┘ │
│                                      │
│  Frequency*            [Daily   ▼]   │
│  Duration (months)     [12      ▼]   │
│                                      │
│  Health purpose (optional)           │
│  ┌──────────────────────────────────┐│
│  │ Blood pressure support           ││
│  └──────────────────────────────────┘│
│                                      │
│  ┌──────────────────────────────────┐│
│  │        [Add Supplement]          ││
│  └──────────────────────────────────┘│
└──────────────────────────────────────┘
```

---

## Loading States

### Skeleton Screens

Used on all dashboards and lists on initial load. Colour: `neutral-200` rectangles. Shimmer: linear gradient `neutral-200` → `neutral-100` → `neutral-200` sweeping left-to-right, 1.5s infinite.

```
┌──────────────────────────────┐
│  ▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓   ▓▓▓▓▓▓  │  Text lines (70-100% width)
│                              │
│  ┌──────────────────────────┐│
│  │        ▓▓▓▓▓▓▓▓          ││  Wellness gauge skeleton (circle)
│  │        ▓▓▓▓▓▓▓▓          ││
│  └──────────────────────────┘│
│  ┌──────────────────────────┐│
│  │ ▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓ ││  Card skeletons
│  │ ▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓ ││
│  └──────────────────────────┘│
│  ┌──────────────────────────┐│
│  │ ▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓ ││
│  │ ▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓ ││
│  └──────────────────────────┘│
└──────────────────────────────┘
```

### AI Analysis Loading (Survey Submitted)

```
┌──────────────────────────────┐
│                              │
│        ┌──────────┐          │
│        │    🧠    │          │  Brain icon, subtle pulse
│        └──────────┘          │  scale 1.0→1.05→1.0, 1.5s
│                              │
│     Analysing your intake... │
│                              │
│     ████████████░░░░  75%    │  ProgressBar: 6px, rounded-full
│                              │
│  ✓ Validating supplement data│  pending → spinner
│  ✓ Checking drug interactions│  current → pulsing dot
│  → Calculating wellness score│  complete → ✓ checkmark
│  ○ Generating recommendations│
│                              │
│  Estimated time: ~3 seconds  │  Mono font, updates dynamically
└──────────────────────────────┘
```

**Polling behaviour:** Submit → task_id → poll `GET /tasks/{id}` every 500ms. Progress from `progress_percent`. Min display: 1.5s. Max: 15s then timeout. On complete → auto-navigate to results.

### Button Loading

Button text replaced with spinner (Loader2 icon). Width locked. Cursor: not-allowed. Same background, slightly lighter.

### Table Loading

5 skeleton rows, 52px height each. Each cell is a `neutral-200` rectangle with shimmer.

---

## Micro-Interactions

### AI Result Reveal

| Element | Animation | Duration | Easing | Delay |
|---|---|---|---|---|
| Wellness gauge arc | Draw 0° to target angle | 800ms | ease-out | 0ms |
| Score number | Count up 0 to target | 800ms | ease-out | 0ms |
| Supplement cards | Slide right + fade | 300ms | spring | 100ms stagger |
| Interaction alerts | Fade + red glow pulse (high) | 500ms + 3×600ms pulse | ease-out | 400ms |
| Recommendations | Fade sequentially | 200ms | ease-in-out | 50ms stagger |

### Alert Pulse

High severity: border opacity 0.6→1.0, 3 cycles, 600ms each. Moderate: gentler, 2 cycles, 500ms. Low: no pulse.

### Lock Animation

On share/access: lock unlocks (rotate 15°, 300ms) → locks (rotate back, 300ms).

### FAB Expand

Tap: icons slide up with stagger (50ms), backdrop fades (200ms). Dismiss: reverse.

### Toast Enter/Exit

Slide in + fade, 200ms ease-out. Exit: fade out 200ms. Stack with 8px gap.

### Transition Timing

| Transition | Duration | Easing |
|---|---|---|
| Screen push | 250ms | ease-out |
| Modal open | 200ms | ease-out |
| Bottom sheet | 300ms | cubic-bezier(0.32, 0.72, 0, 1) |
| AI result reveal | 500-700ms | cubic-bezier(0.34, 1.56, 0.64, 1) |
| Fade content | 200ms | ease-in-out |
| Dark mode toggle | 200ms | ease-in-out |

**`prefers-reduced-motion`** → ALL animations disabled (0ms). Count-up → static. Spinner → "Loading..." text.

---

## Toasts

| Type | Icon | BG | Border Left | Text | Dismiss |
|---|---|---|---|---|---|
| Success | ✓ | `success-050` | 4px `success-600` | `success-600` | 5s |
| Error | ✕ | `danger-050` | 4px `danger-600` | `danger-600` | Persists |
| Warning | ⚠ | `accent-050` | 4px `accent-600` | `accent-700` | 10s |
| Info | ℹ | `info-100` | 4px `info-600` | `neutral-700` | 5s |

- Padding: 12px horiz, 10px vert. Body-sm 14px.
- Desktop: top-right. Mobile: bottom-centre above TabBar.
- Max 3 stacked, newest on top.

**Toast messages:** Survey submitted → "Analysing your intake...", AI complete → "Wellness score: 72", Record uploaded → "Record encrypted. AI summary generating...", Share link → "Secure link created. Expires in 7 days.", Error → "Something went wrong. Please try again.", Emergency → "Emergency access activated. All actions logged."

---

## Empty States

- **No assessments:** Illustration (pill bottle + sparkle). "Complete your first supplement survey. Get your wellness score and safety insights." CTA: [Start Survey]
- **No records:** Illustration (folder + lock). "Your health vault is empty. Upload a medical record." CTA: [Upload Record]
- **Doctor — No patients:** "No patients assigned yet. Contact your administrator." CTA: [Contact Admin]
- **Doctor — No alerts:** Illustration (shield + check). "All patients current. No active alerts." No CTA (positive state).
- **No search results:** "No results found for '{query}'. Try a different search or clear filters." CTA: [Clear Search]
- **Secure viewer — Expired:** "Access expired. Contact the document owner." CTA: [Request Extension]
- **Secure viewer — Revoked:** "Access revoked by the owner."

---

## Error States

**Inline error card:** `danger-100` bg, `danger-600` border. Title + description + [Retry] button.

**Contexts:**
- Dashboard load fails → "Couldn't load dashboard." [Retry]
- Assessment load fails → "Couldn't load results." [Retry]
- Records load fails → "Couldn't load records." [Retry]
- Upload fails → "Upload failed." [Retry] [Choose Different File]
- Share generation fails → "Couldn't create link." [Retry]
- 404 → "Page not found." [Go to Dashboard]
- 403 → "You don't have permission." [Go to Dashboard]

**Network offline banner:** `accent-100` bg, sticky below emergency banner. "You are offline. Some features unavailable."

**Form validation error:** Red 2px border, `danger-050` bg, error message below input in `danger-600` 14px, linked via `aria-describedby`.

---

## AI Content Rules (Mandatory)

ALL AI-generated content blocks MUST include:

1. **Sparkle icon** (`Sparkles` from Lucide, `secondary-600`, 16px)
2. **"AI-GENERATED" overline** (`overline` font, `secondary-600`, uppercase)
3. **Confidence bar** (4px, `neutral-200` track. Fill: green ≥80%, amber 60-79%, red <60%)
4. **Model version** (`mono-sm` 12px, 60% opacity — e.g. "Model: summarise_v2.1.0")
5. **[Flag Inaccuracy]** link — tertiary button, 14px

AI content without these 5 markers = design bug. Treat as blocking.

---

## Mock API Response Types

Use these TypeScript types for all mock data and API client typing.

```ts
// === Auth ===

interface LoginRequest {
  email: string;
  password: string;
}

interface User {
  id: string;
  email: string;
  role: 'patient' | 'doctor' | 'clinic_admin';
  tenant_id: string;
  full_name: string;
  mfa_enabled: boolean;
}

interface LoginResponse {
  access_token: string;
  token_type: 'Bearer';
  user: User;
}

// === Patient ===

interface PatientDashboard {
  wellness_score: number;
  wellness_score_previous: number | null;
  wellness_trend: { date: string; score: number }[];
  active_supplements: { name: string; dosage: string; frequency: string; safety_status: 'safe'|'review'|'stop'; last_assessment_id: string }[];
  upcoming_reminders: { id: string; type: 'medication'|'supplement'|'survey'; item_name: string; scheduled_time: string }[];
  recent_activity: { type: 'assessment'|'record_upload'|'record_shared'; description: string; timestamp: string; resource_id?: string }[];
  last_assessment_id: string | null;
}

interface SurveySubmission {
  supplements: { name: string; dosage: number; unit: string; frequency: 'daily'|'twice_daily'|'weekly'|'monthly'|'as_needed'; duration_months: number; health_purpose?: string }[];
  health_variables: { conditions: string[]; medications: { name: string; dosage: string; frequency: string }[]; allergies: string[] };
  lifestyle?: { diet?: string; exercise_frequency?: string; sleep_hours?: string; alcohol?: string; smoking?: string; caffeine?: string; water_intake?: string; stress_level?: string };
}

interface AssessmentSubmitResponse {
  assessment_id: string;
  task_id: string;
  status: 'pending';
  estimated_completion_seconds: number;
}

interface TaskStatus {
  task_id: string;
  status: 'pending' | 'processing' | 'completed' | 'failed';
  progress_percent: number;
  result_resource_url?: string;
  error_message?: string;
}

interface Assessment {
  id: string;
  status: 'completed' | 'processing' | 'failed';
  risk_score: number | null;
  benefit_level: 'low' | 'moderate' | 'high' | null;
  patient_facing_output: {
    safe_supplements: { name: string; note: string }[];
    review_needed: { name: string; note: string }[];
    warnings: { name: string; note: string }[];
    wellness_tips: string[];
  } | null;
  wellness_score: number | null;
  model_version: string | null;
  created_at: string;
  completed_at: string | null;
}

interface MedicalRecord {
  id: string;
  record_type: string;
  title: string;
  record_date: string;
  mime_type: string;
  ai_summary: string | null;
  ai_summary_confidence: number | null;
  encryption_status: string;
}

interface ShareRequest {
  recipient_type: 'doctor' | 'email';
  recipient_doctor_id?: string;
  recipient_email?: string;
  permissions: { view: boolean; download: boolean; forward: boolean };
  expiry_hours: number;
  access_code?: string;
  note?: string;
}

interface ShareResponse {
  grant_id: string;
  share_url: string;
  access_token: string;
  expires_at: string;
}

// === Doctor ===

interface DoctorDashboard {
  stats: { total_patients: number; pending_reviews: number; active_alerts: { high: number; moderate: number; low: number } };
  priority_alerts: { id: string; patient_id: string; patient_name: string; alert_type: string; severity: 'low'|'moderate'|'high'; message: string; generated_at: string }[];
  patients: { id: string; full_name: string; age: number; last_visit_date: string|null; wellness_score: number|null; wellness_score_prev: number|null; alert_count: { high:number; moderate:number; low:number }; ai_summary_line: string|null }[];
}

interface PatientDetail {
  id: string;
  full_name: string;
  date_of_birth: string;
  sex: string;
  health_profile: { conditions: string[]; medications: any[]; allergies: string[] };
  wellness_score: number | null;
  wellness_trend: { date: string; score: number }[];
  ai_clinical_summary: { text: string; confidence: number; model_version: string; generated_at: string } | null;
  active_alerts: { id: string; type: string; severity: 'low'|'moderate'|'high'; message: string; acknowledged: boolean; generated_at: string }[];
  assessments: { id: string; wellness_score: number|null; risk_score: number|null; supplement_count: number; high_interaction_count: number; completed_at: string }[];
  records: { id: string; record_type: string; title: string; record_date: string; ai_summary: string|null; is_shared_with_doctor: boolean }[];
}

interface EmergencySessionResponse {
  emergency_session_id: string;
  access_token: string;
  expires_at: string;
  patient_id: string;
}

// === Admin ===

interface AdminDashboard {
  patient_count: number;
  doctor_count: number;
  active_shares: number;
  active_emergency_sessions: number;
  recent_emergency_events: { id: string; doctor_name: string; patient_name: string; reason: string; duration_minutes: number; actions_count: number; activated_at: string }[];
  system_health: { api_uptime_pct: number; ai_engine_status: string; storage_used_gb: number; storage_total_gb: number; api_latency_p95_ms: number; last_backup_at: string };
  pending_pdpa_requests: { data_export: number; data_deletion: number };
}

interface AuditEntry {
  id: string;
  actor: { id: string; name: string; role: string };
  action: string;
  target_type: string;
  target_description: string;
  details: Record<string, unknown>;
  ip_address: string;
  outcome: 'success' | 'failure';
  created_at: string;
}

// === Secure Viewer ===

interface SecureViewerData {
  valid: boolean;
  record: { title: string; record_type: string; blob_url: string; watermark_text: string } | null;
  grant: { recipient: string; permissions: { view: boolean; download: boolean; forward: boolean }; expires_at: string; access_code_required: boolean } | null;
  status: 'valid' | 'expired' | 'revoked' | 'access_code_required';
}
```

---

## Mock Data Fixtures

Use these objects as mock API responses for development. All dates in Malaysian time (UTC+8).

```ts
export const MOCK_PATIENT_DASHBOARD: PatientDashboard = {
  wellness_score: 72,
  wellness_score_previous: 58,
  wellness_trend: [
    { date: '2026-07-15', score: 52 },
    { date: '2026-08-15', score: 56 },
    { date: '2026-09-15', score: 58 },
    { date: '2026-09-16', score: 72 },
  ],
  active_supplements: [
    { name: 'Vitamin D', dosage: '1000 IU', frequency: 'Once daily', safety_status: 'safe', last_assessment_id: 'asmt-001' },
    { name: 'Garlic Extract', dosage: '1000 mg', frequency: 'Once daily', safety_status: 'review', last_assessment_id: 'asmt-001' },
    { name: 'Jamu Herbal', dosage: '2 capsules', frequency: 'Once daily', safety_status: 'stop', last_assessment_id: 'asmt-001' },
  ],
  upcoming_reminders: [
    { id: 'rem-1', type: 'medication', item_name: 'Metformin 500mg', scheduled_time: '08:00' },
    { id: 'rem-2', type: 'survey', item_name: 'Update supplement intake', scheduled_time: '20:00' },
  ],
  recent_activity: [
    { type: 'assessment', description: 'Supplement assessment completed', timestamp: '2026-09-16T10:30:00+08:00', resource_id: 'asmt-001' },
    { type: 'record_upload', description: 'HbA1c Test Results uploaded', timestamp: '2026-09-12T09:15:00+08:00', resource_id: 'rec-001' },
  ],
  last_assessment_id: 'asmt-001',
};

export const MOCK_ASSESSMENT_RESULTS: Assessment = {
  id: 'asmt-001',
  status: 'completed',
  risk_score: 65,
  benefit_level: 'moderate',
  patient_facing_output: {
    safe_supplements: [{ name: 'Vitamin D', note: 'Safe at current dosage. Supports bone health.' }],
    review_needed: [{ name: 'Garlic Extract', note: 'May enhance blood pressure medication effect. Monitor BP. Discuss with your doctor.' }],
    warnings: [{ name: 'Jamu Herbal', note: 'Contains undeclared ingredients. Potential liver interaction risk. STOP use and consult your doctor immediately.' }],
    wellness_tips: ['Consider reducing sodium intake given hypertension diagnosis.', 'Continue regular HbA1c monitoring every 3 months.'],
  },
  wellness_score: 72,
  model_version: 'supp_v2.1.0',
  created_at: '2026-09-16T10:30:00+08:00',
  completed_at: '2026-09-16T10:30:03+08:00',
};

export const MOCK_DOCTOR_DASHBOARD: DoctorDashboard = {
  stats: { total_patients: 247, pending_reviews: 12, active_alerts: { high: 3, moderate: 8, low: 156 } },
  priority_alerts: [
    { id: 'alt-1', patient_id: 'pat-001', patient_name: 'Rajan a/l Kumar', alert_type: 'high_severity_interaction', severity: 'high', message: 'Jamu Herbal + Metformin: Hepatotoxic risk. STOP immediately.', generated_at: '2026-09-16T10:30:03+08:00' },
    { id: 'alt-2', patient_id: 'pat-003', patient_name: 'Ahmad bin Ismail', alert_type: 'high_severity_interaction', severity: 'high', message: 'Supplement X + Warfarin: Bleeding risk.', generated_at: '2026-09-12T14:00:00+08:00' },
    { id: 'alt-3', patient_id: 'pat-002', patient_name: 'Priya d/o Ravi', alert_type: 'wellness_score_drop', severity: 'moderate', message: 'Wellness score dropped 18 points since last assessment.', generated_at: '2026-09-14T09:00:00+08:00' },
  ],
  patients: [
    { id: 'pat-001', full_name: 'Rajan a/l Kumar', age: 58, last_visit_date: '2026-09-15', wellness_score: 72, wellness_score_prev: 58, alert_count: { high: 2, moderate: 0, low: 0 }, ai_summary_line: 'DM2, HTN, 3 supplements — 1 high-risk interaction' },
    { id: 'pat-002', full_name: 'Priya d/o Ravi', age: 34, last_visit_date: '2026-09-14', wellness_score: 85, wellness_score_prev: null, alert_count: { high: 0, moderate: 0, low: 0 }, ai_summary_line: 'Hypothyroid — stable on levothyroxine' },
    { id: 'pat-003', full_name: 'Ahmad bin Ismail', age: 62, last_visit_date: '2026-09-12', wellness_score: 45, wellness_score_prev: 63, alert_count: { high: 1, moderate: 0, low: 0 }, ai_summary_line: 'CKD Stage 3, DM2 — declining renal function' },
  ],
};

export const MOCK_RECORDS: MedicalRecord[] = [
  { id: 'rec-001', record_type: 'lab_report', title: 'HbA1c Test Results', record_date: '2026-09-12', mime_type: 'application/pdf', ai_summary: 'HbA1c: 7.2% (improving from 7.8%). Continue current metformin regimen.', ai_summary_confidence: 0.89, encryption_status: 'AES-256' },
  { id: 'rec-002', record_type: 'clinical_note', title: 'Blood Pressure Log', record_date: '2026-08-28', mime_type: 'application/pdf', ai_summary: 'BP: 128/82. Well-controlled on amlodipine 5mg.', ai_summary_confidence: 0.92, encryption_status: 'AES-256' },
  { id: 'rec-003', record_type: 'assessment', title: 'Supplement Assessment', record_date: '2026-09-16', mime_type: 'application/json', ai_summary: 'Wellness Score: 72. 3 supplements. 1 high-risk interaction detected.', ai_summary_confidence: 0.95, encryption_status: 'AES-256' },
];

export const MOCK_PATIENT_DETAIL: PatientDetail = {
  id: 'pat-001',
  full_name: 'Rajan a/l Kumar',
  date_of_birth: '1968-01-15',
  sex: 'male',
  health_profile: {
    conditions: ['Type 2 Diabetes', 'Hypertension'],
    medications: [
      { name: 'Metformin', dosage: '500mg', frequency: 'Twice daily' },
      { name: 'Amlodipine', dosage: '5mg', frequency: 'Once daily' },
      { name: 'Atorvastatin', dosage: '20mg', frequency: 'Once daily' },
    ],
    allergies: ['Penicillin'],
  },
  wellness_score: 72,
  wellness_trend: [
    { date: '2026-07-15', score: 52 },
    { date: '2026-08-15', score: 56 },
    { date: '2026-09-15', score: 58 },
    { date: '2026-09-16', score: 72 },
  ],
  ai_clinical_summary: {
    text: 'Rajan, 58M. DM2 diagnosed 2018. Hypertension since 2019. Current medications: Metformin 500mg BID, Amlodipine 5mg OD, Atorvastatin 20mg OD. Latest HbA1c: 7.2% (Sep 12, improving from 7.8%). Patient takes 3 supplements: Vitamin D (safe), Garlic Extract (moderate risk with Amlodipine — additive hypotensive effect), Jamu Herbal (HIGH RISK — potential hepatotoxic interaction with metformin). Recommend: discontinue Jamu immediately, order LFT within 7 days, monitor BP, consider reducing garlic extract dose. Wellness Score: 72 (up 14 points).',
    confidence: 0.89,
    model_version: 'summarise_v2.1.0',
    generated_at: '2026-09-16T10:30:00+08:00',
  },
  active_alerts: [
    { id: 'alt-1', type: 'high_severity_interaction', severity: 'high', message: 'Jamu Herbal + Metformin: Hepatotoxic risk. STOP immediately. Order LFT.', acknowledged: false, generated_at: '2026-09-16T10:30:03+08:00' },
    { id: 'alt-2', type: 'moderate_severity_interaction', severity: 'moderate', message: 'Garlic Extract + Amlodipine: Additive hypotensive effect. Monitor BP.', acknowledged: false, generated_at: '2026-09-16T10:30:03+08:00' },
  ],
  assessments: [
    { id: 'asmt-001', wellness_score: 72, risk_score: 65, supplement_count: 3, high_interaction_count: 1, completed_at: '2026-09-16T10:30:03+08:00' },
    { id: 'asmt-000', wellness_score: 58, risk_score: 45, supplement_count: 3, high_interaction_count: 0, completed_at: '2026-09-15T14:00:00+08:00' },
  ],
  records: [
    { id: 'rec-001', record_type: 'lab_report', title: 'HbA1c Test Results', record_date: '2026-09-12', ai_summary: 'HbA1c: 7.2% (improving from 7.8%). Continue metformin regimen.', is_shared_with_doctor: true },
    { id: 'rec-002', record_type: 'clinical_note', title: 'Blood Pressure Log', record_date: '2026-08-28', ai_summary: 'BP: 128/82. Well-controlled.', is_shared_with_doctor: true },
  ],
};
```
