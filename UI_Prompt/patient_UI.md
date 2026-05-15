# Claude — Build Healynx Patient Mobile App (Complete End-to-End Patient UI Prompt)

You are designing a complete patient-facing mobile web app for Healynx. Produce a single, Claude-ready design prompt that results in:

- High-fidelity mobile (375px) and responsive (≥768px) frames.
- A developer handoff package: `components.md` (TypeScript prop interfaces + Tailwind token-mapped classes), `mock.json`, `accessibility.md`, and a short `README.md` with fetch examples.

Assumptions (fixed): Next.js 14 (App Router) + React 18 + TypeScript; Tailwind CSS 3.x using CSS custom properties (`--nv-` tokens defined in `doctor_UI.md`); Radix UI primitives (Dialog, Select, Tabs, Switch); TanStack Query 5.x; Zustand 4.x; Recharts 2.x; Lucide React; React Hook Form + Zod. Use `fetch` only and `Intl.DateTimeFormat` for date formatting.

Scope: describe every screen, route, primary and secondary actions (all buttons, toggles, links), keyboard shortcuts, page states, error states, loading placeholders, microcopy, and API contract shapes. Enforce the AI rules (every AI-generated text must be in an `AI Summary Card`).

Shared tokens & color guidance:

- Reference `doctor_UI.md` for the canonical `--nv-` tokens (do not hardcode hex values here). Use token names in all Tailwind class suggestions (e.g., `bg-[var(--nv-primary-600)]` or map tokens into `theme.extend.colors`).

Global UX rules (apply everywhere):

- Mobile-first: base canvas 375px, responsive behavior at 768px and 1024px.
- Accessibility: WCAG AA contrast (4.5:1 body, 3:1 large), focus ring for keyboard focus (`--nv-shadow-focus`), tappable targets ≥44×44px, aria labels for icons and landmark regions, trap focus in modals and sheets.
- Prefers-reduced-motion respected: shorten or remove non-essential animations.
- All AI outputs must use `AISummaryCard` with header `AI-GENERATED` (overline), summary body, confidence bar (0–100%), model-version footer, and `Flag Inaccuracy` action.

Global routes & file structure (complete):

```
app/
├── globals.css
├── layout.tsx                → root providers, theme handling
├── page.tsx                  → redirects to /login when unauthenticated
├── login/page.tsx
├── forgot-password/page.tsx
├── reset-password/[token]/page.tsx
├── register/page.tsx
├── register/verify/page.tsx  → OTP
├── register/consent/page.tsx
├── register/profile/page.tsx
├── (patient)/
│   ├── layout.tsx
│   ├── dashboard/page.tsx
│   ├── survey/page.tsx
│   ├── survey/[assessmentId]/page.tsx
│   ├── records/page.tsx
│   ├── records/[recordId]/page.tsx
│   ├── records/[recordId]/share/page.tsx
│   ├── records/[recordId]/history/page.tsx
│   ├── profile/page.tsx
│   ├── settings/page.tsx
│   ├── settings/appearance/page.tsx
│   ├── settings/accessibility/page.tsx
│   ├── settings/account/devices/page.tsx
│   ├── settings/account/sessions/page.tsx
│   ├── settings/account/security/page.tsx
│   ├── settings/privacy/audit-trail/page.tsx
│   ├── settings/privacy/consent-history/page.tsx
│   ├── notifications/page.tsx
│   ├── interactions/page.tsx
│   ├── medication-interactions/page.tsx
│   ├── reminders/page.tsx
│   ├── family-health/page.tsx
│   └── help/page.tsx
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
│   └── avatar.tsx
lib/
├── api-client.ts
├── cn.ts
└── validators.ts
stores/
├── auth-store.ts
└── survey-draft-store.ts
mock/
└── data.ts
```

Auth flows and pages (every button, field, states):

- `login/page.tsx`:
  - Fields: `email` (or phone) input with `label: Email or mobile number`, `autocomplete="username"`; `password` input with `label: Password`, `type=password`, `autocomplete="current-password"`, show/hide toggle (eye icon) that toggles `aria-pressed`.
  - Buttons: `Sign In` (primary, `disabled` when form invalid or loading), `Continue with SSO` (ghost), `Forgot password?` (link), `Create account` (link).
  - States: `loading` shows spinner on Sign In and disables inputs; `error` shows inline alert card `Invalid credentials. Try again or reset your password.`; `mfa_required` leads to `/login/mfa` or OTP screen.
  - Keyboard: Enter submits, Escape clears password field.

- `forgot-password/page.tsx`:
  - Field: `email_or_phone` input. Button `Send reset code` (primary). After send: show confirmation state `We sent a 6-digit code to your email/phone. Expires in 10 minutes.` with `Resend` (30s cooldown).

