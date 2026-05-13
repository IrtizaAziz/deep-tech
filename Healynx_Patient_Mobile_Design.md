# Healynx — Patient Mobile Design

**Audience:** Patient (mobile-first responsive web, PWA)
**Canvas:** 375px primary, 320px minimum
**Source IPs:** UI 2025002953 (Supplement Intake Assessment) + PI 2025007955 (Secure Medical Records)

---

## Design Tokens

### Colour Palette

**Primary — Deep Teal** (brand, buttons, links):
```
--nv-primary-600: #1E8C8C (base)
Scale: 900 #0D3B3B | 800 #125C5C | 700 #1A7A7A | 600 #1E8C8C | 500 #26A6A6 | 400 #3DBEBE | 300 #6ED4D4 | 200 #A3E5E5 | 100 #D4F3F3 | 050 #EDFAFA
```

**Secondary — Soft Blue** (AI content):
```
--nv-secondary-600: #2563B8
Scale: 700 #1E4D8C | 600 #2563B8 | 500 #3B82D9 | 300 #7EB3F0 | 100 #DCE9FA | 050 #F0F5FD
```

**Accent — Warm Amber** (alerts):
```
--nv-accent-700: #8C5C1E | 600 #D98C2E | 500 #EDA83E | 300 #F4CD84 | 100 #FCF0DA | 050 #FEF9F2
```

**Semantic:**
- Success Green: base `#2D8C4A` | bg `#D9F2E2` — safe supplements
- Danger Red: base `#D92D2D` | bg `#FCE0E0` | dark `#8C1A1A` — high risk, emergency
- Info Blue: base `#3B82D9` | bg `#DCE9FA`

**Neutrals:**
```
900 #111827 | 800 #1F2937 | 700 #374151 | 600 #4B5563 | 500 #6B7280 | 400 #9CA3AF | 300 #D1D5DB | 200 #E5E7EB | 100 #F3F4F6 | 050 #F9FAFB | 000 #FFFFFF
```

### Colour → Icon Rule (never colour alone)
- Safe → Green + `shield-check`
- Review → Amber + `shield-alert`
- Stop → Red + `shield-x`
- AI Content → Blue + `sparkle`
- Encrypted → Teal + `lock`

### Typography

| Token | Font | Size | Weight | Line | Usage |
|---|---|---|---|---|---|
| h1 | Plus Jakarta Sans | 32px | 700 Bold | 1.2 | Page titles |
| h2 | Plus Jakarta Sans | 24px | 700 Bold | 1.3 | Section headings |
| h3 | Plus Jakarta Sans | 20px | 600 SemiBold | 1.3 | Card titles |
| h4 | Plus Jakarta Sans | 18px | 600 SemiBold | 1.35 | Subsections |
| body-lg | Inter | 18px | 400 Regular | 1.6 | Onboarding, empty |
| body | Inter | 16px | 400 Regular | 1.6 | Default |
| body-sm | Inter | 14px | 400 Regular | 1.5 | Metadata, labels |
| caption | Inter | 12px | 400 Regular | 1.5 | Timestamps |
| overline | Inter | 11px | 600 SemiBold | 1.4 | Badges, uppercase |

### Spacing (8px base)
```
xs:4 | sm:8 | md:12 | lg:16 | xl:24 | 2xl:32 | 3xl:40 | 4xl:48 | 5xl:64
```

### Border Radius
```
xs:4px | sm:6px | md:8px | lg:12px | xl:16px | 2xl:24px | full:9999px
```

### Shadows
```
sm:  0 1px 3px rgba(0,0,0,0.08)
md:  0 4px 6px -1px rgba(0,0,0,0.08)
lg:  0 10px 15px -3px rgba(0,0,0,0.08)
xl:  0 20px 25px -5px rgba(0,0,0,0.1)
focus-ring: 0 0 0 3px rgba(30,140,140,0.25)
```

---

## Mobile Grid

| Breakpoint | Columns | Gutters | Margins | Navigation |
|---|---|---|---|---|
| 320–767px | 4 | 16px | 16px | Bottom TabBar (56px + safe-area) |

All cards full-width. Single-column vertical scroll. 16px gaps between cards.

---

## Navigation — Bottom TabBar

```
┌────────────┬────────────┬────────────┬────────────┐
│     🏠     │     💊     │     📁     │     👤     │
│   Home     │ Supplements│  Records   │  Profile   │
└────────────┴────────────┴────────────┴────────────┘
```

