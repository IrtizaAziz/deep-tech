# Healynx — Doctor & Clinic Desktop Design

**Audience:** Doctor (desktop-first, 1440px canvas) + Clinic Admin (desktop)
**Source IPs:** PI 2025007955 (Secure Medical Records) + UI 2025002953 (Supplement Intake Assessment)

---

## Design Tokens

### Colour Palette

**Primary — Deep Teal** (brand, buttons, selected states):
```
--nv-primary-900: #0D3B3B | 800: #125C5C | 700: #1A7A7A | 600: #1E8C8C | 500: #26A6A6 | 400: #3DBEBE | 300: #6ED4D4 | 200: #A3E5E5 | 100: #D4F3F3 | 050: #EDFAFA
```

**Secondary — Soft Blue** (AI content, chatbot):
```
--nv-secondary-700: #1E4D8C | 600: #2563B8 | 500: #3B82D9 | 300: #7EB3F0 | 100: #DCE9FA | 050: #F0F5FD
```

**Accent — Warm Amber** (alerts, caution):
```
--nv-accent-700: #8C5C1E | 600: #D98C2E | 500: #EDA83E | 100: #FCF0DA | 050: #FEF9F2
```

**Semantic:**
- Success Green: base `#2D8C4A` | bg `#D9F2E2` — safe
- Danger Red: base `#D92D2D` | bg `#FCE0E0` | dark `#8C1A1A` — emergency, high risk
- Info Blue: base `#3B82D9` | bg `#DCE9FA`

**Neutrals:**
```
900 #111827 | 800 #1F2937 | 750 #283040 | 700 #374151 | 600 #4B5563 | 500 #6B7280 | 400 #9CA3AF | 300 #D1D5DB | 200 #E5E7EB | 150 #EEF0F3 | 100 #F3F4F6 | 050 #F9FAFB | 000 #FFFFFF
```

### Typography

| Token | Font | Size | Weight | Line | Usage |
|---|---|---|---|---|---|
| h1 | Plus Jakarta Sans | 32px | 700 Bold | 1.2 | Page titles |
| h2 | Plus Jakarta Sans | 24px | 700 Bold | 1.3 | Section headings |
| h3 | Plus Jakarta Sans | 20px | 600 SemiBold | 1.3 | Card titles |
| h4 | Plus Jakarta Sans | 18px | 600 SemiBold | 1.35 | Subsections |
| h5 | Inter | 16px | 600 SemiBold | 1.4 | Minor headings |
| h6 | Inter | 13px | 600 SemiBold | 1.4 | KPI labels, uppercase, ls:1px |
| body | Inter | 16px | 400 Regular | 1.6 | Default |
| body-sm | Inter | 14px | 400 Regular | 1.5 | Metadata |
| caption | Inter | 12px | 400 Regular | 1.5 | Timestamps |
| overline | Inter | 11px | 600 SemiBold | 1.4 | Badges, uppercase |
| mono | JetBrains Mono | 14px | 450 Medium | 1.5 | Lab values, timer |
| mono-sm | JetBrains Mono | 12px | 450 Medium | 1.5 | IDs, model versions |

### Spacing, Radius, Shadows — same as mobile token set.

---

## Desktop Grid

| Breakpoint | Columns | Gutters | Max Width | Navigation |
|---|---|---|---|---|
| 1024–1439px | 12 | 24px | 1280px centred | Persistent sidebar 256px |
| 1440px+ | 12 | 24px | 1280px centred | Persistent sidebar 256px |

Content area = remaining width after sidebar. Multi-column layouts use the 12-column grid within content area.

---

## Navigation — Sidebar

```
┌──────────────────────────────┐
│  ┌──────┐                    │
│  │ Logo │  Healynx           │
│  └──────┘                    │
│  ────────────────────────────│
│                              │
│  🏠  Dashboard               │
│  👥  Patients                │
│  ⚠️  Alerts             [3]  │  Count badge
│  🤖  AI Chatbot              │
│  🔒  Emergency Access        │
│  📊  Reports                 │
│  ⚙️  Settings                │
│                              │
│  ────────────────────────────│
│                              │
│  ┌──┐                        │
│  │DA│  Dr. Amira             │
│  └──┘  General Practitioner  │
│         Klinik Amira         │
│                   [🚪 Logout]│
└──────────────────────────────┘
```