- `reset-password/[token]/page.tsx`:
  - Fields: `new_password`, `confirm_password` with strength meter. Buttons: `Set new password` (primary), `Back to login` (link). Validation: passwords match, strength ≥ medium.

- `register/page.tsx` & `register/verify` & `register/consent` & `profile`:
  - Register fields: `full_name`, `email`, `phone` (+ country prefix), `password` (policy hints), `agree_to_terms` (checkbox) - required.
  - OTP input: 6 single-digit boxes (auto-focus, paste support), `Resend` cooldown 30s.
  - Consent page: checkboxes `I agree to processing` (required), `I consent to anonymised research` (optional). CTA `Agree & Continue` disabled until required checked.
  - Profile: DOB (date picker), sex (select), existing conditions (multi-select), current medications (repeatable inputs), allergies. Buttons: `Save & Continue`, `Skip for now`.

Global session & MFA behavior:

- Session timeout: after idle 15 minutes show `Session Expiring` modal with countdown `2:00` and buttons `Extend session` (primary) and `Log out` (ghost). Extend calls `POST /auth/extend`.
- MFA support: TOTP flow: show `Enter 6-digit code` with `Use backup code` link. Buttons: `Verify` (primary), `Resend` (if SMS), `Use recovery code`.

Navigation & global controls (all pages):

- Offline indicator: show persistent banner at top when network unavailable: `🔴 You're offline — Changes will sync when online` with dismiss option. Cache key screens (dashboard, records list) for offline access.
- TabBar (mobile): 4 tabs — `Home` (dashboard), `Supplements`, `Records`, `Profile`. Each tab: icon + label. Active state: primary token. Sync status badge (small dot or animated icon) on Home tab when syncing; offline indicator if no network. Long-press on tab opens quick actions (e.g., long-press `Records` → `Upload new`).
- Sync status: on Dashboard, show `Last sync: {time}` + `Sync now` button. During sync, show badge on Home tab with spinner. After offline edit, show `Syncing changes...` label on affected forms.
- Top global actions (in patient layout): `Search` (magnifier leading icon) — tapping opens `SearchSheet` bottom-sheet. Keyboard shortcut: `Cmd/Ctrl+K` opens search. Notification bell icon (with unread badge) opens `/notifications` center.
- Global FAB: visible on dashboard and records — primary circular `+` opens action sheet: `Take Photo` (camera), `Upload File`, `Start Survey`.
- Global toasts: position bottom-center above TabBar. Variants: success, error, warning, info. Auto-dismiss times: 5s/10s as appropriate.
- Hamburger/menu: none on mobile; use profile tab for settings.

Detailed screens & every control (ordered by build priority):

1. Dashboard — `app/(patient)/dashboard/page.tsx` (S01)

- Top: Greeting `Good morning, {firstName}` + small `Last sync: {time}` (caption). Buttons: `Sync now` (ghost, icon), `Share health summary` (secondary) opens `ShareSheet`.
- WellnessGauge card: shows `score` (big number), `trend arrow`, `last assessment date`. Secondary buttons in card: `View details` (link), `Retake assessment` (primary-small), `View history` (link opens `/records/[recordId]/history`).
- Wellness Streaks widget (opt-in, shown if enabled in settings): badge showing `🔥 {N} day streak` with encouragement message `Keep it up! You're on a roll.` + toggle in settings to show/hide.
- Health Milestones widget (opt-in, shown if enabled in settings): horizontal scrollable badges: `✓ Completed 1st survey`, `✓ Shared 1st record`, `✓ 30-day streak`, `Invite clinician` (unlock), `Export 1st report` (unlock). Milestones are read-only achievements; toggle in settings to show/hide widget. Tap milestone to see unlock conditions.
- AISummaryCard (if present): includes `Confidence` meter, `Model` tag, `Flag` action. Footer: `Explain more` expands detailed reasoning (accordion); `Source` links to evidence.
- Supplement Tracker: list of `SupplementCard` rows. Each `SupplementCard` has actions: `Edit` (pencil), `Snooze` (clock), `Remove` (trash, destructive modal), `More` (ellipsis opens bottom sheet with `History`, `Interactions`, `Add note`).
- Upcoming Reminders card: rows with time, medication/supplement, `Mark as taken` (checkbox or button), `Snooze 10m` (menu). Reminders triggering: local notifications permission prompt.
- Recent Activity timeline: each row: icon, title, timestamp, `View` link.
- Empty state: illustration + `Complete your first supplement survey — it takes 3 minutes.` + CTA `Start survey`.

2. Supplement Survey (multi-step) — `app/(patient)/survey/page.tsx` (S02)