- Fixed bottom, 56px height + safe-area padding
- White bg, 0.5px `neutral-200` top border
- Active: `primary-600` icon + label. Inactive: `neutral-500`
- Icons: 24px. Labels: `caption` 12px

---

## Patient Components

### Button

Variants: primary (filled teal), secondary (outlined teal), danger (filled red), danger-secondary (outlined red), ghost (text grey).
Sizes: sm 32px, md 40px, lg 48px. All full-width in forms.
States: hover (lighten 5%), active (darken 8% + scale 0.98), disabled (40% opacity), loading (spinner).
Focus: `focus-ring` shadow.

### Input Field

Types: text, email, password (show/hide toggle), search, number, textarea.
Height: 44px (md). Border: 1.5px `neutral-300`. Focus: 2px `primary-600` + focus-ring.
Error: 2px `danger-600` + `danger-050` bg + error text below in 14px `danger-600`.
Label above, helper or error below. Required fields: red * on label.

### Card

Variants: standard (white, 1px `neutral-200`, shadow-sm), elevated (shadow-md), ai-content (`secondary-050` bg, `secondary-300` border), alert-amber (`accent-100` bg), alert-red (`danger-100` bg).
Padding: 16px (lg). Optional header (icon + title + action), body, optional footer.

### Bottom Sheet

Slides up from bottom, max 90vh. Drag handle: 32×4px grey bar at top. Close: drag down, X button, backdrop tap. Backdrop: `rgba(0,0,0,0.5)`. Enter: slide up 300ms ease-out.

### Modal (Emergency variant)

Centred, max 560px. 2px `danger-600` border, `danger-050` header. Backdrop non-dismissible.

### Toast

Variants: success (green), error (red), warning (amber), info (blue).
Position: bottom-centre above TabBar. Auto-dismiss: 5s success/info, 10s warning, persist error.
Max 3 stacked. Icon + message + close.

### Badge / Chip

Colours: green, amber, red, blue, grey, teal. Font: `overline` 11px semibold uppercase.
Count badge: red circle 18×18px, white text, "99+" for >99.

### Loading States

Spinner: Lucide `Loader2` rotating 1s infinite. Sizes: 16/20/24/32/48px.
Skeleton: `neutral-200` rect with shimmer sweep 1.5s.
Progress bar: 6px height, `neutral-200` track, `primary-600` fill, rounded-full.

### File Upload Zone

Default: dashed 2px `neutral-300` border, centred icon + "Tap to select. PDF, JPEG, PNG, TIFF (Max 25MB)".
Drag-over: solid `primary-600` border, `primary-050` bg.
Uploading: progress bar + "Encrypting...". Complete: checkmark + "AI summary generating...".
Mobile: camera capture and photo library options.

### FAB (Floating Action Button)

56×56px circle, `primary-600` bg, white + icon. Position: bottom-right, 24px from edges (above TabBar). Expands to action sheet on tap.

### Wellness Gauge

Semi-circular radial gauge. Arc colour: Red (0–40) → Amber (41–60) → Teal (61–80) → Green (81–100).
Score: 48px, Plus Jakarta Sans Bold 700. Trend arrow: ▲ green (up) / ▼ red (down) + delta.
Label below: "Needs Attention" (0–40), "Fair" (41–60), "Good" (61–80), "Excellent" (81–100).
Animated: arc draws 0→value (800ms ease-out), number counts up.

### AI Summary Card

`secondary-050` bg, `secondary-300` border. Header: sparkle icon + "AI-GENERATED" overline in `secondary-600`.
Confidence bar: 4px, `neutral-200` track, `primary-600` fill. Below 60%: amber + warning text.
Footer: model version in mono 12px at 60% opacity. [Flag Inaccuracy] link.

**Rule:** ALL AI content must use this card. No exceptions.

### Supplement Card

Per-supplement, colour-coded:
- Green card (`success` bg): 🟢 + "Safe" badge + explanation
- Amber card (`accent` bg): 🟡 + "Review" badge + explanation
- Red card (`danger` bg): 🔴 + "STOP" badge + explanation
Content: name, dosage/frequency, safety icon + badge, explanation, evidence line, expandable "Learn More".

### Interaction Alert Row

Colour-coded left border: 3px grey (low), 3px amber (moderate), 3px red (high).
Content: supplement + drug names, severity badge, mechanism, recommendation, evidence source.