- Width: 256px (expandable to 64px icon-only on toggle)
- Background: `neutral-050` light / `neutral-900` dark
- Active item: `primary-050` bg, `primary-600` text, 3px left `primary-600` border
- Hover: `neutral-100` bg
- Icons: 20px. Labels: body-sm. Items: 48px height.
- Alerts item shows red count badge.

---

## Top Bar

```
┌──────────────────────────────────────────────────────────────┐
│  🔍 Search patients, records... (Ctrl+K)    🔔 [3]  👤 DA  ⚙️│
└──────────────────────────────────────────────────────────────┘
```

- Height: 56px. Background: white / `neutral-800` dark. Bottom border: 0.5px `neutral-200`.
- Global search triggers modal on Ctrl+K.
- Notification bell with unread count badge.
- User avatar with dropdown menu.

---

## Doctor Components

### Button

Same variant/size system as mobile. Not full-width on desktop (intrinsic width).
Sizes: sm 32px, md 40px, lg 48px.

### Input / Select / Toggle / Checkbox / Radio

Same as mobile specs. Desktop touch targets: 40×40px minimum.

### Card

Same variants as mobile. Additional: interactive (hover: shadow-sm + border `primary-300`).

### Modal

Centred on screen, max 560px (narrow) / 720px (wide). Backdrop: `rgba(0,0,0,0.5)` blur 4px.
Close: X, Escape, backdrop click. Enter: fade 200ms + scale 0.95→1.0. Focus trapped.

### Toast

Position: top-right. Same variants and durations as mobile.

### Badge / Chip / Tooltip

Same as mobile.

### Data Table

```
┌──────────────────────────────────────────────────────────────────┐
│  🔍 Search...   Filter: [Severity ▼] [Date ▼]          [Export] │
│                                                                  │
│ ┌────────┬────────┬──────────┬──────────┬────────────────┬─────┐│
│ │Patient▲│Score ▼│Alerts   ▼│Last Visit│AI Summary      │     ││
│ ├────────┼────────┼──────────┼──────────┼────────────────┼─────┤│
│ │Rajan K.│ 72 ▲+14│🔴2       │Sep 15    │DM2,HTN,3 supps │ [→] ││
│ │Priya R.│ 85     │🟢0       │Sep 14    │Hypothyroid     │ [→] ││
│ │Ahmad I.│ 45 ▼-18│🔴1       │Sep 12    │CKD St3, DM2    │ [→] ││
│ └────────┴────────┴──────────┴──────────┴────────────────┴─────┘│
│                        ← 1 2 3 ... 12 →    [50 per page]        │
└──────────────────────────────────────────────────────────────────┘
```

- Header: `neutral-100` bg, h6 font, sortable (▲▼ arrows)
- Row: 52px height, 0.5px `neutral-200` bottom border
- Hover: `neutral-050` bg. Selected: `primary-050` bg.
- Pagination at bottom.

### Timeline

Vertical line 2px `neutral-200`. Dots 12px (filled primary for recent, outlined for older). Cards indented 24px.

### AI Summary Card

**Mandatory marker for all AI content:**
```
┌──────────────────────────────────────────────────────────┐
│  ✦ AI-GENERATED CLINICAL SUMMARY            [Flag Error] │
│                                                          │
│  [Summary text...]                                       │
│                                                          │
│  ────────────────────────────────────────────────────    │
│  Confidence: ████████████░░ 89%   Model: summarise_v2.1  │
└──────────────────────────────────────────────────────────┘
```
- `secondary-050` bg, `secondary-300` border
- Confidence bar: 4px, green ≥80%, amber 60–79%, red <60%
- All AI content MUST use this card. No exceptions.

### Wellness Gauge — Compact

Used in table cells and summary cards:
```
┌──────────┐
│ 72  ▲+14│  36px number
│ ─────── │  mini arc
│  /100   │
└──────────┘
```

### Alert Banner