- Header: `Step x of 5` + `Save draft` autosave indicator.
- Steps and controls:
  - Step 1 (Supplements): repeatable form rows: `Supplement name` (autocomplete), `dosage`, `unit`, `frequency` (select), `route` (oral/topical), `start_date`, `notes`. Buttons: `Add supplement` (ghost), `Remove` (trash small). Validation: require at least one supplement.
  - Step 2 (Medications & Conditions): searchable multi-select for current meds (name + dose), existing conditions checkboxes. `Add medication` opens inline modal for full med details.
  - Step 3 (Lifestyle): dropdowns for diet, exercise, sleep, alcohol, smoking. Toggles for `Prefer not to say`.
  - Step 4 (Family Health History) (NEW): optional section capturing family medical history. Checkboxes for conditions: Diabetes, Hypertension, Heart disease, Cancer, Stroke, Kidney disease, Liver disease, Autoimmune conditions, Mental health conditions. Multi-select relatives affected: Mother, Father, Siblings, Grandparents (maternal/paternal). Optional notes field: `Any other health conditions in your family?`. Label: `This helps our AI better assess your health risks.` Skip option available.
  - Step 5 (Review): read-only cards showing entered data, `I confirm this info is accurate` checkbox (required). Buttons: `Back` (ghost), `Submit & Analyse` (primary).
- On submit: POST `/assessments` returns `{ task_id }`. Navigate to `/survey/{assessmentId}?taskId=xxx`. Poll `GET /tasks/{taskId}` every 500ms.
- Autosave: store draft in `survey-draft-store` and `localStorage`. Show `Draft saved` toast.

3. AI Analysis Loading + Results (S03)

- Loading: staged progress indicator with 6 steps (validate → compute drug interactions → compute supplement interactions → scoring → recommendations → complete). Show linear progress bar and cancel button (cancel cancels background task via `DELETE /tasks/{taskId}`). Minimum display 1.5s.
- Results: animated WellnessGauge, RecommendationList (each recommendation row: icon, text, severity, `Discuss with doctor` CTA), **MedicationInteractionAlerts** (NEW, severity chips: High/Moderate/Low — detects conflicts between medicines from different doctors), InteractionAlerts (supplement interactions), SupplementCards (with suggested changes). Buttons: `Share with clinician` (primary), `Save changes` (secondary), `Export PDF` (ghost).
- **NEW: Medication Conflict Detection**: If AI detects drug-drug conflicts (e.g., Patient A takes Metformin from Doctor 1 AND Glipizide from Doctor 2, which both lower blood glucose — synergistic risk), show alert card:
  - Alert header: `⚠️ Multiple medications may interact`
  - Affected drugs listed: "Metformin (prescribed by Dr. Smith, 5 Jan)" + "Glipizide (prescribed by Dr. Patel, 10 Jan)"
  - Severity badge: High/Moderate with explanation (e.g., "Both drugs lower blood sugar — risk of hypoglycemia")
  - Action button: `Schedule follow-up with your latest doctor` (opens share-with-clinician modal pre-filled with conflict details)
  - Dismiss option: checkbox at bottom, enabled after doctor reviews; label: "Doctor confirmed they reviewed this"

4. Records Vault (S04)

- List page: search input with filters (type chips: All, Lab Reports, Imaging, Prescriptions, Assessments). Each record row: icon, title, date, `AI summary snippet` (sparkle), `Share` (link icon), `More` (menu: `Download`, `Share`, `Version history`, `Delete`). Long-press row shows `Delete record` (destructive) with confirmation modal.
- Upload flow (`file-upload.tsx`): drag area / tap to select, validation (type + size), client-side encryption step (show `Encrypting...` progress), upload progress bar, `Cancel upload` button. On success: show toast `Record uploaded and encrypted — AI summary is being generated.` and display placeholder `AI summary: generating...` until ready.
- Record detail: header with title, date, source, `AI Summary Card` (if AI processing complete), `View document` (opens secure viewer), `Share` (opens share modal with template options), `Download` (if permitted), `Clinician feedback` badge (if doctor added notes), `Add note` (bottom sheet). Buttons states: `Download` disabled if `view-only` share.
- **NEW: Version history** — `app/(patient)/records/[recordId]/history/page.tsx` shows timeline: previous supplements in record, past wellness scores side-by-side, comparison view (diff highlight), export each version. Header: `{recordTitle} history`, back link, export all button. Timeline entries show date, version label (e.g., "v1", "v2"), changes badge (e.g., "3 supplements added"). Tap entry to see full diff.
- **NEW: Clinician feedback** — if shared record has doctor annotation, show dismissible badge `👨‍⚕️ Doctor added a note` + expandable section with read-only note display, timestamp, doctor name, small avatar. Feedback is attached to share event, not the record itself. Patient can reply via `Share` screen or `Discuss with doctor` link.
- **NEW: Selective deletion** — long-press record shows `Delete this record` (destructive modal: "Are you sure? This action cannot be undone." with confirm button). Alternatively, in `More` menu: `Delete` option. Deletion is soft (record archived/hidden) or hard based on backend policy. User receives confirmation toast: `Record deleted`. Different from account deletion; user can delete individual records without account impact.