### Recommendation List

Bulleted with icons: ✓ green (continue), ⚡ amber (modify), ✕ red (stop), 💡 blue (consider).
16px spacing, 20px icon + body text.

### Empty State

Centred: line-art illustration + title + description + CTA button.
Used for: no assessments, no records, no alerts.

### Avatar

32px circle. Photo or initials fallback (`primary-600` bg, white text, first char of first+last name).

### Search Bar

Search icon leading, clear trailing. 44px height. 300ms debounce.

### Multi-Step Progress Indicator

4 circles (24px), connected by lines. Completed: filled `primary-600` + white check. Current: `primary-600` border + white fill. Upcoming: `neutral-300` border + grey. Labels below in `caption`. Mobile: labels hidden, dots only.

### Access Timer Bar

4px height, `neutral-200` track. Fill: `primary-600` (>24h), `accent-600` (<24h), `danger-600` (<1h). Text above: body-sm + mono.

---

## Auth Screens

### Login

```
┌──────────────────────────────┐
│                              │
│         ┌──────┐             │
│         │ Logo │  Healynx    │
│         └──────┘             │
│                              │
│  Your AI-powered preventive  │
│  health companion            │
│                              │
│  ┌──────────────────────────┐│
│  │ Email address            ││
│  └──────────────────────────┘│
│                              │
│  ┌──────────────────────────┐│
│  │ Password          [👁️]   ││
│  └──────────────────────────┘│
│                              │
│  [Forgot password?]          │
│                              │
│  ┌──────────────────────────┐│
│  │        [Log In]          ││  primary, full-width, 48px
│  └──────────────────────────┘│
│                              │
│  Don't have an account?      │
│  [Create Account]            │
└──────────────────────────────┘
```

Centred card, max 440px. Logo + tagline. Email + password inputs. Primary button full-width.

### Register — Step 1: Account

```
┌──────────────────────────────┐
│  ← Back       Create Account │
│  ────────────────────────────│
│                              │
│  Full Name*                  │
│  ┌──────────────────────────┐│
│  │ Rajan a/l Kumar          ││
│  └──────────────────────────┘│
│                              │
│  Email Address*              │
│  ┌──────────────────────────┐│
│  │ rajan@email.com          ││
│  └──────────────────────────┘│
│                              │
│  Phone Number*               │
│  ┌──────┐ ┌─────────────────┐│
│  │ +60 ▼│ │ 12 345 6789    ││
│  └──────┘ └─────────────────┘│
│                              │
│  Password*                   │
│  ┌──────────────────────────┐│
│  │ ••••••••••        [👁️]  ││
│  └──────────────────────────┘│
│  Min 8 chars, 1 uppercase,   │
│  1 number                    │
│                              │
│  ┌──────────────────────────┐│
│  │    [Verify & Continue]   ││
│  └──────────────────────────┘│
└──────────────────────────────┘
```

### Register — Step 2: OTP

```
┌──────────────────────────────┐
│  ← Back     Verify Phone     │
│  ────────────────────────────│
│                              │
│  Enter the 6-digit code      │
│  sent to +60 12 345 6789     │
│                              │
│  ┌──┐ ┌──┐ ┌──┐ ┌──┐ ┌──┐ ┌──┐│
│  │  │ │  │ │  │ │  │ │  │ │  ││
│  └──┘ └──┘ └──┘ └──┘ └──┘ └──┘│
│                              │
│  Didn't receive code?        │
│  [Resend] (wait 30s)         │
│                              │
│  ┌──────────────────────────┐│
│  │       [Verify]           ││
│  └──────────────────────────┘│
└──────────────────────────────┘
```

### Register — Step 3: PDPA Consent

```
┌──────────────────────────────┐
│  ← Back        Data Consent  │
│  ────────────────────────────│
│                              │
│  WHAT WE COLLECT             │
│  • Health conditions & meds  │
│  • Supplement intake history │
│  • Medical records you upload│
│  • Lifestyle information     │
│                              │
│  HOW WE PROTECT YOUR DATA    │
│  • AES-256 encryption at rest│
│  • TLS 1.3 encryption transit│
│  • Data stored in Malaysia   │
│  • PDPA 2010 compliant       │
│                              │
│  YOUR RIGHTS                 │
│  • Access your data anytime  │
│  • Request data deletion     │
│  • Withdraw consent          │
│  • Export your data          │
│                              │
│  ┌──────────────────────────┐│
│  │ [✓] I consent to health  ││
│  │ data processing           ││
│  └──────────────────────────┘│
│                              │
│  ┌──────────────────────────┐│
│  │ [ ] Optional: use        ││
│  │ anonymised data for      ││
│  │ research to improve AI   ││
│  └──────────────────────────┘│
│                              │
│  ┌──────────────────────────┐│
│  │    [Agree & Continue]    ││  disabled until consent checked
│  └──────────────────────────┘│
└──────────────────────────────┘
```