Variants: info (blue), warning (amber), critical (red).

**Emergency Active Banner** (sticky top, non-dismissible):
```
┌──────────────────────────────────────────────────────────────────┐
│  🔴 EMERGENCY ACCESS ACTIVE — ALL ACTIONS LOGGED    ⏱️ 58:23     │
│  Patient: Rajan a/l Kumar · Reason: ER admission                │
│                                               [End Emergency]    │
└──────────────────────────────────────────────────────────────────┘
```
- Full-width, sticky top, z-index highest
- `danger-700` bg, white text
- Mono font timer counting down
- Cannot dismiss — must click End or expire

### Access Timer Bar

4px height. Fill: `primary-600` (>24h), `accent-600` (<24h), `danger-600` (<1h). Text above: body-sm + mono.

### Secure Document Viewer

```
┌──────────────────────────────────────────────────────────────────┐
│  🔒 Secure Viewer                     ⏱️ Expires in 6d 23h 58m  │
├──────────────────────────────────────────────────────────────────┤
│                                                                  │
│  ┌────────────────────────────────────────────────────────────┐  │
│  │                                                            │  │
│  │  cardiologist@hospital.com | Sep 16, 2026 | 203.106.12.45 │  │
│  │                                                            │  │
│  │              [DOCUMENT CONTENT]                             │  │
│  │                                                            │  │
│  │  cardiologist@hospital.com | Sep 16, 2026 | 203.106.12.45 │  │
│  │                                                            │  │
│  └────────────────────────────────────────────────────────────┘  │
│                                                                  │
│  ⚠️ View-only. Copy, download, print, screenshots prohibited.    │
│  All access is logged.                   [Request Extension]     │
└──────────────────────────────────────────────────────────────────┘
```

- Dark surround (`neutral-900` bg). Document area: white, centred, max 900px.
- Watermark: diagonal repeating `{recipient} | {datetime} | {ip}` at 12% opacity, `neutral-600`.
- Anti-copy: no right-click, block Ctrl+C/S/P, no print.
- 15-min inactivity timeout overlay.
- Emergency mode: red-tinted watermark + "EMERGENCY ACCESS" prefix.

### Empty State

Centred illustration (line-art) + title + description + CTA button.

### Chatbot Panel

Side panel, right 30% of content area, resizable. Header: 🤖 bot icon + "AI Clinical Assistant". Messages: user (right, `primary-600` bg) / AI (left, `secondary-050` bg). AI responses show sparkle icon + source citations + medical disclaimer. Input at bottom with suggested queries.

---

## Screen S06 — Doctor Dashboard

```
┌─────────┬──────────────────────────────────────────────────────────────┐
│ Sidebar │  Healynx — Doctor    🔍 Ctrl+K   🔔 [3]   👤 DA   ⚙️       │
│ 256px   │  ────────────────────────────────────────────────────────────│
│         │                                                              │
│ 🏠 Dash │  Good afternoon, Dr. Amira                    📅 Sep 16, 2026│
│ 👥 Pat. │                                                              │
│ ⚠️ Alert│  ┌───────────────┬───────────────┬─────────────────────────┐ │
│ 🤖 Chat │  │Total Patients │Pending Reviews│Active Alerts            │ │
│ 🔒 Emer │  │     247       │      12       │🔴3  🟡8  🟢156          │ │
│ 📊 Rep. │  └───────────────┴───────────────┴─────────────────────────┘ │
│ ⚙️ Set. │                                                              │
│         │  ┌─────────────────────────────────┐  ┌───────────────────┐  │
│         │  │ ⚠️ PRIORITY ALERTS               │  │ QUICK ACTIONS     │  │
│         │  │                                 │  │                   │  │
│         │  │ 🔴 Rajan K. — Jamu + Metformin  │  │ [+ New Patient]   │  │
│         │  │ Hepatotoxic risk. STOP. [View]  │  │ [Emergency Access]│  │
│         │  │ ────────────────────────────── │  │ [View All Alerts] │  │
│         │  │ 🔴 Ahmad I. — Supp X + Warfarin│  └───────────────────┘  │
│         │  │ Bleed risk.            [View]  │                          │
│         │  │ ────────────────────────────── │                          │
│         │  │ 🟡 Priya R. — Score drop 18pts │                          │
│         │  │                         [View] │                          │
│         │  └─────────────────────────────────┘                          │
│         │                                                              │
│ Dr.Amira│  ┌──────────────────────────────────────────────────────────┐│
│ GP      │  │ PATIENT LIST                          🔍 Search...       ││
│ Klinik A│  │ ┌────────┬──────┬──────────┬──────────┬────────────────┐ ││
│    🚪   │  │ │Patient │Score │Alerts   │Last Visit│AI Summary      │ ││
│         │  │ ├────────┼──────┼──────────┼──────────┼────────────────┤ ││
│         │  │ │Rajan K.│72 ▲  │🔴2       │Sep 15    │DM2,HTN,3 supps │ ││
│         │  │ │Priya R.│85    │🟢0       │Sep 14    │Hypothyroid     │ ││
│         │  │ │Ahmad I.│45 ▼  │🔴1       │Sep 12    │CKD St3, DM2    │ ││
│         │  │ │...     │      │          │          │                │ ││
│         │  │ └────────┴──────┴──────────┴──────────┴────────────────┘ ││
│         │  │                   ← 1 2 3 ... 12 →   [50 per page]       ││
│         │  └──────────────────────────────────────────────────────────┘│
└─────────┴──────────────────────────────────────────────────────────────┘
```