5. Record Share (S05)

- Share modal/sheet fields: `recipient` (search connected clinicians or enter email), `permissions` toggles: `View` (default ON & locked for owner), `Download` (off), `Forward` (off), `Expiry` presets (24h/7d/30d/custom), `Require PIN` (toggle) + `Set PIN` (6-digit input), `Message` (optional note).
- **NEW: Shareable summary templates** — radio group with presets: `Full record`, `Supplement list only`, `AI summary only`, `Interaction alerts only`, `3-month wellness report` (generates filtered view). Select template before generate. Template descriptions in gray text beneath each option (e.g., "Supplement list only" → "Shows only supplement name, dose, frequency").
- Buttons: `Generate Secure Link` (primary), `Cancel` (ghost). After generate: show link with `Copy` button (copy + `Copied!` toast), `Share via` quick actions (WhatsApp, Email), `Revoke link` destructive (requires confirmation), `Audit entry` shown with timestamp and actor.

6. Secure Viewer — `app/records/[recordId]/page.tsx` or `app/share/[token]/page.tsx`

- Minimal chrome, dark surround, watermark overlay containing `{recipient} | {datetime} | {ip}` rotated. Anti-copy behaviors: `user-select:none`, prevent right-click, intercept Ctrl/Cmd+C/P/S. Show `Access timer` bar with remaining time and `Request extension` link. Buttons: `Report issue` (link), `Close` (top-right). If PIN required: show `Enter 6-digit PIN` before viewing (3 attempts max, then lockout).

7. Profile & Settings (S07)

- Profile page: display `avatar`, `full_name`, `email`, `phone`, `DOB`, `emergency_contact` (editable). Buttons: `Edit` (pencil), `Save` (primary), `Cancel` (ghost). Inline validations with friendly messages.
- Settings page: tab or accordion structure with sections (on mobile: vertical sections; on tablet/desktop: side navigation tabs). Main sections:

  **Appearance:**
  - Theme toggle: radio group `System default`, `Light`, `Dark` (stored in `localStorage` and theme context).
  - Each theme option shows preview swatch (small color palette sample).
  - Button: `Apply theme` (changes immediately; no save button needed).
  - Label: "Light mode: optimal for bright environments. Dark mode: easier on eyes at night."

  **Accessibility:**
  - Text size scaling: radio group `Small`, `Normal`, `Large`. Live preview of text sizes in sample area (e.g., "The quick brown fox" in each size).
  - High-contrast mode toggle: `Enable high-contrast colors` (applies darker borders, stronger color tokens). Label: `Improves visibility in bright sunlight`.
  - Dyslexia-friendly font toggle: `Use OpenDyslexic font` (loads web font, applies to body). Label: `Optimized letter shapes for easier reading`.
  - Reduce-motion toggle: `Respect "reduce motion" preference` (applies `prefers-reduced-motion: reduce` rules globally). Label: `Disables animations and transitions`.
  - Focus indicator: shows visual example of keyboard focus ring (color: `--nv-shadow-focus`). Label: "Example focus ring for keyboard navigation".

  **Account:**
  - `Change password` (link opens modal: current password field, new password field + strength meter, confirm password field).
  - `Enable MFA` (toggle → setup flow).
  - **Login security checklist** (NEW): read-only status cards with checkmarks:
    - `✓ Password set` (always true for logged-in user).
    - `✓ MFA enabled` (toggle indicator: check or empty circle). Tap to enable/disable MFA.
    - `○ Biometric login` (suggested future feature, disabled, gray text: "Set up in your device settings").
  - **Manage devices** (NEW) — link to `settings/account/devices/page.tsx`: list of active devices (browser/app). Table columns: Device type (Chrome on macOS, Safari on iPhone), IP address (truncated for privacy), Last active (relative time), Actions: `Sign out` (destructive). Show device with checkmark if current device. Bulk action: `Sign out from all other devices` (requires confirmation).
  - **Manage sessions** (NEW) — link to `settings/account/sessions/page.tsx`: list of active sessions. Each session: device type (icon + name), IP (truncated), location (approximate, e.g., "San Francisco, CA" based on IP), last active (relative time), session start time. Actions: `Sign out this session` (destructive), or in `More` menu. Bulk action: `Sign out everywhere else` (requires re-auth modal with password).
  - **Security checklist** (NEW) — link to `settings/account/security/page.tsx`: shows: recovery codes (status badge: "Generated {date}", `Download codes` button), password strength indicator (bar + label: weak/normal/strong), recent login attempts (if suspicious activity detected, show warning banner with device details). Buttons: `Download recovery codes` (PDF), `View login activity` (opens expanded list with timestamps, devices, IPs).
  - `Logout everywhere` button (danger text, opens confirmation modal).

  **Notifications:**
  - Toggles with descriptions: `Important alerts (SMS)` (on), `Reminders (push)` (on), `Product updates (email)` (off).
  - Each toggle: icon + label + description (e.g., `Important alerts (SMS)` → `Urgent medication changes, severe interactions`).
  - Buttons: `Save preferences` and `Cancel` (or auto-save on toggle with toast "Preferences saved").

  **Privacy & Data:**
  - `Download my data` button (opens modal with format options):
    - Radio group: `CSV (spreadsheet)`, `PDF (formatted report)`, `JSON (raw data)`.
    - Label: `Download all your health data in your chosen format. File will be emailed when ready.`
    - Button: `Download` (initiates background task, shows "Download queued" toast, sends file when ready via email notification).
  - **Data export history** (NEW): collapsible section showing past exports (date, format, file size, status: completed/pending, `Download` link if still available).
  - **Audit trail** (NEW) — link to `settings/privacy/audit-trail/page.tsx`: read-only timeline view:
    - Events: `Logged in` (device type, IP), `Password changed`, `MFA enabled/disabled`, `Shared record [title]`, `Revoked share [title]`, `Data exported [format]`, `Emergency access requested`.
    - Each event row: timestamp (absolute + relative), actor ("You"), device/IP (truncated), action description. Expandable rows to see full IP and details.
    - Filter chips: `All`, `Account`, `Sharing`, `Data`, `Security`. Default: `All`.
    - Export button: `Download audit log as PDF/CSV` (generates file).
    - Pagination: show 20 events per page, `Load more` button or infinite scroll.
  - **Consent history** (NEW) — link to `settings/privacy/consent-history/page.tsx`: read-only timeline:
    - Entries: agreement date, privacy policy version, data processing consent (checked/unchecked), research opt-in (checked/unchecked), digital signature or checksum (for audit).
    - Each entry row: date (absolute), policy version link (e.g., "v2.1"), `View policy` (opens PDF in new tab), `View detailed consent` (expandable to show all checkboxes at time of agreement).
    - Withdraw consent section: warning box `Withdrawing consent may limit app functionality. Are you sure?` with acknowledge checkbox + `Withdraw consent` button (destructive, shows confirmation modal).
  - `PDPA consent` checkboxes (existing).
  - `Research opt-in` toggle (existing).
  - `Delete my account` (destructive flow requiring identity verification: OTP + type `DELETE` to confirm).

  **Connected clinicians:**
  - List of clinicians with access to patient data.
  - Each clinician: avatar, name, email, status (active/pending/revoked), date connected, `Revoke access` button (confirmation modal: "Are you sure? {name} will lose access to your records.").
  - Add clinician: `Invite clinician` action/button (opens sheet: search by email, send invite).

  **Billing & subscription** (if applicable):
  - `Manage subscription` link to external billing provider.
  - `View invoices` list (date, amount, status, `Download PDF`, `Send receipt` email).
  - Upgrade/downgrade plan link.