### Register — Step 4: Health Profile

```
┌──────────────────────────────┐
│  ← Back   Health Profile     │
│  ────────────────────────────│
│                              │
│  Date of Birth*              │
│  ┌──────────────────────────┐│
│  │ January 15, 1968  [📅]  ││
│  └──────────────────────────┘│
│                              │
│  Sex*          [Male    ▼]   │
│                              │
│  Known Conditions            │
│  ┌──────────────────────────┐│
│  │ Type 2 Diabetes     [✕] ││
│  │ Hypertension        [✕] ││
│  └──────────────────────────┘│
│  [+ Add Condition]           │
│                              │
│  Current Medications         │
│  ┌──────────────────────────┐│
│  │ Metformin 500mg BID     ││
│  │ Amlodipine 5mg Daily    ││
│  └──────────────────────────┘│
│  [+ Add Medication]          │
│                              │
│  Allergies                   │
│  ┌──────────────────────────┐│
│  │ Penicillin          [✕] ││
│  └──────────────────────────┘│
│  [+ Add Allergy]             │
│                              │
│  ┌──────────────────────────┐│
│  │    [Save & Continue]     ││
│  └──────────────────────────┘│
└──────────────────────────────┘
```

### Onboarding — Tutorial Overlay

After profile setup, a 3-step guided tour highlights Dashboard, Records Vault, Supplement Tracker. Each step: highlighted UI element + tooltip card with description + [Next] / [Skip Tutorial].
Exit: patient lands on Dashboard with welcome toast.

---

## Screen S01 — Patient Dashboard

```
┌──────────────────────────────┐
│  Healynx               👤 RK │  Top bar 44px
│  ────────────────────────────│
│                              │
│  Good morning, Rajan  Sep 16 │  Greeting h5 + caption
│                              │
│  ┌──────────────────────────┐│
│  │  YOUR WELLNESS SCORE     ││  CARD elevated
│  │                          ││
│  │        ┌──────┐          ││
│  │        │  72  │  ▲ +14  ││  WellnessGauge animated
│  │        │ /100 │          ││
│  │        └──────┘          ││
│  │   ▓▓▓▓▓▓▓▓▓▓▓░░  Good   ││  Arc gauge
│  └──────────────────────────┘│
│                              │  16px gap
│  ┌──────────────────────────┐│
│  │ ✦ AI INSIGHT             ││  AI-Summary-Card blue
│  │ Based on your last        ││
│  │ assessment, consider      ││
│  │ reducing garlic extract   ││
│  │ and discussing jamu use   ││
│  │ with Dr. Amira.    [View] ││
│  └──────────────────────────┘│
│                              │
│  ┌──────────────────────────┐│
│  │ SUPPLEMENT TRACKER       ││  CARD standard
│  │ Updated: Just now        ││
│  │                          ││
│  │ 🟢 Vitamin D    Safe     ││  SupplementCards
│  │ 🟡 Garlic Ext   Review   ││
│  │ 🔴 Jamu Herbal  STOP 🆕  ││
│  │                          ││
│  │ [Update Supplement Intake]││  Button primary, full-width
│  └──────────────────────────┘│
│                              │
│  ┌──────────────────────────┐│
│  │ UPCOMING                 ││  CARD standard
│  │ 💊 Metformin · 8:00 AM  ││
│  │ 📋 Update survey · 5d    ││
│  └──────────────────────────┘│
│                              │
│  ┌──────────────────────────┐│
│  │ RECENT ACTIVITY          ││  CARD standard
│  │ ● Assessment · Sep 16    ││  Timeline compact
│  │ ● Record upload · Sep 12 ││
│  └──────────────────────────┘│
│                              │
│  ────────────────────────────│
│  [🏠]  [💊]  [📁]  [👤]      │  TabBar 56px
└──────────────────────────────┘
```