**Grid:** KPI cards (4-col each, 3 in row) | Priority Alerts (8-col) + Quick Actions (4-col) | Patient List (full 12-col).

**States:** No alerts → "All patients current. No active alerts." green message. Emergency active → red sticky banner across top.

---

## Screen S07 — Patient Detail View

```
┌─────────┬──────────────────────────────────────────────────────────────┐
│ Sidebar │  ← Patient List   Rajan a/l Kumar, 58M    [Emergency Access]│
│         │  ────────────────────────────────────────────────────────────│
│         │                                                              │
│         │  ┌──────────────────────────────────────────────────────┐   │
│         │  │ Rajan a/l Kumar · NRIC ●●●●●●-●●-●●●● · DOB Jan 15  │   │
│         │  │ DM2 (2018), HTN (2019)              ┌──────────────┐ │   │
│         │  │ Primary: Dr. Amira                  │      72      │ │   │
│         │  └─────────────────────────────────────│     /100     │ │   │
│         │                                         │     ▲ +14   │ │   │
│         │  [Overview] [Records] [Supplements]     └──────────────┘ │   │
│         │  ───────────────────────────────────────────────────    │   │
│         │                                                          │   │
│         │  ┌────────────────────────────────┬─────────────────────┐│   │
│         │  │ ✦ AI CLINICAL SUMMARY  [Flag ✏️]│ ⚠️ ACTIVE ALERTS(2)││   │
│         │  │                                │                     ││   │
│         │  │ Rajan, 58M. DM2 (2018). HTN    │ 🔴 Jamu + Metformin ││   │
│         │  │ (2019). Metformin 500mg BID,   │ Hepatotoxic risk.   ││   │
│         │  │ Amlodipine 5mg OD, Atorva      │ STOP immediately.   ││   │
│         │  │ 20mg OD. HbA1c: 7.2% (Sep 12).│ [Acknowledge]       ││   │
│         │  │ 3 supplements: Vitamin D(safe),│                     ││   │
│         │  │ Garlic Extract (moderate risk  │ 🟡 Garlic + Amlodip.││   │
│         │  │ with Amlodipine), Jamu Herbal  │ Additive hypotensive││   │
│         │  │ (🔴 HIGH RISK). Score: 72.     │ Monitor BP.         ││   │
│         │  │                                │ [Acknowledge]       ││   │
│         │  │ Confidence: ██████████░░ 89%   │                     ││   │
│         │  │ Model: summarise_v2.1          │                     ││   │
│         │  └────────────────────────────────┴─────────────────────┘│   │
│         │                                                          │   │
│         │  ┌──────────────────────────────────────────────────────┐│   │
│         │  │ SUPPLEMENT ASSESSMENT HISTORY                        ││   │
│         │  │                                                      ││   │
│         │  │ ● Sep 16, 2026                                       ││   │
│         │  │ │ ┌────────────────────────────────────────────────┐ ││   │
│         │  │ │ │ Wellness: 72 ▲+14 · 3 supps · 🔴 1 interaction│ ││   │
│         │  │ │ │                                        [View →]│ ││   │
│         │  │ │ └────────────────────────────────────────────────┘ ││   │
│         │  │ ● Sep 15, 2026                                       ││   │
│         │  │ │ ┌────────────────────────────────────────────────┐ ││   │
│         │  │ │ │ Wellness: 58 · 3 supps · 🟡 1 interaction     │ ││   │
│         │  │ │ │                                        [View →]│ ││   │
│         │  │ │ └────────────────────────────────────────────────┘ ││   │
│         │  └──────────────────────────────────────────────────────┘│   │
│         │                                                          │   │
│         │  [🤖 Ask AI Chatbot about this patient]                  │   │
└─────────┴──────────────────────────────────────────────────────────────┘
```