8. Help & Support

- Help page: searchable FAQs, `Contact support` (open message sheet), `Report an issue` with attachments. Buttons: `Send` (primary) with reply expected time estimate. Auto-attach logs (with permission).

9. Notification Center — `app/(patient)/notifications/page.tsx` (S09 NEW)

- Header: `Notifications` with back link or close button (if opened as sheet).
- List of all notifications (alert, reminder, update, message) with read/unread states.
- Each notification row: icon (type indicator: bell for alert, clock for reminder, megaphone for update), title, brief summary, timestamp (relative), read indicator (dot or visual). Swipe left: `Archive` (move to history). Swipe right: `Delete` (destructive, confirmation). Tap to expand or see full detail.
- Filter tabs: `All`, `Unread`, `Alerts`, `Reminders`, `Updates`. Active tab: underline.
- Notification history section: collapsible `Show archived` to reveal dismissed notifications (grayed out text, smaller font).
- Empty state: `You're all caught up! No new notifications.` with illustration.
- Sort options (dropdown in header): `Newest first`, `Oldest first`, `Unread first`.
- Bulk action: `Mark all as read` button (if unread items present). Confirmation: "Mark {N} items as read?" with buttons.
- Notification settings link: `Manage notification preferences` (link to settings/notifications).

10. Interaction History — `app/(patient)/interactions/page.tsx` (S10 NEW)

- Header: `Interaction History` with back link or close button.
- Timeline view: past and current supplement interactions detected by AI.
- Each entry: severity badge (High/Moderate/Low with color tokens — red/orange/yellow), interaction description (e.g., "Vitamin K may reduce Warfarin effectiveness"), affected supplements (chip list: "Vitamin K", "Warfarin"), date detected (relative + absolute), `Learn more` link (expands explanation, shows evidence if available).
- Filter: `All`, `High severity`, `Moderate`, `Low`. Default: `All`.
- Actions per item: `Mark as reviewed` (removes from dashboard alert, changes opacity/strikethrough), `Discuss with doctor` (opens share modal for that record), `Dismiss` (hides from timeline).
- Empty state: `No interactions detected. Keep up with your supplementing!` with illustration.
- Sort options: `Newest first`, `Severity (High first)`.
- Pagination or infinite scroll for large lists.

