# Healynx — UX Application Flow Document

**Document Version:** 1.0  
**Date:** 13 May 2026  
**Author:** UX Design  
**Classification:** Confidential — For Design & Engineering Handoff  
**Reference PRD:** Healynx_AI_PRD.md v1.0  
**Reference TRD:** Healynx_AI_TRD.md v1.0  
**Source IPs:**
| IP | Patent No. | UX Scope |
|---|---|---|
| Interactive Patient Supplement Intake & Disclosure Practice Assessment Method | UI 2025002953 | Survey instrument, AI results display, interaction alerts, wellness score UX |
| Computer-Implemented Method for Secure Management of Medical Records | PI 2025007955 | Record vault, AI summaries, secure viewer, emergency access, sharing, audit |

---

## Table of Contents

1. [Navigation Architecture](#1-navigation-architecture)
2. [Flow 1: Patient — Supplement Assessment & Results](#flow-1-patient--supplement-assessment--results)
3. [Flow 2: Doctor — Clinical Review & Emergency Access](#flow-2-doctor--clinical-review--emergency-access)
4. [Flow 3: Secure Record Sharing & Management](#flow-3-secure-record-sharing--management)
5. [Flow 4: Onboarding & First-Time Setup](#flow-4-onboarding--first-time-setup)
6. [Flow 5: Admin — System Oversight & PDPA Operations](#flow-5-admin--system-oversight--pdpa-operations)
7. [Global Error & Edge Case Patterns](#7-global-error--edge-case-patterns)
8. [Responsive Breakpoints & Layout Strategy](#8-responsive-breakpoints--layout-strategy)
9. [IP Traceability in UX](#9-ip-traceability-in-ux)

---

## 1. Navigation Architecture

### 1.1 Role-Based Navigation Maps

```
┌─────────────────────────────────────────────────────────────────────────┐
│                        Healynx — GLOBAL NAVIGATION                 │
│                                                                          │
│  ┌──────────────────────────────┐                                        │
│  │        LOGIN GATE            │  (All roles enter here)                │
│  │   • Email/Password           │                                        │
│  │   • MFA (TOTP) if enabled    │                                        │
│  │   • Forgot Password          │                                        │
│  │   • Role Detection → Route   │                                        │
│  └─────────────┬────────────────┘                                        │
│                │                                                         │
│    ┌───────────┼───────────┬──────────────────────┐                     │
│    ▼           ▼           ▼                      ▼                     │
│ ┌────────┐ ┌────────┐ ┌──────────────┐ ┌─────────────────────┐         │
│ │PATIENT │ │DOCTOR  │ │CLINIC ADMIN  │ │EMERGENCY ACCESS     │         │
│ │APP     │ │APP     │ │DASHBOARD     │ │(Temporary Override) │         │
│ └────┬───┘ └────┬───┘ └──────┬───────┘ └──────────┬──────────┘         │
│      │          │            │                     │                     │
└──────┼──────────┼────────────┼─────────────────────┼─────────────────────┘
       │          │            │                     │
       ▼          ▼            ▼                     ▼
```

### 1.2 Patient Navigation Bar (Bottom Tab Bar — Mobile)

```
┌─────────────────────────────────────────────────────┐
│ [Dashboard]  [My Records]  [Supplement]  [Profile]   │
│     🏠            📁            💊           👤        │
└─────────────────────────────────────────────────────┘
```

### 1.3 Doctor Navigation Bar (Sidebar — Desktop / Hamburger — Mobile)

```
┌────────────────────────┐
│ 🏠 Dashboard            │
│ 👥 Patient List         │
│ ⚠️ Alerts (3)           │
│ 🤖 AI Chatbot           │
│ 🔒 Emergency Access     │
│ 📊 Reports              │
│ ⚙️ Settings             │
└────────────────────────┘
```

### 1.4 Admin Navigation Bar (Sidebar — Desktop)

```
┌────────────────────────────┐
│ 🏠 Admin Dashboard          │
│ 👥 User & Role Management   │
│ 📋 Audit Logs               │
│ 📤 PDPA Requests            │
│ 🏥 Clinic Settings          │
│ 📊 System Health            │
│ ⚙️ Billing & Subscription    │
└────────────────────────────┘
```

### 1.5 Screen Transition Map

```
┌─────────────────────────────────────────────────────────────────────────┐
│                         SCREEN CONNECTION MAP                            │
│                                                                          │
│                    ┌─────────────────┐                                   │
│                    │   Splash /      │                                   │
│                    │   Welcome       │                                   │
│                    └───────┬─────────┘                                   │
│                            │                                             │
│              ┌─────────────┼─────────────┐                              │
│              ▼             ▼             ▼                              │
│        ┌──────────┐ ┌──────────┐ ┌──────────────┐                       │
│        │  Login   │ │ Register │ │ Forgot Pass   │                       │
│        └────┬─────┘ └────┬─────┘ └──────────────┘                       │
│             │            │                                               │
│             └─────┬──────┘                                               │
│                   ▼                                                      │
│             ┌──────────┐                                                 │
│             │  MFA     │  (if enabled)                                   │
│             └────┬─────┘                                                 │
│                  │                                                       │
│     ┌────────────┼───────────────┬───────────────┐                      │
│     ▼            ▼               ▼               ▼                      │
│ ┌─────────┐ ┌──────────┐ ┌────────────┐ ┌────────────────┐             │
│ │Patient  │ │Onboarding│ │Doctor      │ │Admin           │             │
│ │Dashboard│ │(new user)│ │Dashboard   │ │Dashboard       │             │
│ └────┬────┘ └────┬─────┘ └─────┬──────┘ └───────┬────────┘             │
│      │           │             │                │                       │
│   ┌──┴───────────┴──┐      ┌──┴──────────┐  ┌──┴───────────┐           │
│   │ Supplement      │      │ Patient      │  │ Audit Logs   │           │
│   │ Survey          │      │ Profile      │  │ User Mgmt    │           │
│   │   → Results     │      │   → Chatbot  │  │ PDPA Tools   │           │
│   │                 │      │   → Emergency│  │ System Health│           │
│   │ My Records      │      │   → Alerts   │  └──────────────┘           │
│   │   → Upload      │      └──────────────┘                             │
│   │   → Share       │                                                   │
│   │   → Secure View │                                                   │
│   └─────────────────┘                                                   │
└─────────────────────────────────────────────────────────────────────────┘
```

---

## Flow 1: Patient — Supplement Assessment & Results

**IP Traceability:** IP 2 (UI 2025002953) — Core interactive supplement intake and AI assessment method  
**PRD Traceability:** FR-01, FR-02, FR-03, FR-04, FR-05, FR-06, FR-14, FR-16  
**Actor(s):** Patient, System (AI Engine, Notification Service)

### 1.0 Entry & Exit

| Attribute | Detail |
|---|---|
| **Entry Point** | Patient taps "Update Supplement Intake" widget on Dashboard OR navigates via bottom tab "Supplement" |
| **Exit Point (Happy Path)** | Results screen; patient taps "Done" → returns to Dashboard with updated wellness score and new trend data point |
| **Exit Point (Share Path)** | Patient taps "Share with Doctor" → transitions to Flow 3, Step 4 (Share Configuration) |
| **Exit Point (Abandoned)** | Patient navigates away mid-survey → autosave to draft; draft notification badge appears on Dashboard widget |

### 1.1 Step-by-Step Flow

```
┌──────────────────────────────────────────────────────────────────────────┐
│ FLOW 1: SUPPLEMENT ASSESSMENT — PATIENT JOURNEY                           │
│                                                                           │
│  DASHBOARD        SURVEY                 LOADING          RESULTS         │
│  ┌──────────┐    ┌──────────┐    ┌──────────────┐    ┌──────────────┐   │
│  │Wellness   │    │Screen 1  │    │              │    │Wellness Score│   │
│  │Score: 58  │    │Your      │    │  ANALYSING   │    │     72       │   │
│  │           │    │Supplements│   │              │    │   ⬆ +14     │   │
│  │[Update    │───►│[+ Add]   │───►│  ████████░░  │───►│              │   │
│  │ Intake]   │    │          │    │  (progress)  │    │🟢 Vitamin D  │   │
│  │           │    │          │    │              │    │🟡 Garlic Ext.│   │
│  │Last: Sep  │    │Screen 2  │    │  Checking    │    │🔴 Jamu Herbal│   │
│  │15, 2026   │    │Health &  │    │  interactions│    │              │   │
│  └──────────┘    │Meds      │    │  ...         │    │[Share with   │   │
│                  │          │    │              │    │ Doctor]      │   │
│                  │Screen 3  │    │  ✅ Complete │    │              │   │
│                  │Lifestyle │    └──────────────┘    │[Done]        │   │
│                  │          │                        └──────────────┘   │
│                  │Screen 4  │                                             │
│                  │Review &  │                                             │
│                  │Confirm   │                                             │
│                  └──────────┘                                             │
└──────────────────────────────────────────────────────────────────────────┘
```

---

#### Step 1 — Dashboard: Entry Widget

**Screen:** Patient Dashboard (Home)

```
┌─────────────────────────────────────────┐
│  Healynx              👤 Profile  │
├─────────────────────────────────────────┤
│                                         │
│  Good morning, Rajan         📅 Sep 16  │
│                                         │
│  ┌─────────────────────────────────┐    │
│  │  YOUR WELLNESS SCORE            │    │
│  │                                 │    │
│  │         ┌───────┐               │    │
│  │         │  58   │    ⬆ +2      │    │
│  │         │ /100  │   from last  │    │
│  │         └───────┘   assessment  │    │
│  │  ▓▓▓▓▓▓▓▓▓▓▓░░░░░░░░░░░░░      │    │
│  │  Fair — Room for improvement    │    │
│  └─────────────────────────────────┘    │
│                                         │
│  ┌─────────────────────────────────┐    │
│  │  SUPPLEMENT TRACKER       [➤]   │    │
│  │  Last updated: Sep 15, 2026     │    │
│  │                                 │    │
│  │  Vitamin D   🟢 Safe            │    │
│  │  Garlic Ext  🟡 Review          │    │
│  │  Jamu Herbal 🔴 Warning         │    │
│  │                                 │    │
│  │  [Update Supplement Intake]     │    │
│  └─────────────────────────────────┘    │
│                                         │
│  ┌─────────────────────────────────┐    │
│  │  UPCOMING REMINDERS             │    │
│  │  💊 Metformin 500mg — 8:00 AM   │    │
│  │  📋 Update survey — Due in 5d   │    │
│  └─────────────────────────────────┘    │
│                                         │
│  [🏠]    [📁]    [💊]    [👤]            │
└─────────────────────────────────────────┘
```

**System Actions at Dashboard Load:**
- `[SYSTEM]` Fetch `PatientProfile.wellness_score` from PostgreSQL → Redis cache (5-min TTL)
- `[SYSTEM]` Fetch latest `SupplementAssessment` with status `completed`
- `[SYSTEM]` Fetch active `SupplementInteraction` rows for current supplement regimen
- `[SYSTEM]` Fetch upcoming `Reminder` rows for today
- `[SYSTEM]` Render wellness score trend graph (last N assessments) via Recharts
- `[AUDIT]` Log `dashboard_view` event

**Edge Cases:**
| Case | Behaviour |
|---|---|
| First-time user — no prior assessment | Wellness Score card shows "Complete your first supplement survey" CTA instead of score; supplement tracker widget hidden |
| Assessment in progress (status: `pending`/`processing`) | Widget shows "Analysis in progress — results ready soon" with spinner; tapping navigates to loading screen with task ID |
| Last assessment > 90 days | Widget shows amber "Overdue" badge; push notification sent if reminders enabled (FR-20 trigger) |

**Error States:**
| Error | Handling |
|---|---|
| Network timeout fetching dashboard data | Show skeleton loader cards; each card loads independently; failed cards show "Tap to retry" |
| Wellness score computation failure | Score card shows "—" with "Recalculating..." text; backend retriggers async computation |
| Session expired while loading | Redirect to login with toast: "Session expired. Please log in again." |

---

#### Step 2 — Navigate to Supplement Survey

**Trigger:** Patient taps "Update Supplement Intake" button on Dashboard

**Entry Transition:** Button → Slide-left transition to Survey Screen 1. Dashboard fades to background.

```
┌─────────────────────────────────────────┐
│  ← Back    Supplement Intake    1 of 4  │
├─────────────────────────────────────────┤
│                                         │
│  What supplements are you currently     │
│  taking?                                │
│                                         │
│  ┌─────────────────────────────────┐    │
│  │ Vitamin D                      │    │
│  │ 1000 IU — Once daily           │    │
│  │ For: Bone health               │    │
│  │                        [✏️] [🗑️]│    │
│  └─────────────────────────────────┘    │
│                                         │
│  ┌─────────────────────────────────┐    │
│  │ Garlic Extract                  │    │
│  │ 1000 mg — Once daily           │    │
│  │ For: Blood pressure             │    │
│  │                        [✏️] [🗑️]│    │
│  └─────────────────────────────────┘    │
│                                         │
│  [+ Add Another Supplement]            │
│                                         │
│  ─────────────────────────────────────  │
│  ● Screen 1   ○ Screen 2               │
│  ○ Screen 3   ○ Screen 4               │
│  ─────────────────────────────────────  │
│                                         │
│              [Continue →]               │
└─────────────────────────────────────────┘
```

**System Actions at Survey Load:**
- `[SYSTEM]` Pre-populate with supplements from last completed assessment (if any) — fetched from `SupplementAssessment.survey_responses`
- `[SYSTEM]` Load supplement autocomplete catalog from PostgreSQL → Redis cache (product name, common dosages, units)
- `[SYSTEM]` Check for saved draft in `SurveyDraft` table → if exists, prompt: "You have a saved draft. Continue where you left off?"

**Edge Cases:**
| Case | Behaviour |
|---|---|
| No prior assessment | Empty form; no pre-populated supplements shown |
| Patient taking >10 supplements | Show first 5 with "Show all (N)" expander; scrollable list |
| Supplement name not in catalog | Free-text entry allowed; flagged for admin review to enrich catalog |
| Patient taps Back arrow | Save current entries to `SurveyDraft` (auto-save); show confirmation: "Save progress? You can continue later." |

**Error States:**
| Error | Handling |
|---|---|
| Autocomplete catalog fetch fails | Fallback to free-text input for all supplement names; catalog fetch retried silently on next screen load |
| Draft save fails (network) | Save to localStorage as fallback; sync to server on next successful connection |

---

#### Step 3 — Multi-Step Survey Screens

**Screen 2: Health Conditions & Medications**

```
┌─────────────────────────────────────────┐
│  ← Back    Supplement Intake    2 of 4  │
├─────────────────────────────────────────┤
│                                         │
│  Tell us about your health conditions   │
│  and current medications                │
│                                         │
│  HEALTH CONDITIONS                      │
│  ┌─────────────────────────────────┐    │
│  │ Type 2 Diabetes            [✕] │    │
│  │ Hypertension               [✕] │    │
│  └─────────────────────────────────┘    │
│  [+ Add Condition]                      │
│                                         │
│  CURRENT MEDICATIONS                    │
│  ┌─────────────────────────────────┐    │
│  │ Metformin 500mg — Twice daily  │    │
│  │ Amlodipine 5mg — Once daily    │    │
│  │ Atorvastatin 20mg — Once daily │    │
│  └─────────────────────────────────┘    │
│  [+ Add Medication]                     │
│                                         │
│  ALLERGIES                              │
│  ┌─────────────────────────────────┐    │
│  │ Penicillin                   [✕]│    │
│  └─────────────────────────────────┘    │
│  [+ Add Allergy]                        │
│                                         │
│  ─────────────────────────────────────  │
│  ○ Screen 1   ● Screen 2               │
│  ○ Screen 3   ○ Screen 4               │
│  ─────────────────────────────────────  │
│                                         │
│        [← Previous]   [Continue →]      │
└─────────────────────────────────────────┘
```

**System Actions on Screen 2:**
- `[SYSTEM]` Fetch condition catalog from PostgreSQL (ICD-10 mapped to simplified labels)
- `[SYSTEM]` Fetch medication catalog with autocomplete (generic + brand names)
- `[SYSTEM]` Pre-populate from `PatientProfile.health_profile` JSONB if previously saved

**Edge Cases:**
| Case | Behaviour |
|---|---|
| Patient unsure of medication name | "I don't know the name" toggle → prompts to upload a photo of medication packaging (deferred processing) |
| Condition not in dropdown | Free-text "Other" entry with optional description field |
| Multiple medications for same condition | Allow; no deduplication — AI engine handles polypharmacy assessment |

---

**Screen 3: Lifestyle Factors**

```
┌─────────────────────────────────────────┐
│  ← Back    Supplement Intake    3 of 4  │
├─────────────────────────────────────────┤
│                                         │
│  Lifestyle factors that may affect      │
│  your supplement needs                  │
│                                         │
│  DIET                     [Omnivore  ▼] │
│                                         │
│  EXERCISE FREQUENCY       [2-3x/week ▼] │
│                                         │
│  SLEEP (hours/night)      [6-7 hrs  ▼]  │
│                                         │
│  ALCOHOL CONSUMPTION      [Occasional ▼]│
│                                         │
│  SMOKING STATUS           [Non-smoker ▼]│
│                                         │
│  CAFFEINE INTAKE          [2-3 cups ▼]  │
│                                         │
│  WATER INTAKE (glasses/day) [5-8   ▼]   │
│                                         │
│  STRESS LEVEL (self-rated)  [Moderate ▼]│
│                                         │
│  ─────────────────────────────────────  │
│  ○ Screen 1   ○ Screen 2               │
│  ● Screen 3   ○ Screen 4               │
│  ─────────────────────────────────────  │
│                                         │
│        [← Previous]   [Continue →]      │
└─────────────────────────────────────────┘
```

**System Actions on Screen 3:**
- `[SYSTEM]` Pre-populate from prior assessment's lifestyle factors if available
- None of these fields are mandatory — all have "Prefer not to say" option

**Edge Cases:**
| Case | Behaviour |
|---|---|
| Patient skips all lifestyle fields | Allowed; wellness score calculation uses neutral defaults; recommendation engine notes "limited lifestyle data" |
| Patient values change dramatically from last assessment | Not flagged at survey stage; wellness score trend shows the delta; clinician alert triggered if score drops ≥15 points (FR-20) |

---

**Screen 4: Review & Confirm**

```
┌─────────────────────────────────────────┐
│  ← Back    Supplement Intake    4 of 4  │
├─────────────────────────────────────────┤
│                                         │
│  Review your supplement intake          │
│                                         │
│  ┌─────────────────────────────────┐    │
│  │ SUPPLEMENTS (3)                 │    │
│  │ • Vitamin D — 1000 IU Daily     │    │
│  │ • Garlic Extract — 1000mg Daily │    │
│  │ • Jamu Herbal — 2 caps Daily    │    │
│  └─────────────────────────────────┘    │
│                                         │
│  ┌─────────────────────────────────┐    │
│  │ CONDITIONS (2)                  │    │
│  │ • Type 2 Diabetes               │    │
│  │ • Hypertension                  │    │
│  └─────────────────────────────────┘    │
│                                         │
│  ┌─────────────────────────────────┐    │
│  │ MEDICATIONS (3)                 │    │
│  │ • Metformin 500mg BID           │    │
│  │ • Amlodipine 5mg Daily          │    │
│  │ • Atorvastatin 20mg Daily       │    │
│  └─────────────────────────────────┘    │
│                                         │
│  ┌─────────────────────────────────┐    │
│  │ LIFESTYLE                       │    │
│  │ Diet: Omnivore | Exercise: 2-3x │    │
│  │ Sleep: 6-7h | Alcohol: Occasional│   │
│  └─────────────────────────────────┘    │
│                                         │
│  □ I confirm this information is       │
│    accurate to the best of my knowledge │
│                                         │
│  ─────────────────────────────────────  │
│  ○ Screen 1   ○ Screen 2               │
│  ○ Screen 3   ● Screen 4               │
│  ─────────────────────────────────────  │
│                                         │
│  [← Previous]   [Submit & Analyse →]    │
└─────────────────────────────────────────┘
```

**System Actions on Screen 4:**
- `[SYSTEM]` Client-side validation: confirmation checkbox must be ticked; at least 1 supplement OR 1 condition OR 1 medication must be entered
- `[SYSTEM]` If no changes detected from last submission, show warning: "No changes detected since your last assessment on Sep 15. Submit anyway?"

**Edge Cases:**
| Case | Behaviour |
|---|---|
| Patient taps "Edit" on any section | Navigates back to that screen's step; progress preserved |
| Patient taps Back from Screen 4 | Returns to Screen 3 with data intact |
| Network check before submission | Silent ping to health-check endpoint; if offline, queue submission locally and show: "You're offline. We'll submit when you reconnect." |

**Error States:**
| Error | Handling |
|---|---|
| Confirmation not ticked | Button disabled; shake animation on tap with tooltip: "Please confirm the information is accurate" |
| Zero supplements/conditions/medications entered | Warning dialog: "You haven't added any supplements, conditions, or medications. The assessment will have limited insights. Submit anyway?" — [Submit Anyway] [Go Back] |

---

#### Step 4 — Submission & Loading State

**Screen:** Analysis Progress

```
┌─────────────────────────────────────────┐
│                                         │
│                                         │
│           ┌─────────────┐               │
│           │    🔬       │               │
│           │  (animated  │               │
│           │   icon)     │               │
│           └─────────────┘               │
│                                         │
│        Analysing your intake...         │
│                                         │
│         ████████████░░░░░░  75%         │
│                                         │
│     ✓ Validating supplement data        │
│     ✓ Checking drug interactions        │
│     → Calculating wellness score        │
│     ○ Generating recommendations        │
│                                         │
│     Estimated time: ~3 seconds          │
│                                         │
│                                         │
└─────────────────────────────────────────┘
```

**System Actions (Async Pipeline — IP 2 Dual Engine):**

| Stage | System Action | IP |
|---|---|---|
| 1. Validate | `[SYSTEM]` Pydantic schema validation on survey JSON payload. Reject if mandatory fields missing. Return 422 with field-level errors. | IP 2 |
| 2. Normalise | `[SYSTEM]` Map supplement and medication names to canonical IDs via catalog lookup. Flag unrecognised entries for admin review. | IP 2 |
| 3. AI Algorithm | `[SYSTEM]` Execute Neo4j Cypher query: find all INTERACTS_WITH and CONTRAINDICATED_IN edges for the patient's supplement + medication + condition set. | IP 2 |
| 4. Statistical Model | `[SYSTEM]` Compute risk score = f(interaction_count, max_severity, dosage_ratio, comorbidity_multiplier, polypharmacy_factor). Compute benefit level = g(supplement_evidence, condition_match, dosage_appropriateness). | IP 2 |
| 5. Recommendation Generation | `[SYSTEM]` Execute rule engine: map interaction severity → recommendation template. Call LLM for patient-facing plain-language generation (with guardrails). | IP 2 |
| 6. Wellness Score | `[SYSTEM]` Recalculate wellness score; update `PatientProfile.wellness_score`. | IP 2 |
| 7. Persist | `[SYSTEM]` Write `SupplementAssessment` row (status: completed); write `SupplementInteraction` rows; update `PatientProfile.survey_updated_at`. | IP 2 |
| 8. Alert Check | `[SYSTEM]` If high-severity interactions detected → create `Alert` row for assigned doctor → dispatch WebSocket push to doctor dashboard (FR-05). | IP 2 |
| 9. Audit | `[AUDIT]` Log `assessment_create` event with actor, timestamp, model_version. | IP 1 |

**Loading State UX:**
- Minimum display time: 1.5 seconds (even if backend completes faster — prevents jarring flash)
- Maximum poll time: 15 seconds (if exceeded, show timeout error)
- Poll interval: Client polls `GET /tasks/{task_id}` every 500ms
- Progress bar segments derived from the 5 pipeline stages above
- Cancel not available (once submitted, pipeline runs to completion)

**Edge Cases:**
| Case | Behaviour |
|---|---|
| Task completes in <1.5s | Artificial delay to show completed animation; all checkmarks appear with staggered animation |
| Task takes 3–5 seconds | Progress bar completes naturally; no artificial delay needed |
| Task takes >5 seconds (p95 exceed) | Progress bar stalls at 80%; show: "Taking longer than usual..."; continue polling up to 15s |
| Patient closes app during loading | Task continues server-side; on next app open, Dashboard shows assessment in `processing` state; WebSocket or next poll fetches result; push notification sent on completion |

**Error States:**
| Error | Handling |
|---|---|
| Task timeout (>15 seconds) | Show: "Analysis is taking longer than expected. We'll notify you when results are ready." [Notify Me] [Go to Dashboard]. Results delivered via push notification when ready. |
| AI pipeline failure (exception) | Set assessment status to `failed`; write error to `SupplementAssessment.error_message`; show: "Something went wrong. Our team has been notified. Please try again." [Retry] [Contact Support] |
| Catalog mapping failure (unrecognised supplement name) | Assessment proceeds without mapping for that entry; supplement flagged with ⚠️ "Unverified product — analysis may be limited" on results screen |

---

#### Step 5 — Results Screen

**Screen:** AI Analysis Results

```
┌─────────────────────────────────────────┐
│  ← Dashboard    Assessment Results      │
├─────────────────────────────────────────┤
│                                         │
│  ┌─────────────────────────────────┐    │
│  │  YOUR WELLNESS SCORE            │    │
│  │                                 │    │
│  │         ┌───────┐               │    │
│  │         │  72   │    ⬆ +14     │    │
│  │         │ /100  │   since last  │    │
│  │         └───────┘               │    │
│  │  ▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓░░░░░░  Good │    │
│  └─────────────────────────────────┘    │
│                                         │
│  ┌─────────────────────────────────┐    │
│  │  SUPPLEMENT SAFETY              │    │
│  │                                 │    │
│  │  🟢 Vitamin D                   │    │
│  │  Safe at current dosage.        │    │
│  │  Supports bone health.          │    │
│  │                                 │    │
│  │  🟡 Garlic Extract              │    │
│  │  May enhance blood pressure     │    │
│  │  medication effect. Monitor BP. │    │
│  │  [Learn More ▾]                │    │
│  │                                 │    │
│  │  🔴 Jamu Herbal Preparation     │    │
│  │  ⚠️ Contains undeclared         │    │
│  │  ingredients. Potential liver   │    │
│  │  interaction risk. STOP use and │    │
│  │  consult your doctor.           │    │
│  │  [See Details ▾]               │    │
│  └─────────────────────────────────┘    │
│                                         │
│  ┌─────────────────────────────────┐    │
│  │  DRUG-SUPPLEMENT INTERACTIONS   │    │
│  │                                 │    │
│  │  ⚠️ Garlic Extract + Amlodipine │    │
│  │  Severity: MODERATE             │    │
│  │  Additive hypotensive effect.   │    │
│  │  May cause dizziness.           │    │
│  │                                 │    │
│  │  ⚠️ Jamu + Metformin            │    │
│  │  Severity: HIGH                 │    │
│  │  Potential hepatotoxic effect   │    │
│  │  with unregulated herbal prep.  │    │
│  └─────────────────────────────────┘    │
│                                         │
│  ┌─────────────────────────────────┐    │
│  │  RECOMMENDATIONS                │    │
│  │                                 │    │
│  │  ✓ Continue Vitamin D           │    │
│  │  ⚡ Reduce Garlic Extract to    │    │
│  │    500mg or switch to aged      │    │
│  │    garlic extract               │    │
│  │  ✕ Stop Jamu immediately.       │    │
│  │    Show to your doctor.         │    │
│  │  💡 Consider CoQ10 supplement   │    │
│  │    to offset statin side effects│    │
│  └─────────────────────────────────┘    │
│                                         │
│  [Share with Doctor]    [Done]          │
│                                         │
└─────────────────────────────────────────┘
```

**System Actions at Results Load:**
- `[SYSTEM]` Fetch completed `SupplementAssessment` with `patient_facing_output` JSONB
- `[SYSTEM]` Fetch associated `SupplementInteraction` rows for interaction detail cards
- `[SYSTEM]` Calculate wellness score delta from previous assessment
- `[AUDIT]` Log `assessment_view` event
- `[SYSTEM]` Update `PatientProfile.wellness_score` and `wellness_score_updated_at`

**Interaction Design Notes:**
| Element | Behaviour |
|---|---|
| Wellness Score gauge | Animated fill from previous score to new score (1s easing animation) |
| Supplement cards (Green/Amber/Red) | Expandable: tap to see detailed evidence, mechanism of action, alternative suggestions |
| "Learn More" link | Opens bottom sheet with: mechanism of interaction, clinical evidence summary (plain language), source references |
| "Share with Doctor" button | Primary CTA; navigates to Flow 3 — Share Configuration (pre-populated with this assessment) |
| "Done" button | Returns to Dashboard with updated wellness score; toast: "Assessment saved. Your wellness score is now 72." |

**Edge Cases:**
| Case | Behaviour |
|---|---|
| All supplements green (no risks) | Celebratory message: "All supplements are safe at current dosages. Keep it up!" — still show wellness score and general recommendations |
| Multiple high-severity interactions (>3) | Prominent red banner at top: "⚠️ Multiple high-risk interactions detected. Please consult your doctor before continuing any supplements." |
| First assessment (no prior score) | No delta shown; wellness score card says "Baseline established" instead of trend arrow |
| Supplement flagged as unverified | Yellow tag on supplement card: "Unverified product — our database doesn't recognise this item. Analysis may be incomplete." |

---

#### Step 6 — Share with Doctor (Transition)

If patient taps "Share with Doctor", the flow transitions to **Flow 3, Step 4** (Share Configuration screen), with the current assessment pre-selected as the item to share.

---

#### Step 7 — Return to Dashboard

**Screen:** Updated Dashboard

```
┌─────────────────────────────────────────┐
│  Healynx              👤 Profile  │
├─────────────────────────────────────────┤
│                                         │
│  Good morning, Rajan         📅 Sep 16  │
│                                         │
│  ┌─────────────────────────────────┐    │
│  │  YOUR WELLNESS SCORE 🆕         │    │
│  │                                 │    │
│  │         ┌───────┐               │    │
│  │         │  72   │    ⬆ +14     │    │
│  │         │ /100  │   from Sep 15│    │
│  │         └───────┘               │    │
│  │  ▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓░░░░░░  Good │    │
│  └─────────────────────────────────┘    │
│                                         │
│  ┌─────────────────────────────────┐    │
│  │  WELLNESS TREND                 │    │
│  │                                 │    │
│  │  80│                     ●      │    │
│  │  70│         ●                  │    │
│  │  60│    ●                       │    │
│  │  50│●                          │    │
│  │    └─────────────────────       │    │
│  │    Jul   Aug   Sep   Oct        │    │
│  └─────────────────────────────────┘    │
│                                         │
│  ┌─────────────────────────────────┐    │
│  │  SUPPLEMENT TRACKER       [➤]   │    │
│  │  Updated: Just now              │    │
│  │                                 │    │
│  │  Vitamin D   🟢 Safe            │    │
│  │  Garlic Ext  🟡 Review          │    │
│  │  Jamu Herbal 🔴 Warning  🆕     │    │
│  └─────────────────────────────────┘    │
│                                         │
│  [🏠]    [📁]    [💊]    [👤]            │
└─────────────────────────────────────────┘
```

**System Actions:**
- `[SYSTEM]` Dashboard data refreshed from server (invalidate cache keys for this patient)
- `[SYSTEM]` Recharts trend graph updated with new data point
- `[SYSTEM]` If high-severity alert generated, WebSocket push to primary doctor's dashboard

---

## Flow 2: Doctor — Clinical Review & Emergency Access

**IP Traceability:** IP 1 (PI 2025007955) — AI summarisation, RBAC, smart querying, emergency access, audit logging. IP 2 (UI 2025002953) — Supplement interaction alerts via doctor dashboard.  
**PRD Traceability:** FR-05, FR-09, FR-10, FR-13, FR-18, FR-19, FR-20, FR-21  
**Actor(s):** Doctor, System (AI Engine, Audit Service, Notification Service)

### 2.0 Entry & Exit

| Attribute | Detail |
|---|---|
| **Entry Point** | Doctor logs in successfully → routed to Doctor Dashboard |
| **Exit Point (Normal)** | Doctor completes review, returns to Patient List; alerts acknowledged |
| **Exit Point (Emergency)** | Emergency session expires or doctor manually ends; audit trail updated; admin + patient notified |

### 2.1 Step-by-Step Flow

```
┌──────────────────────────────────────────────────────────────────────────┐
│ FLOW 2: DOCTOR CLINICAL REVIEW & EMERGENCY ACCESS                          │
│                                                                           │
│  DOCTOR DASHBOARD      PATIENT PROFILE       AI RESULTS      EMERGENCY    │
│  ┌──────────────┐     ┌──────────────┐     ┌──────────┐    ┌──────────┐  │
│  │Patient List   │     │AI Summary    │     │Assessment│    │Emergency │  │
│  │🔴 3 High      │     │Timeline      │     │Details   │    │Override  │  │
│  │🟡 8 Moderate  │────►│Supplement    │────►│Interact. │───►│Active    │  │
│  │              │     │History       │     │Recom.     │    │Banner    │  │
│  │Search: [___] │     │Risk Alerts   │     │Acknowledge│    │Audit Log │  │
│  │              │     │Shared Docs   │     │           │    │Auto-expiry│  │
│  │Select Patient│     │AI Chatbot    │     │           │    │          │  │
│  └──────────────┘     └──────────────┘     └──────────┘    └──────────┘  │
└──────────────────────────────────────────────────────────────────────────┘
```

---

#### Step 1 — Doctor Dashboard

**Screen:** Doctor Dashboard

```
┌────────────────────────────────────────────────────────────┐
│  Healynx — Doctor            Dr. Amira   ⚙️ Logout  │
├─────────────┬──────────────────────────────────────────────┤
│ Sidebar     │                                              │
│             │  Good afternoon, Dr. Amira    📅 Sep 16, 2026 │
│ 🏠 Dashboard│                                              │
│ 👥 Patients │  ┌────────────────┬─────────────────────────┐│
│ ⚠️ Alerts(3)│  │ ALERT SUMMARY  │ QUICK ACTIONS           ││
│ 🤖 Chatbot  │  │                │                         ││
│ 🔒 Emergency│  │ 🔴 3 High      │ [New Patient]           ││
│ 📊 Reports  │  │ 🟡 8 Moderate  │ [Emergency Access]      ││
│ ⚙️ Settings │  │ 🟢 156 Low     │ [View All Alerts →]     ││
│             │  └────────────────┴─────────────────────────┘│
│             │                                              │
│             │  Search patients... [________________] 🔍    │
│             │  Filter: [All ▼] [All Alerts ▼] [Any Date ▼]│
│             │                                              │
│             │ ┌──────────────────────────────────────────┐ │
│             │ │ Patient         Last Visit  Score  Alerts │ │
│             │ │──────────────────────────────────────────│ │
│             │ │ Rajan a/l Kumar  Sep 15    72    🔴 2    │ │
│             │ │ DM2, HTN                                       │ │
│             │ │──────────────────────────────────────────│ │
│             │ │ Priya d/o Ravi   Sep 14    85    🟢 0    │ │
│             │ │ Hypothyroid                                    │ │
│             │ │──────────────────────────────────────────│ │
│             │ │ Ahmad bin Ismail Sep 12    45    🔴 1    │ │
│             │ │ CKD Stage 3, DM2                               │ │
│             │ │──────────────────────────────────────────│ │
│             │ │ ...                                        │ │
│             │ └──────────────────────────────────────────┘ │
│             │                          ← 1 2 3 ... 12 →   │
└─────────────┴──────────────────────────────────────────────┘
```

**System Actions at Dashboard Load:**
- `[SYSTEM]` Query patients assigned to this doctor (`PatientProfile.primary_doctor_id = current_user.id`)
- `[SYSTEM]` For each patient, fetch: latest wellness score, unacknowledged alert count (by severity), AI one-line summary (FR-18)
- `[SYSTEM]` Compute alert summary counts across panel
- `[SYSTEM]` Run panel-wide scan if last scan >24 hours ago (FR-20): detect high-severity interactions, wellness score drops ≥15, overdue surveys >90 days; generate `Alert` rows
- `[AUDIT]` Log `dashboard_view` event

**Edge Cases:**
| Case | Behaviour |
|---|---|
| Doctor has 0 assigned patients | Empty state: "No patients assigned yet. Contact your clinic administrator to get started." with CTA to contact admin |
| Doctor has >500 patients | Paginated list (50 per page); search bar with typeahead; indexing on patient name for sub-second search |
| New patient added while dashboard open | WebSocket push updates patient list without full page refresh |

---

#### Step 2 — Patient Profile View

**Screen:** Patient Profile

Doctor selects a patient from the list → navigates to patient profile.

```
┌────────────────────────────────────────────────────────────┐
│  ← Patient List    Rajan a/l Kumar     [Emergency Access]  │
├─────────────┬──────────────────────────────────────────────┤
│             │                                              │
│             │  ┌──────────────────────────────────────┐    │
│             │  │ AI CLINICAL SUMMARY            📋    │    │
│             │  │                                      │    │
│             │  │ Rajan, 58M. DM2 (diagnosed 2018),   │    │
│             │  │ Hypertension (2019). Current meds:   │    │
│             │  │ Metformin 500mg BID, Amlodipine 5mg │    │
│             │  │ OD, Atorvastatin 20mg OD. Latest     │    │
│             │  │ HbA1c: 7.2% (Sep 12, improving from │    │
│             │  │ 7.8%). Patient takes 3 supplements:  │    │
│             │  │ Vitamin D (safe), Garlic Extract     │    │
│             │  │ (moderate risk with Amlodipine),     │    │
│             │  │ Jamu Herbal (HIGH RISK — hepatotoxic │    │
│             │  │ potential). Wellness Score: 72       │    │
│             │  │ (⬆+14). See alert details below.    │    │
│             │  │                                      │    │
│             │  │ Confidence: High (0.89)              │    │
│             │  │ [Flag Inaccuracy]  [Regenerate]      │    │
│             │  └──────────────────────────────────────┘    │
│             │                                              │
│             │  ┌──────────────────────────────────────┐    │
│             │  │ ⚠️  ACTIVE ALERTS (2)                 │    │
│             │  │                                      │    │
│             │  │ 🔴 HIGH — Jamu Herbal + Metformin    │    │
│             │  │ Potential hepatotoxic interaction.   │    │
│             │  │ Recommend immediate discontinuation. │    │
│             │  │                       [Acknowledge]  │    │
│             │  │                                      │    │
│             │  │ 🟡 MODERATE — Garlic Extract +       │    │
│             │  │ Amlodipine. Additive hypotensive     │    │
│             │  │ effect. Monitor BP.                  │    │
│             │  │                       [Acknowledge]  │    │
│             │  └──────────────────────────────────────┘    │
│             │                                              │
│             │  Tabs: [Timeline] [Records] [Assessments]    │
│             │  ┌──────────────────────────────────────┐    │
│             │  │ SUPPLEMENT ASSESSMENT HISTORY         │    │
│             │  │                                      │    │
│             │  │ Sep 16, 2026  Wellness: 72  ⬆+14     │    │
│             │  │ 3 supplements | 🔴 1 interaction     │    │
│             │  │                              [View →]│    │
│             │  │───────────────────────────────────── │    │
│             │  │ Sep 15, 2026  Wellness: 58            │    │
│             │  │ 3 supplements | 🟡 1 interaction     │    │
│             │  │                              [View →]│    │
│             │  │───────────────────────────────────── │    │
│             │  │ ...                                    │    │
│             │  └──────────────────────────────────────┘    │
│             │                                              │
│             │  [🤖 Ask AI Chatbot about this patient]      │
└─────────────┴──────────────────────────────────────────────┘
```

**System Actions:**
- `[SYSTEM]` Fetch AI-generated clinical summary from `MedicalRecord.ai_summary` (most recent) — Redis cached, 5-min TTL (FR-09)
- `[SYSTEM]` Fetch unacknowledged `Alert` rows for this patient
- `[SYSTEM]` Fetch `SupplementAssessment` history ordered by `completed_at DESC`
- `[SYSTEM]` Fetch shared `MedicalRecord` entries via `AccessGrant` where grantee = this doctor
- `[AUDIT]` Log `record_view` event with target = patient profile

**Edge Cases:**
| Case | Behaviour |
|---|---|
| No AI summary generated yet | Summary card shows "No clinical summary available. Upload patient records to generate." with CTA to upload |
| Patient has 0 assessments | Assessment history tab shows "No supplement assessments yet. Encourage patient to complete the intake survey." |
| Patient shared records with this doctor | "Shared Records" section visible with expiry timer and access type |

**Error States:**
| Error | Handling |
|---|---|
| AI summary stale (>30 days, no new records) | Summary shows amber "Outdated" badge; "Regenerate" button prominent |
| Alert acknowledgment fails (network) | Optimistic UI: alert disappears, queued for server sync; if sync fails, reappears with error toast |

---

#### Step 3 — Assessment Detail View

Doctor taps "View" on a specific assessment → full AI results with clinical orientation.

```
┌────────────────────────────────────────────────────────────┐
│  ← Patient Profile           Assessment — Sep 16, 2026     │
├─────────────┬──────────────────────────────────────────────┤
│             │                                              │
│             │  ┌──────────────────────────────────────┐    │
│             │  │ INTERACTION ANALYSIS (Clinician View) │    │
│             │  │                                      │    │
│             │  │ Supplement: Jamu Herbal Preparation   │    │
│             │  │ Interacts With: Metformin             │    │
│             │  │ Type: Drug-Supplement                 │    │
│             │  │ Severity: 🔴 HIGH                     │    │
│             │  │ Mechanism: Unregulated herbal prep    │    │
│             │  │ may contain hepatotoxic compounds     │    │
│             │  │ that interact with metformin's        │    │
│             │  │ hepatic metabolism pathway.           │    │
│             │  │ Evidence: Case reports of elevated    │    │
│             │  │ LFTs with concurrent use.             │    │
│             │  │ (Source: Expert-labelled UM dataset,  │    │
│             │  │  validated by clinical pharmacist)     │    │
│             │  │                                      │    │
│             │  │ Recommendation: Discontinue Jamu.     │    │
│             │  │ Order LFT within 7 days. Document     │    │
│             │  │ in patient notes.                     │    │
│             │  │                                      │    │
│             │  │ [Acknowledge & Add to Record]        │    │
│             │  └──────────────────────────────────────┘    │
│             │                                              │
│             │  ┌──────────────────────────────────────┐    │
│             │  │ RISK SCORE BREAKDOWN                  │    │
│             │  │ Overall: 65/100 (Moderate Risk)       │    │
│             │  │                                      │    │
│             │  │ Interaction Risk:    ████████░░ 72%   │    │
│             │  │ Dosage Risk:         ████░░░░░░ 35%   │    │
│             │  │ Comorbidity Factor:  ██████░░░░ 58%   │    │
│             │  └──────────────────────────────────────┘    │
│             │                                              │
│             │  Model: supp_v2.1.0 | Scoring: v1.3.0        │
│             │                                              │
│             │  [Back to Patient]  [Next Assessment →]      │
└─────────────┴──────────────────────────────────────────────┘
```

**System Actions:**
- `[SYSTEM]` Fetch `clinician_facing_output` from `SupplementAssessment`
- `[SYSTEM]` Fetch `SupplementInteraction` rows for risk score breakdown
- `[AUDIT]` Log `assessment_view` event

**Interaction Design:**
- "Acknowledge & Add to Record" — marks alert as acknowledged, adds the AI recommendation as a clinical note in the patient's record
- Risk score breakdown — expandable bars showing contribution of each factor

---

#### Step 4 — AI Chatbot for Clinical Query

Doctor taps "Ask AI Chatbot" from Patient Profile → chatbot panel opens (overlay or side panel).

```
┌────────────────────────────────────────────────────────────┐
│  AI Clinical Assistant — Chat              [−] [✕]        │
├────────────────────────────────────────────────────────────┤
│                                                            │
│  You: Has this patient ever reported chest                 │
│       pain with supplement X?                              │
│                                                            │
│  ┌──────────────────────────────────────────┐             │
│  │ 🤖 AI Assistant                          │             │
│  │                                          │             │
│  │ Based on this patient's records:         │             │
│  │                                          │             │
│  │ • No chest pain reported in association  │             │
│  │   with supplement X in any of 12         │             │
│  │   supplement assessments.                │             │
│  │                                          │             │
│  │ • Supplement X (Garlic Extract) has a    │             │
│  │   moderate interaction with Amlodipine   │             │
│  │   (BP medication). Monitor for dizziness │             │
│  │   or hypotension.                        │             │
│  │                                          │             │
│  │ • Latest BP reading: 128/82 (Sep 12).    │             │
│  │   Within target range.                   │             │
│  │                                          │             │
│  │ Sources: Supplement Assessment Sep 16,   │             │
│  │ Lab Report Sep 12, UM Expert Dataset.    │             │
│  │                                          │             │
│  │ ⚠️ This is an AI-generated response.     │             │
│  │ Always verify with clinical judgment.    │             │
│  └──────────────────────────────────────────┘             │
│                                                            │
│  [_______________________________] [Send]                  │
│                                                            │
│  Suggested queries:                                        │
│  • "Show me all abnormal lab results from last 6 months"   │
│  • "What supplements interact with metformin?"             │
│  • "Any patient with wellness score below 50?"              │
└────────────────────────────────────────────────────────────┘
```

**System Actions (IP 1 — Chatbot Query Pipeline):**
- `[SYSTEM]` Intent detection: classify query type (record lookup / supplement question / general)
- `[SYSTEM]` RAG: embed query → pgvector similarity search against patient's record chunks → retrieve top-5
- `[SYSTEM]` LLM generation: Mistral 7B / Llama 3 8B (self-hosted) with prompt constrained to retrieved context
- `[SYSTEM]` Guardrails: PII filter on output, hallucination check (ROUGE-L vs context), medical disclaimer injection
- `[AUDIT]` Log chatbot interaction (query, response, sources) for clinical review

**Edge Cases:**
| Case | Behaviour |
|---|---|
| Query outside patient's records scope | "I could not find information about [query] in this patient's records." |
| Query about unassigned patient | "You do not have access to that patient's records." (RBAC enforced at RAG retrieval layer) |
| Ambiguous query | "Could you clarify? Did you mean [interpretation A] or [interpretation B]?" |
| Hallucination detected (guardrail) | Response blocked; "I cannot answer this question with sufficient confidence. Please review the records directly." → Flagged for admin review |

---

#### Step 5 — Emergency Access Flow (Sub-Flow)

**Trigger:** Doctor taps "Emergency Access" button on Patient Profile or from global sidebar.

```
┌────────────────────────────────────────────────────────────┐
│                  EMERGENCY ACCESS FLOW                       │
│                                                             │
│  STEP 5a: Reason Capture                                    │
│  ┌──────────────────────────────────────────┐              │
│  │  ⚠️ EMERGENCY ACCESS                     │              │
│  │                                          │              │
│  │  You are requesting unrestricted access  │              │
│  │  to this patient's full medical records. │              │
│  │  All actions will be logged and audited. │              │
│  │                                          │              │
│  │  Patient: Rajan a/l Kumar               │              │
│  │                                          │              │
│  │  Reason for emergency access:*           │              │
│  │  ┌──────────────────────────────────┐    │              │
│  │  │ Patient unconscious — suspected   │    │              │
│  │  │ drug interaction. ER admission.    │    │              │
│  │  │ No next-of-kin available.          │    │              │
│  │  └──────────────────────────────────┘    │              │
│  │                                          │              │
│  │  Quick reasons:                          │              │
│  │  [Unconscious Patient]                   │              │
│  │  [Life-Threatening Emergency]            │              │
│  │  [Severe Allergic Reaction]              │              │
│  │  [Acute Psychiatric Crisis]              │              │
│  │  [Other — free text]                     │              │
│  │                                          │              │
│  │  [Cancel]           [Confirm Emergency]  │              │
│  └──────────────────────────────────────────┘              │
│                         │                                   │
│                         ▼                                   │
│  STEP 5b: Emergency Session Active                          │
│  ┌──────────────────────────────────────────┐              │
│  │ 🔴 EMERGENCY ACCESS ACTIVE — ALL ACTIONS │              │
│  │    LOGGED | Session expires in 58:23     │              │
│  │                                          │              │
│  │  [Patient Records — Full Access]         │              │
│  │  • All records visible                   │              │
│  │  • All assessments visible               │              │
│  │  • Download enabled (urgent care)        │              │
│  │  • Share disabled (emergency context)    │              │
│  │                                          │              │
│  │  [End Emergency Access]                  │              │
│  └──────────────────────────────────────────┘              │
│                         │                                   │
│          ┌──────────────┼──────────────┐                   │
│          ▼              ▼              ▼                   │
│  Doctor manually   30-min expiry    Admin force-revokes     │
│  ends session      auto-ends        from admin dashboard    │
│          │              │              │                   │
│          └──────────────┼──────────────┘                   │
│                         ▼                                   │
│  STEP 5c: Session Ended                                     │
│  ┌──────────────────────────────────────────┐              │
│  │  Emergency access ended.                 │              │
│  │  A full audit log has been recorded.     │              │
│  │  Patient and admin have been notified.   │              │
│  │                                          │              │
│  │  [Return to Patient Profile]             │              │
│  └──────────────────────────────────────────┘              │
└────────────────────────────────────────────────────────────┘
```

**System Actions (IP 1 — Emergency Access Method, FR-13):**

| Stage | System Action |
|---|---|
| Activation | `[SYSTEM]` Create `AuditEntry` with action `emergency_access_activate`, actor, patient, reason, timestamp. `[SYSTEM]` Generate short-lived JWT with `scope: emergency_access`, 30-min expiry. `[SYSTEM]` Dispatch notification to patient (email/SMS/push) and clinic admin (dashboard + email). `[SYSTEM]` WebSocket push to admin dashboard showing active emergency session. |
| During session | `[AUDIT]` Every record view/action during emergency session logged with `emergency_session_id` tag. `[SYSTEM]` Red banner displayed on every screen. Timer visible. |
| Expiry/End | `[SYSTEM]` Invalidate emergency JWT. `[AUDIT]` Log `emergency_access_deactivate` with session duration and total actions taken. `[SYSTEM]` Dispatch session summary to admin. |

**Edge Cases:**
| Case | Behaviour |
|---|---|
| Doctor attempts emergency access without selecting reason | "Confirm Emergency" button disabled; reason field required |
| Emergency session still active, doctor tries to start another for different patient | "You already have an active emergency session for Rajan a/l Kumar. End it before starting a new one." — [End Current] [Cancel] |
| Admin views active emergency session | Admin dashboard shows real-time: doctor name, patient name, reason, elapsed time, actions count; admin can force-revoke |
| Patient has no registered contact for notification | Notification queued; "Patient could not be notified — no contact information on file" logged in audit |

---

#### Step 6 — Return to Patient List

Doctor taps "Back to Patient List" → dashboard refreshes with updated alert counts (acknowledged alerts removed).

---

## Flow 3: Secure Record Sharing & Management

**IP Traceability:** IP 1 (PI 2025007955) — Encrypted cloud storage, time-limited sharing, secure viewer, audit logging  
**PRD Traceability:** FR-07, FR-09, FR-11, FR-12, FR-21  
**Actor(s):** Patient (sharer), Doctor (recipient), System (Encryption, Secure Viewer, Audit Service, Notification Service)

**v1 Scope Note:** Record upload and AI summary generation are v1. Time-limited sharing and secure viewer are v2 per PRD scope. This flow documents the full UX — v1 sections are marked.

### 3.0 Entry & Exit

| Attribute | Detail |
|---|---|
| **Entry Point (Upload)** | Patient taps "My Records" bottom tab → Records Vault → taps [+ Upload] |
| **Entry Point (Share)** | Patient taps "Share" on a record or assessment → Share Configuration (Step 4) |
| **Exit Point** | Recipient's access expires or is revoked; patient returns to Records Vault or Dashboard |

### 3.1 Step-by-Step Flow

```
┌──────────────────────────────────────────────────────────────────────────┐
│ FLOW 3: SECURE RECORD SHARING & MANAGEMENT                                 │
│                                                                           │
│  RECORDS VAULT       UPLOAD               SHARE CONFIG      SECURE VIEWER │
│  ┌──────────┐       ┌──────────┐         ┌──────────┐      ┌──────────┐  │
│  │Categories │       │Select File│         │Recipient │      │Watermark │  │
│  │• Lab Rep. │       │Photo/Cam │         │Permiss.  │      │No Copy   │  │
│  │• Prescrip.│──────►│Type=Tag  │────────►│Expiry    │─────►│No Print  │  │
│  │• Scans    │       │          │         │Note      │      │Timer     │  │
│  │• Assess.  │       │Uploading │         │          │      │Request   │  │
│  │           │       │Progress  │         │Link Sent │      │Extension │  │
│  │[Upload]   │       │AI Summ.  │         │          │      │Expiry    │  │
│  │[Share]    │       │Generated │         │          │      │Screen    │  │
│  └──────────┘       └──────────┘         └──────────┘      └──────────┘  │
└──────────────────────────────────────────────────────────────────────────┘
```

---

#### Step 1 — Records Vault

**Screen:** My Records

```
┌─────────────────────────────────────────┐
│  Healynx      My Records  [+ Upload]│
├─────────────────────────────────────────┤
│                                         │
│  [All] [Lab Reports] [Prescriptions]    │
│  [Scans] [Assessments]                  │
│                                         │
│  ┌─────────────────────────────────┐    │
│  │ 📄 HbA1c Test Results          │    │
│  │ Lab Report — Sep 12, 2026      │    │
│  │                                │    │
│  │ AI Summary: HbA1c 7.2%          │    │
│  │ (improving from 7.8%). Continue │    │
│  │ current metformin regimen.      │    │
│  │                           [➤]  │    │
│  └─────────────────────────────────┘    │
│                              [Share]    │
│                                         │
│  ┌─────────────────────────────────┐    │
│  │ 📄 Blood Pressure Log           │    │
│  │ Clinical Note — Aug 28, 2026    │    │
│  │                                │    │
│  │ AI Summary: BP 128/82. Well-   │    │
│  │ controlled on amlodipine 5mg.   │    │
│  │                           [➤]  │    │
│  └─────────────────────────────────┘    │
│                              [Share]    │
│                                         │
│  ┌─────────────────────────────────┐    │
│  │ 📄 Supplement Assessment        │    │
│  │ Assessment — Sep 16, 2026       │    │
│  │                                │    │
│  │ Wellness Score: 72. 3 supps.    │    │
│  │ 1 high-risk interaction.        │    │
│  │                           [➤]  │    │
│  └─────────────────────────────────┘    │
│                              [Share]    │
│                                         │
│  ─────────────────────────────────────  │
│  [🏠]    [📁]    [💊]    [👤]            │
└─────────────────────────────────────────┘
```

**System Actions:**
- `[SYSTEM]` Fetch `MedicalRecord` rows for this patient, ordered by `created_at DESC`
- `[SYSTEM]` Fetch `SupplementAssessment` rows (displayed as records in vault)
- `[SYSTEM]` Filter by category via `record_type` field
- `[AUDIT]` Log `record_view` for vault access

---

#### Step 2 — View Record Detail

Patient taps a record → Record Detail view.

```
┌─────────────────────────────────────────┐
│  ← My Records    HbA1c Test Results     │
├─────────────────────────────────────────┤
│                                         │
│  Record Type: Lab Report                │
│  Date: September 12, 2026               │
│  Uploaded: Sep 12, 2026 by Dr. Amira    │
│                                         │
│  ┌─────────────────────────────────┐    │
│  │ AI-GENERATED SUMMARY            │    │
│  │                                 │    │
│  │ Patient: Rajan a/l Kumar (58M). │    │
│  │ HbA1c lab report dated 12 Sep   │    │
│  │ 2026. HbA1c: 7.2% (target:      │    │
│  │ <7.0%). Trend: improving from   │    │
│  │ 7.8% three months prior.         │    │
│  │ Current medication: Metformin   │    │
│  │ 500mg BID. Assessment: Adequate │    │
│  │ glycemic control. Next HbA1c in │    │
│  │ 3 months.                        │    │
│  │                                 │    │
│  │ Confidence: High (0.89)         │    │
│  └─────────────────────────────────┘    │
│                                         │
│  [View Original Document]              │
│  [Share This Record]                   │
│  [Delete Record]                       │
│                                         │
└─────────────────────────────────────────┘
```

**System Actions:**
- `[SYSTEM]` Fetch `MedicalRecord` with `ai_summary` field
- `[SYSTEM]` "View Original Document" → generates S3 pre-signed URL (60-second expiry) for inline viewing
- `[AUDIT]` Log `record_view` event

---

#### Step 3 — Upload New Record [v1]

**Screen:** Upload Record

```
┌─────────────────────────────────────────┐
│  ← Cancel          Upload Record        │
├─────────────────────────────────────────┤
│                                         │
│  ┌─────────────────────────────────┐    │
│  │        📁                       │    │
│  │   Tap to select a file          │    │
│  │   or drag & drop (desktop)      │    │
│  │                                 │    │
│  │   Supported: PDF, JPEG, PNG,    │    │
│  │   TIFF (Max 25MB)              │    │
│  └─────────────────────────────────┘    │
│                                         │
│  [📷 Take Photo]  [📁 Browse Files]     │
│                                         │
│  ─── or ───                             │
│                                         │
│  Patient: Rajan a/l Kumar               │
│                                         │
│  Record Type:*                          │
│  ┌─────────────────────────────────┐    │
│  │ Lab Report               [  ▼] │    │
│  └─────────────────────────────────┘    │
│                                         │
│  Title:*                                │
│  ┌─────────────────────────────────┐    │
│  │ HbA1c Quarterly Results         │    │
│  └─────────────────────────────────┘    │
│                                         │
│  Description (optional):                │
│  ┌─────────────────────────────────┐    │
│  │ Diabetes monitoring — Q3 2026   │    │
│  └─────────────────────────────────┘    │
│                                         │
│  Date of document:*                     │
│  ┌─────────────────────────────────┐    │
│  │ September 12, 2026     [📅]     │    │
│  └─────────────────────────────────┘    │
│                                         │
│  [Upload & Process]                     │
└─────────────────────────────────────────┘
```

**System Actions on Upload:**
- `[SYSTEM]` Client-side file validation: type (MIME check), size ≤25MB, not empty
- `[SYSTEM]` Generate DEK (Fernet), encrypt file blob with AES-256-GCM
- `[SYSTEM]` Upload encrypted blob to S3 with SSE-KMS
- `[SYSTEM]` Write `MedicalRecord` row (status: `stored`)
- `[SYSTEM]` Trigger Celery task: OCR extraction → AI summarisation (async)
- `[SYSTEM]` WebSocket push: update record status to `processing` → `completed` with summary
- `[AUDIT]` Log `record_create` event

**Upload Progress UX:**
```
┌─────────────────────────────────────────┐
│  Uploading HbA1c Results...             │
│  ████████████████████░░░░  85%          │
│                                         │
│  ✓ File encrypted                       │
│  ✓ Uploaded to secure cloud             │
│  → Generating AI summary...             │
│                                         │
└─────────────────────────────────────────┘
```

**Edge Cases:**
| Case | Behaviour |
|---|---|
| File type not supported | Inline validation error: "File type not supported. Please upload PDF, JPEG, PNG, or TIFF." |
| File too large (>25MB) | "File exceeds 25MB limit. Please compress or split the document." |
| Upload interrupted (network loss) | Resume support via multipart upload (S3). On reconnect, prompt: "Upload was interrupted. Resume?" |
| Duplicate file detected (same checksum) | "This file appears to have been uploaded already on Sep 12. Upload anyway?" |

---

#### Step 4 — Share Configuration [v2]

**Screen:** Share Record

```
┌─────────────────────────────────────────┐
│  ← Cancel          Share Record         │
├─────────────────────────────────────────┤
│                                         │
│  Sharing: HbA1c Test Results            │
│                                         │
│  Recipient                              │
│  ┌─────────────────────────────────┐    │
│  │ 🔍 Search connected doctors...  │    │
│  └─────────────────────────────────┘    │
│                                         │
│  Suggested:                              │
│  ○ Dr. Amira (Primary Care)             │
│  ○ Dr. Wei (Cardiologist)               │
│                                         │
│  ─── or ───                             │
│                                         │
│  Email address:                          │
│  ┌─────────────────────────────────┐    │
│  │ cardiologist@hospital.com        │    │
│  └─────────────────────────────────┘    │
│                                         │
│  ─────────────────────────────────────  │
│                                         │
│  Permissions                            │
│  ☑ View document                        │
│  ☐ Allow download                       │
│  ☐ Allow forward/sharing                │
│                                         │
│  ─────────────────────────────────────  │
│                                         │
│  Access expires after:                   │
│  ○ 24 hours                             │
│  ● 7 days                               │
│  ○ 30 days                              │
│  ○ Custom: [____] days                  │
│                                         │
│  ─────────────────────────────────────  │
│                                         │
│  Access Code (optional):                │
│  ┌─────────────────────────────────┐    │
│  │ 1 2 3 4 5 6                     │    │
│  └─────────────────────────────────┘    │
│  Recipient must enter this code to view │
│                                         │
│  ─────────────────────────────────────  │
│                                         │
│  Note to recipient (optional):          │
│  ┌─────────────────────────────────┐    │
│  │ Please review HbA1c before our  │    │
│  │ appointment next week.          │    │
│  └─────────────────────────────────┘    │
│                                         │
│  [Generate Secure Link]                 │
└─────────────────────────────────────────┘
```

**System Actions:**
- `[SYSTEM]` Generate cryptographically random access token (128-bit entropy)
- `[SYSTEM]` Create `AccessGrant` row with permissions JSONB, expiry, optional access code (hashed)
- `[SYSTEM]` Dispatch notification to recipient via email/SMS/in-app
- `[AUDIT]` Log `share_grant_create` event

**Confirmation Screen:**
```
┌─────────────────────────────────────────┐
│           ✅ Link Created!               │
│                                         │
│  Your record has been shared securely.  │
│                                         │
│  Recipient: cardiologist@hospital.com    │
│  Expires: September 23, 2026 (7 days)   │
│  Permissions: View only                  │
│                                         │
│  Share link (tap to copy):               │
│  ┌─────────────────────────────────┐    │
│  │ Healynx.ai/share/xK9mW7qR2t  │ [📋]│
│  └─────────────────────────────────┘    │
│                                         │
│  [Share via WhatsApp] [Share via Email]  │
│  [Done]                                  │
└─────────────────────────────────────────┘
```

**Edge Cases:**
| Case | Behaviour |
|---|---|
| Recipient is an existing Healynx doctor | Internal notification (in-app + dashboard), no external link needed; grant appears in doctor's "Shared With Me" section |
| Access code set | Code displayed on confirmation screen; "Share this code separately from the link for extra security" |
| Patient revokes access early | "Active Shares" view → tap "Revoke" → confirmation dialog → `AccessGrant.is_revoked = true` → recipient sees "Access Revoked" on next link open |

---

#### Step 5 — Recipient Opens Secure Viewer [v2]

**Screen:** Secure Viewer (Recipient's browser)

```
┌────────────────────────────────────────────────────────────┐
│  🔒 Healynx — Secure Viewer                         │
│                                                            │
│  ┌──────────────────────────────────────────────────────┐ │
│  │ ⏱️ Access expires in: 6 days 23 hours 58 minutes     │ │
│  │                                                      │ │
│  │ ┌──────────────────────────────────────────────────┐ │ │
│  │ │                                                  │ │ │
│  │ │     [DOCUMENT RENDERED HERE]                     │ │ │
│  │ │                                                  │ │ │
│  │ │  cardiologist@hospital.com                       │ │ │
│  │ │  Accessed: Sep 16, 2026 10:45 AM                 │ │ │
│  │ │  IP: 203.106.12.45                               │ │ │
│  │ │  (repeating watermark overlay)                    │ │ │
│  │ │                                                  │ │ │
│  │ └──────────────────────────────────────────────────┘ │ │
│  │                                                      │ │
│  │  ⚠️ This document is view-only. Copying,             │ │
│  │  downloading, printing, and screenshots are          │ │
│  │  prohibited. All access is logged.                   │ │
│  │                                                      │ │
│  │  [Request Extension]                                  │ │
│  └──────────────────────────────────────────────────────┘ │
│                                                            │
│  ┌──────────────────────────────────────────────────────┐ │
│  │ Shared by: Rajan a/l Kumar                           │ │
│  │ Note: "Please review HbA1c before our appointment."  │ │
│  └──────────────────────────────────────────────────────┘ │
└────────────────────────────────────────────────────────────┘
```

**System Actions (IP 1 — Secure Viewer, FR-12):**
- `[SYSTEM]` Validate access token: not expired, not revoked, access code correct (if set)
- `[SYSTEM]` Decrypt record blob from S3, render server-side with watermark overlay
- `[SYSTEM]` Serve with strict CSP headers, no-download HTML, JS event blockers
- `[AUDIT]` Log `share_view` event with accessor email, IP, user-agent, outcome
- `[SYSTEM]` Start session inactivity timer (15 minutes)

**Enforcement Mechanisms:**
| Restriction | Implementation |
|---|---|
| No right-click | `contextmenu` event prevented |
| No keyboard shortcuts | Ctrl+C, Ctrl+S, Ctrl+P, Ctrl+Shift+I blocked via `keydown` handler |
| No download | Content served as server-rendered HTML/canvas; no downloadable URL |
| No print | `@media print` CSS hides content, shows watermark only |
| Watermark | Server-side rendered: `{recipient_email} | {datetime} | {ip_address}` repeating across document |

**Edge Cases:**
| Case | Behaviour |
|---|---|
| Access code required | Prompt overlay before document renders: "Enter the access code shared by the document owner." 3 incorrect attempts → token invalidated → access blocked |
| Token expired | "This shared link has expired. Contact the document owner to request a new link." [Request Extension] → sends email to patient |
| Token revoked | "Access to this document has been revoked by the owner." No further action possible |
| Inactivity timeout (15 min) | "Session timed out due to inactivity. Reload the link to view again." |

---

#### Step 6 — Access Expiry & Revocation

**Expiry Screen (Recipient):**
```
┌─────────────────────────────────────────┐
│  Healynx                          │
│                                         │
│         ⏰ Access Expired               │
│                                         │
│  This shared link has expired.          │
│  The document owner has been notified.  │
│                                         │
│  [Request New Access]                   │
│                                         │
└─────────────────────────────────────────┘
```

**Active Shares View (Patient):**
```
┌─────────────────────────────────────────┐
│  ← My Records      Active Shares        │
├─────────────────────────────────────────┤
│                                         │
│  ┌─────────────────────────────────┐    │
│  │ HbA1c Results                   │    │
│  │ Shared with: cardio@hospital.com│    │
│  │ Expires: Sep 23, 2026 (7d left) │    │
│  │ Permissions: View only          │    │
│  │ Views: 2                        │    │
│  │                        [Revoke] │    │
│  └─────────────────────────────────┘    │
│                                         │
│  ┌─────────────────────────────────┐    │
│  │ Supplement Assessment Sep 16    │    │
│  │ Shared with: Dr. Wei            │    │
│  │ Expires: Sep 18, 2026 (2d left) │    │
│  │ Permissions: View + Download    │    │
│  │ Views: 1                        │    │
│  │                        [Revoke] │    │
│  └─────────────────────────────────┘    │
│                                         │
│  ┌─────────────────────────────────┐    │
│  │ Blood Pressure Log (EXPIRED)     │    │
│  │ Shared with: nurse@clinic.com    │    │
│  │ Expired: Sep 10, 2026           │    │
│  │ Permissions: View only          │    │
│  │                        [Reshare]│    │
│  └─────────────────────────────────┘    │
└─────────────────────────────────────────┘
```

**System Actions:**
- `[SYSTEM]` Scheduled job runs every 5 minutes: marks `AccessGrant` entries where `expiry_at < NOW()` as expired
- `[SYSTEM]` On revocation: set `is_revoked = true`, `revoked_by = user_id`, `revoked_at = NOW()`
- `[AUDIT]` Log `share_grant_revoke` event
- `[SYSTEM]` Dispatch notification to patient confirming revocation
- `[SYSTEM]` Recipient sees "Access Revoked" on next link visit

---

## Flow 4: Onboarding & First-Time Setup

**IP Traceability:** IP 2 — First supplement survey as onboarding step. IP 1 — First record upload links to secure storage.  
**PRD Traceability:** FR-01, FR-07, NFR-02 (PDPA consent)  
**Actor(s):** Patient, System

### 4.0 Entry & Exit

| Attribute | Detail |
|---|---|
| **Entry Point** | App install / first visit to Healynx.ai → Welcome screen |
| **Exit Point** | Tutorial complete → populated Dashboard |

### 4.1 Step-by-Step Flow

```
┌──────────────────────────────────────────────────────────────────────────┐
│ FLOW 4: ONBOARDING & FIRST-TIME SETUP                                     │
│                                                                           │
│  WELCOME    REGISTER      HEALTH       FIRST        FIRST       TUTORIAL  │
│                       PROFILE       SURVEY       RECORD                  │
│  ┌──────┐  ┌──────┐  ┌──────┐  ┌──────┐  ┌──────┐  ┌──────────┐        │
│  │Logo  │  │Email │  │DOB   │  │Survey│  │Upload│  │Step 1:   │        │
│  │Tag   │  │Phone │  │Sex   │  │Screen│  │Record│  │Dashboard │        │
│  │line  │──►│OTP   │──►│Cond. │──►│1-4   │──►│Photo │──►│Step 2:   │──►Dash│
│  │      │  │Pass  │  │Meds  │  │      │  │File  │  │Records   │        │
│  │Create│  │PDPA  │  │Aller.│  │Submit│  │Type  │  │Step 3:   │        │
│  │Acct  │  │Consent│ │      │  │      │  │      │  │Supplement│        │
│  └──────┘  └──────┘  └──────┘  └──────┘  └──────┘  └──────────┘        │
└──────────────────────────────────────────────────────────────────────────┘
```

---

#### Step 1 — Welcome & Registration

```
┌─────────────────────────────────────────┐
│                                         │
│         ┌─────────────┐                │
│         │  Healynx │                │
│         │     AI      │                │
│         └─────────────┘                │
│                                         │
│   Your AI-powered preventive health    │
│   companion. Supplement safety,        │
│   secure records, personalised         │
│   insights.                            │
│                                         │
│   ┌─────────────────────────────────┐   │
│   │       [Create Account]          │   │
│   └─────────────────────────────────┘   │
│                                         │
│   Already have an account? [Log In]     │
│                                         │
│   ─────────────────────────────────    │
│   By continuing, you agree to our       │
│   [Terms of Service] and [Privacy Policy]│
└─────────────────────────────────────────┘
```

---

#### Step 2 — Registration Form

```
┌─────────────────────────────────────────┐
│  ← Back           Create Account        │
├─────────────────────────────────────────┤
│                                         │
│  Full Name*                             │
│  ┌─────────────────────────────────┐    │
│  │ Rajan a/l Kumar                  │    │
│  └─────────────────────────────────┘    │
│                                         │
│  Email Address*                         │
│  ┌─────────────────────────────────┐    │
│  │ rajan@email.com                  │    │
│  └─────────────────────────────────┘    │
│                                         │
│  Phone Number*                          │
│  ┌────────┐ ┌──────────────────────┐    │
│  │ +60  ▼ │ │ 12 345 6789          │    │
│  └────────┘ └──────────────────────┘    │
│                                         │
│  Password*                              │
│  ┌─────────────────────────────────┐    │
│  │ ••••••••••         [👁️]        │    │
│  └─────────────────────────────────┘    │
│  Must be at least 8 characters         │
│                                         │
│  ┌─────────────────────────────────┐    │
│  │  📱 Verify Phone Number          │    │
│  │  Enter the 6-digit code sent to  │    │
│  │  +60 12 345 6789                 │    │
│  │                                  │    │
│  │  ┌──┐ ┌──┐ ┌──┐ ┌──┐ ┌──┐ ┌──┐ │    │
│  │  │  │ │  │ │  │ │  │ │  │ │  │ │    │
│  │  └──┘ └──┘ └──┘ └──┘ └──┘ └──┘ │    │
│  │                                  │    │
│  │  Didn't receive code? [Resend]   │    │
│  └─────────────────────────────────┘    │
│                                         │
│  [Verify & Continue]                    │
└─────────────────────────────────────────┘
```

**System Actions:**
- `[SYSTEM]` Validate phone number format (Malaysian: +60 prefix)
- `[SYSTEM]` Send OTP via SMS (Twilio or local gateway)
- `[SYSTEM]` Rate limit: max 3 OTP requests per 10 minutes per phone number
- `[SYSTEM]` Password strength check: ≥8 chars, ≥1 uppercase, ≥1 digit
- `[SYSTEM]` Create `User` row (role: `patient`), create `PatientProfile` row

**Error States:**
| Error | Handling |
|---|---|
| Phone already registered | "This phone number is already registered. [Log In] instead?" |
| OTP expired (10 min) | "Code expired. Request a new one." |
| OTP incorrect (3 attempts) | "Too many attempts. Request a new code." |
| Weak password | Inline validation: "Password must include at least one uppercase letter and one number." |

---

#### Step 3 — PDPA Consent

```
┌─────────────────────────────────────────┐
│  ← Back           Data Consent          │
├─────────────────────────────────────────┤
│                                         │
│  To provide you with personalised       │
│  supplement safety analysis, Healynx │
│  AI needs to collect and process your   │
│  health data.                           │
│                                         │
│  ┌─────────────────────────────────┐    │
│  │ WHAT WE COLLECT                  │    │
│  │ • Health conditions & medications│    │
│  │ • Supplement intake history      │    │
│  │ • Medical records you upload     │    │
│  │ • Lifestyle information          │    │
│  └─────────────────────────────────┘    │
│                                         │
│  ┌─────────────────────────────────┐    │
│  │ HOW WE PROTECT YOUR DATA         │    │
│  │ • AES-256 encryption at rest    │    │
│  │ • TLS 1.3 encryption in transit │    │
│  │ • Data stored in Malaysia only  │    │
│  │ • PDPA 2010 compliant           │    │
│  └─────────────────────────────────┘    │
│                                         │
│  ┌─────────────────────────────────┐    │
│  │ YOUR RIGHTS                      │    │
│  │ • Access your data anytime       │    │
│  │ • Request data deletion          │    │
│  │ • Withdraw consent anytime       │    │
│  │ • Export your data (JSON/PDF)    │    │
│  └─────────────────────────────────┘    │
│                                         │
│  ☑ I understand and consent to the     │
│    collection and processing of my      │
│    health data as described above.      │
│                                         │
│  ☐ I consent to anonymised data being  │
│    used for research to improve AI.     │
│    (Optional — your care is unaffected) │
│                                         │
│  [Agree & Continue]                     │
└─────────────────────────────────────────┘
```

**System Actions:**
- `[SYSTEM]` Create `ConsentRecord` row with consent version, timestamp, IP
- `[SYSTEM]` Consent version is immutable; if consent terms change, users must re-consent on next login
- `[AUDIT]` Log consent acceptance event

---

#### Step 4 — Health Profile Setup

```
┌─────────────────────────────────────────┐
│  ← Back     Set Up Your Health Profile  │
├─────────────────────────────────────────┤
│                                         │
│  Let's build your baseline. This helps  │
│  the AI provide accurate insights.      │
│                                         │
│  Date of Birth*                         │
│  ┌─────────────────────────────────┐    │
│  │ January 15, 1968        [📅]    │    │
│  └─────────────────────────────────┘    │
│                                         │
│  Sex*                    [Male    ▼]    │
│                                         │
│  Blood Type              [Unknown ▼]    │
│                                         │
│  Known Conditions                        │
│  ┌─────────────────────────────────┐    │
│  │ Type 2 Diabetes            [✕] │    │
│  │ Hypertension               [✕] │    │
│  └─────────────────────────────────┘    │
│  [+ Add Condition]                      │
│                                         │
│  Current Medications                     │
│  ┌─────────────────────────────────┐    │
│  │ Metformin 500mg — Twice daily  │    │
│  │ Amlodipine 5mg — Once daily    │    │
│  │ Atorvastatin 20mg — Once daily │    │
│  └─────────────────────────────────┘    │
│  [+ Add Medication]                     │
│                                         │
│  Allergies                               │
│  ┌─────────────────────────────────┐    │
│  │ Penicillin                   [✕]│    │
│  └─────────────────────────────────┘    │
│  [+ Add Allergy]                        │
│                                         │
│  [Save & Continue]                      │
│                                         │
│  ─────────────────────────────────────  │
│  You can update this anytime in Profile │
└─────────────────────────────────────────┘
```

**System Actions:**
- `[SYSTEM]` Update `PatientProfile` with demographics, health_profile JSONB
- `[AUDIT]` Log profile creation event

---

#### Step 5 — First Supplement Survey

Patient is prompted to complete their first supplement survey.
Flow follows Flow 1 (Supplement Assessment) but with onboarding context:

```
┌─────────────────────────────────────────┐
│  ← Back     🎉 You're almost done!      │
├─────────────────────────────────────────┤
│                                         │
│  Complete your first supplement intake  │
│  survey to get your personalised        │
│  wellness score and safety insights.    │
│                                         │
│  Takes about 3 minutes.                 │
│                                         │
│  [Start Survey]                         │
│  [Skip for now]                         │
│                                         │
└─────────────────────────────────────────┘
```

**Edge Cases:**
| Case | Behaviour |
|---|---|
| Patient skips survey | "Skip for now" → no baseline assessment; Dashboard shows "Complete your first supplement survey" CTA; reminder sent after 24 hours if not completed |
| Patient completes survey | Onboarding continues to Step 6 |

---

#### Step 6 — First Medical Record Upload

```
┌─────────────────────────────────────────┐
│  ← Back     One more step!              │
├─────────────────────────────────────────┤
│                                         │
│  Upload a recent medical record to      │
│  start building your secure health      │
│  vault.                                 │
│                                         │
│  Suggestions:                            │
│  • Recent lab results (blood test, etc.)│
│  • Prescription list from your doctor   │
│  • Vaccination record                   │
│                                         │
│  ┌─────────────────────────────────┐    │
│  │        📁                       │    │
│  │   Tap to upload a file          │    │
│  │   or take a photo               │    │
│  └─────────────────────────────────┘    │
│                                         │
│  [Upload Record]  [Skip for now]        │
│                                         │
└─────────────────────────────────────────┘
```

**Edge Cases:**
| Case | Behaviour |
|---|---|
| Patient skips record upload | "Skip for now" → Records Vault shows empty state CTA; no penalty |
| Patient uploads record | Upload flow as per Flow 3, Step 3 |

---

#### Step 7 — Tutorial Overlay

```
┌─────────────────────────────────────────┐
│  Healynx                          │
│                                         │
│  ┌─────────────────────────────────┐    │
│  │  👋 Welcome to Healynx!    │    │
│  │                                 │    │
│  │  Here's a quick tour of your    │    │
│  │  health companion.              │    │
│  │                         1 of 3  │    │
│  │  ┌───────────────────────────┐  │    │
│  │  │ 🏠 Dashboard               │  │    │
│  │  │                           │  │    │
│  │  │ Your home screen. See     │  │    │
│  │  │ your wellness score,      │  │    │
│  │  │ supplement safety status, │  │    │
│  │  │ health trends, and        │  │    │
│  │  │ reminders at a glance.    │  │    │
│  │  └───────────────────────────┘  │    │
│  │                                 │    │
│  │  [Next →]     [Skip Tutorial]  │    │
│  └─────────────────────────────────┘    │
│                                         │
│  (Overlay highlights the Dashboard tab) │
│                                         │
│  ─────────────────────────────────────  │
│  [🏠]    [📁]    [💊]    [👤]            │
└─────────────────────────────────────────┘
```

**Tutorial Steps:**
| Step | Highlight | Content |
|---|---|---|
| 1 of 3 | Dashboard tab 🏠 | "Your home screen. See wellness score, supplement status, trends, and reminders." |
| 2 of 3 | Records tab 📁 | "Your secure health vault. Upload, view, and share medical records with AI summaries." |
| 3 of 3 | Supplement tab 💊 | "Update your supplement intake anytime. Get AI-powered safety analysis and personalised recommendations." |

**Exit:** Tutorial complete → Patient lands on populated Dashboard with onboarding-specific welcome toast: "Welcome, Rajan! Your health journey starts here."

---

## Flow 5: Admin — System Oversight & PDPA Operations

**IP Traceability:** IP 1 — RBAC management, audit log viewing, system monitoring  
**PRD Traceability:** FR-10, FR-13, FR-21, NFR-02  
**Actor(s):** Clinic Admin, System

### 5.0 Entry & Exit

| Attribute | Detail |
|---|---|
| **Entry Point** | Admin logs in → Admin Dashboard |
| **Exit Point** | Admin completes task → returns to Admin Dashboard |

### 5.1 Step-by-Step Flow

```
┌──────────────────────────────────────────────────────────────────────────┐
│ FLOW 5: ADMIN — SYSTEM OVERSIGHT & PDPA OPERATIONS                        │
│                                                                           │
│  ADMIN DASHBOARD    USER MGMT        AUDIT LOGS       PDPA TOOLS         │
│  ┌────────────┐    ┌──────────┐    ┌──────────┐    ┌──────────────┐     │
│  │Clinic Stats│    │Add Doctor│    │Filter by │    │Data Export   │     │
│  │Active Users│    │Set Role  │    │Actor     │    │Data Deletion │     │
│  │Alerts      │───►│Permiss.  │───►│Action    │───►│Consent       │     │
│  │Emergency   │    │Deactivate│    │Date Range│    │Management    │     │
│  │Sessions    │    │Reset MFA │    │Export CSV│    │Breach Log    │     │
│  │System      │    │          │    │          │    │              │     │
│  │Health      │    │          │    │          │    │              │     │
│  └────────────┘    └──────────┘    └──────────┘    └──────────────┘     │
└──────────────────────────────────────────────────────────────────────────┘
```

---

#### Step 1 — Admin Dashboard

```
┌────────────────────────────────────────────────────────────┐
│  Healynx — Admin    Ms. Linda   ⚙️   🔔   👤        │
├──────────────┬──────────────────────────────────────────────┤
│ Sidebar      │                                              │
│              │  Clinic: Klinik Amira      📅 Sep 16, 2026  │
│ 🏠 Dashboard │                                              │
│ 👥 Users     │  ┌──────────┬──────────┬──────────┬───────┐ │
│ 📋 Audits    │  │Patients  │Doctors   │Active    │Emerg. │ │
│ 📤 PDPA      │  │  247     │   5      │Shares    │Sess.  │ │
│ 🏥 Clinic    │  │          │          │  12      │  0    │ │
│ 📊 Health    │  └──────────┴──────────┴──────────┴───────┘ │
│ ⚙️ Billing   │                                              │
│              │  ┌──────────────────────────────────────┐    │
│              │  │ RECENT EMERGENCY ACCESS EVENTS        │    │
│              │  │ (Last 7 days)                         │    │
│              │  │                                       │    │
│              │  │ Sep 15 — Dr. Amira → Rajan a/l Kumar │    │
│              │  │ Reason: ER admission, unconscious     │    │
│              │  │ Duration: 22 min | Actions: 14        │    │
│              │  │                               [View]  │    │
│              │  │───────────────────────────────────── │    │
│              │  │ No other emergency events this week.  │    │
│              │  └──────────────────────────────────────┘    │
│              │                                              │
│              │  ┌──────────────────────────────────────┐    │
│              │  │ SYSTEM HEALTH                          │    │
│              │  │ API Uptime: 99.97% 🟢                 │    │
│              │  │ AI Engine: Operational 🟢             │    │
│              │  │ Storage: 12.4 GB / 50 GB (24.8%)      │    │
│              │  │ API Latency (p95): 2.1s 🟢            │    │
│              │  │ Last Backup: Sep 16, 04:00 ✅         │    │
│              │  └──────────────────────────────────────┘    │
│              │                                              │
│              │  ┌──────────────────────────────────────┐    │
│              │  │ PENDING PDPA REQUESTS                  │    │
│              │  │ Data Export: 1 pending                │    │
│              │  │ Data Deletion: 0 pending              │    │
│              │  │ Consent Changes: 0 pending            │    │
│              │  └──────────────────────────────────────┘    │
└──────────────┴──────────────────────────────────────────────┘
```

**System Actions:**
- `[SYSTEM]` Aggregate counts: active patients, active doctors, active shares, active emergency sessions
- `[SYSTEM]` Fetch system health metrics (from monitoring dashboards API)
- `[SYSTEM]` Fetch pending PDPA requests
- `[AUDIT]` Log `admin_dashboard_view`

---

#### Step 2 — User & Role Management

```
┌────────────────────────────────────────────────────────────┐
│  ← Dashboard          User Management                      │
├──────────────┬──────────────────────────────────────────────┤
│              │                                              │
│              │  [+ Add Doctor]    Search users... [_______] │
│              │                                              │
│              │ ┌──────────────────────────────────────────┐│
│              │ │ User            Role    Status   Actions  ││
│              │ │──────────────────────────────────────────││
│              │ │ Dr. Amira       Doctor  Active   [✏️][🔒] ││
│              │ │ dr.amira@email  (Primary)                ││
│              │ │──────────────────────────────────────────││
│              │ │ Dr. Wei          Doctor  Active   [✏️][🔒] ││
│              │ │ dr.wei@hospital  (Cardio)                ││
│              │ │──────────────────────────────────────────││
│              │ │ Ms. Linda       Admin   Active    [✏️]    ││
│              │ │ linda@clinic    (Self)                   ││
│              │ │──────────────────────────────────────────││
│              │ │ Rajan a/l Kumar Patient Active   [✏️][🔒] ││
│              │ └──────────────────────────────────────────┘│
│              │                                              │
│              │  ─────────────────────────────────────────  │
│              │  [Deactivated Users ▾] (3)                  │
└──────────────┴──────────────────────────────────────────────┘
```

**Add Doctor Flow (Sub-flow):**
```
┌─────────────────────────────────────────┐
│  ← Users            Add Doctor         │
├─────────────────────────────────────────┤
│                                         │
│  Full Name*                             │
│  ┌─────────────────────────────────┐    │
│  │ Dr. Nurul Hayati                │    │
│  └─────────────────────────────────┘    │
│                                         │
│  Email*                                 │
│  ┌─────────────────────────────────┐    │
│  │ dr.nurul@clinic.com              │    │
│  └─────────────────────────────────┘    │
│                                         │
│  Role*                   [Doctor ▼]     │
│                                         │
│  Permissions:                            │
│  ☑ View patient records                 │
│  ☑ Edit patient records                 │
│  ☑ View AI analyses & alerts            │
│  ☑ Acknowledge alerts                   │
│  ☑ Generate share links                 │
│  ☑ Emergency access                     │
│  ☐ Manage users (admin only)            │
│  ☐ View audit logs (admin only)         │
│                                         │
│  [Send Invitation]                      │
└─────────────────────────────────────────┘
```

**System Actions:**
- `[SYSTEM]` Create `User` row with role `doctor`, tenant-scoped
- `[SYSTEM]` Send invitation email with registration link (48-hour expiry)
- `[AUDIT]` Log `user_create` event
- `[SYSTEM]` On deactivation: revoke all active sessions, invalidate refresh tokens

---

#### Step 3 — Audit Log Review

```
┌────────────────────────────────────────────────────────────┐
│  ← Dashboard            Audit Logs                         │
├──────────────┬──────────────────────────────────────────────┤
│              │                                              │
│              │  Filters:                                    │
│              │  Actor: [All ▼]  Action: [All ▼]            │
│              │  Target: [All ▼]  Patient: [_______]        │
│              │  From: [Sep 1, 2026]  To: [Sep 16, 2026]   │
│              │  [Apply Filters]  [Clear]  [Export CSV]     │
│              │                                              │
│              │ ┌──────────────────────────────────────────┐│
│              │ │ Timestamp         Actor    Action  Target ││
│              │ │──────────────────────────────────────────││
│              │ │ Sep 16 10:45:03  Dr.Amira emerg. Rajan K.││
│              │ │ Sep 16 10:30:00  Rajan K. assess. Submtd ││
│              │ │ Sep 16 09:15:22  Dr.Amira record  HbA1c  ││
│              │ │                       view               ││
│              │ │ Sep 16 08:05:11  Rajan K. login  Success ││
│              │ │ Sep 15 14:22:00  Dr.Wei   share_  HbA1c  ││
│              │ │                       grant              ││
│              │ │──────────────────────────────────────────││
│              │ │                 ← 1 2 3 ... 47 →         ││
│              │ └──────────────────────────────────────────┘│
│              │                                              │
│              │  Click any row to view full details          │
└──────────────┴──────────────────────────────────────────────┘
```

**Audit Detail Expanded View:**
```
┌─────────────────────────────────────────┐
│  Audit Entry Detail                     │
├─────────────────────────────────────────┤
│                                         │
│  Entry ID: aud_7f3a9b2c...             │
│                                         │
│  Actor: Dr. Amira (ID: user_4b2c...)   │
│  Role: Doctor                           │
│                                         │
│  Action: emergency_access_activate      │
│                                         │
│  Target Type: PatientProfile            │
│  Target ID: patient_9d1e...             │
│  Target Name: Rajan a/l Kumar           │
│                                         │
│  Details:                               │
│  ┌─────────────────────────────────┐    │
│  │ Reason: Patient unconscious —    │    │
│  │ suspected drug interaction. ER   │    │
│  │ admission. No Nok available.     │    │
│  │                                  │    │
│  │ Session Duration: 22 minutes     │    │
│  │ Actions During Session: 14       │    │
│  └─────────────────────────────────┘    │
│                                         │
│  IP Address: 203.106.12.45             │
│  User Agent: Mozilla/5.0 (Windows...)  │
│  Outcome: Success                       │
│  Timestamp: Sep 16, 2026 10:45:03 MYT  │
│                                         │
│  [Close]                                │
└─────────────────────────────────────────┘
```

**System Actions:**
- `[SYSTEM]` Query `AuditEntry` table with filters (actor, action, target_type, target_id, date range)
- `[SYSTEM]` Pagination: 50 entries per page
- `[SYSTEM]` Export: generate CSV or PDF with filtered results; `[AUDIT]` log `data_export` event

---

#### Step 4 — PDPA Request Handling

```
┌────────────────────────────────────────────────────────────┐
│  ← Dashboard            PDPA Requests                      │
├──────────────┬──────────────────────────────────────────────┤
│              │                                              │
│              │  Tabs: [Pending] [In Progress] [Completed]   │
│              │                                              │
│              │ ┌──────────────────────────────────────────┐│
│              │ │ ⚠️ Data Export Request                    ││
│              │ │ Patient: Rajan a/l Kumar                  ││
│              │ │ Requested: Sep 16, 2026                   ││
│              │ │ Deadline: Oct 16, 2026 (30 days left)    ││
│              │ │                                          ││
│              │ │ [Process Export]  [Request More Info]    ││
│              │ └──────────────────────────────────────────┘│
│              │                                              │
│              │ ┌──────────────────────────────────────────┐│
│              │ │ Data Deletion Request                     ││
│              │ │ Patient: Ahmad bin Ismail                 ││
│              │ │ Requested: Sep 10, 2026                   ││
│              │ │ Status: In Progress                      ││
│              │ │ Verified: Pending ID confirmation        ││
│              │ │                                          ││
│              │ │ [Confirm Identity]  [Reject]             ││
│              │ └──────────────────────────────────────────┘│
└──────────────┴──────────────────────────────────────────────┘
```

**Data Export Flow (Admin):**
1. Admin taps "Process Export"
2. System generates full patient data package: profile, all assessments, all records metadata, sharing history
3. Admin reviews (anonymised preview)
4. Admin confirms → package encrypted → secure download link generated → patient notified
5. `[AUDIT]` Log `data_export` event with admin actor

**Data Deletion Flow (Admin):**
1. Admin verifies patient identity (ID document upload or in-person verification)
2. Admin taps "Confirm Identity" → initiates deletion workflow
3. System: (a) annotations PII fields in PatientProfile, (b) hard-deletes S3 record blobs, (c) retains anonymised clinical data for 7-year audit obligation
4. `[AUDIT]` Log `record_delete` events for all affected records
5. Patient notified of completion

---

#### Step 5 — System Health Monitoring

```
┌────────────────────────────────────────────────────────────┐
│  ← Dashboard          System Health                        │
├──────────────┬──────────────────────────────────────────────┤
│              │                                              │
│              │  Last updated: Just now [🔄 Refresh]         │
│              │                                              │
│              │  ┌────────────────┬────────────────────────┐│
│              │  │ SERVICE STATUS │                        ││
│              │  │                │                        ││
│              │  │ API Gateway 🟢 │ Uptime: 99.97% (30d)  ││
│              │  │ Auth Service 🟢 │ Uptime: 99.99%        ││
│              │  │ AI Engine   🟢 │ Queue: 12 pending     ││
│              │  │ OCR Pipeline🟢 │ Idle                  ││
│              │  │ Database    🟢 │ Conn: 34/200          ││
│              │  │ Redis       🟢 │ Memory: 42%           ││
│              │  │ S3 Storage  🟢 │ 12.4/50 GB           ││
│              │  └────────────────┴────────────────────────┘│
│              │                                              │
│              │  ┌────────────────────────────────────────┐  │
│              │  │ AI ENGINE METRICS (Last 24h)           │  │
│              │  │ Assessments processed: 47              │  │
│              │  │ Avg processing time: 2.1s              │  │
│              │  │ p95 processing time: 3.8s              │  │
│              │  │ Error rate: 0.0%                       │  │
│              │  │ Summaries generated: 12                │  │
│              │  │ Avg summary time: 6.2s                 │  │
│              │  └────────────────────────────────────────┘  │
│              │                                              │
│              │  ┌────────────────────────────────────────┐  │
│              │  │ RECENT INCIDENTS (Last 7 days)         │  │
│              │  │ No incidents. All systems operational. │  │
│              │  └────────────────────────────────────────┘  │
└──────────────┴──────────────────────────────────────────────┘
```

---

## 7. Global Error & Edge Case Patterns

### 7.1 Network & Connectivity

| State | Pattern | UX |
|---|---|---|
| **Offline** | Detect via `navigator.onLine` + periodic health-check ping | Non-blocking banner at top: "You are offline. Some features may be unavailable." Survey responses saved to localStorage queue. |
| **Slow connection** | Request timeout >10s | Skeleton loaders; timeout toast: "Connection is slow. Retrying..." with manual [Retry] button |
| **Intermittent** | Retry with exponential backoff (3 attempts: 1s, 2s, 4s) | Inline error on failed component: "Couldn't load data. [Tap to retry]" |

### 7.2 Authentication & Session

| State | Pattern | UX |
|---|---|---|
| **Session expired** | 401 response from any API call | Redirect to login; preserve intended destination in URL param `?redirect=/records/123`; post-login redirect to intended page |
| **Token refresh failed** | Refresh token expired/revoked | Full logout; toast: "Session expired. Please log in again." |
| **Concurrent session limit** | New login from different device | Toast on old device: "You've logged in from another device. This session has ended." |

### 7.3 AI Processing

| State | Pattern | UX |
|---|---|---|
| **Task queued** | Task ID returned, status `pending` | Loading state with estimated wait time |
| **Task processing** | Status `processing`, progress available | Progress bar with stage indicators |
| **Task completed** | Status `completed` | Auto-navigate to results (or WebSocket push) |
| **Task failed** | Status `failed`, error_message populated | Error screen with "Retry" and "Contact Support" options; error logged to monitoring |
| **Task timeout** | >15 seconds without completion | "Taking longer than usual" message; option to receive push notification when complete |

### 7.4 RBAC & Access Denied

| State | Pattern | UX |
|---|---|---|
| **Access denied (role)** | OPA policy rejects request | "You don't have permission to view this. Contact your administrator." with specific action description |
| **Access denied (tenant)** | RLS blocks query | Generic "Not found" (don't reveal existence) — security through obscurity for cross-tenant attempts |
| **Emergency access expired** | JWT scope expired | Session ends; redirect to patient list; toast: "Emergency access session has expired." |

### 7.5 Data Integrity

| State | Pattern | UX |
|---|---|---|
| **File upload checksum mismatch** | MD5/SHA256 comparison post-upload | "Upload verification failed. The file may have been corrupted. Please try again." Auto-retry once |
| **Record version conflict** | Two users edit same record simultaneously | Last-write-wins with version history; "This record was updated by Dr. Wei while you were viewing it. [View latest version]" |
| **Deletion confirmation** | Destructive action guard | Modal: "Are you sure you want to delete this record? This action cannot be undone." [Cancel] [Delete] |

---

## 8. Responsive Breakpoints & Layout Strategy

### 8.1 Breakpoint Table

| Breakpoint | Min Width | Target Devices | Layout |
|---|---|---|---|
| **Mobile S** | 320px | Small smartphones | Single column, stacked cards, full-width buttons, bottom tab navigation |
| **Mobile L** | 428px | Large smartphones, small phablets | Single column, larger touch targets |
| **Tablet** | 768px | iPad Mini, iPad, Android tablets | Two-column grid for dashboard, side-by-side for survey screens 3-4 |
| **Desktop S** | 1024px | Small laptops | Sidebar navigation, 2-3 column dashboard grid, modal dialogs for forms |
| **Desktop L** | 1440px+ | Large monitors | Sidebar + full grid, multi-panel views (patient list + detail side-by-side) |

### 8.2 Component Adaptation

| Component | Mobile | Desktop |
|---|---|---|
| **Navigation** | Bottom tab bar (4 tabs) | Left sidebar (icon + label, collapsible) |
| **Patient List** | Full-width cards, scroll vertically | Table with sortable columns, search bar inline |
| **Survey** | Full-screen swipeable steps | Multi-step wizard with progress sidebar |
| **Dashboard** | Single-column stacked cards | 2-column grid (wellness + alerts left, trends + reminders right) |
| **Secure Viewer** | Full-screen document, pinch-to-zoom | Document in centre panel, watermark overlay |
| **Chatbot** | Full-screen overlay, dismissible | Side panel (right 30%), resizable |
| **Modals/Dialogs** | Bottom sheet | Centred modal |

---

## 9. IP Traceability in UX

| UX Component | IP Source | How IP Manifests in UX |
|---|---|---|
| Supplement survey (multi-step wizard) | IP 2 (UI 2025002953) | The structured survey instrument is the core user-facing embodiment of IP 2. Every field, validation rule, and pre-population logic traces to the IP's data collection method. |
| AI results screen (wellness score, interactions, recommendations) | IP 2 (UI 2025002953) | The dual-engine output (AI algorithm + statistical scoring model) is presented as the colour-coded supplement cards, interaction detail cards, and personalised recommendation list. |
| Drug-supplement interaction alerts on doctor dashboard | IP 2 (UI 2025002953) | The real-time alert cards on the doctor dashboard (FR-05) are the clinical-facing delivery mechanism for the AI algorithm's expert-validated interaction detection. |
| Encrypted record upload & vault | IP 1 (PI 2025007955) | The record upload flow, vault browsing, and "encrypted" badge communicate the secure storage method to users. |
| AI clinical summary (patient profile) | IP 1 (PI 2025007955) | The AI-summarised medical history card on the patient profile and record detail screens is the direct user-facing output of the AI record summarisation method. |
| Role-based access control (navigation, visibility) | IP 1 (PI 2025007955) | The different navigation structures (patient vs doctor vs admin), hidden UI elements based on role, and "Access Denied" states all reflect the RBAC method. |
| Secure viewer (watermarked, no-copy) | IP 1 (PI 2025007955) | The view-only document viewer with watermark, disabled interactions, and timer is the front-end implementation of the secure viewer method. |
| Emergency access banner + audit notification | IP 1 (PI 2025007955) | The red "EMERGENCY ACCESS ACTIVE" banner, reason capture dialog, timer, and auto-notification are the UX of the emergency access method. |
| Audit log interface (admin) | IP 1 (PI 2025007955) | The filterable, searchable, exportable audit log view is the admin-facing embodiment of the comprehensive audit logging method. |
| AI chatbot (patient & clinician) | IP 1 (PI 2025007955) | The chatbot interface for contextual, query-based record navigation directly implements the intelligent chatbot method from IP 1. |

---

*This UX Application Flow Document defines the complete user experience for Healynx v1. All screen designs, interaction patterns, and user flows shall reference the steps and states defined herein. UX deviations require review against PRD FR-XX requirements and IP traceability in Section 9.*