**States:**
- Loading: skeletons for all cards with shimmer
- Empty (first-time): wellness card → "Complete your first supplement survey" CTA. Supplement tracker and AI insight hidden.

---

## Screen S02 — Supplement Survey (Multi-Step)

**Navigation:** Top header with back arrow + "Supplement Intake" + step indicator. TabBar hidden during survey.

### Step 1 of 4 — Supplements

```
┌──────────────────────────────┐
│  ← Back  Supplement Intake   │  Header 44px
│          ●○○○  1 of 4        │
│  ────────────────────────────│
│                              │
│  What supplements are you    │
│  currently taking?           │
│                              │
│  ┌──────────────────────────┐│
│  │ Vitamin D                ││  Supplement entry card
│  │ 1000 IU — Once daily     ││  (pre-populated from last)
│  │ For: Bone health         ││
│  │                  [✏️][🗑️] ││
│  └──────────────────────────┘│
│                              │
│  ┌──────────────────────────┐│
│  │ Garlic Extract            ││
│  │ 1000 mg — Once daily     ││
│  │ For: Blood pressure      ││
│  │                  [✏️][🗑️] ││
│  └──────────────────────────┘│
│                              │
│  ┌──────────────────────────┐│
│  │ [+ Add Another]          ││  Secondary button
│  └──────────────────────────┘│
│                              │
│  ────────────────────────────│
│  ● Step1 ○ Step2 ○ Step3 ○ Step4 │  ProgressIndicator
│  ────────────────────────────│
│                              │
│  ┌──────────────────────────┐│
│  │       [Continue →]       ││  Primary, full-width, 48px
│  └──────────────────────────┘│
└──────────────────────────────┘
```

**Add Supplement Bottom Sheet:**
```
┌──────────────────────────────┐
│  ─── (drag handle) ──── [✕] │
│  Add Supplement              │
│  ────────────────────────────│
│                              │
│  Supplement Name*            │
│  ┌──────────────────────────┐│
│  │ 🔍 Search or type name   ││
│  └──────────────────────────┘│
│                              │
│  Dosage*                     │
│  ┌────────┐ ┌───────────────┐│
│  │ 1000   │ │ IU       [▼] ││
│  └────────┘ └───────────────┘│
│                              │
│  Frequency*     [Daily  ▼]   │
│  Duration (mo)  [12     ▼]   │
│  Purpose                    │
│  ┌──────────────────────────┐│
│  │ Bone health              ││
│  └──────────────────────────┘│
│                              │
│  ┌──────────────────────────┐│
│  │    [Add Supplement]      ││
│  └──────────────────────────┘│
└──────────────────────────────┘
```

### Step 2 of 4 — Health & Medications

```
┌──────────────────────────────┐
│  ← Back  Health & Meds       │
│          ○●○○  2 of 4        │
│  ────────────────────────────│
│                              │
│  HEALTH CONDITIONS           │
│  ┌──────────────────────────┐│
│  │ 🔍 Search conditions...  ││  Multi-select searchable
│  └──────────────────────────┘│
│  [Type 2 Diabetes ✕]         │  Chips
│  [Hypertension ✕]            │
│                              │
│  CURRENT MEDICATIONS         │
│  ┌──────────────────────────┐│
│  │ Metformin                 ││
│  │ 500mg — Twice daily      ││
│  │                  [✏️][🗑️] ││
│  └──────────────────────────┘│
│  [+ Add Medication]          │
│                              │
│  ALLERGIES                   │
│  ┌──────────────────────────┐│
│  │ Penicillin           [✕] ││
│  └──────────────────────────┘│
│  [+ Add Allergy]             │
│                              │
│  ────────────────────────────│
│  ○ Step1 ● Step2 ○ Step3 ○ Step4 │
│  ────────────────────────────│
│                              │
│  ┌──────────┐ ┌────────────┐ │
│  │← Previous│ │[Continue →]│ │
│  └──────────┘ └────────────┘ │
└──────────────────────────────┘
```

### Step 3 of 4 — Lifestyle