11. Medication Interactions Checker — `app/(patient)/medication-interactions/page.tsx` (S11 NEW)

- Patient-facing medication safety tool: allows patient to enter any medications and check for interactions (not limited to current medications).
- Header: `Check medication interactions` with info icon explaining purpose.
- Input form: repeatable rows for medication name (autocomplete from known drugs database), dosage, frequency. Buttons: `Add medication` (ghost), `Remove` (trash).
- Analyse button: `Check for interactions` (primary, calls `POST /medication-interactions/check`).
- Results: table of all pairwise interactions detected:
  - Columns: Drug 1, Drug 2, Interaction type, Severity badge, Description, `Learn more` link.
  - Color-coded severity: Red (High), Amber (Moderate), Green (Low/Informational).
  - Expandable rows for detailed explanation, mechanism, clinical recommendations (e.g., "Monitor for low blood sugar. Consider dose adjustment.").
  - Actions: `Save to my medical records` (saves checked meds snapshot with timestamp), `Discuss with doctor` (opens share-with-clinician flow, pre-fills interaction details).
- Empty state: `Add medications to check interactions.`
- Footer disclaimer (red/warning box): `This tool is for educational purposes only. Always consult your doctor or pharmacist before starting, stopping, or changing medications.`
- Historical checks: section showing past interaction checks with timestamps, ability to re-run same check.

12. Smart Medication Reminders — `app/(patient)/reminders/page.tsx` (S12 NEW)

- Header: `Medication & Supplement Reminders`.
- Existing reminders list (from FR-15) with collapsible detail sections.
- For each reminder, show:
  - Medication/supplement name (bold)
  - Time of day (e.g., "8:00 AM")
  - Days of week (e.g., "Mon, Wed, Fri")
  - Dosage instructions (e.g., "One 500mg tablet")
  - **NEW: Context-aware reminder options** — toggles with labels:
    - `Take with food` → patient receives notification: "Time to take {med} with a meal or snack"
    - `Take on empty stomach` → "Time to take {med}. Best on an empty stomach: take 1 hour before or 2 hours after eating"
    - `Avoid alcohol` → "Time to take {med}. ⚠️ Do not consume alcohol with this medication"
    - `Avoid dairy/caffeine` → "Time to take {med}. Avoid dairy products and caffeine for 2 hours"
    - `Take in morning only` / `Take in evening only` → restricts reminder to specific time window
    - `Track side effects` → optional field to specify what to monitor (e.g., "dizziness", "nausea") and adds reminder: "How are you feeling? Any {symptom} today?"
  - Notification type: multi-select `Push`, `SMS`, `Email`
  - Edit button: expands inline editing; Save button applies changes
  - Delete button: destructive, confirmation modal
- Add new reminder: `+ Add reminder` button opens form with same fields as above.
- **Reminder preview section**: shows exact notification text patient will receive, updates in real-time as user changes settings.
- Adherence summary: card showing medication adherence over past 7/30 days with percentage, chart, and `Improve adherence` link to tips.
- Bulk actions: `Edit all reminders` button (modal allowing apply same settings — time, context, notification type — to multiple reminders at once).

13. Family Health History — `app/(patient)/family-health/page.tsx` (S13 NEW)

- Header: `Family Health History` with info icon: "This helps our AI understand your inherited health risks."
- Form with sections:
  - **Section 1: Family conditions by relative**. Table rows for: Mother, Father, Sibling 1, Sibling 2, etc. (with `+ Add sibling` button). Columns: Relative name, Conditions (multi-select chips: Diabetes, Hypertension, Heart disease, Cancer, Stroke, Kidney disease, Liver disease, Autoimmune, Mental health, Other). Optional column: Age at diagnosis (number input, placeholder: "e.g., 45").
  - **Section 2: Family health summary** — textarea, label: `Any other important family health information?` (optional).
  - **Section 3: Your ancestry/ethnicity** — info: "Some health risks vary by genetic background." Options: Southeast Asian, South Asian, East Asian, European, African, Middle Eastern, Other (optional; helps AI tailor risk assessment).
- Buttons: `Save family health` (primary, saves to `POST /family-health`), `Cancel` (ghost).
- Privacy callout box (gray background): `Your family health information is kept confidential and encrypted. It is used only to personalise your health recommendations and is never shared with anyone without your consent.`
- Last updated timestamp at bottom: "Last updated: {date} · Edit anytime"