**Tabs:**
- Overview: AI summary (8-col) + Active Alerts (4-col) + Assessment timeline
- Records: Record cards with AI summary snippets, share status
- Supplements: Timeline of assessments with trend sparkline

**Emergency Active State:** Red banner across top. All records accessible. Timer counting down.

---

## Screen S08 — Secure Viewer (Shared Record)

```
┌──────────────────────────────────────────────────────────────────────────┐
│  🔒 Healynx — Secure Viewer            ⏱️ Access expires in 6d 23h 58m  │
├──────────────────────────────────────────────────────────────────────────┤
│                                                                          │
│  ┌────────────────────────────────────────────────────────────────────┐  │
│  │                                                                    │  │
│  │  cardiologist@hospital.com | Sep 16, 2026 10:45 | 203.106.12.45  │  │
│  │                                                                    │  │
│  │                    [DOCUMENT CONTENT]                               │  │
│  │                                                                    │  │
│  │  cardiologist@hospital.com | Sep 16, 2026 10:45 | 203.106.12.45  │  │
│  │                                                                    │  │
│  └────────────────────────────────────────────────────────────────────┘  │
│                                                                          │
│  ⚠️ View-only document. Copying, downloading, printing prohibited.       │
│  Logged for audit.                                                       │
│                                                                          │
│  Shared by: Rajan a/l Kumar                                             │
│  Note: "Please review HbA1c before our appointment next week."           │
│                                                                          │
│  [Request Extension]                                                     │
└──────────────────────────────────────────────────────────────────────────┘
```

**States:** Valid, Expired, Revoked, Inactivity timeout, Access code prompt (3 attempts max).

---

## Admin Screens

### Admin Dashboard

```
┌─────────┬──────────────────────────────────────────────────────────────┐
│ Sidebar │  Healynx — Admin    Ms. Linda    ⚙️   🔔   👤              │
│ 256px   │  ────────────────────────────────────────────────────────────│
│         │                                                              │
│ 🏠 Dash │  Clinic: Klinik Amira                        📅 Sep 16, 2026│
│ 👥 Users│                                                              │
│ 📋 Audit│  ┌──────────┬──────────┬──────────┬────────────────────────┐│
│ 📤 PDPA │  │Patients  │Doctors   │Active    │Emergency Sessions      ││
│ 🏥 Clin │  │   247    │    5     │Shares 12 │          0              ││
│ 📊 Heal │  └──────────┴──────────┴──────────┴────────────────────────┘│
│         │                                                              │
│         │  ┌──────────────────────────────────────────────────────────┐│
│         │  │ RECENT EMERGENCY ACCESS EVENTS                           ││
│         │  │ Sep 15 — Dr. Amira → Rajan a/l Kumar                    ││
│         │  │ Reason: ER admission, unconscious. Duration: 22 min     ││
│         │  │ Actions: 14                                   [View]    ││
│         │  └──────────────────────────────────────────────────────────┘│
│         │                                                              │
│         │  ┌──────────────────────────┐  ┌───────────────────────────┐ │
│         │  │ SYSTEM HEALTH            │  │ PENDING PDPA REQUESTS     │ │
│         │  │ API Uptime: 99.97% 🟢   │  │ Data Export: 1 pending    │ │
│         │  │ AI Engine: Operational 🟢│  │ Data Deletion: 0          │ │
│         │  │ Storage: 12.4/50 GB     │  │                           │ │
│         │  │ Latency p95: 2.1s 🟢    │  │ [View All]                │ │
│         │  └──────────────────────────┘  └───────────────────────────┘ │
└─────────┴──────────────────────────────────────────────────────────────┘
```