```
┌──────────────────────────────┐
│  ← Back  Lifestyle           │
│          ○○●○  3 of 4        │
│  ────────────────────────────│
│                              │
│  Diet            [Omnivore ▼]│  All dropdown selects
│  Exercise        [2-3x/wk ▼]│  All optional
│  Sleep (hrs)     [6-7     ▼]│
│  Alcohol         [Occas.  ▼]│
│  Smoking         [Non-smk ▼]│
│  Caffeine        [2-3 cups▼]│
│  Water (glasses) [5-8     ▼]│
│  Stress Level    [Moderate▼]│
│                              │
│  "Prefer not to say" option  │
│  on every field              │
│                              │
│  ────────────────────────────│
│  ○ Step1 ○ Step2 ● Step3 ○ Step4 │
│  ────────────────────────────│
│                              │
│  ┌──────────┐ ┌────────────┐ │
│  │← Previous│ │[Continue →]│ │
│  └──────────┘ └────────────┘ │
└──────────────────────────────┘
```

### Step 4 of 4 — Review & Confirm

```
┌──────────────────────────────┐
│  ← Back  Review Your Intake  │
│          ○○○●  4 of 4        │
│  ────────────────────────────│
│                              │
│  ┌──────────────────────────┐│
│  │ SUPPLEMENTS (3)          ││
│  │ • Vitamin D 1000 IU Daily││
│  │ • Garlic Extract 1000mg  ││
│  │ • Jamu Herbal 2 caps     ││
│  └──────────────────────────┘│
│                              │
│  ┌──────────────────────────┐│
│  │ CONDITIONS (2)           ││
│  │ • Type 2 Diabetes        ││
│  │ • Hypertension           ││
│  └──────────────────────────┘│
│                              │
│  ┌──────────────────────────┐│
│  │ MEDICATIONS (3)          ││
│  │ • Metformin 500mg BID    ││
│  │ • Amlodipine 5mg Daily   ││
│  │ • Atorvastatin 20mg Daily││
│  └──────────────────────────┘│
│                              │
│  ┌──────────────────────────┐│
│  │ [✓] I confirm this       ││  Checkbox required
│  │ information is accurate   ││
│  └──────────────────────────┘│
│                              │
│  ────────────────────────────│
│  ○ Step1 ○ Step2 ○ Step3 ● Step4 │
│  ────────────────────────────│
│                              │
│  ┌──────────┐ ┌────────────┐ │
│  │← Previous│ │[Submit &   ││  Disabled until checkbox ticked
│  └──────────┘ │ Analyse →] ││
│               └────────────┘ │
└──────────────────────────────┘
```

---

## Screen S03 — AI Analysis Loading

```
┌──────────────────────────────┐
│                              │
│                              │
│        ┌──────────┐          │
│        │    🧠    │          │  Brain icon pulsing
│        └──────────┘          │
│                              │
│     Analysing your intake... │
│                              │
│     ████████████░░░░  75%    │  ProgressBar
│                              │
│  ✓ Validating supplement data│  Stage indicators
│  ✓ Checking drug interactions│  (spinner → checkmark)
│  → Calculating wellness score│
│  ○ Generating recommendations│
│                              │
│  Estimated time: ~3 seconds  │
└──────────────────────────────┘
```

**Behaviour:** Poll task every 500ms. Min display 1.5s. Max 15s then timeout. On completion → auto-navigate to results.

---

## Screen S03b — AI Analysis Results