Microcopy — every button & small text (examples, use throughout):

- Primary CTAs: `Sign In`, `Sign Up`, `Submit & Analyse`, `Generate Secure Link`, `Save`, `Confirm`, `Start survey`, `Upload`.
- Secondary: `Cancel`, `Back`, `Edit`, `View details`, `Share`, `Copy link`.
- Destructive: `Delete`, `Remove`, `Revoke link`, `End Emergency Access` (red, confirm modal).
- Inline helper text: `Passwords must be 8+ chars, with a number and a capital letter.`
- Inline success: `Changes saved` toast.

Component-level props & button specs (require Claude to output full TS interfaces):

- `ButtonProps`: `{ variant?: 'primary'|'secondary'|'ghost'|'danger'|'link'; size?: 'sm'|'md'|'lg'; loading?: boolean; disabled?: boolean; icon?: ReactNode; ariaLabel?: string }` — Loading shows spinner, disabled applies `opacity-40 cursor-not-allowed`.
- `InputProps`: `{ label: string; name: string; type?: 'text'|'email'|'password'|'tel'|'search'; placeholder?: string; required?: boolean; error?: string; icon?: ReactNode; autoComplete?: string }` — For password show strength meter.
- `AISummaryCardProps`: `{ summary: string; confidence: number; modelVersion: string; sources?: string[]; onFlag?: () => void }` — Renders overline `AI-GENERATED`, summary, confidence bar with color thresholds, model tag, and `Flag Inaccuracy` link.
- `FileUploadProps`: `{ accept: string[]; maxSizeMB: number; onUpload: (file) => Promise<void> }` — Show client-side encryption step and progress.

Keyboard shortcuts (patient app):

- `Cmd/Ctrl+K` → Open global search
- `G` then `D` → Go to Dashboard
- `G` then `R` → Go to Records
- `G` then `N` → Go to Notifications
- `G` then `M` → Go to Medication Interactions checker
- `?` → Open shortcuts help sheet

APIs (surface-level contracts — require Claude to expand in output):