### User Management

```
┌─────────┬──────────────────────────────────────────────────────────────┐
│         │  ← Dashboard          User Management                        │
│         │  ────────────────────────────────────────────────────────────│
│         │                                                              │
│         │  [+ Add Doctor]    🔍 Search users...                        │
│         │                                                              │
│         │  ┌───────────────┬──────────┬──────────┬───────────────────┐│
│         │  │User           │Role      │Status    │Actions            ││
│         │  ├───────────────┼──────────┼──────────┼───────────────────┤│
│         │  │Dr. Amira      │Doctor    │Active    │[✏️] [🔒]          ││
│         │  │Dr. Wei        │Doctor    │Active    │[✏️] [🔒]          ││
│         │  │Ms. Linda      │Admin     │Active    │[✏️]               ││
│         │  │Rajan a/l Kumar│Patient   │Active    │[✏️] [🔒]          ││
│         │  └───────────────┴──────────┴──────────┴───────────────────┘│
│         │                                                              │
│         │  ─────────────────────────────────────────────────────────── │
│         │  [Deactivated Users ▾] (3)                                   │
└─────────┴──────────────────────────────────────────────────────────────┘
```

### Add Doctor Modal

```
┌──────────────────────────────────┐
│  ← Users          Add Doctor     │
│  ────────────────────────────────│
│                                  │
│  Full Name*                      │
│  ┌──────────────────────────────┐│
│  │ Dr. Nurul Hayati             ││
│  └──────────────────────────────┘│
│                                  │
│  Email*                          │
│  ┌──────────────────────────────┐│
│  │ dr.nurul@clinic.com          ││
│  └──────────────────────────────┘│
│                                  │
│  Role*            [Doctor   ▼]   │
│                                  │
│  Permissions:                    │
│  ☑ View patient records         │
│  ☑ Edit patient records         │
│  ☑ View AI analyses & alerts    │
│  ☑ Acknowledge alerts           │
│  ☑ Generate share links         │
│  ☑ Emergency access             │
│  ☐ Manage users (admin only)    │
│  ☐ View audit logs (admin only) │
│                                  │
│  ┌──────────────────────────────┐│
│  │     [Send Invitation]        ││
│  └──────────────────────────────┘│
└──────────────────────────────────┘
```

### Audit Logs

```
┌─────────┬──────────────────────────────────────────────────────────────┐
│         │  ← Dashboard            Audit Logs                            │
│         │  ────────────────────────────────────────────────────────────│
│         │                                                              │
│         │  Filters:                                                    │
│         │  Actor: [All ▼]  Action: [All ▼]  Target: [All ▼]           │
│         │  From: [Sep 1, 2026]  To: [Sep 16, 2026]                    │
│         │  [Apply]  [Clear]  [Export CSV]                              │
│         │                                                              │
│         │  ┌──────────────┬──────────┬──────────────┬─────────────────┐│
│         │  │Timestamp     │Actor     │Action        │Target           ││
│         │  ├──────────────┼──────────┼──────────────┼─────────────────┤│
│         │  │Sep 16 10:45  │Dr. Amira │emerg_access  │Rajan a/l Kumar  ││
│         │  │Sep 16 10:30  │Rajan K.  │assessment    │Submitted        ││
│         │  │Sep 16 09:15  │Dr. Amira │record_view   │HbA1c Results    ││
│         │  │Sep 15 14:22  │Dr. Wei   │share_grant   │HbA1c Results    ││
│         │  └──────────────┴──────────┴──────────────┴─────────────────┘│
│         │                      ← 1 2 3 ... 47 →                        │
└─────────┴──────────────────────────────────────────────────────────────┘
```