```
┌──────────────────────────────┐
│  ← Dashboard  Analysis Done  │
│  ────────────────────────────│
│                              │
│  ┌──────────────────────────┐│
│  │  YOUR WELLNESS SCORE     ││  CARD elevated
│  │        ┌──────┐          ││
│  │        │  72  │  ▲ +14  ││  Animated gauge
│  │        │ /100 │          ││  (arc + count-up)
│  │        └──────┘          ││
│  │   ▓▓▓▓▓▓▓▓▓▓▓░░  Good   ││
│  └──────────────────────────┘│
│                              │
│  ┌──────────────────────────┐│
│  │ SUPPLEMENT SAFETY        ││  CARD standard
│  │                          ││
│  │ ┌──────────────────────┐ ││
│  │ │🟢 Vitamin D   [Safe] │ ││  SupplementCard green
│  │ │Safe at current dose. │ ││  Stagger from right
│  │ │               [▾]    │ ││
│  │ └──────────────────────┘ ││
│  │ ┌──────────────────────┐ ││
│  │ │🟡 Garlic Ext [Review]│ ││  SupplementCard amber
│  │ │May enhance BP med    │ ││
│  │ │effect. Monitor BP.   │ ││
│  │ │               [▾]    │ ││
│  │ └──────────────────────┘ ││
│  │ ┌──────────────────────┐ ││
│  │ │🔴 Jamu Herbal [STOP] │ ││  SupplementCard red
│  │ │⚠️ Undeclared ingred.  │ ││
│  │ │Potential liver risk. │ ││
│  │ │STOP & consult doctor.│ ││
│  │ │               [▾]    │ ││
│  │ └──────────────────────┘ ││
│  └──────────────────────────┘│
│                              │
│  ┌──────────────────────────┐│
│  │ ⚠️ INTERACTIONS (2)      ││  CARD alert-amber
│  │                          ││
│  │ Garlic + Amlodipine      ││  InteractionAlertRow
│  │ 🟡 MODERATE              ││  amber left border
│  │                          ││
│  │ Jamu + Metformin         ││  InteractionAlertRow
│  │ 🔴 HIGH                  ││  red left border
│  └──────────────────────────┘│
│                              │
│  ┌──────────────────────────┐│
│  │ YOUR RECOMMENDATIONS     ││  RecommendationList
│  │ ✓ Continue Vitamin D     ││
│  │ ⚡ Reduce Garlic to 500mg││
│  │ ✕ Stop Jamu immediately ││
│  │ 💡 Consider CoQ10        ││
│  └──────────────────────────┘│
│                              │
│  ┌──────────────────────────┐│
│  │ [Share with Doctor]      ││  Primary
│  └──────────────────────────┘│
│  ┌──────────────────────────┐│
│  │ [Done]                   ││  Secondary
│  └──────────────────────────┘│
└──────────────────────────────┘
```

**State: No risks found** → Interactions section: "✅ No drug-supplement interactions detected. All supplements safe." with green tone.

**State: Processing** → Skeleton cards with scan line animation + progress bar.

---

## Screen S04 — Medical Records Vault

```
┌──────────────────────────────┐
│  Healynx     My Records      │
│  ────────────────────────────│
│                              │
│  🔍 Search records...        │  SearchBar
│                              │
│  [All] [Lab Reports] [Prescriptions] │  Filter chips
│  [Scans] [Assessments]       │
│                              │
│  ┌──────────────────────────┐│
│  │ 📄 HbA1c Test Results    ││  Record card
│  │ Lab Report · Sep 12, 2026││
│  │                          ││
│  │ ✦ AI Summary: HbA1c 7.2% ││  Sparkle icon + snippet
│  │ (improving from 7.8%).   ││
│  │                     [➤]  ││
│  └──────────────────────────┘│
│  ┌──────────┐                │
│  │ [🔒 AES] │  [Share]      │  Encryption badge + action
│  └──────────┘                │
│                              │
│  ┌──────────────────────────┐│
│  │ 📄 Blood Pressure Log    ││
│  │ Clinical Note · Aug 28   ││
│  │ ✦ BP 128/82. Controlled. ││
│  │                     [➤]  ││
│  └──────────────────────────┘│
│  ┌──────────┐                │
│  │ [🔒 AES] │  [Share]      │
│  └──────────┘                │
│                              │
│  ┌──────────────────────────┐│
│  │ 📊 Supplement Assessment ││
│  │ Assessment · Sep 16, 2026││
│  │ Wellness: 72 · 3 supps   ││
│  │                     [➤]  ││
│  └──────────────────────────┘│
│                              │
│                         ┌──┐ │
│                         │+ │ │  FAB 56×56px, primary-600
│                         └──┘ │  bottom-right 24px
│                              │
│  ────────────────────────────│
│  [🏠]  [💊]  [📁]  [👤]      │
└──────────────────────────────┘
```

**FAB Menu (on tap):**
```
                    ┌──────────────────┐
                    │ 📷 Take Photo    │
                    │ 🖼️ Library       │
                    │ 📁 Browse Files  │
                    └──────────────────┘
                             ▲
                           [ + ]
```

**Upload Screen:**
```
┌──────────────────────────────┐
│  ← Cancel     Upload Record  │
│  ────────────────────────────│
│                              │
│  ┌──────────────────────────┐│
│  │        📁                ││  FileUploadZone
│  │  Tap to select a file    ││  dashed border
│  │  PDF, JPEG, PNG, TIFF    ││
│  │  (Max 25MB)              ││
│  └──────────────────────────┘│
│                              │
│  Record Type*  [Lab Report▼]│
│  Title*        [__________] │
│  Description   [__________] │
│  Date*         [📅]         │
│                              │
│  ┌──────────────────────────┐│
│  │   [Upload & Process]     ││
│  └──────────────────────────┘│
└──────────────────────────────┘
```