- `POST /auth/login` { email, password } → 200 `{ token, mfa_required?: boolean }` or 401
- `POST /auth/mfa/verify` { code, temp_token } → 200 `{ token }`
- `POST /assessments` → 202 `{ task_id }`
- `GET /tasks/{taskId}` → `{ status: 'running'|'completed'|'failed', progress: number, resultUrl?: string }`
- `GET /patients/me/dashboard` → `PatientDashboard` JSON (mock) — includes streaks, milestones, sync timestamp, medication conflicts
- `POST /records` (multipart) → 201 `{ recordId }` (server starts AI analysis)
- `POST /share` { recordId, recipients[], permissions, expiry, requirePin, template? } → 201 `{ shareToken, expiresAt }`
- `POST /auth/extend` → 200 `{ renewedExpiresAt }`
- `GET /records/{recordId}/history` → `{ versions: [{timestamp, supplements, score, ...}] }` (paginated)
- `GET /notifications` → `{ items: [{ id, type, title, summary, timestamp, read }], unreadCount }` (paginated)
- `POST /notifications/{notificationId}/read` → 204
- `POST /notifications/{notificationId}/archive` → 204
- `GET /audit-trail` { filter?: 'account'|'sharing'|'data'|'security', page?, limit? } → `{ events: [{ timestamp, action, details, device, ip }], total, pageCount }` (immutable, paginated)
- `GET /interactions` → `{ items: [{ severity, description, supplements, date, reviewed }] }` (paginated)
- `POST /data-export` { format: 'csv'|'pdf'|'json' } → 202 `{ exportId, status, estimatedTime }` (async job)
- `GET /data-exports` → `{ exports: [{ id, format, createdAt, status, fileUrl? }] }` (history)
- `GET /devices` → `{ devices: [{ id, type, ip, lastActive, isCurrent, location? }] }`
- `POST /devices/{deviceId}/logout` → 204 (revoke device)
- `POST /devices/logout-all-others` → 204 (revoke all except current)
- `GET /sessions` → `{ sessions: [{ id, device, ip, location, startTime, lastActive }] }`
- `POST /sessions/{sessionId}/logout` → 204 (revoke session)
- `POST /sessions/logout-all-others` → 204 (requires re-auth)
- `GET /preferences` { keys: ['theme', 'textSize', 'highContrast', 'dyslexicFont', 'reduceMotion', 'showStreaks', 'showMilestones']? } → `{ theme, textSize, highContrast, dyslexicFont, reduceMotion, showStreaks, showMilestones, ... }`
- `POST /preferences` { theme?, textSize?, highContrast?, dyslexicFont?, reduceMotion?, showStreaks?, showMilestones? } → 200 (persist preferences)
- `GET /consent-history` → `{ entries: [{ date, policyVersion, processingConsent, researchOptIn, signature? }] }`
- `POST /consent/withdraw` { reason?: string } → 200 (user withdraws data processing consent; warning: limits functionality)
- **NEW: Drug-Drug Interaction Detection (v1 Enhancement):**
  - `POST /medication-interactions/check` { medications: [{ name, dosage, frequency }] } → 200 `{ interactions: [{ drug1, drug2, type, severity, description, mechanism, recommendation }], overallRiskScore }` (check for all pairwise drug-drug interactions; used by medication interaction checker tool)
  - `GET /patients/me/medication-conflicts` → `{ conflicts: [{ drugA, drugB, prescriber1, prescriber2, dateA, dateB, severity: 'High'|'Moderate'|'Low', explanation, recommendation }], lastDetected }` (returns detected multi-doctor med conflicts from patient's current medication list)
  - `POST /medication-conflicts/{conflictId}/acknowledge` { clinicianId?, clinicianNotes? } → 204 (doctor acknowledges conflict review; adds clinician note to record)
- **NEW: Family Health History (v1 Enhancement):**
  - `POST /family-health` { familyMembers: [{ relation, conditions: [], diagnosisAge? }], additionalNotes?, ancestry? } → 200 `{ familyHealthId, createdAt }` (persist family health history)
  - `GET /family-health` → `{ familyMembers, additionalNotes, ancestry, lastUpdated }` (retrieve family health history)
  - `PUT /family-health` { ...update fields } → 200 (update family health)
- **NEW: Smart Medication Reminders (v1 Enhancement):**
  - `POST /reminders` { medicationId, time: 'HH:MM', daysOfWeek: [0-6], dosage, context: ['takeWithFood', 'avoidAlcohol', 'trackSideEffects']?, trackingLabel?, notificationType: ['push'|'sms'|'email'] } → 201 `{ reminderId, nextFireTime }` (create reminder with context-aware options)
  - `GET /reminders` → `{ reminders: [{ id, medication, time, daysOfWeek, context, adherenceRate7d, adherenceRate30d, nextFireTime }] }` (list all reminders with adherence metrics)
  - `PUT /reminders/{reminderId}` { ...fields } → 200 (update reminder settings)
  - `DELETE /reminders/{reminderId}` → 204 (delete reminder)
  - `GET /reminders/{reminderId}/adherence` { days: 7|30|90 } → `{ adherencePercentage, adherenceTrend, missedDates: [{date, reason?}], takenDates: [{date, confirmedBy: 'app'|'manual'}] }` (adherence history and stats)

Error handling patterns (consistent microcopy & UX):

- Generic network: show inline `Something went wrong. Check your connection and try again.` + `Retry` button.
- Validation errors: field-level messages in `danger-600` with `aria-describedby` linking.
- Upload errors: `Upload failed (file too large). Try a smaller file or compress.` with `Retry` and `Report issue`.

Security notes to include in prompt:

- Client-side encryption before upload (explain UX: `Encrypting...` progress, mention where keys are stored: client key derived then uploaded encrypted).
- Share links must be JWT signed, time-limited, and optionally PIN-protected.
- Audit log: every share/generate/revoke/emergency action appends an immutable audit entry with actor, timestamp, IP, and reason.

Emergency & escalation flows (patient view and notes):

- Patient can `Request emergency access` when they want urgent clinician access; this opens a small modal explaining implications and collects `reason` and an optional `phone` for call-back. Button `Request Emergency` (primary) submits to `/emergency-requests` and shows `Request submitted` toast with expected next steps.

Testing & QA checklist (include in prompt output):

- Verify keyboard navigation across all modals and sheets.
- Verify color contrast on both themes.
- Simulate slow network for uploads and AI analysis (10s delay) and verify loading states.
- Verify session expiry modal and re-auth flow.

Developer handoff output requirements for Claude (explicit):

1. `components.md` — List each component with TS props, default classNames (Tailwind using token refs), states (hover, focus, disabled), accessibility notes, and one inline example of usage.
2. `mock.json` — include full example objects: `PatientDashboard`, `Assessment`, `TaskStatus`, `Record`, `ShareLink`.
3. `accessibility.md` — checklist and keyboard interactions per screen.
4. `README.md` — top-level instructions: where to drop tokens (`globals.css`), how to wire `AISummaryCard` component, and example fetch calls.

Output format instruction (strict): ask Claude to deliver the handoff as structured JSON with keys: `frames` (array of screen frame metadata), `components` (markdown string), `mock` (JSON object), `accessibility` (markdown string), `readme` (markdown string). Also provide a short prioritized build plan (3–5 day sprint) with recommended order and estimated points.

Finish prompt with a short developer checklist and ask Claude to summarize any areas requiring product approval (e.g., emergency policy wording, data retention durations).

---

End of comprehensive patient UI design prompt.
End of patient UI design prompt.