Click any row expands full audit detail (actor, role, action, target, details JSON, IP, user agent, outcome).

### PDPA Requests

```
┌─────────┬──────────────────────────────────────────────────────────────┐
│         │  ← Dashboard          PDPA Requests                           │
│         │  ────────────────────────────────────────────────────────────│
│         │                                                              │
│         │  [Pending] [In Progress] [Completed]                         │
│         │                                                              │
│         │  ┌──────────────────────────────────────────────────────────┐│
│         │  │ ⚠️ Data Export — Rajan a/l Kumar                         ││
│         │  │ Requested: Sep 16 · Deadline: Oct 16 (30 days)           ││
│         │  │ [Process Export]  [Request More Info]                    ││
│         │  └──────────────────────────────────────────────────────────┘│
│         │                                                              │
│         │  ┌──────────────────────────────────────────────────────────┐│
│         │  │ Data Deletion — Ahmad bin Ismail                          ││
│         │  │ Requested: Sep 10 · Status: Pending ID verification      ││
│         │  │ [Confirm Identity]  [Reject]                             ││
│         │  └──────────────────────────────────────────────────────────┘│
└─────────┴──────────────────────────────────────────────────────────────┘
```

### System Health

```
┌─────────┬──────────────────────────────────────────────────────────────┐
│         │  ← Dashboard          System Health                  [🔄]    │
│         │  ────────────────────────────────────────────────────────────│
│         │                                                              │
│         │  ┌─────────────────────┬────────────────────────────────────┐│
│         │  │ SERVICE STATUS      │                                    ││
│         │  │ API Gateway     🟢  │ Uptime: 99.97% (30 days)          ││
│         │  │ Auth Service    🟢  │ Uptime: 99.99%                    ││
│         │  │ AI Engine       🟢  │ Queue: 12 pending                 ││
│         │  │ OCR Pipeline    🟢  │ Idle                              ││
│         │  │ Database        🟢  │ Connections: 34/200               ││
│         │  │ Redis           🟢  │ Memory: 42%                       ││
│         │  │ S3 Storage      🟢  │ 12.4/50 GB used                   ││
│         │  └─────────────────────┴────────────────────────────────────┘│
│         │                                                              │
│         │  ┌──────────────────────────────────────────────────────────┐│
│         │  │ AI ENGINE METRICS (Last 24h)                             ││
│         │  │ Assessments processed: 47   Avg time: 2.1s    p95: 3.8s ││
│         │  │ Summaries generated: 12     Avg time: 6.2s    Errors: 0  ││
│         │  └──────────────────────────────────────────────────────────┘│
└─────────┴──────────────────────────────────────────────────────────────┘
```

---

## Dark Mode

Doctor dashboard: **dark mode is the default.** Clinical environments use dark mode to reduce eye strain during long shifts. Light mode available as toggle.

Dark mode overrides:
- Page bg: `neutral-900`. Cards: `neutral-800`. Sidebar: `neutral-900`.
- Primary text: `neutral-050`. Secondary text: `neutral-400`.
- Data tables: `neutral-800` bg, `neutral-750` header, `neutral-700` borders.
- Modals: `neutral-800` bg, `neutral-700` border.
- Shadows replaced with borders.
- Secure viewer: always dark surround regardless of mode.

---

## General Rules

- All screens must render at 1024px minimum width. Test at 1024px, 1280px, 1440px, 1920px.
- Sidebar: always visible on desktop. Collapse toggle available.
- Data tables: full columns on desktop. Do not collapse to cards (that's mobile behaviour).
- All colour-coded info: icon + text. Never colour alone.
- All AI content: sparkle icon + "AI-GENERATED" label + confidence + model version.
- All encrypted content: lock icon + "AES-256" badge.
- All forms: labels linked, errors described, required marked.
- Focus rings on all interactive elements.
- Keyboard: full tab order through sidebar, top bar, main content, modals.
- `prefers-reduced-motion: reduce` → disable ALL animations.
