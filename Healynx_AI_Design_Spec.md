# Healynx — UI/UX Design Specification

**Document Version:** 1.0  
**Date:** 13 May 2026  
**Author:** Senior UI/UX Designer  
**Classification:** Confidential — For Design System & Front-End Engineering Handoff  
**Reference UX Flow:** Healynx_AI_UX_Flow.md v1.0  
**Reference PRD:** Healynx_AI_PRD.md v1.0  
**Source IPs:**
| IP | Patent No. | Design Impact |
|---|---|---|
| Interactive Patient Supplement Intake & Disclosure Practice Assessment Method | UI 2025002953 | Survey UI, AI results cards, wellness gauge, interaction alerts, recommendation display |
| Computer-Implemented Method for Secure Management of Medical Records | PI 2025007955 | Record vault, AI summary cards, secure viewer, RBAC navigation, emergency banner, audit displays |

---

## Table of Contents

1. [Design Principles](#1-design-principles)
2. [Visual Identity & Brand Direction](#2-visual-identity--brand-direction)
   - [Colour Palette](#21-colour-palette)
   - [Typography](#22-typography)
   - [Iconography](#23-iconography)
   - [Spacing & Grid](#24-spacing--grid)
   - [Elevation & Shadows](#25-elevation--shadows)
   - [Border Radius](#26-border-radius)
3. [Component Library](#3-component-library)
4. [Key Screen Designs](#4-key-screen-designs)
5. [Interaction Patterns & Micro-Interactions](#5-interaction-patterns--micro-interactions)
6. [Accessibility Requirements](#6-accessibility-requirements)
7. [Responsive Behaviour](#7-responsive-behaviour)
8. [Dark Mode Strategy](#8-dark-mode-strategy)
9. [Design Tokens Cross-Reference](#9-design-tokens-cross-reference)
10. [IP Traceability in Design](#10-ip-traceability-in-design)

---

## 1. Design Principles

### Principle 1 — Trust & Security

> Every element must visually communicate that patient data is protected, access is controlled, and the platform is medically serious.

**In practice:**
- Lock icons precede every secured action (upload, share, view)
- Encrypted file badges ("🔒 AES-256") appear on record cards
- Watermark overlays on shared documents with recipient identity
- Emergency access mode drenches the UI in a persistent red banner — zero ambiguity
- TLS/encryption indicators in subtle footer on all authenticated screens
- Data residency badge: "Data stored in Malaysia" on profile and settings screens

### Principle 2 — Clarity over Cleverness

> Medical information must be instantly scannable. Doctors read in seconds between patients. Decoration never obstructs data.

**In practice:**
- Supplement safety cards use colour (green/amber/red) AND icon AND label — never colour alone
- AI confidence scores displayed alongside every AI-generated output
- Patient list rows show the AI one-liner summary — not hidden behind a click
- Interaction alerts use severity hierarchy: High (red, top), Moderate (amber, middle), Low (grey, collapsed)
- Plain-language patient recommendations; clinical-terminology doctor recommendations — same data, two presentations
- No decorative illustrations in data-dense screens — reserved for empty states and onboarding only

### Principle 3 — Frictionless

> Minimise taps and clicks for the top 3 clinical workflows: review patient (3 clicks max), review supplement risk (2 clicks), emergency access (2 clicks).

**In practice:**
- Patient list on doctor dashboard: click row → full profile with AI summary visible instantly
- Supplement survey: auto-save every field on blur; pre-populate from last assessment
- Emergency access: global sidebar button → reason selection → confirm → active (3 interactions total)
- Share configuration: recipient pre-selected from connected doctors; view-only is default
- FAB for record upload on mobile — always one tap away
- "Resume draft" prompt on survey if previously abandoned

### Principle 4 — Accessible

> WCAG 2.1 AA minimum. Doctors work in harsh fluorescent light; elderly patients use large text.

**In practice:**
- All colour-coded information duplicated with icons and text labels
- Touch targets minimum 44×44px on mobile, 40×40px on desktop
- Focus rings visible on all interactive elements (2px offset, high-contrast colour)
- Type scale supports 200% zoom without horizontal scroll
- Dark mode for clinical environments (reduces eye strain)
- Screen reader: all AI content tagged with `aria-label="AI-generated summary"`, all form errors linked via `aria-describedby`

### Principle 5 — Intelligent, Not Intrusive

> AI insights should feel like a helpful colleague, not a replacement. Every AI output must signal its source and confidence.

**In practice:**
- All AI-generated content marked with a sparkle icon (✦) and "AI-Generated" label
- AI confidence score always visible (gauge bar or percentage)
- "Flag inaccuracy" action on every AI summary card
- Model version displayed in fine print (clinician trust = transparency)
- AI chatbot responses cite sources from the patient's records
- Human-in-the-loop messaging: "AI suggests. You decide."

---

## 2. Visual Identity & Brand Direction

### 2.1 Colour Palette

#### 2.1.1 Primary Palette

| Token | Name | Hex | HSL | Usage |
|---|---|---|---|---|
| `--nv-primary-900` | Teal Darkest | `#0D3B3B` | 180°, 64%, 14% | Primary button text on light, headings on dark |
| `--nv-primary-800` | Teal Dark | `#125C5C` | 180°, 68%, 22% | Hover state for primary elements |
| `--nv-primary-700` | Teal Medium-Dark | `#1A7A7A` | 180°, 65%, 29% | Active/pressed state |
| `--nv-primary-600` | **Teal Base** | `#1E8C8C` | 180°, 65%, 33% | **Primary brand colour.** Buttons, links, selected states, sidebar active |
| `--nv-primary-500` | Teal Light | `#26A6A6` | 180°, 63%, 40% | Hover on dark backgrounds |
| `--nv-primary-400` | Teal Lighter | `#3DBEBE` | 180°, 51%, 49% | Focus rings, subtle highlights |
| `--nv-primary-300` | Teal Tint | `#6ED4D4` | 180°, 55%, 63% | Badge backgrounds, illustration accents |
| `--nv-primary-200` | Teal Pale | `#A3E5E5` | 180°, 56%, 77% | Light backgrounds for cards, selected rows |
| `--nv-primary-100` | Teal Wash | `#D4F3F3` | 180°, 56%, 89% | Page backgrounds, section dividers |
| `--nv-primary-050` | Teal Mist | `#EDFAFA` | 180°, 57%, 95% | Surface backgrounds, hover on light rows |

#### 2.1.2 Secondary Palette — Soft Blue (AI / Technology)

| Token | Name | Hex | HSL | Usage |
|---|---|---|---|---|
| `--nv-secondary-700` | Blue Dark | `#1E4D8C` | 225°, 65%, 33% | Secondary button text |
| `--nv-secondary-600` | **Blue Base** | `#2563B8` | 222°, 68%, 43% | **AI-related UI.** Chatbot avatar, AI sparkle icon, AI summary card border |
| `--nv-secondary-500` | Blue Light | `#3B82D9` | 221°, 67%, 54% | Hover on AI elements |
| `--nv-secondary-300` | Blue Tint | `#7EB3F0` | 221°, 75%, 72% | AI loading skeleton, chatbot typing indicator |
| `--nv-secondary-100` | Blue Wash | `#DCE9FA` | 221°, 71%, 92% | AI summary card background, chatbot message bubble (AI side) |
| `--nv-secondary-050` | Blue Mist | `#F0F5FD` | 221°, 76%, 97% | Chatbot panel background |

#### 2.1.3 Accent Palette — Warm Amber (Alerts / Urgency)

| Token | Name | Hex | HSL | Usage |
|---|---|---|---|---|
| `--nv-accent-700` | Amber Dark | `#8C5C1E` | 34°, 65%, 33% | Amber text on light backgrounds |
| `--nv-accent-600` | **Amber Base** | `#D98C2E` | 34°, 69%, 52% | **Moderate alerts.** Caution icons, amber supplement cards, "review needed" badges |
| `--nv-accent-500` | Amber Light | `#EDA83E` | 34°, 83%, 59% | Hover on amber elements |
| `--nv-accent-300` | Amber Tint | `#F4CD84` | 34°, 71%, 74% | Alert banner background (amber) |
| `--nv-accent-100` | Amber Wash | `#FCF0DA` | 35°, 85%, 92% | Amber card background |
| `--nv-accent-050` | Amber Mist | `#FEF9F2` | 35°, 86%, 97% | Hover on amber rows |

#### 2.1.4 Semantic Colours

| Token | Name | Hex | HSL | Usage |
|---|---|---|---|---|
| `--nv-success-600` | **Green Base** | `#2D8C4A` | 145°, 51%, 36% | **Safe supplement.** Success toasts, "safe" badges, wellness score high range |
| `--nv-success-500` | Green Light | `#3AA85C` | 145°, 49%, 44% | Green supplement card icon, checkmark |
| `--nv-success-100` | Green Wash | `#D9F2E2` | 145°, 49%, 90% | Green card/supplement background |
| `--nv-success-050` | Green Mist | `#F0FAF4` | 145°, 50%, 96% | Green row hover |

| Token | Name | Hex | HSL | Usage |
|---|---|---|---|---|
| `--nv-warning-600` | **Amber Caution** | `#BF7A1E` | 34°, 73%, 43% | (Alias to accent for semantic clarity in code) |
| `--nv-warning-100` | Amber Wash | `#FCF0DA` | 35°, 85%, 92% | Moderate alert cards |

| Token | Name | Hex | HSL | Usage |
|---|---|---|---|---|
| `--nv-danger-700` | Red Darkest | `#8C1A1A` | 0°, 69%, 33% | Emergency banner background |
| `--nv-danger-600` | **Red Base** | `#D92D2D` | 0°, 69%, 51% | **High risk.** Interaction warnings, emergency access, stop supplements |
| `--nv-danger-500` | Red Light | `#E84A4A` | 0°, 77%, 60% | Hover on danger elements |
| `--nv-danger-100` | Red Wash | `#FCE0E0` | 0°, 88%, 93% | Red supplement card background |
| `--nv-danger-050` | Red Mist | `#FEF5F5` | 0°, 82%, 98% | Red alert row hover |

| Token | Name | Hex | HSL | Usage |
|---|---|---|---|---|
| `--nv-info-600` | **Info Blue** | `#3B82D9` | 221°, 67%, 54% | Informational alerts, tips |
| `--nv-info-100` | Info Wash | `#DCE9FA` | 221°, 71%, 92% | Info banner background |

#### 2.1.5 Neutral Palette — Greys

| Token | Name | Hex | Usage |
|---|---|---|---|
| `--nv-neutral-900` | Grey 900 | `#111827` | Primary text on light backgrounds |
| `--nv-neutral-800` | Grey 800 | `#1F2937` | Headings |
| `--nv-neutral-700` | Grey 700 | `#374151` | Body text |
| `--nv-neutral-600` | Grey 600 | `#4B5563` | Secondary text, captions |
| `--nv-neutral-500` | Grey 500 | `#6B7280` | Placeholder text, disabled text |
| `--nv-neutral-400` | Grey 400 | `#9CA3AF` | Disabled button text, inactive icons |
| `--nv-neutral-300` | Grey 300 | `#D1D5DB` | Borders, dividers |
| `--nv-neutral-200` | Grey 200 | `#E5E7EB` | Input backgrounds, card borders |
| `--nv-neutral-150` | Grey 150 | `#EEF0F3` | Hover on light surfaces |
| `--nv-neutral-100` | Grey 100 | `#F3F4F6` | Page background, sidebar background |
| `--nv-neutral-050` | Grey 050 | `#F9FAFB` | Card backgrounds, surface |
| `--nv-neutral-000` | White | `#FFFFFF` | White cards, modals, elevated surfaces |

#### 2.1.6 Semantic Colour Assignments

| Concept | Colour | Token | Icon |
|---|---|---|---|
| Safe / OK / Supplement Good | Green | `--nv-success-600` | 🟢 `shield-check` |
| Caution / Review / Moderate Risk | Amber | `--nv-accent-600` | 🟡 `shield-alert` |
| Danger / Stop / High Risk / Emergency | Red | `--nv-danger-600` | 🔴 `shield-x` |
| AI-Generated Content | Blue | `--nv-secondary-600` | ✦ `sparkle` |
| Encrypted / Secure | Teal | `--nv-primary-600` | 🔒 `lock` |
| Expired / Inactive | Grey 400 | `--nv-neutral-400` | ⏰ `clock-off` |
| Processing / Loading | Blue 300 | `--nv-secondary-300` | `loader` |
| New / Unread | Teal 500 | `--nv-primary-500` | `dot` badge |

---

### 2.2 Typography

#### 2.2.1 Font Families

| Role | Font Family | Fallback | Rationale |
|---|---|---|---|
| **Primary (Body, UI)** | Inter | system-ui, -apple-system, Segoe UI, Roboto, sans-serif | Exceptional legibility at small sizes. Optimised for screen reading. Clean, neutral personality suitable for medical data. |
| **Secondary (Headings)** |Plus Jakarta Sans | Inter, system-ui, sans-serif | Slightly rounded geometry conveys approachability for patients while remaining professional. Distinct from body text. |
| **Monospace (Data, Codes)** | JetBrains Mono | Consolas, Monaco, monospace | Lab values, NRIC numbers, access codes, model versions. Coding ligatures for readability. |

**Font Loading Strategy:**
- Subset to Latin + Malay (covers Bahasa Malaysia characters)
- `font-display: swap` to prevent FOIT (Flash of Invisible Text)
- Self-hosted via `@font-face` (no Google Fonts dependency — PDPA privacy consideration)

#### 2.2.2 Type Scale

| Token | Role | Font | Size | Line Height | Weight | Letter Spacing |
|---|---|---|---|---|---|---|
| `--nv-text-h1` | Page title (patient dashboard) | Plus Jakarta Sans | 32px / 2rem | 1.2 (38px) | 700 (Bold) | -0.5px |
| `--nv-text-h2` | Section heading | Plus Jakarta Sans | 24px / 1.5rem | 1.3 (31px) | 700 (Bold) | -0.3px |
| `--nv-text-h3` | Card title | Plus Jakarta Sans | 20px / 1.25rem | 1.3 (26px) | 600 (SemiBold) | -0.2px |
| `--nv-text-h4` | Subsection heading | Plus Jakarta Sans | 18px / 1.125rem | 1.35 (24px) | 600 (SemiBold) | 0 |
| `--nv-text-h5` | Minor heading | Inter | 16px / 1rem | 1.4 (22px) | 600 (SemiBold) | 0 |
| `--nv-text-h6` | Overline / KPI label | Inter | 13px / 0.8125rem | 1.4 (18px) | 600 (SemiBold) | 1px (uppercase) |
| `--nv-text-body-lg` | Large body (onboarding, empty states) | Inter | 18px / 1.125rem | 1.6 (29px) | 400 (Regular) | 0 |
| `--nv-text-body` | Default body text | Inter | 16px / 1rem | 1.6 (25.6px) | 400 (Regular) | 0 |
| `--nv-text-body-sm` | Small body (record metadata, labels) | Inter | 14px / 0.875rem | 1.5 (21px) | 400 (Regular) | 0 |
| `--nv-text-caption` | Captions, timestamps, fine print | Inter | 12px / 0.75rem | 1.5 (18px) | 400 (Regular) | 0.2px |
| `--nv-text-overline` | Badge text, chip labels, overlines | Inter | 11px / 0.6875rem | 1.4 (15px) | 600 (SemiBold) | 1px (uppercase) |
| `--nv-text-mono` | Lab values, codes, model versions | JetBrains Mono | 14px / 0.875rem | 1.5 (21px) | 450 (Medium) | 0 |
| `--nv-text-mono-sm` | Small mono (table data, IDs) | JetBrains Mono | 12px / 0.75rem | 1.5 (18px) | 450 (Medium) | 0 |

#### 2.2.3 Text Colour Tokens

| Token | Hex | Usage |
|---|---|---|
| `--nv-text-primary` | `--nv-neutral-900` (#111827) | Primary text — all body, headings on light surfaces |
| `--nv-text-secondary` | `--nv-neutral-600` (#4B5563) | Secondary text — captions, metadata, helper text |
| `--nv-text-tertiary` | `--nv-neutral-500` (#6B7280) | Placeholder text, disabled state text |
| `--nv-text-inverse` | `#FFFFFF` | Text on dark/coloured backgrounds (buttons, banners) |
| `--nv-text-link` | `--nv-primary-600` (#1E8C8C) | Hyperlinks, interactive text |
| `--nv-text-link-hover` | `--nv-primary-700` (#1A7A7A) | Link hover state |
| `--nv-text-danger` | `--nv-danger-600` (#D92D2D) | Error text, high-severity labels |
| `--nv-text-success` | `--nv-success-600` (#2D8C4A) | Success text, safe supplement labels |
| `--nv-text-warning` | `--nv-accent-700` (#8C5C1E) | Caution text, moderate alert labels |
| `--nv-text-ai` | `--nv-secondary-600` (#2563B8) | AI-generated content labels, model version text |

---

### 2.3 Iconography

#### 2.3.1 Icon Set

**Library:** Lucide Icons (open-source, MIT licensed, consistent 2px stroke, rounded corners)

**Sizing:**
| Size Token | Dimensions | Usage |
|---|---|---|
| `--nv-icon-xs` | 14×14px | Badge icons, inline with caption text |
| `--nv-icon-sm` | 16×16px | Inline in body text, table cell icons |
| `--nv-icon-md` | 20×20px | Button icons, card icons, list item icons |
| `--nv-icon-lg` | 24×24px | Navigation icons, feature icons, empty states |
| `--nv-icon-xl` | 32×32px | Dashboard KPI icons, prominent CTAs |
| `--nv-icon-2xl` | 48×48px | Empty state illustrations, onboarding |

**Stroke:** 2px consistently; 1.5px for `xs` and `sm` sizes

#### 2.3.2 Icon Catalogue by Domain

**Health / Medical:**
| Icon | Name | Usage |
|---|---|---|
| 🩺 | `stethoscope` | Doctor dashboard, clinician context |
| 💊 | `pill` | Supplement-related actions, medication |
| 🏥 | `building-2` | Clinic profile, tenant settings |
| 🩻 | `scan` | Medical scans, imaging records |
| 🧬 | `dna` | Lab reports, test results |
| ❤️ | `heart-pulse` | Health conditions, wellness |
| 📋 | `clipboard-list` | Prescriptions, clinical notes |
| 💉 | `syringe` | Vaccination records |
| 🩹 | `bandage` | Minor treatment records |

**Security / Access:**
| Icon | Name | Usage |
|---|---|---|
| 🔒 | `lock` | Encrypted storage, secure actions, record vault |
| 🔓 | `lock-open` | Shared access active, temporary unlock |
| 🛡️ | `shield` | Security status, compliance |
| 🛡️✓ | `shield-check` | Safe supplement, verified access |
| 🛡️⚠ | `shield-alert` | Caution supplement, moderate risk |
| 🛡️✕ | `shield-x` | High risk supplement, access denied |
| 🔑 | `key` | Access code, authentication, MFA |
| 👁️ | `eye` | View record, password visibility toggle |
| 👁️‍🗨️ | `eye-off` | Hidden data, password mask |
| 📝 | `file-signature` | Audit log, consent record |

**AI / Intelligence:**
| Icon | Name | Usage |
|---|---|---|
| ✦ | `sparkle` | AI-generated content indicator — **universal marker** |
| 🤖 | `bot` | Chatbot avatar, AI assistant |
| 🧠 | `brain` | AI engine status, ML model |
| 📊 | `bar-chart-3` | Trend data, analytics, wellness graph |
| 🎯 | `target` | Personalised recommendations |
| ⚡ | `zap` | Real-time processing, quick actions |

**Actions:**
| Icon | Name | Usage |
|---|---|---|
| ↗️ | `share-2` | Share record (time-limited) |
| ⬆️ | `upload` | Upload medical record |
| ⬇️ | `download` | Download (restricted/disabled in secure viewer) |
| ➕ | `plus` | Add supplement, add record, FAB |
| ✏️ | `pencil` | Edit profile, edit supplement entry |
| 🗑️ | `trash-2` | Delete record, revoke share |
| 🔍 | `search` | Global search, patient search |
| ⏱️ | `timer` | Time-limited access, session expiry |
| 🔔 | `bell` | Notifications, alerts |
| ❌ | `x` | Close, dismiss, cancel |
| ✓ | `check` | Confirmation, completed status |
| ← | `arrow-left` | Back navigation |
| ⋮ | `more-vertical` | Overflow menu |
| ⚙️ | `settings` | Settings, preferences |
| 🚪 | `log-out` | Logout |
| ❓ | `help-circle` | Help, tooltip trigger |
| ⚠️ | `alert-triangle` | Warning, interaction risk |
| 🚨 | `siren` | Emergency access |

**Status:**
| Icon | Name | Usage |
|---|---|---|
| 🟢 | `circle-check` | Safe, complete, active |
| 🟡 | `circle-alert` | Caution, pending review |
| 🔴 | `circle-x` | Risk, expired, revoked |
| ⏳ | `loader-2` | Processing (animated spin) |
| 📅 | `calendar` | Date picker, expiry date |

#### 2.3.3 AI Content Visual Markers

All AI-generated content must carry the following visual treatment:

```
┌─────────────────────────────────────────┐
│  ✦ AI-Generated Summary                 │  ← Sparkle icon + "AI-Generated" label in blue
│                                         │
│  [content]                              │
│                                         │
│  Confidence: ████████░░ 89%             │  ← Confidence bar
│  Model: summarise_v2.1.0                │  ← Model version in mono
│                        [Flag Inaccuracy]│
└─────────────────────────────────────────┘
```

**Rules:**
1. Sparkle icon (`✦`) prefix on ALL AI-generated text blocks — no exceptions
2. "AI-Generated" overline label in `--nv-text-ai` colour
3. Confidence score displayed as horizontal bar (0–100%)
4. Model version displayed in `--nv-text-mono-sm` at 60% opacity
5. "Flag Inaccuracy" link always present for clinician feedback loop

---

### 2.4 Spacing & Grid

#### 2.4.1 Base Grid

**Base unit:** 8px (0.5rem)

All spacing values are multiples of 8px.

#### 2.4.2 Spacing Scale

| Token | Value | Usage |
|---|---|---|
| `--nv-space-xs` | 4px (0.25rem) | Icon-to-text gap inside badges, chip padding |
| `--nv-space-sm` | 8px (0.5rem) | Inline spacing, checkbox-to-label gap, button icon gap |
| `--nv-space-md` | 12px (0.75rem) | Compact card padding, list item gap |
| `--nv-space-lg` | 16px (1rem) | **Default spacing.** Card padding, section gap, form field gap |
| `--nv-space-xl` | 24px (1.5rem) | Section-to-section gap, dashboard card grid gap |
| `--nv-space-2xl` | 32px (2rem) | Page section padding top/bottom |
| `--nv-space-3xl` | 40px (2.5rem) | Major section dividers |
| `--nv-space-4xl` | 48px (3rem) | Hero section padding |
| `--nv-space-5xl` | 64px (4rem) | Page-level top/bottom padding |

#### 2.4.3 Layout Grids

**Mobile (320px – 767px) — 4-column:**
```
| 16px | col | 16px | col | 16px | col | 16px | col | 16px |
       gutter       gutter       gutter       gutter
```
- 4 equal columns
- 16px gutters
- 16px page margins
- Content spans 1–4 columns

**Tablet (768px – 1023px) — 8-column:**
```
| 24px | col | 24px | col | 24px | col | 24px | col | 24px | col | 24px | col | 24px | col | 24px | col | 24px |
```
- 8 equal columns
- 24px gutters
- 24px page margins

**Desktop (1024px+) — 12-column:**
```
| 24px | col | 24px | col | 24px | col | 24px | col | 24px | col | 24px | col | 24px | col | 24px | col | 24px | col | 24px | col | 24px | col | 24px |
```
- 12 equal columns
- 24px gutters
- Max content width: 1280px (centred with auto margins)

**Doctor Dashboard Grid Override:**
- Sidebar: fixed 256px (collapsible to 64px icon-only)
- Content area: remaining width, 12-column grid within
- Patient detail view: 8-column main + 4-column side panel

---

### 2.5 Elevation & Shadows

#### 2.5.1 Shadow Tokens

| Token | Value | Usage |
|---|---|---|
| `--nv-shadow-none` | `none` | Flat surfaces — page backgrounds, input fields |
| `--nv-shadow-xs` | `0 1px 2px 0 rgba(0,0,0,0.05)` | Subtle elevation — cards on white backgrounds |
| `--nv-shadow-sm` | `0 1px 3px 0 rgba(0,0,0,0.08), 0 1px 2px -1px rgba(0,0,0,0.05)` | Standard card elevation, dropdown panels |
| `--nv-shadow-md` | `0 4px 6px -1px rgba(0,0,0,0.08), 0 2px 4px -2px rgba(0,0,0,0.05)` | Elevated cards, modals on mobile (bottom sheets) |
| `--nv-shadow-lg` | `0 10px 15px -3px rgba(0,0,0,0.08), 0 4px 6px -4px rgba(0,0,0,0.05)` | Modals, dialogs, popovers |
| `--nv-shadow-xl` | `0 20px 25px -5px rgba(0,0,0,0.10), 0 8px 10px -6px rgba(0,0,0,0.05)` | Full-screen overlays, emergency access modal |
| `--nv-shadow-ring` | `0 0 0 3px rgba(30,140,140,0.25)` | Focus ring (teal primary colour at 25% opacity) |
| `--nv-shadow-ring-danger` | `0 0 0 3px rgba(217,45,45,0.25)` | Focus ring on danger elements |

**Dark mode shadows:** Replace shadow with border or subtle glow. Shadows are imperceptible on dark backgrounds.

---
### 2.6 Border Radius

| Token | Value | Usage |
|---|---|---|
| `--nv-radius-xs` | 4px | Checkboxes, small badges, code blocks |
| `--nv-radius-sm` | 6px | Input fields, select dropdowns, small buttons |
| `--nv-radius-md` | 8px | **Default.** Cards, buttons, modals, chips |
| `--nv-radius-lg` | 12px | Large cards, bottom sheets, patient profile cards |
| `--nv-radius-xl` | 16px | Modals (mobile), wellness gauge container |
| `--nv-radius-2xl` | 24px | Large promotional cards, onboarding screens |
| `--nv-radius-full` | 9999px | Pills, badges, toggle switches, avatar, FAB |

---

## 3. Component Library

### 3.1 Component Index

| # | Component | Category | Appears In |
|---|---|---|---|
| C01 | Button | Core | All flows, all screens |
| C02 | Input Field | Core | Survey, registration, search, share |
| C03 | Select / Dropdown | Core | Survey (conditions, medications, lifestyle) |
| C04 | Toggle / Switch | Core | Permissions, settings, notifications |
| C05 | Checkbox & Radio Group | Core | Consent, survey confirmation, filter |
| C06 | Card | Core | Dashboard, records vault, results, profiles |
| C07 | Modal / Bottom Sheet | Core | Emergency access, confirmation, file upload |
| C08 | Toast / Snackbar | Core | Success, error, warning feedback |
| C09 | Tooltip | Core | Icon labels, AI confidence, help |
| C10 | Badge / Chip | Core | Supplement status, record type, alert count |
| C11 | Loading States | Core | AI processing, dashboard skeleton, upload |
| C12 | Tab Bar | Navigation | Patient bottom nav, doctor patient detail |
| C13 | Sidebar | Navigation | Doctor dashboard, admin dashboard |
| C14 | Data Table | Data | Doctor patient list, audit logs |
| C15 | Timeline | Data | Supplement assessment history, record uploads |
| C16 | File Upload Zone | Input | Record upload, document sharing |
| C17 | Secure Document Viewer | Specialised | Shared record viewing (IP 1) |
| C18 | AI Summary Card | Specialised | AI-generated summaries, chatbot responses |
| C19 | Wellness Gauge | Specialised | Patient wellness score display (IP 2) |
| C20 | Alert Banner | Specialised | Emergency mode, interaction risks, warnings |
| C21 | Progress Indicator | Specialised | Survey multi-step, AI analysis progress |
| C22 | Supplement Card | Specialised | Supplement safety display per item (IP 2) |
| C23 | Interaction Alert Row | Specialised | Drug-supplement interaction flag (IP 2) |
| C24 | Recommendation List | Specialised | Personalised AI recommendations (IP 2) |
| C25 | Share Link Card | Specialised | Active shares, link copy, revocation (IP 1) |
| C26 | Access Timer Bar | Specialised | Secure viewer countdown, emergency session (IP 1) |
| C27 | Watermark Overlay | Specialised | Secure viewer document watermark (IP 1) |
| C28 | Empty State | Pattern | First-time user, no records, no alerts |
| C29 | Search Bar | Input | Patient search, record search, audit filter |
| C30 | Avatar | Core | Patient photo, doctor photo, user initials fallback |

---

### 3.2 Component Specifications

#### C01 — Button

**Purpose:** Primary call-to-action across all flows.

**Variants:**
| Variant | Background | Text | Border | Usage |
|---|---|---|---|---|
| Primary | `--nv-primary-600` | White | None | Main CTA: "Submit Survey", "Generate Secure Link" |
| Secondary | Transparent | `--nv-primary-600` | 1.5px `--nv-primary-600` | Secondary actions: "Save Draft", "Skip for Now" |
| Tertiary | Transparent | `--nv-primary-600` | None | Inline actions: "Learn More", "View Details" |
| Danger | `--nv-danger-600` | White | None | Destructive: "Delete Record", "Revoke Access" |
| Danger Secondary | Transparent | `--nv-danger-600` | 1.5px `--nv-danger-600` | Emergency Access button (prominent) |
| Ghost | Transparent | `--nv-neutral-600` | None | Minimal: "Dismiss", icon-only toolbar buttons |

**Sizes:**
| Size | Height | Padding H | Font | Icon Size | Min Touch Target |
|---|---|---|---|---|---|
| sm | 32px | 12px | `--nv-text-body-sm` (14px) | 16px | 32×32px |
| md | 40px | 16px | `--nv-text-body` (16px) | 20px | 40×40px |
| lg | 48px | 24px | `--nv-text-body-lg` (18px) | 24px | 48×48px |

**States:**
| State | Visual |
|---|---|
| Default | As per variant |
| Hover | Lighten background by 5%; darken border by 5% |
| Active/Pressed | Darken background by 8%; scale 0.98 |
| Focus | `box-shadow: --nv-shadow-ring`; outline: none |
| Disabled | Opacity 40%; `cursor: not-allowed`; no hover effect |
| Loading | Replace text with spinner (same size as icon); button width locked to prevent layout shift |

**Full-Width:** On mobile (width <768px), primary buttons in forms and modals are full-width. Desktop buttons are intrinsic width.

**Icon Position:** Icons can be `leading` (left of text) or `trailing` (right of text). Leading icons for navigation actions; trailing for forward actions.

**Button Group:** When two buttons appear side-by-side (e.g., Cancel + Confirm), spacing is `--nv-space-sm` (8px). On mobile, stack vertically (Primary on bottom, Secondary on top).

---

#### C02 — Input Field

**Purpose:** Text, password, search, and numeric input across forms, search, and configuration screens.

**Anatomy:**
```
┌────────────────────────────────────────────────┐
│  Label *                     [Character Count] │  ← Label row
│  ┌──────────────────────────────────────────┐  │
│  │ 🔍  Placeholder text...              [✕] │  │  ← Input row (icon + text + clear)
│  └──────────────────────────────────────────┘  │
│  Helper text or error message                  │  ← Below input
└────────────────────────────────────────────────┘
```

**Sizes:** (Height: sm=36px, md=44px, lg=52px — md is default)

**Types:**
| Type | Features |
|---|---|
| `text` | Standard text entry |
| `email` | Email validation, keyboard: email on mobile |
| `password` | Masked input, show/hide toggle (eye icon), strength meter |
| `search` | Magnifying glass icon (leading), clear button (trailing on input), debounce 300ms |
| `number` | Numeric keyboard on mobile, min/max/step attributes |
| `tel` | Phone keyboard on mobile, country code prefix |
| `textarea` | Multi-line, auto-resize (min 3 rows, max 10), character count |

**States:**
| State | Border | Background |
|---|---|---|
| Default | `--nv-neutral-300` (1.5px) | `--nv-neutral-000` (white) |
| Hover | `--nv-neutral-400` | `--nv-neutral-000` |
| Focus | `--nv-primary-600` (2px) + `--nv-shadow-ring` | `--nv-neutral-000` |
| Filled | `--nv-neutral-300` | `--nv-neutral-000` |
| Disabled | `--nv-neutral-200` | `--nv-neutral-100` (opacity 60%) |
| Error | `--nv-danger-600` (2px) | `--nv-danger-050` |
| Success | `--nv-success-600` (1.5px) | `--nv-neutral-000` |
| Read-only | No border (transparent) | `--nv-neutral-050` |

**Error Message:** Displayed below input in `--nv-text-danger`, `--nv-text-body-sm` (14px) font. Icon: `alert-circle` 16px leading. Error message linked to input via `aria-describedby`.

**Helper Text:** Displayed below input in `--nv-text-tertiary`, `--nv-text-caption` (12px) font.

**Required indicator:** Red asterisk (*) appended to label. ARIA: `aria-required="true"`.

---

#### C03 — Select / Dropdown

**Purpose:** Single or multi-select from predefined lists (conditions, medications, lifestyle factors, record types).

**Anatomy:**
```
┌─────────────────────────────────────┐
│  Label *                            │
│  ┌─────────────────────────────────┐│
│  │ Select condition...        [▼]  ││  ← Chevon toggles dropdown
│  └─────────────────────────────────┘│
│  ┌─────────────────────────────────┐│
│  │ 🔍 Type to search...            ││  ← Searchable variant
│  │ ─────────────────────────────   ││
│  │ Type 2 Diabetes                 ││  ← Option rows
│  │ Hypertension              [✓]   ││  ← Selected indicator
│  │ Coronary Artery Disease         ││
│  │ Asthma                          ││
│  └─────────────────────────────────┘│
└─────────────────────────────────────┘
```

**Variants:**
| Variant | Behaviour |
|---|---|
| Single select | One value; clicking an option selects it and closes dropdown |
| Multi select | Multiple values; checkmark on selected; chips appear in input field |
| Searchable | Text input at top of dropdown filters options; 3-character minimum |
| With icon | Leading icon in the trigger (e.g., 💊 for medication type) |

**States:** (Same as Input Field: Default, Hover, Focus, Disabled, Error)

**Dropdown Panel:**
- Max height: 320px (scrollable)
- Option height: 44px (touch-friendly on mobile)
- Hover: `--nv-neutral-050` background
- Selected: `--nv-primary-050` background, `--nv-primary-600` checkmark
- Keyboard: Arrow keys navigate, Enter selects, Escape closes

**Multi-Select Chips:** Selected values appear as removable chips inside the trigger:
```
┌──────────────────────────────────────────────────────────┐
│ [Type 2 Diabetes ✕] [Hypertension ✕]  [+3 more ▾] [▼] │
└──────────────────────────────────────────────────────────┘
```

---

#### C04 — Toggle / Switch

**Purpose:** Binary on/off settings (permissions, notification preferences, feature toggles).

```
┌──────────────────────────────────────────────────────┐
│  Allow Download                                      │
│  Recipient can download this document      [○────●]  │
└──────────────────────────────────────────────────────┘
```

**Anatomy:**
- Track: 44px wide × 24px tall, `--nv-neutral-300` (off), `--nv-primary-600` (on)
- Thumb: 20px circle, white with `--nv-shadow-sm`
- Label: left-aligned, body text
- Description: optional `--nv-text-caption` below label

**States:**
| State | Track | Thumb |
|---|---|---|
| Off | `--nv-neutral-300` | Left position |
| On | `--nv-primary-600` | Right position |
| Disabled Off | `--nv-neutral-200`, 40% opacity | Left |
| Disabled On | `--nv-primary-600`, 40% opacity | Right |
| Focus | `--nv-shadow-ring` on track | |

**Animation:** Thumb translates with 200ms `ease-out` cubic-bezier.

---

#### C05 — Checkbox & Radio Group

**Purpose:** Selection from lists (consent, survey confirmation, filters).

**Checkbox:**
```
┌──────────────────────────────────────────┐
│  [✓] I confirm this information is       │
│      accurate                            │
│                                          │
│  [ ] I consent to anonymised data use    │
│      for research (optional)             │
└──────────────────────────────────────────┘
```

**Radio Group:**
```
┌──────────────────────────────────────────┐
│  Access expires after:                    │
│                                          │
│  (●) 24 hours                            │
│  ( ) 7 days                              │
│  ( ) 30 days                             │
│  ( ) Custom: [____] days                 │
└──────────────────────────────────────────┘
```

**Specs:**
- Checkbox/Radio: 20×20px touch area (with 12px invisible padding extending to 44×44px)
- Checkbox border-radius: `--nv-radius-xs` (4px)
- Radio: full circle
- Selected colour: `--nv-primary-600` fill, white check/dot
- Indeterminate state (checkbox only): dash instead of check
- Focus: `--nv-shadow-ring`

---

#### C06 — Card

**Purpose:** Container for related content — the primary building block of all screens.

**Variants:**

| Variant | Background | Border | Shadow | Usage |
|---|---|---|---|---|
| Standard | `--nv-neutral-000` (white) | 1px `--nv-neutral-200` | `--nv-shadow-xs` | Default content container |
| Interactive | `--nv-neutral-000` | 1px `--nv-neutral-200` | `--nv-shadow-xs`; on hover: `--nv-shadow-sm`, border `--nv-primary-300` | Clickable cards (patient list rows, record cards) |
| Elevated | `--nv-neutral-000` | None | `--nv-shadow-md` | Modals, prominent cards |
| Alert Amber | `--nv-accent-100` | 1px `--nv-accent-300` | None | Moderate alerts, caution supplement cards |
| Alert Red | `--nv-danger-100` | 1px `--nv-danger-500` (20% opacity) | None | High risk alerts, danger supplement cards |
| AI Content | `--nv-secondary-050` | 1px `--nv-secondary-300` | None | AI-generated summaries, chatbot responses |
| Flat | `--nv-neutral-050` | None | None | Background containers, section dividers |

**Padding:** `--nv-space-lg` (16px) default; `--nv-space-xl` (24px) for prominent cards

**Card Anatomy:**
```
┌──────────────────────────────────────────┐
│  [Icon]  CARD TITLE           [Action →] │  ← Header row (optional)
│  ──────────────────────────────────────  │  ← Divider (optional)
│                                          │
│  Card body content here                  │  ← Body (flexible)
│                                          │
│  ──────────────────────────────────────  │  ← Divider (optional)
│  Secondary text / metadata               │  ← Footer (optional)
└──────────────────────────────────────────┘
```

---

#### C07 — Modal / Bottom Sheet

**Purpose:** Focused user attention for confirmations, forms, and emergency access prompts.

**Modal (Desktop):**
- Centred on screen
- Max width: 560px (narrow), 720px (wide for forms)
- Max height: 85vh (scrollable body if content overflows)
- Backdrop: `rgba(0,0,0,0.5)` with `backdrop-filter: blur(4px)`
- Close: X button (top-right), Escape key, click backdrop (configurable)
- Entrance: fade in 200ms, scale 0.95→1.0

**Bottom Sheet (Mobile):**
- Slides up from bottom
- Max height: 90vh
- Drag handle: 32px × 4px grey bar at top (visual affordance)
- Close: drag down, X button, backdrop tap
- Entrance: slide up 300ms `ease-out`

**Emergency Access Modal (Special Variant):**
```
┌──────────────────────────────────────────┐
│  [🚨] EMERGENCY ACCESS                   │
│                                          │
│  You are requesting unrestricted access  │
│  to Rajan a/l Kumar's medical records.   │
│  All actions will be logged and audited. │
│                                          │
│  Reason: [__________________________]    │
│                                          │
│  [Cancel]      [Confirm Emergency →]     │
└──────────────────────────────────────────┘
```
- Red border: 2px `--nv-danger-600`
- Header background: `--nv-danger-050`
- Confirm button: Danger variant
- Backdrop: Non-dismissible (must explicitly cancel or confirm)

---

#### C08 — Toast / Snackbar

**Purpose:** Non-blocking feedback after actions (success, error, warning, info).

**Position:** Top-right on desktop; bottom-centre (above tab bar) on mobile.

**Variants:**
| Type | Icon | Background | Border Left | Text |
|---|---|---|---|---|
| Success | ✓ `circle-check` | `--nv-success-050` | 3px `--nv-success-600` | `--nv-text-success` |
| Error | ✕ `circle-x` | `--nv-danger-050` | 3px `--nv-danger-600` | `--nv-text-danger` |
| Warning | ⚠ `alert-triangle` | `--nv-accent-050` | 3px `--nv-accent-600` | `--nv-text-warning` |
| Info | ℹ `info` | `--nv-info-100` | 3px `--nv-info-600` | `--nv-neutral-700` |

**Anatomy:**
```
┌──────────────────────────────────────────────────────┐
│ [Icon]  Assessment saved. Wellness score: 72.  [✕]  │
└──────────────────────────────────────────────────────┘
```

**Behaviour:**
- Auto-dismiss: 5 seconds (success/info), 10 seconds (warning), persists until dismissed (error)
- Swipe-to-dismiss on mobile
- Max 3 toasts visible (stacked with 8px gap)
- Entrance: slide + fade 200ms

---

#### C09 — Tooltip

**Purpose:** Contextual help on icons, abbreviations, AI confidence, and disabled actions.

**Trigger:** Hover (desktop) or long-press (mobile) on `?` icon or underlined text.

**Anatomy:**
```
        ┌──────────────────────────┐
        │ Explanation text here.   │
        └────────────┬─────────────┘
                     ▼
              [Trigger Element]
```

- Max width: 280px
- Padding: `--nv-space-md` (12px)
- Background: `--nv-neutral-800` (dark)
- Text: white, `--nv-text-body-sm` (14px)
- Shadow: `--nv-shadow-md`
- Arrow: 6px triangle pointing to trigger
- Entry delay: 300ms (hover), exit delay: 100ms

---

#### C10 — Badge / Chip

**Purpose:** Status indicators, categorisation, and counts.

**Badge Variants:**
| Variant | Background | Text | Border | Usage |
|---|---|---|---|---|
| Safe / Green | `--nv-success-100` | `--nv-success-600` | None | Supplement: safe, Record: active share |
| Caution / Amber | `--nv-accent-100` | `--nv-accent-700` | None | Supplement: review, Alert: moderate |
| Risk / Red | `--nv-danger-100` | `--nv-danger-600` | None | Supplement: stop, Alert: high, Emergency |
| Info / Blue | `--nv-secondary-100` | `--nv-secondary-700` | None | AI-generated, new feature |
| Neutral / Grey | `--nv-neutral-100` | `--nv-neutral-600` | None | Record type, expired, inactive |
| Primary / Teal | `--nv-primary-100` | `--nv-primary-700` | None | Active, verified, secure |

**Chip Variants (Removable):**
```
[Vitamin D ✕]  [Garlic Extract ✕]  [+3 more]
```
- Same colour mapping as badges
- ✕ icon on right to remove (12px, `--nv-neutral-500`, hover: `--nv-danger-600`)
- Used in multi-select inputs and filter bars

**Count Badge (Notification):**
```
┌───┐
│ 3 │  ← Red circle, white text, 18×18px
└───┘
```
- Positioned top-right of parent icon
- Numbers >99 show "99+"
- Pulse animation on new count

---

#### C11 — Loading States

**Spinner:**
- `Loader2` icon from Lucide, rotating 360° continuously (1s linear infinite)
- Sizes: 16px, 20px, 24px, 32px, 48px
- Colour: `--nv-primary-600` (default), `--nv-neutral-000` (on dark buttons)

**Skeleton Screen:**
```
┌──────────────────────────────────────────────┐
│  ▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓  │  ← Text line
│  ▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓       │  ← Shorter text
│                                              │
│  ┌──────────────────────────────────────┐    │  ← Card placeholder
│  │ ▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓ │    │
│  │ ▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓ │    │
│  └──────────────────────────────────────┘    │
│  ┌──────────────────────────────────────┐    │
│  │ ▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓ │    │
│  └──────────────────────────────────────┘    │
└──────────────────────────────────────────────┘
```
- Skeleton colour: `--nv-neutral-200` with shimmer animation
- Shimmer: linear gradient moving left-to-right (1.5s animation)
- Used for: Dashboard cards, patient list, record vault, AI results loading

**Progress Bar:**
```
Analysis in progress...
▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓░░░░░░░░░░░░  65%
```
- Height: 6px
- Track: `--nv-neutral-200`
- Fill: `--nv-primary-600` (gradient with subtle shimmer)
- Border-radius: `--nv-radius-full`
- Width: 100% of container
- Used for: AI analysis pipeline, upload progress

**AI Analysis Loading (Special):**
```
┌──────────────────────────────────────────────┐
│                                              │
│           [🧠 animated pulsing]              │
│                                              │
│         Analysing your intake...             │
│         ▓▓▓▓▓▓▓▓▓▓▓▓▓▓░░░░░░░░  75%        │
│                                              │
│      ✓ Validating supplement data            │
│      ✓ Checking drug interactions            │
│      → Calculating wellness score            │
│      ○ Generating recommendations            │
│                                              │
│      Estimated time: ~3 seconds              │
└──────────────────────────────────────────────┘
```
- Scan line animation sweeps top-to-bottom across the card
- Stage indicators animate: spinning → checkmark (✓) → next starts spinning
- Estimated time updates dynamically based on task progress

---

#### C12 — Tab Bar (Mobile Bottom Navigation)

**Patient Bottom Tab Bar:**
```
┌────────────┬────────────┬────────────┬────────────┐
│     🏠     │     💊     │     📁     │     👤     │
│   Home     │ Supplements│  Records   │  Profile   │
└────────────┘────────────┘────────────┘────────────┘
```
- Height: 56px + safe area padding (iOS)
- Background: `--nv-neutral-000` (white)
- Border top: 0.5px `--nv-neutral-200`
- Active tab: icon + label in `--nv-primary-600`
- Inactive tab: icon + label in `--nv-neutral-500`
- Active indicator: subtle 3px top border on active tab (or pill background on icon)
- Icon: 24px; Label: `--nv-text-caption` (12px)

**Top Tab Bar (Content Switcher):**
```
┌──────────────────────────────────────────────┐
│  [Overview]  [Records]  [Supplements]  [History] │
└──────────────────────────────────────────────┘
```
- Used within patient detail view (doctor) and results views
- Active: `--nv-primary-600` text + 2px bottom border
- Inactive: `--nv-neutral-500` text

---

#### C13 — Sidebar (Desktop Navigation)

**Doctor/Admin Sidebar:**
```
┌──────────────────────────────┐
│  ┌──────┐                    │
│  │ Logo │  Healynx     │  ← Brand header
│  └──────┘                    │
│  ─────────────────────────   │
│                              │
│  🏠  Dashboard               │  ← Nav items
│  👥  Patients                │
│  ⚠️  Alerts            [3]   │  ← With count badge
│  🤖  AI Chatbot              │
│  🔒  Emergency Access        │
│  📊  Reports                 │
│  ⚙️  Settings                │
│                              │
│  ─────────────────────────   │
│                              │
│  ┌──┐                       │
│  │DR│  Dr. Amira             │  ← User info
│  └──┘  General Practitioner  │
│         Klinik Amira         │
│                    [🚪 Logout]│
└──────────────────────────────┘
```
- Width: 256px (expanded), 64px (collapsed — icons only)
- Background: `--nv-neutral-050` (light mode), `--nv-neutral-900` (dark mode)
- Active item: `--nv-primary-050` background, `--nv-primary-600` text, 3px left border
- Hover: `--nv-neutral-100` background
- Count badge on Alerts: red dot/badge
- Collapse toggle: hamburger/chevron at bottom

---

#### C14 — Data Table

**Purpose:** Doctor patient list, audit logs, user management tables.

**Anatomy:**
```
┌──────────────────────────────────────────────────────────────────┐
│  Search: [________________] 🔍   Filter: [Severity ▼] [Date ▼] │
│                                                                  │
│ ┌──────────┬──────────┬──────────┬──────────┬──────────┬──────┐ │
│ │ Patient ▲│ Score  ▼│ Alerts ▼│ Last Visit│ Summary  │      │ │
│ ├──────────┼──────────┼──────────┼──────────┼──────────┼──────┤ │
│ │ Rajan K. │    72    │  🔴 2    │ Sep 15   │ DM2, HTN │ [→]  │ │
│ │ Priya R. │    85    │  🟢 0    │ Sep 14   │ Hypothy. │ [→]  │ │
│ │ Ahmad I. │    45    │  🔴 1    │ Sep 12   │ CKD, DM2 │ [→]  │ │
│ ├──────────┼──────────┼──────────┼──────────┼──────────┼──────┤ │
│ │                     ← 1 2 3 ... 12 →                      │ │
│ └──────────┴──────────┴──────────┴──────────┴──────────┴──────┘ │
└──────────────────────────────────────────────────────────────────┘
```

**Specs:**
- Header row: `--nv-neutral-100` background, `--nv-text-h6` font (13px, semi-bold, uppercase)
- Row height: 52px (comfortable for data scanning)
- Row hover: `--nv-neutral-050` background
- Row selected: `--nv-primary-050` background
- Row border-bottom: 0.5px `--nv-neutral-200`
- Sortable columns: arrow icon (▲▼) next to header; clicking toggles asc/desc
- Pagination: at bottom, showing "← 1 2 3 ... 12 →" with per-page selector
- Empty state: "No patients found matching your filters." with illustration

**On Mobile:** Table collapses to card list — each row becomes a card with stacked fields.

---

#### C15 — Timeline

**Purpose:** Supplement assessment history, record upload chronology.

```
┌──────────────────────────────────────────────────────┐
│  ●  Sep 16, 2026                                     │
│  │  ┌────────────────────────────────────────────┐   │
│  │  │ Supplement Assessment                       │   │
│  │  │ Wellness Score: 72  ⬆ +14                  │   │
│  │  │ 3 supplements · 🔴 1 high-risk interaction │   │
│  │  │                                    [View →]│   │
│  │  └────────────────────────────────────────────┘   │
│  │                                                    │
│  ●  Sep 12, 2026                                     │
│  │  ┌────────────────────────────────────────────┐   │
│  │  │ HbA1c Lab Report (Uploaded)                 │   │
│  │  │ HbA1c: 7.2% · AI Summary Available         │   │
│  │  │                                    [View →]│   │
│  │  └────────────────────────────────────────────┘   │
│  │                                                    │
│  ●  Sep 10, 2026                                     │
│  │  ┌────────────────────────────────────────────┐   │
│  │  │ Blood Pressure Log                          │   │
│  │  │ BP: 128/82 · Uploaded by Dr. Amira          │   │
│  │  │                                    [View →]│   │
│  │  └────────────────────────────────────────────┘   │
│  │                                                    │
│  ○  Older entries...                                  │
└──────────────────────────────────────────────────────┘
```

**Specs:**
- Vertical line: 2px `--nv-neutral-200`, starting from first dot
- Dots: 12px diameter; filled (`--nv-primary-600`) for most recent, outlined for older
- Cards: indent 24px from line; standard card variant
- Expand/collapse: "Show older entries" at bottom

---

#### C16 — File Upload Zone

**Purpose:** Upload medical documents and capture photos.

**States:**
| State | Visual |
|---|---|
| Default | Dashed border (2px `--nv-neutral-300`), centred icon + text: "Tap to select a file or drag & drop. PDF, JPEG, PNG, TIFF (Max 25MB)" |
| Drag Over | Dashed border becomes solid `--nv-primary-600`, background `--nv-primary-050`, icon animates (bounce) |
| File Selected | Replace with file preview (thumbnail for images, PDF icon for documents) + file name + size + [Remove] button |
| Uploading | Progress bar + "Encrypting and uploading..." text |
| Complete | ✓ checkmark + "Upload complete. AI summary generating..." |
| Error | ✕ + error message + [Retry] button |

**Mobile Variants:**
- Camera capture: "Take Photo" button opens device camera
- Photo library: "Choose from Library" opens photo picker

---

#### C17 — Secure Document Viewer (IP 1)

**Purpose:** View-only document display with watermark, anti-copy enforcement, and access timer.

```
┌──────────────────────────────────────────────────────────────┐
│  🔒 Secure Viewer                  ⏱️ Expires in 6d 23h 58m  │
├──────────────────────────────────────────────────────────────┤
│                                                              │
│  ┌────────────────────────────────────────────────────────┐  │
│  │                                                        │  │
│  │    [DOCUMENT CONTENT RENDERED HERE]                    │  │
│  │                                                        │  │
│  │    cardiologist@hospital.com                           │  │
│  │    Sep 16, 2026 10:45 AM · IP: 203.106.12.45          │  │
│  │    (repeating watermark in diagonal pattern —           │  │
│  │     15% opacity, --nv-neutral-600 colour)              │  │
│  │                                                        │  │
│  └────────────────────────────────────────────────────────┘  │
│                                                              │
│  ⚠️ View-only. Copy, download, print, and screenshots are    │
│  prohibited. All access is logged for audit.                 │
│                                                              │
│  [Request Extension]                                         │
└──────────────────────────────────────────────────────────────┘
```

**Specs:**
- Background: `--nv-neutral-900` (dark surround, medical imaging standard — reduces glare)
- Document area: white background, centred, max-width 900px
- Watermark: server-side rendered repeating pattern with `{recipient_email} | {datetime} | {ip_address}` at 15% opacity, `--nv-neutral-600`, rotated -20°
- Access timer bar: persistent top bar, `--nv-neutral-800` background, white text, `--nv-text-mono` font
- Anti-copy: `user-select: none`, `-webkit-user-select: none`, prevent `contextmenu` event, block `Ctrl+C/S/P` keys
- Print: `@media print { body { display: none; } }` — renders blank
- Inactivity timeout: 15 minutes → overlay: "Session timed out. Reload to view again."
- Session watermark colour changes when in emergency access mode (red tint)

---

#### C18 — AI Summary Card (IP 1)

**Purpose:** Display AI-generated summaries with confidence scores and model attribution.

```
┌──────────────────────────────────────────────────────────┐
│  ✦ AI-GENERATED CLINICAL SUMMARY            [Flag Error] │
│                                                          │
│  Rajan a/l Kumar, 58M. DM2 (diagnosed 2018),             │
│  Hypertension (2019). Current meds: Metformin 500mg       │
│  BID, Amlodipine 5mg OD, Atorvastatin 20mg OD.           │
│  Latest HbA1c: 7.2% (Sep 12, improving from 7.8%).       │
│  Patient takes 3 supplements: Vitamin D (safe),           │
│  Garlic Extract (moderate risk with Amlodipine),          │
│  Jamu Herbal (HIGH RISK — hepatotoxic potential).        │
│  Wellness Score: 72 (⬆+14).                               │
│                                                          │
│  ────────────────────────────────────────────────────    │
│  Confidence: ████████████░░ 89%     Model: summarise_v2.1 │
└──────────────────────────────────────────────────────────┘
```

**Specs:**
- Card variant: `AI Content` (blue-tinted)
- Header: Sparkle icon + "AI-GENERATED" overline in `--nv-text-ai`
- Flag Error: tertiary button, `--nv-text-body-sm`, for clinician feedback
- Confidence bar: 4px height, `--nv-neutral-200` track, `--nv-primary-600` fill
- If confidence <60%: confidence bar turns amber (`--nv-accent-600`); warning text: "Low confidence — verify before clinical use"

---

#### C19 — Wellness Gauge (IP 2)

**Purpose:** Display patient wellness score prominently.

```
┌────────────────────────────────────────┐
│         YOUR WELLNESS SCORE            │
│                                        │
│            ┌───────────┐               │
│            │           │               │
│            │    72     │   ⬆ +14      │
│            │   /100    │               │
│            │           │               │
│            └───────────┘               │
│         ▓▓▓▓▓▓▓▓▓▓▓▓▓▓░░░             │
│              Good                      │
│                                        │
│         Since your last assessment     │
│          on September 15, 2026         │
└────────────────────────────────────────┘
```

**Design:**
- Radial gauge (semi-circle) or large numeric display with coloured arc
- Arc colour gradient: Red (0–40) → Amber (41–60) → Teal (61–80) → Green (81–100)
- Score number: `Plus Jakarta Sans`, 48px, `--nv-text-h1`, Bold (700)
- Trend arrow: ⬆ (green, improving) or ⬇ (red, declining) with delta number
- Qualitative label below score: "Needs Attention" (0–40), "Fair" (41–60), "Good" (61–80), "Excellent" (81–100)
- Animated entrance: arc draws from 0 to score value (1s ease-out); number counts up

**Compact Variant (Doctor Patient List):**
```
┌────────────┐
│  72  ⬆+14 │
│  ───────   │
│   /100     │
└────────────┘
```
- Used in table cells and summary cards
- 36px number, scaled-down arc

---

#### C20 — Alert Banner

**Purpose:** Persistent notifications for emergency mode, system status, and critical warnings.

**Variants:**

| Variant | Background | Border Left | Icon | Usage |
|---|---|---|---|---|
| Info | `--nv-info-100` | 4px `--nv-info-600` | ℹ️ `info` | System maintenance, tips |
| Warning | `--nv-accent-100` | 4px `--nv-accent-600` | ⚠️ `alert-triangle` | Moderate risks, overdue surveys |
| Critical | `--nv-danger-050` | 4px `--nv-danger-600` | 🚨 `siren` | High-risk interactions |
| Emergency Active | `--nv-danger-700` | Full red background | 🚨 `siren` white | Emergency access mode (persistent) |

**Emergency Active Banner (Persistent — IP 1):**
```
┌──────────────────────────────────────────────────────────────────┐
│  🔴 EMERGENCY ACCESS ACTIVE — ALL ACTIONS LOGGED     ⏱️ 58:23   │
│  Patient: Rajan a/l Kumar · Reason: ER admission                │
│                                             [End Emergency Access]│
└──────────────────────────────────────────────────────────────────┘
```
- Full-width, sticks to top of viewport (position: sticky)
- Red background (`--nv-danger-700`), white text
- Timer: `--nv-text-mono` font, counts down in real-time
- Cannot be dismissed — must click "End Emergency Access" or wait for expiry
- Z-index: highest (above modals)

---

#### C21 — Progress Indicator (Multi-Step)

**Purpose:** Survey multi-step navigation and progress tracking.

```
● Screen 1 ──── ● Screen 2 ──── ○ Screen 3 ──── ○ Screen 4
  Supplements     Health & Meds    Lifestyle       Review
```

**Specs:**
- Steps: 24px circles with step number or icon
- Completed: filled `--nv-primary-600`, white checkmark
- Current: `--nv-primary-600` border (2px), white fill, bold number
- Upcoming: `--nv-neutral-300` border, grey number
- Connector line: 2px, `--nv-neutral-300` (completed segments: `--nv-primary-300`)
- Labels below circles: `--nv-text-caption` (12px), `--nv-text-secondary` (upcoming), `--nv-text-primary` (current/completed)

**On Mobile:** Connector lines shortened; labels hidden — only dots + step number visible.

---

#### C22 — Supplement Card (IP 2)

**Purpose:** Per-supplement safety display in AI results.

```
┌──────────────────────────────────────────────┐
│  🟢 Vitamin D                         [Safe] │
│  1000 IU — Once daily                        │
│                                              │
│  Safe at current dosage. Supports bone       │
│  health and calcium absorption.              │
│                                              │
│  Evidence: Consistent with RDA guidelines    │
│  for adults over 50.                         │
│                              [Learn More ▾]  │
└──────────────────────────────────────────────┘
```

```
┌──────────────────────────────────────────────┐
│  🔴 Jamu Herbal Preparation          [STOP]  │
│  2 capsules — Once daily                     │
│                                              │
│  ⚠️ HIGH RISK: Contains undeclared           │
│  ingredients with potential liver toxicity.  │
│  STOP use immediately and consult your       │
│  doctor.                                     │
│                                              │
│  Evidence: Case reports of elevated LFTs     │
│  with concurrent metformin use.              │
│                              [See Details ▾] │
└──────────────────────────────────────────────┘
```

**Colour Mapping:**
| Status | Card Border | Badge | Icon |
|---|---|---|---|
| Safe | Green (`--nv-success-600`, 20% opacity) | Green `Safe` | 🟢 `shield-check` |
| Review | Amber (`--nv-accent-600`, 20% opacity) | Amber `Review` | 🟡 `shield-alert` |
| Stop | Red (`--nv-danger-600`, 25% opacity) | Red `STOP` | 🔴 `shield-x` |

**Expandable Detail:** "Learn More" / "See Details" toggles a collapsible section with: mechanism of interaction, clinical evidence summary, alternative supplement suggestions.

---

#### C23 — Interaction Alert Row (IP 2)

**Purpose:** Drug-supplement or supplement-supplement interaction flag.

```
┌──────────────────────────────────────────────┐
│  ⚠️ Garlic Extract + Amlodipine              │
│  Severity: MODERATE                          │
│                                              │
│  Additive hypotensive effect. May cause      │
│  dizziness or excessively low blood pressure.│
│                                              │
│  Recommendation: Monitor BP. Consider        │
│  reducing garlic extract to 500mg or         │
│  switching to aged garlic extract.           │
│                                              │
│  Evidence: Expert-labelled UM dataset        │
│  (validated by clinical pharmacist).         │
│                                              │
│  [Acknowledge]                               │
└──────────────────────────────────────────────┘
```

**Severity Design:**
| Severity | Row Border Left | Badge |
|---|---|---|
| Low | 3px `--nv-neutral-400` | Grey `Info` |
| Moderate | 3px `--nv-accent-600` | Amber `Caution` |
| High | 3px `--nv-danger-600` | Red `Danger` |

---

#### C24 — Recommendation List (IP 2)

**Purpose:** Personalised, actionable recommendations from AI analysis.

```
┌──────────────────────────────────────────┐
│  YOUR RECOMMENDATIONS                    │
│                                          │
│  ✓  Continue Vitamin D at current dose   │
│                                          │
│  ⚡  Reduce Garlic Extract to 500mg or   │
│     switch to aged garlic extract.       │
│                                          │
│  ✕  Stop Jamu Herbal immediately. Show   │
│     this result to your doctor.          │
│                                          │
│  💡 Consider CoQ10 supplement to offset  │
│     statin side effects (discuss with    │
│     your doctor).                        │
└──────────────────────────────────────────┘
```

**Icon mapping:**
- ✓ `check` (green) — Continue / Safe action
- ⚡ `zap` (amber) — Modify / Adjust
- ✕ `x` (red) — Stop / Discontinue
- 💡 `lightbulb` (blue) — Consider / Optional tip

Each recommendation is a row with 16px spacing, icon + text. Icons are 20px.

---

#### C25 — Share Link Card (IP 1)

**Purpose:** Display active shares for a record, with revocation option.

```
┌──────────────────────────────────────────────┐
│  HbA1c Test Results                          │
│  ────────────────────────────────────────    │
│  Shared with: cardiologist@hospital.com      │
│  Expires: Sep 23, 2026 (6 days left)         │
│  Permissions: View only                      │
│  Views: 2                                     │
│                                              │
│  Share link: Healynx.ai/share/xK9mW [📋]  │
│                                              │
│  [Revoke Access]                             │
└──────────────────────────────────────────────┘
```

**Expired variant:** Card faded to 60% opacity; "Expired Sep 10" badge; [Reshare] button instead of [Revoke].

---

#### C26 — Access Timer Bar (IP 1)

**Purpose:** Visual countdown for time-limited access (secure viewer and emergency sessions).

```
⏱️ Access expires in 6 days 23 hours 58 minutes
▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓░░░░ 85%
```

- Bar height: 4px (subtle, non-intrusive)
- Background: `--nv-neutral-200`
- Fill colour: `--nv-primary-600` (>24h remaining), `--nv-accent-600` (<24h), `--nv-danger-600` (<1h)
- Text above bar: `--nv-text-body-sm`, `--nv-text-mono` for time values
- Emergency session: bar colour is always `--nv-danger-600`

---

#### C27 — Watermark Overlay (IP 1)

**Purpose:** Server-side rendered watermark on all shared documents.

```
┌──────────────────────────────────────────────────────┐
│                                                      │
│   cardiologist@hospital.com  Sep 16, 2026 10:45 AM   │
│                                                      │
│   cardiologist@hospital.com  Sep 16, 2026 10:45 AM   │
│                                                      │
│   [DOCUMENT CONTENT]          cardiologist@hospital  │
│                                                      │
│   cardiologist@hospital.com  Sep 16, 2026 10:45 AM   │
│                                                      │
│   cardiologist@hospital.com  Sep 16, 2026 10:45 AM   │
│                                                      │
└──────────────────────────────────────────────────────┘
```

**Specs:**
- Pattern: diagonal, repeating every 200px vertically and 300px horizontally
- Text: `{recipient_identifier} | {access_datetime} | {accessor_ip}`
- Font: Inter, 12px, `--nv-neutral-600` at 12% opacity
- Rotation: -20°
- Z-index: above document content, below any interactive UI
- Emergency mode: watermark tinted red (`--nv-danger-600` at 15%) with "EMERGENCY ACCESS" prefix

---

#### C28 — Empty State

**Purpose:** Guide first-time users and handle zero-data scenarios.

**General Pattern:**
```
┌──────────────────────────────────────────────┐
│                                              │
│              [Illustration]                  │
│                                              │
│           No records yet                     │
│                                              │
│     Upload your first medical record         │
│     to start building your secure            │
│     health vault.                            │
│                                              │
│        [Upload First Record →]               │
│                                              │
└──────────────────────────────────────────────┘
```

**Illustration style:** Simple line illustrations in `--nv-primary-200` colour, friendly but not cartoonish. Reserved for empty states and onboarding — never in data-dense screens.

**Empty States Defined:**
| Screen | Empty State Message | CTA |
|---|---|---|
| Patient Dashboard (no assessment) | "Complete your first supplement survey to get your wellness score." | [Start Survey] |
| Records Vault (no records) | "Your health vault is empty. Upload a medical record to get started." | [Upload Record] |
| Doctor Patient List (no patients) | "No patients assigned yet. Contact your clinic administrator." | [Contact Admin] |
| Doctor Alerts (no alerts) | "No active alerts. All patients are current." | (None — positive state) |
| Active Shares (no shares) | "You haven't shared any records yet." | [Learn About Sharing] |
| Search (no results) | "No results found for '[query]'." | [Clear Search] |

---

#### C29 — Search Bar

```
┌──────────────────────────────────────────────┐
│  🔍  Search patients, records, supplements   │
└──────────────────────────────────────────────┘
```

- Height: 44px (mobile), 40px (desktop)
- Icon: `search` 20px, `--nv-neutral-500`, leading inside input
- Clear button: `x` icon appears when text entered
- Debounce: 300ms before triggering search
- Results: dropdown panel below (max 8 items, "See all results" link)
- Shortcut hint: "Ctrl+K" on desktop (global search modal trigger)

---

#### C30 — Avatar

**Purpose:** User and patient identification across the platform.

**Variants:**
| Variant | Usage | Size |
|---|---|---|
| Photo | User has uploaded profile photo | sm: 32px, md: 40px, lg: 64px, xl: 96px |
| Initials | Fallback when no photo | Same sizes; background: `--nv-primary-600`, text: white |
| Patient Icon | Generic patient with no photo | Same sizes; background: `--nv-neutral-200`, icon: `user` in `--nv-neutral-500` |

**Initials extraction:** First character of first name + first character of last name. "Rajan a/l Kumar" → "RK". Single name → first two characters.

**Doctor Badge:** Small teal circle with stethoscope icon overlapping avatar bottom-right if user is a doctor.

---

## 4. Key Screen Designs

### 4.1 Screen Index

| # | Screen | Audience | Platform Priority | IP |
|---|---|---|---|---|
| S01 | Patient Dashboard | Patient | Mobile-First | IP 1 + IP 2 |
| S02 | Supplement Survey (Multi-Step) | Patient | Mobile-First | IP 2 |
| S03 | AI Analysis Results | Patient | Mobile-First | IP 2 |
| S04 | Medical Records Vault | Patient | Mobile-First | IP 1 |
| S05 | Record Share Screen | Patient | Mobile-First | IP 1 |
| S06 | Doctor Dashboard | Doctor | Desktop-First | IP 1 + IP 2 |
| S07 | Patient Detail View | Doctor | Desktop-First | IP 1 + IP 2 |
| S08 | Secure Viewer (Shared Record) | Recipient | Desktop-First | IP 1 |

---

### 4.2 Patient Screens (Mobile-First: 375px canvas)

#### S01 — Patient Dashboard (Home)

```
┌────────────────────────────────────────────┐  320–428px width
│  Healynx                     👤 RK  │  Status bar: 44px
│  ──────────────────────────────────────── │
│                                            │
│  Good morning, Rajan          📅 Sep 16   │  Greeting: h5 + caption
│                                            │
│  ┌────────────────────────────────────┐   │
│  │  YOUR WELLNESS SCORE               │   │  CARD: Elevated
│  │                                    │   │
│  │           ┌──────────┐             │   │
│  │           │    72    │  ⬆ +14     │   │  Score: 48px Bold
│  │           │   /100   │             │   │
│  │           └──────────┘             │   │
│  │        ▓▓▓▓▓▓▓▓▓▓▓▓▓▓░░           │   │  Arc gauge
│  │             Good                   │   │  Qualitative label
│  └────────────────────────────────────┘   │
│                                            │  gap: 16px
│  ┌────────────────────────────────────┐   │
│  │  AI INSIGHT                   ✦    │   │  CARD: AI Content
│  │  Based on your last assessment,    │   │
│  │  consider reducing garlic extract  │   │
│  │  and discussing your jamu herbal   │   │
│  │  use with Dr. Amira.               │   │
│  │                           [View →] │   │
│  └────────────────────────────────────┘   │
│                                            │  gap: 16px
│  ┌────────────────────────────────────┐   │
│  │  SUPPLEMENT TRACKER                │   │  CARD: Standard
│  │  Updated: Just now                 │   │
│  │                                    │   │
│  │  🟢 Vitamin D       Safe           │   │  Supplement rows
│  │  🟡 Garlic Extract   Review        │   │  with status icons
│  │  🔴 Jamu Herbal      STOP 🆕       │   │
│  │                                    │   │
│  │  [Update Supplement Intake]        │   │  Primary button
│  └────────────────────────────────────┘   │
│                                            │  gap: 16px
│  ┌────────────────────────────────────┐   │
│  │  UPCOMING                          │   │  CARD: Standard
│  │  💊 Metformin 500mg · 8:00 AM     │   │
│  │  📋 Update survey · Due in 5 days  │   │
│  └────────────────────────────────────┘   │
│                                            │  gap: 16px
│  ┌────────────────────────────────────┐   │
│  │  RECENT ACTIVITY                   │   │  CARD: Standard
│  │  ● Assessment completed · Sep 16   │   │
│  │  ● Record uploaded · Sep 12        │   │
│  │  ○ View all activity →             │   │
│  └────────────────────────────────────┘   │
│                                            │
│  ──────────────────────────────────────── │
│  [🏠 Home]  [💊 Suppl]  [📁 Records]  [👤]  │  Tab Bar: 56px
└────────────────────────────────────────────┘
```

**Grid:** All cards full-width in single column. 16px horizontal page margins. 16px vertical gaps between cards.

**Scroll behaviour:** Vertical scroll. Pull-to-refresh triggers dashboard data refresh.

**Empty State (First Time):**
- Wellness gauge card replaced with: "Welcome, Rajan! Complete your first supplement survey to get your wellness score." [Start Survey →]
- AI Insight card: Hidden (no data)
- Supplement Tracker: Hidden
- Recent Activity: "No activity yet. Your health journey starts here."

---

#### S02 — Supplement Survey (Multi-Step)

```
┌────────────────────────────────────────────┐
│  ← Back     Supplement Intake     ●○○○     │  Header: 44px
│             1 of 4                          │
│  ──────────────────────────────────────── │
│                                            │
│  What supplements are you                  │  Question: h4
│  currently taking?                         │
│                                            │
│  ┌────────────────────────────────────┐   │
│  │ Vitamin D                          │   │  Supplement Entry Card
│  │ 1000 IU — Once daily               │   │  (pre-populated from last)
│  │ For: Bone health                   │   │
│  │                          [✏️] [🗑️] │   │
│  └────────────────────────────────────┘   │
│                                            │  gap: 12px
│  ┌────────────────────────────────────┐   │
│  │ Garlic Extract                     │   │
│  │ 1000 mg — Once daily               │   │
│  │ For: Blood pressure                │   │
│  │                          [✏️] [🗑️] │   │
│  └────────────────────────────────────┘   │
│                                            │  gap: 12px
│  ┌────────────────────────────────────┐   │
│  │ Jamu Herbal Preparation            │   │
│  │ 2 capsules — Once daily            │   │
│  │ For: General wellness              │   │
│  │                          [✏️] [🗑️] │   │
│  └────────────────────────────────────┘   │
│                                            │
│  ┌────────────────────────────────────┐   │
│  │  [+ Add Another Supplement]        │   │  Secondary button
│  └────────────────────────────────────┘   │
│                                            │
│  ⚠️ Jamu Herbal is unverified in our       │  Warning for unrecognised
│  database. Analysis may be limited.        │  products
│                                            │
│  ──────────────────────────────────────── │
│  Step 1 ●───○ Step 2 ○ Step 3 ○ Step 4   │  Progress indicator
│  ──────────────────────────────────────── │
│                                            │
│  ┌────────────────────────────────────┐   │
│  │         [Continue →]               │   │  Primary button: full-width
│  └────────────────────────────────────┘   │  48px height
└────────────────────────────────────────────┘
```

**"Add Supplement" Form (bottom sheet):**
```
┌────────────────────────────────────────────┐
│  ──── (drag handle) ────                   │
│  Add Supplement                    [✕]     │
│  ──────────────────────────────────────── │
│                                            │
│  Supplement Name*                          │
│  ┌────────────────────────────────────┐   │
│  │ 🔍 Search or type name...           │   │
│  └────────────────────────────────────┘   │
│                                            │
│  Dosage*                                   │
│  ┌──────────┐ ┌──────────────────────┐    │
│  │  1000    │ │  IU            [▼]   │    │
│  └──────────┘ └──────────────────────┘    │
│                                            │
│  Frequency*                 [Daily  ▼]    │
│                                            │
│  Duration (months)          [12     ▼]    │
│                                            │
│  Health purpose (optional)                 │
│  ┌────────────────────────────────────┐   │
│  │ Bone health                         │   │
│  └────────────────────────────────────┘   │
│                                            │
│  ┌────────────────────────────────────┐   │
│  │         [Add Supplement]            │   │
│  └────────────────────────────────────┘   │
└────────────────────────────────────────────┘
```

**Screen 2 — Health & Medications:** Same structure; condition/multi-select dropdowns; medication name + dosage fields; allergies input.

**Screen 3 — Lifestyle:** Dropdown selects for diet, exercise, sleep, alcohol, smoking, caffeine, water, stress. All optional.

**Screen 4 — Review:**
```
┌────────────────────────────────────────────┐
│  ← Back     Review Your Intake      ●●●●   │
│  ──────────────────────────────────────── │
│                                            │
│  ┌────────────────────────────────────┐   │
│  │ SUPPLEMENTS (3)                    │   │
│  │ • Vitamin D — 1000 IU Daily        │   │
│  │ • Garlic Extract — 1000mg Daily    │   │
│  │ • Jamu Herbal — 2 caps Daily       │   │
│  └────────────────────────────────────┘   │
│                                            │
│  ┌────────────────────────────────────┐   │
│  │ HEALTH CONDITIONS (2)              │   │
│  │ • Type 2 Diabetes                  │   │
│  │ • Hypertension                     │   │
│  └────────────────────────────────────┘   │
│                                            │
│  ┌────────────────────────────────────┐   │
│  │ MEDICATIONS (3)                    │   │
│  │ • Metformin 500mg BID              │   │
│  │ • Amlodipine 5mg Daily             │   │
│  │ • Atorvastatin 20mg Daily          │   │
│  └────────────────────────────────────┘   │
│                                            │
│  ┌────────────────────────────────────┐   │
│  │ LIFESTYLE                          │   │
│  │ Diet: Omnivore · Exercise: 2-3x/wk │   │
│  │ Sleep: 6-7h · Alcohol: Occasional  │   │
│  └────────────────────────────────────┘   │
│                                            │
│  ┌────────────────────────────────────┐   │
│  │ [✓] I confirm this information is  │   │  Checkbox (required)
│  │     accurate to my knowledge.       │   │
│  └────────────────────────────────────┘   │
│                                            │
│  ⚠️ No changes from your last assessment   │  Warning if unchanged
│  on Sep 15. Submit anyway?                 │
│                                            │
│  ┌────────────────────────────────────┐   │
│  │      [Submit & Analyse →]          │   │
│  └────────────────────────────────────┘   │
└────────────────────────────────────────────┘
```

---

#### S03 — AI Analysis Results

```
┌────────────────────────────────────────────┐
│  ← Dashboard     Analysis Complete         │
│  ──────────────────────────────────────── │
│                                            │
│  ┌────────────────────────────────────┐   │
│  │         YOUR WELLNESS SCORE        │   │  CARD: Elevated
│  │                                    │   │
│  │            ┌──────────┐            │   │
│  │            │    72    │  ⬆ +14   │   │  Animated count-up
│  │            │   /100   │            │   │
│  │            └──────────┘            │   │
│  │         ▓▓▓▓▓▓▓▓▓▓▓▓▓▓░░          │   │  Arc gauge animated
│  │              Good                  │   │
│  │                                    │   │
│  │   Since your last assessment       │   │
│  │   on September 15, 2026            │   │
│  └────────────────────────────────────┘   │
│                                            │
│  ┌────────────────────────────────────┐   │
│  │  SUPPLEMENT SAFETY                 │   │  CARD: Standard
│  │                                    │   │
│  │  ┌──────────────────────────────┐  │   │
│  │  │ 🟢 Vitamin D          [Safe] │  │   │  Supplement Card: Green
│  │  │ Safe at current dosage.      │  │   │
│  │  │                      [▾]    │  │   │
│  │  └──────────────────────────────┘  │   │
│  │                                    │   │
│  │  ┌──────────────────────────────┐  │   │
│  │  │ 🟡 Garlic Extract  [Review]  │  │   │  Supplement Card: Amber
│  │  │ May enhance BP medication    │  │   │
│  │  │ effect. Monitor BP.          │  │   │
│  │  │                      [▾]    │  │   │
│  │  └──────────────────────────────┘  │   │
│  │                                    │   │
│  │  ┌──────────────────────────────┐  │   │
│  │  │ 🔴 Jamu Herbal       [STOP]  │  │   │  Supplement Card: Red
│  │  │ ⚠️ Undeclared ingredients.   │  │   │
│  │  │ Potential liver risk.        │  │   │
│  │  │ STOP & consult doctor.       │  │   │
│  │  │                      [▾]    │  │   │
│  │  └──────────────────────────────┘  │   │
│  └────────────────────────────────────┘   │
│                                            │
│  ┌────────────────────────────────────┐   │
│  │  ⚠️ INTERACTIONS FOUND (2)         │   │  CARD: Alert Amber
│  │                                    │   │
│  │  Garlic Extract + Amlodipine       │   │
│  │  🟡 MODERATE                       │   │
│  │  Additive hypotensive effect.      │   │
│  │                                    │   │
│  │  Jamu + Metformin                  │   │
│  │  🔴 HIGH                           │   │
│  │  Potential hepatotoxic effect.     │   │
│  └────────────────────────────────────┘   │
│                                            │
│  ┌────────────────────────────────────┐   │
│  │  YOUR RECOMMENDATIONS              │   │  CARD: Standard
│  │                                    │   │
│  │  ✓ Continue Vitamin D              │   │
│  │  ⚡ Reduce Garlic Extract to 500mg │   │
│  │  ✕ Stop Jamu immediately          │   │
│  │  💡 Consider CoQ10 supplement     │   │
│  └────────────────────────────────────┘   │
│                                            │
│  ┌────────────────────────────────────┐   │
│  │  [Share with Doctor]               │   │  Primary button
│  └────────────────────────────────────┘   │
│  ┌────────────────────────────────────┐   │
│  │  [Done]                            │   │  Secondary button
│  └────────────────────────────────────┘   │
└────────────────────────────────────────────┘
```

**State: No Risks Found**
- Supplement Safety section: all green cards
- Interactions section: replaced with "✅ No drug-supplement interactions detected. All supplements are safe at current dosages."
- Wellness score card gets a subtle green glow effect

**State: Processing (Skeleton)**
- Wellness gauge: pulsing skeleton circle
- Supplement cards: 3 skeleton rectangles
- Interactions: skeleton card
- Loading indicator: "Analysing your intake..." with progress bar

---

#### S04 — Medical Records Vault

```
┌────────────────────────────────────────────┐
│  Healynx       My Records    [+ Upload]│
│  ──────────────────────────────────────── │
│                                            │
│  🔍 Search records...                      │  Search bar
│                                            │
│  [All] [Lab Reports] [Prescriptions]       │  Filter chips
│  [Scans] [Assessments]                     │
│                                            │
│  ┌────────────────────────────────────┐   │  Record cards
│  │ 📄 HbA1c Test Results              │   │
│  │ Lab Report · Sep 12, 2026          │   │
│  │                                    │   │
│  │ ✦ AI Summary: HbA1c 7.2%           │   │
│  │ (improving from 7.8%). Continue    │   │
│  │ metformin regimen.                 │   │
│  │                           [➤]     │   │
│  └────────────────────────────────────┘   │
│  ┌──────────┐                              │
│  │ [🔒 AES] │  [Share]                    │  Badge + action
│  └──────────┘                              │
│                                            │
│  ┌────────────────────────────────────┐   │
│  │ 📄 Blood Pressure Log              │   │
│  │ Clinical Note · Aug 28, 2026       │   │
│  │ ✦ BP 128/82. Well-controlled.     │   │
│  │                           [➤]     │   │
│  └────────────────────────────────────┘   │
│  ┌──────────┐                              │
│  │ [🔒 AES] │  [Share]                    │
│  └──────────┘                              │
│                                            │
│  ┌────────────────────────────────────┐   │
│  │ 📊 Supplement Assessment           │   │
│  │ Assessment · Sep 16, 2026          │   │
│  │ Wellness Score: 72 · 3 supps       │   │
│  │                           [➤]     │   │
│  └────────────────────────────────────┘   │
│  ┌──────────┐                              │
│  │ [🔒 AES] │  [Share]                    │
│  └──────────┘                              │
│                                            │
│                                   ┌────┐   │
│                                   │ +  │   │  FAB: 56×56px circle
│                                   └────┘   │  `--nv-primary-600`
│                                            │  Position: bottom-right
│  ──────────────────────────────────────── │  24px from edges
│  [🏠 Home]  [💊 Suppl]  [📁 Records]  [👤]  │
└────────────────────────────────────────────┘
```

**FAB Menu (on tap):**
```
                    ┌──────────────────────┐
                    │ 📷 Take Photo        │
                    │ 🖼️ Choose from Library│
                    │ 📁 Browse Files       │
                    └──────────────────────┘
                             ▲
                           [ + ]
```
- Menu expands upward with 3 options
- Backdrop: semi-transparent overlay
- Each option has icon + label, 48px height

**Empty State:**
- Search and filters hidden
- Illustration centred
- "Your health vault is empty. Upload a medical record to get started."
- [Upload First Record →] button

---

#### S05 — Record Share Screen

```
┌────────────────────────────────────────────┐
│  ← Cancel           Share Record           │
│  ──────────────────────────────────────── │
│                                            │
│  Sharing: HbA1c Test Results (PDF, 245KB) │  Document info
│                                            │
│  ──────────────────────────────────────── │
│  RECIPIENT                                 │  Section header
│  ┌────────────────────────────────────┐   │
│  │ 🔍 Search connected doctors...      │   │  Searchable select
│  └────────────────────────────────────┘   │
│                                            │
│  Suggested:                                │
│  ┌────────────────────────────────────┐   │
│  │ ○ Dr. Amira (Primary Care)         │   │  Radio options
│  │ ○ Dr. Wei (Cardiologist)           │   │
│  └────────────────────────────────────┘   │
│                                            │
│  ── or enter email ──                      │
│  ┌────────────────────────────────────┐   │
│  │ cardiologist@hospital.com           │   │  Email input
│  └────────────────────────────────────┘   │
│                                            │
│  ──────────────────────────────────────── │
│  PERMISSIONS                               │
│                                            │
│  ┌────────────────────────────────────┐   │
│  │ ☑ View document              [●──] │   │  Toggle: ON (default)
│  └────────────────────────────────────┘   │
│  ┌────────────────────────────────────┐   │
│  │ ☐ Allow download              [──○] │   │  Toggle: OFF
│  └────────────────────────────────────┘   │
│  ┌────────────────────────────────────┐   │
│  │ ☐ Allow forward/sharing       [──○] │   │  Toggle: OFF
│  └────────────────────────────────────┘   │
│                                            │
│  ──────────────────────────────────────── │
│  ACCESS EXPIRES AFTER                      │
│                                            │
│  ┌──────┬──────┬──────────┬────────┐      │
│  │ 24h  │ 7d ● │  30d     │ Custom │      │  Segmented control
│  └──────┴──────┴──────────┴────────┘      │
│                                            │
│  ──────────────────────────────────────── │
│  ACCESS CODE (optional)                    │
│  ┌────────────────────────────────────┐   │
│  │ ┌──┐ ┌──┐ ┌──┐ ┌──┐ ┌──┐ ┌──┐    │   │  6-digit PIN input
│  │ │ 1│ │ 2│ │ 3│ │ 4│ │ 5│ │ 6│    │   │
│  │ └──┘ └──┘ └──┘ └──┘ └──┘ └──┘    │   │
│  └────────────────────────────────────┘   │
│  Recipient must enter this code to view    │
│                                            │
│  ──────────────────────────────────────── │
│  NOTE TO RECIPIENT (optional)              │
│  ┌────────────────────────────────────┐   │
│  │ Please review before our appoint-  │   │  Textarea
│  │ ment next week.                    │   │
│  └────────────────────────────────────┘   │
│                                            │
│  ┌────────────────────────────────────┐   │
│  │   [Generate Secure Link]           │   │  Primary button
│  └────────────────────────────────────┘   │
└────────────────────────────────────────────┘
```

**Confirmation State:**
```
┌────────────────────────────────────────────┐
│                                            │
│              ✅ Link Created!               │
│                                            │
│  Your record has been shared securely.     │
│                                            │
│  Recipient: cardiologist@hospital.com      │
│  Expires: Sep 23, 2026 (7 days)            │
│  Permissions: View only                    │
│                                            │
│  ┌────────────────────────────────────┐   │
│  │ Healynx.ai/share/xK9mW7qR [📋] │   │  Copyable link
│  └────────────────────────────────────┘   │
│                                            │
│  [Share via WhatsApp]  [Share via Email]   │
│  [Done]                                    │
└────────────────────────────────────────────┘
```

---

### 4.3 Doctor Screens (Desktop-First: 1440px canvas)

#### S06 — Doctor Dashboard

```
┌──────────┬─────────────────────────────────────────────────────────────────┐
│ Sidebar  │  Healynx — Doctor    🔍 Ctrl+K   🔔 [3]  👤 Dr. Amira  ⚙️│  Top bar: 56px
│ 256px    │  ───────────────────────────────────────────────────────────  │
│          │                                                                 │
│ 🏠 Dash  │  Good afternoon, Dr. Amira                      📅 Sep 16, 2026 │
│ 👥 Pat.  │                                                                 │
│ ⚠️ Alerts│  ┌──────────────────┬──────────────────┬────────────────────┐  │
│ 🤖 Chat  │  │ Total Patients   │ Pending Reviews  │ Active Alerts      │  │  KPI Cards
│ 🔒 Emerg │  │       247        │       12         │   🔴3 🟡8 🟢156   │  │  3-col grid
│ 📊 Rep.  │  └──────────────────┴──────────────────┴────────────────────┘  │
│ ⚙️ Set.  │                                                                 │
│          │  ┌────────────────────────────┐  ┌──────────────────────────┐  │
│          │  │ ⚠️ PRIORITY ALERTS          │  │ QUICK ACTIONS            │  │
│          │  │                            │  │                          │  │
│          │  │ 🔴 Rajan K. — Jamu + Metf. │  │ [+ New Patient]          │  │
│          │  │ Hepatotoxic risk. STOP.    │  │ [Emergency Access]       │  │
│          │  │                    [View]  │  │ [View All Alerts →]      │  │
│          │  │────────────────────────── │  │                          │  │
│          │  │ 🔴 Ahmad I. — Supp X +    │  │                          │  │
│          │  │ Warfarin. Bleed risk.     │  │                          │  │
│          │  │                    [View]  │  │                          │  │
│          │  │────────────────────────── │  │                          │  │
│          │  │ 🟡 Priya R. — Wellness    │  │                          │  │
│          │  │ Score dropped 18 pts.     │  │                          │  │
│          │  │                    [View]  │  │                          │  │
│          │  └────────────────────────────┘  └──────────────────────────┘  │
│          │                                                                 │
│ Dr.Amira │  ┌──────────────────────────────────────────────────────────┐  │
│ GP       │  │ PATIENT LIST                            🔍 Search...     │  │
│ Klinik A │  │ ┌────────────┬──────┬───────┬──────────┬──────────────┐ │  │
│     🚪   │  │ │ Patient    │Score │Alerts │Last Visit│AI Summary    │ │  │
│          │  │ ├────────────┼──────┼───────┼──────────┼──────────────┤ │  │
│          │  │ │ Rajan K.   │ 72▲  │ 🔴2   │ Sep 15   │ DM2, HTN,    │ │  │
│          │  │ │            │      │       │          │ 3 supps      │ │  │
│          │  │ │ Priya R.   │ 85   │ 🟢0   │ Sep 14   │ Hypothyroid  │ │  │
│          │  │ │ Ahmad I.   │ 45▼  │ 🔴1   │ Sep 12   │ CKD St3, DM2 │ │  │
│          │  │ │ ...         │      │       │          │              │ │  │
│          │  │ └────────────┴──────┴───────┴──────────┴──────────────┘ │  │
│          │  │                       ← 1 2 3 ... 12 →    [50 per page] │  │
│          │  └──────────────────────────────────────────────────────────┘  │
└──────────┴─────────────────────────────────────────────────────────────────┘
```

**Grid Layout (12-column):**
- KPI cards: 4 columns each (3 cards in row)
- Priority Alerts: 8 columns
- Quick Actions: 4 columns
- Patient List: full 12 columns

---

#### S07 — Patient Detail View

```
┌──────────┬─────────────────────────────────────────────────────────────────┐
│ Sidebar  │  ← Patient List    Rajan a/l Kumar, 58M        [Emergency Access]│
│          │  ───────────────────────────────────────────────────────────  │
│          │                                                                 │
│          │  ┌──────────────────────────────────────────────────────────┐  │
│          │  │ Patient: Rajan a/l Kumar · NRIC: ●●●●●●-●●-●●●●         │  │
│          │  │ DOB: Jan 15, 1968 · DM2 (2018), HTN (2019)              │  │
│          │  │ Primary Doctor: Dr. Amira          ┌──────────┐         │  │
│          │  └────────────────────────────────────│    72    │─────────┘  │
│          │                                        │   /100   │            │
│          │  [Overview] [Records] [Supplements]    │   ⬆+14  │            │
│          │  ────────────────────────────────      └──────────┘            │
│          │                                                                 │
│          │  ┌──────────────────────────────────────┬───────────────────┐  │
│          │  │ ✦ AI CLINICAL SUMMARY      [Flag ✏️] │ ⚠️ ACTIVE ALERTS │  │
│          │  │                                      │                   │  │
│          │  │ Rajan, 58M. DM2 diagnosed 2018.      │ 🔴 Jamu + Metf.  │  │
│          │  │ HTN since 2019. Current meds:         │ Hepatotoxic.     │  │
│          │  │ Metformin 500mg BID, Amlodipine       │ STOP immediately.│  │
│          │  │ 5mg OD, Atorvastatin 20mg OD.         │ [Acknowledge]    │  │
│          │  │ Latest HbA1c: 7.2% (Sep 12).          │                   │  │
│          │  │ 3 supplements: Vitamin D (safe),       │ 🟡 Garlic + Amlo.│  │
│          │  │ Garlic Extract (moderate risk with     │ Additive hypot.  │  │
│          │  │ Amlodipine), Jamu Herbal (🔴 HIGH     │ Monitor BP.      │  │
│          │  │ RISK). Wellness Score: 72 (⬆+14).     │ [Acknowledge]    │  │
│          │  │                                      │                   │  │
│          │  │ Confidence: ██████████░░ 89%          │                   │  │
│          │  └──────────────────────────────────────┴───────────────────┘  │
│          │                                                                 │
│          │  ┌──────────────────────────────────────────────────────────┐  │
│          │  │ SUPPLEMENT ASSESSMENT HISTORY                             │  │
│          │  │                                                          │  │
│          │  │ ● Sep 16, 2026                                           │  │
│          │  │ │ ┌──────────────────────────────────────────────────┐   │  │
│          │  │ │ │ Wellness: 72 ⬆+14 · 3 supps · 🔴 1 interaction   │   │  │
│          │  │ │ │                                          [View →] │   │  │
│          │  │ │ └──────────────────────────────────────────────────┘   │  │
│          │  │ ● Sep 15, 2026                                           │  │
│          │  │ │ ┌──────────────────────────────────────────────────┐   │  │
│          │  │ │ │ Wellness: 58 · 3 supps · 🟡 1 interaction        │   │  │
│          │  │ │ │                                          [View →] │   │  │
│          │  │ │ └──────────────────────────────────────────────────┘   │  │
│          │  └──────────────────────────────────────────────────────────┘  │
│          │                                                                 │
│          │  [🤖 Ask AI Chatbot about this patient]                         │
└──────────┴─────────────────────────────────────────────────────────────────┘
```

**Grid Layout (12-column):**
- Patient header bar: full 12 columns
- AI Summary + Alerts: 8 columns (summary) + 4 columns (alerts)
- Assessment History: full 12 columns (timeline)

**Emergency Access Active State:** Red persistent banner across top: "🔴 EMERGENCY ACCESS ACTIVE — ALL ACTIONS LOGGED | ⏱️ 58:23 remaining | [End Emergency Access]". All records visible. Audit counter visible.

---

#### S08 — Secure Viewer (Shared Record)

```
┌──────────────────────────────────────────────────────────────────────────────┐
│  🔒 Healynx — Secure Viewer          ⏱️ Access expires in 6d 23h 58m  │
├──────────────────────────────────────────────────────────────────────────────┤
│                                                                              │
│  ┌────────────────────────────────────────────────────────────────────────┐  │
│  │                                                                        │  │
│  │                                                                        │  │
│  │    cardiologist@hospital.com  |  Sep 16, 2026 10:45 AM  |  203.106... │  │
│  │                                                                        │  │
│  │                    [DOCUMENT CONTENT]                                   │  │
│  │                                                                        │  │
│  │    cardiologist@hospital.com  |  Sep 16, 2026 10:45 AM  |  203.106... │  │
│  │                                                                        │  │
│  │    cardiologist@hospital.com  |  Sep 16, 2026 10:45 AM  |  203.106... │  │
│  │                                                                        │  │
│  │                                                                        │  │
│  └────────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  ⚠️ View-only document. Copying, downloading, printing, and screenshots      │
│  are prohibited. All access is logged.                                       │
│                                                                              │
│  Shared by: Rajan a/l Kumar                                                  │
│  Note: "Please review HbA1c before our appointment next week."               │
│                                                                              │
│  [Request Extension]                                                          │
└──────────────────────────────────────────────────────────────────────────────┘
```

**Specs:**
- Minimal chrome — no sidebar, no navigation
- Background: `--nv-neutral-900` (dark, medical imaging standard)
- Document area: white, centred, max-width 900px, padding 32px
- Watermark: diagonal repeating pattern at 12% opacity, rotated -20°
- Timer bar: persistent, coloured by remaining time (green→amber→red)
- All browser chrome interactions disabled

---

## 5. Interaction Patterns & Micro-Interactions

### 5.1 AI Processing

**Skeleton + Scan Line Animation:**
- During AI analysis (3s average), the results screen shows skeleton cards
- A subtle blue scan line sweeps top-to-bottom across the card area (1.5s per sweep, repeating)
- Stage indicator row shows real-time pipeline progress:
  - Pending → spinning loader
  - Active → pulsing dot
  - Complete → checkmark with brief scale-up animation (300ms)

**Result Reveal (on completion):**
1. Wellness score gauge: arc draws from 0 to target value (800ms `ease-out`)
2. Score number: counts up from 0 to target (800ms, `ease-out`)
3. Supplement cards: stagger in from right, 100ms delay between each (spring animation)
4. Interaction alerts (if any): fade in with subtle red glow on high-severity (500ms)
5. Recommendations: list items fade in sequentially (50ms stagger)

### 5.2 Secure Actions

**Lock/Unlock Animation:**
- When sharing or accessing a record: lock icon animates from locked→unlocked→locked (600ms total)
- Lock body shakes briefly (rotate ±15°) on the unlock moment
- "Secured" label appears with typewriter effect

**Share Link Generated:**
- Link card slides up with spring animation
- Confetti-like particle burst (subtle, teal-coloured, 3–5 particles)
- "Copied!" toast slides in

**Emergency Access:**
- Activation: screen briefly flashes with red overlay (200ms)
- Persistent banner slides down from top (300ms)
- Timer digits use mono font, animate digit-by-digit
- On expiry/end: banner slides up, screen briefly flashes green (200ms)

### 5.3 Alerts & Risks

**Alert Appearance:**
- New alert badge on bell icon: scale from 0→1.2→1.0 (300ms spring)
- Alert card in list: slides in from top (300ms)
- High-severity (red): subtle pulse animation on border (opacity 0.6→1.0, 3 cycles, 600ms each)
- Moderate (amber): gentle pulse (opacity 0.7→1.0, 2 cycles, 500ms each)
- Low (grey): no pulse — static

**Alert Acknowledgement:**
- Tap "Acknowledge": alert card fades out (300ms), neighbouring cards slide up to fill gap
- Alert count badge decrements with brief scale animation

### 5.4 Pull to Refresh

**Mobile Dashboards:**
- Pull down gesture
- Refresh indicator: teal-coloured activity indicator (iOS-style for iOS, Material for Android)
- "Pull to refresh" → "Release to refresh" → spinner
- Medical cross icon (➕) or shield icon rotates during refresh
- Data updates with fade transition (200ms)

### 5.5 Empty States

**First-Time Empty States:**
- Friendly illustration (line-art style, `--nv-primary-200` colour)
- Illustration bounces in on scroll-into-view (600ms, slight overshoot)
- CTA button pulses subtly after 2 seconds of visibility (to draw attention)

**Zero-Result Empty States:**
- Subdued illustration (no animation — avoid drawing attention to negative state)
- Clear, actionable message and CTA

### 5.6 Haptics (Mobile Only, Optional — Respects System Setting)

| Trigger | Haptic Pattern |
|---|---|
| AI analysis complete | Success notification haptic (2 short taps) |
| Emergency access activated | Heavy warning haptic (long vibration) |
| Share link generated | Light success haptic (1 short tap) |
| Form validation error | Light error haptic (1 short tap) |
| Pull-to-refresh triggered | Light impact haptic |

### 5.7 Transitions

| Transition | Duration | Easing | Usage |
|---|---|---|---|
| Screen push (navigation) | 250ms | `ease-out` | Standard screen-to-screen |
| Screen modal (overlay) | 200ms | `ease-out` | Modals, dialogs |
| Bottom sheet (slide up) | 300ms | `cubic-bezier(0.32, 0.72, 0, 1)` | Mobile sheets |
| AI result reveal | 500–700ms | `cubic-bezier(0.34, 1.56, 0.64, 1)` | Spring overshoot |
| Fade in (content) | 200ms | `ease-in-out` | Tab switches, filter changes |
| List item stagger | 50ms/item | `ease-out` | Card lists, timelines |
| Toast enter/exit | 200ms | `ease-out` | Toasts |

---

## 6. Accessibility Requirements

### 6.1 Colour Contrast

All text must meet WCAG 2.1 AA contrast ratios:
- **Normal text (<18px):** 4.5:1 minimum against background
- **Large text (≥18px or ≥14px bold):** 3:1 minimum against background
- **UI components and graphical objects:** 3:1 minimum

**Validated combinations:**
| Foreground | Background | Ratio | Status |
|---|---|---|---|
| `--nv-neutral-900` (#111827) | `--nv-neutral-000` (#FFFFFF) | 17.4:1 | ✅ AAA |
| `--nv-neutral-700` (#374151) | `--nv-neutral-000` (#FFFFFF) | 10.2:1 | ✅ AAA |
| `--nv-neutral-600` (#4B5563) | `--nv-neutral-000` (#FFFFFF) | 7.5:1 | ✅ AAA |
| `--nv-primary-600` (#1E8C8C) | `--nv-neutral-000` (#FFFFFF) | 4.57:1 | ✅ AA |
| `--nv-danger-600` (#D92D2D) | `--nv-neutral-000` (#FFFFFF) | 5.1:1 | ✅ AA |
| `--nv-neutral-000` (#FFFFFF) | `--nv-primary-600` (#1E8C8C) | 4.57:1 | ✅ AA |
| `--nv-neutral-000` (#FFFFFF) | `--nv-danger-700` (#8C1A1A) | 8.3:1 | ✅ AAA |
| `--nv-accent-700` (#8C5C1E) | `--nv-accent-100` (#FCF0DA) | 6.1:1 | ✅ AA |

**Colour is never the sole differentiator:** All semantic colours (green/amber/red) are always paired with icons and text labels.

### 6.2 Touch Targets

- **Mobile:** Minimum 44×44px for all interactive elements (WCAG 2.5.5)
- **Desktop:** Minimum 40×40px
- Inline links in body text may be smaller but must have sufficient spacing (24px minimum between adjacent links)

### 6.3 Screen Reader Support

**ARIA Labels Required On:**
| Element | ARIA Attribute | Value |
|---|---|---|
| AI-generated content block | `aria-label` | "AI-generated summary. Confidence: 89 percent." |
| Wellness gauge | `aria-label` | "Wellness score: 72 out of 100. Up 14 points since last assessment." |
| Supplement status icon (🟢) | `aria-label` | "Vitamin D: Safe at current dosage" |
| Supplement status icon (🔴) | `aria-label` | "Jamu Herbal: High risk. Stop use and consult doctor." |
| Alert badge on bell icon | `aria-label` | "3 unread alerts" |
| Secure lock badge | `aria-label` | "AES-256 encrypted" |
| Emergency banner | `role="alert"` | "Emergency access active. All actions logged. 58 minutes remaining." |
| AI Chatbot responses | `aria-label` | "AI Assistant response. Sources: Supplement Assessment from September 16, Lab Report from September 12." |

**Form Accessibility:**
- All inputs have associated `<label>` elements
- Error messages linked via `aria-describedby="error-{fieldId}"`
- Required fields marked with `aria-required="true"`
- Form validation errors announced via `role="alert"` on error container

**Landmarks:**
- `<header role="banner">` — top navigation bar
- `<nav role="navigation">` — sidebar, tab bar
- `<main role="main">` — primary content area
- `<footer role="contentinfo">` — (if applicable)

### 6.4 Keyboard Navigation

**Tab Order (Doctor Dashboard):**
1. Skip to main content link (visible on focus)
2. Global search (Ctrl+K)
3. Notification bell
4. User menu
5. Sidebar navigation items (arrow keys within)
6. Main content — KPI cards
7. Alert feed (arrow keys to navigate between alerts)
8. Patient list table (arrow keys for cells, Enter to select patient)
9. Pagination controls

**Focus Styles:**
- All interactive elements receive visible focus ring: `box-shadow: 0 0 0 3px rgba(30, 140, 140, 0.5)` (teal)
- Focus ring offset: 2px from element edge
- Danger elements: `box-shadow: 0 0 0 3px rgba(217, 45, 45, 0.5)` (red)
- Never use `outline: none` without a replacement focus indicator

**Modal Focus Trap:**
- When modal opens, focus moves to first focusable element
- Tab cycles through modal elements only (focus trapped)
- Escape closes modal
- When modal closes, focus returns to triggering element

### 6.5 Motion & Animation

**`prefers-reduced-motion: reduce`:**
- All animations disabled (duration: 0ms)
- Transitions replaced with instant state changes
- Auto-advancing content (carousels) paused
- Loading spinners replaced with static "Loading..." text
- Count-up animations replaced with static final value
- Scan-line and pulse animations disabled

### 6.6 Font Scaling

- Supports browser zoom up to 200% without horizontal scrolling
- Layout uses relative units (`rem`, `%`, `vw`) — no fixed pixel widths on layout containers
- Text truncation with ellipsis + tooltip for long content
- `html { font-size: 100%; }` — respects user's browser default font size

---

## 7. Responsive Behaviour

### 7.1 Breakpoint Definitions

| Breakpoint | Range | Target | Grid Columns | Page Margin | Navigation |
|---|---|---|---|---|---|
| **xs** | 320–375px | Small phones | 4-col | 16px | Bottom tab bar |
| **sm** | 376–767px | Large phones | 4-col | 16px | Bottom tab bar |
| **md** | 768–1023px | Tablets | 8-col | 24px | Side drawer (hamburger) |
| **lg** | 1024–1279px | Small laptops | 12-col | 24px | Persistent sidebar (256px) |
| **xl** | 1280–1439px | Desktops | 12-col, max-width 1280px | auto (centred) | Persistent sidebar (256px) |
| **2xl** | 1440px+ | Large monitors | 12-col, max-width 1280px | auto (centred) | Persistent sidebar (256px) |

### 7.2 Component Adaptations

#### Patient Dashboard

| Element | Mobile (<768px) | Tablet (768–1023px) | Desktop (1024px+) |
|---|---|---|---|
| Layout | Single column stacked cards | 2-column grid | 3-column grid |
| Wellness Gauge | Full-width, prominent | 2-column span | 2-column span in grid |
| AI Insight Card | Full-width | 2-column span | 2-column span |
| Supplement Tracker | Full-width | 1 column | 1 column |
| Quick Actions | Full-width buttons | 2-column button group | Inline button row |
| Reminders | Full-width list | 1 column | 1 column |
| Activity Feed | Full-width | 2-column span | 2-column span |
| Navigation | Bottom tab bar (56px) | Bottom tab bar | Sidebar (256px) |

#### Doctor Dashboard

| Element | Mobile (<768px) | Tablet (768–1023px) | Desktop (1024px+) |
|---|---|---|---|
| Navigation | Bottom tab bar | Side drawer (toggle) | Persistent sidebar (256px) |
| KPI Cards | Stacked (1-col) | 2-col grid | 3-col or 4-col grid |
| Alert Feed | Full-width cards | Full-width cards | 8-column, with quick actions in 4-column |
| Patient List | Card list (table→cards) | Data table (condensed) | Full data table |
| Patient Detail | Stacked tabs | Side tabs | Sidebar + content panel |

#### Data Table → Card List (Mobile)

```
DESKTOP TABLE:                          MOBILE CARD LIST:

┌──────┬──────┬──────┬──────┐          ┌────────────────────┐
│Patient│Score│Alerts│Action│          │ Rajan a/l Kumar    │
├──────┼──────┼──────┼──────┤          │ Score: 72 ⬆+14     │
│Rajan │  72  │ 🔴2  │ [→]  │    →    │ 🔴 2 Alerts        │
│Priya │  85  │ 🟢0  │ [→]  │          │ DM2, HTN · Sep 15  │
│Ahmad │  45  │ 🔴1  │ [→]  │          │              [→]   │
└──────┴──────┴──────┴──────┘          └────────────────────┘
                                       ┌────────────────────┐
                                       │ Priya d/o Ravi     │
                                       │ Score: 85          │
                                       │ 🟢 No Alerts       │
                                       │ Hypothyroid·Sep 14 │
                                       │              [→]   │
                                       └────────────────────┘
```

#### Survey Multi-Step

| Element | Mobile | Desktop |
|---|---|---|
| Layout | Full-screen pages, swipeable between steps | Centred card (max 720px), all steps visible in sidebar progress |
| Progress | Dot indicators (●●●○) | Vertical step list with labels |
| Navigation | Bottom "Continue" button | Right-side button or sidebar click |
| Back | Top-left arrow | Sidebar or top breadcrumb |

#### Modal → Bottom Sheet

| Element | Desktop | Mobile |
|---|---|---|
| Emergency Access | Centred modal (560px) | Full-width bottom sheet (90vh) |
| Share Configuration | Centred modal (600px) | Full-screen page |
| File Upload | Centred modal (480px) | Bottom sheet with camera/library/file actions |

---

## 8. Dark Mode Strategy

### 8.1 Dark Mode Rationale

**Patient App:** Follows device system preference by default, with manual toggle in Profile > Settings. Dark mode for evening/night-time use when checking health data before bed.

**Doctor Dashboard:** Dark mode is **recommended default** for clinical settings. Clinics have harsh fluorescent lighting; dark interfaces reduce eye strain during 8+ hour shifts. Light mode available as an option.

**Secure Viewer:** Always dark surround (regardless of mode) — consistent with medical imaging workstations (DICOM viewer standard). The document itself remains light.

### 8.2 Dark Mode Colour Tokens

#### Surface Colours

| Token | Light | Dark | Usage |
|---|---|---|---|
| `--nv-bg-page` | `--nv-neutral-050` (#F9FAFB) | `--nv-neutral-900` (#111827) | Page background |
| `--nv-bg-surface` | `--nv-neutral-000` (#FFFFFF) | `--nv-neutral-800` (#1F2937) | Card, modal, sidebar background |
| `--nv-bg-surface-hover` | `--nv-neutral-050` (#F9FAFB) | `--nv-neutral-700` (#374151) | Card hover |
| `--nv-bg-surface-elevated` | `--nv-neutral-000` (#FFFFFF) | `--nv-neutral-750` (#283040) | Elevated cards, dropdowns |
| `--nv-bg-input` | `--nv-neutral-000` (#FFFFFF) | `--nv-neutral-800` (#1F2937) | Input fields |
| `--nv-bg-sidebar` | `--nv-neutral-050` (#F9FAFB) | `--nv-neutral-900` (#111827) | Sidebar |

#### Text Colours

| Token | Light | Dark |
|---|---|---|
| `--nv-text-primary` | `--nv-neutral-900` (#111827) | `--nv-neutral-050` (#F9FAFB) |
| `--nv-text-secondary` | `--nv-neutral-600` (#4B5563) | `--nv-neutral-400` (#9CA3AF) |
| `--nv-text-tertiary` | `--nv-neutral-500` (#6B7280) | `--nv-neutral-500` (#6B7280) |

#### Semantic Colours (Dark Mode Adjustments)

| Colour | Light | Dark | Rationale |
|---|---|---|---|
| Green (success) | #2D8C4A on #D9F2E2 | #4ADE80 on #14532D | Brighter green for dark mode readability |
| Amber (warning) | #8C5C1E on #FCF0DA | #FBBF24 on #4A3000 | Brighter amber |
| Red (danger) | #D92D2D on #FCE0E0 | #F87171 on #4A1515 | Brighter red |
| AI Blue | #2563B8 on #DCE9FA | #60A5FA on #1A2744 | Brighter blue |

#### Shadows (Dark Mode)

Shadows are replaced with border + subtle glow:
- Card: `border: 1px solid --nv-neutral-700` instead of `box-shadow`
- Elevated: `border: 1px solid --nv-neutral-600; box-shadow: 0 0 0 1px rgba(255,255,255,0.05)`

#### Brand Colours (Dark Mode)

Primary teal and secondary blue are slightly brightened for dark mode readability:
- `--nv-primary-600` (#1E8C8C) → `--nv-primary-400` (#3DBEBE) for text
- `--nv-primary-600` remains as button background (white text ensures 4.5:1)

### 8.3 Dark Mode Transition

- Transition on `color`, `background-color`, `border-color`, `box-shadow`: 200ms `ease-in-out`
- System preference change: instant theme swap (no animation — user explicitly changed setting)

### 8.4 Exceptions (Always-Light / Always-Dark)

| Element | Mode | Rationale |
|---|---|---|
| Document content area (Secure Viewer) | Always light | Medical documents must be read in their original form |
| Secure Viewer surround | Always dark | Medical imaging workstation standard |
| Emergency Access banner background | Always red (`--nv-danger-700`) | High visibility in any mode |
| Watermark overlay | Always as specified | Server-side rendered, mode-independent |
| Printed output | Always light | Printer-friendly |

---

## 9. Design Tokens Cross-Reference

### 9.1 CSS Custom Properties

All design tokens are defined as CSS custom properties on `:root` (light) and `[data-theme="dark"]` (dark) and consumed via Tailwind CSS config.

**Example token definition:**
```css
:root {
  --nv-primary-600: #1E8C8C;
  --nv-text-primary: #111827;
  --nv-bg-page: #F9FAFB;
  --nv-bg-surface: #FFFFFF;
  --nv-radius-md: 8px;
  --nv-shadow-sm: 0 1px 3px 0 rgba(0,0,0,0.08);
  /* ... all tokens */
}

[data-theme="dark"] {
  --nv-primary-600: #3DBEBE;
  --nv-text-primary: #F9FAFB;
  --nv-bg-page: #111827;
  --nv-bg-surface: #1F2937;
  /* ... dark overrides */
}
```

### 9.2 Tailwind CSS Configuration

Tokens are mapped into Tailwind's `theme.extend`:
```js
// tailwind.config.js
colors: {
  primary: {
    600: 'var(--nv-primary-600)',
    // ...
  },
  // ...
},
fontFamily: {
  heading: ['"Plus Jakarta Sans"', 'Inter', 'system-ui', 'sans-serif'],
  body: ['Inter', 'system-ui', '-apple-system', 'sans-serif'],
  mono: ['"JetBrains Mono"', 'Consolas', 'monospace'],
},
```

---

## 10. IP Traceability in Design

Every UI element involving IP-derived functionality must carry the following design markers:

| IP | Design Marker | Visual Implementation |
|---|---|---|
| **IP 2 (UI 2025002953)** — Supplement Survey | Structured survey instrument | Multi-step wizard with progress indicator; auto-save; pre-population from prior assessments |
| **IP 2** — AI Algorithm | "AI-Powered Analysis" label | Blue secondary colour palette; sparkle icon; AI content card variant |
| **IP 2** — Statistical Scoring Model | Wellness gauge + risk score | Radial gauge with 0–100 scale; quantitative + qualitative labels; trend indicator |
| **IP 2** — Personalised Recommendations | Recommendation list with action icons | C23 component: green/amber/red supplement cards; actionable bullet list with icons |
| **IP 2** — Interaction Detection | Interaction alert rows | C22 component: drug-supplement interaction cards with severity colour-coding and evidence citation |
| **IP 1 (PI 2025007955)** — Encrypted Storage | AES-256 badge + lock icon | `[🔒 AES-256]` chip on every record card; lock animation on upload; encryption status in record detail |
| **IP 1** — AI Summarisation | AI Summary Card with confidence | C18 component: blue-tinted card; sparkle + "AI-GENERATED" label; confidence bar; "Flag Inaccuracy" action |
| **IP 1** — RBAC | Role-appropriate navigation and visibility | Different nav structures per role; hidden/disabled UI elements; "Access Denied" state |
| **IP 1** — Time-Limited Sharing | Share configuration + timer bar | C26 component: expiry selector; access timer countdown; C25 share link cards with revocation |
| **IP 1** — Secure Viewer | Watermark overlay + anti-copy | C27 component: diagonal watermark with recipient identity; disabled interactions; dark surround |
| **IP 1** — Emergency Access | Red persistent banner + reason modal | C20 emergency variant; non-dismissible banner; reason capture modal with quick-reason buttons |
| **IP 1** — Audit Logging | Audit log table in admin dashboard | C14 data table with filterable columns; export functionality; timestamp precision |
| **IP 1** — AI Chatbot | Blue chatbot interface | Chatbot panel with secondary blue palette; source citations; RAG context indicators |

---

*This Design Specification defines the complete visual language, component library, interaction patterns, and accessibility requirements for Healynx v1. All UI implementation shall reference tokens prefixed `--nv-` and components prefixed `C01`–`C30`. Deviations require a design review with the senior UI/UX designer.*