---

## Screen S05 — Record Share (v2)

```
┌──────────────────────────────┐
│  ← Cancel     Share Record   │
│  ────────────────────────────│
│                              │
│  Sharing: HbA1c Results      │
│                              │
│  RECIPIENT                   │
│  ┌──────────────────────────┐│
│  │ 🔍 Search doctors...     ││
│  └──────────────────────────┘│
│  ○ Dr. Amira (Primary Care)  │
│  ○ Dr. Wei (Cardiologist)    │
│  ── or ──                    │
│  ┌──────────────────────────┐│
│  │ cardio@hospital.com      ││
│  └──────────────────────────┘│
│                              │
│  PERMISSIONS                 │
│  ☑ View document      [●──] │  Toggle on (default)
│  ☐ Allow download     [──○] │  Toggle off
│  ☐ Allow forward      [──○] │
│                              │
│  EXPIRY                      │
│  ┌────┬──────┬──────┬──────┐│
│  │24h │ 7d ● │ 30d  │Custom││  Segmented control
│  └────┴──────┴──────┴──────┘│
│                              │
│  ACCESS CODE (optional)      │
│  ┌──┐┌──┐┌──┐┌──┐┌──┐┌──┐   │
│  │  ││  ││  ││  ││  ││  │   │  6-digit PIN
│  └──┘└──┘└──┘└──┘└──┘└──┘   │
│                              │
│  NOTE (optional)             │
│  ┌──────────────────────────┐│
│  │ Please review before our ││
│  │ appointment next week.   ││
│  └──────────────────────────┘│
│                              │
│  ┌──────────────────────────┐│
│  │ [Generate Secure Link]   ││
│  └──────────────────────────┘│
└──────────────────────────────┘
```

**Confirmation:**
```
┌──────────────────────────────┐
│         ✅ Link Created!     │
│                              │
│  Recipient: cardio@hospital  │
│  Expires: Sep 23 (7 days)    │
│  Permissions: View only      │
│                              │
│  healynx.ai/share/xK9mW [📋]│
│                              │
│  [Share via WhatsApp]        │
│  [Share via Email]           │
│  [Done]                      │
└──────────────────────────────┘
```

---

## Patient Profile Screen

```
┌──────────────────────────────┐
│  ←            Profile        │
│  ────────────────────────────│
│                              │
│  ┌────┐                      │
│  │ RK │  Rajan a/l Kumar     │  Avatar lg + name
│  └────┘  rajan@email.com     │
│                              │
│  ────────────────────────────│
│  Personal Information        │
│  DOB: Jan 15, 1968           │
│  Sex: Male                   │
│  Blood Type: O+              │
│                              │
│  ────────────────────────────│
│  Health Profile              │
│  Conditions: DM2, HTN        │
│  Medications: 3 active       │
│  Allergies: Penicillin       │
│  [Edit Health Profile]       │
│                              │
│  ────────────────────────────│
│  My Doctor                   │
│  Dr. Amira — GP              │
│  Klinik Amira, Petaling Jaya │
│                              │
│  ────────────────────────────│
│  Settings                    │
│   Theme     [System      ▼]  │
│   Notifications   [●──]      │
│   Haptics         [●──]      │
│                              │
│  ────────────────────────────│
│  Data & Privacy              │
│  [Export My Data]            │
│  [Request Data Deletion]     │
│  [Manage Consent]            │
│                              │
│  ────────────────────────────│
│  [Log Out]                   │  Danger-secondary
│                              │
│  ────────────────────────────│
│  [🏠]  [💊]  [📁]  [👤]      │
└──────────────────────────────┘
```

---

## General Rules

- Every screen works at 320px width minimum. Test at 320px, 375px, 414px.
- All colour-coded info must have icon + text. Never colour alone.
- Touch targets: 44×44px minimum.
- All AI content: sparkle icon + "AI-GENERATED" label + confidence + model version.
- All encrypted content: lock icon + "AES-256" badge.
- Forms: labels linked to inputs, errors via `aria-describedby`, required via `aria-required`.
- Focus rings visible on all interactive elements.
- `prefers-reduced-motion: reduce` → disable ALL animations.
