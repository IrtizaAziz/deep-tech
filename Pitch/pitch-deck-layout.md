# Healynx — Pitch Deck
## National Deep Tech Challenge 2026 | Universiti Malaya
### 12 Slides · 5-Minute Runtime · IT × Health Track

---

## Design System Reference

### Color Tokens (from `UI_Prompt/doctor_UI.md` `:root`)

| Token | Hex | Usage |
|---|---|---|
| `--nv-primary-600` | `#1e8c8c` | Primary brand color — headers, CTAs, highlight borders |
| `--nv-primary-500` | `#26a6a6` | Primary hover states, accent lines |
| `--nv-primary-100` | `#d4f3f3` | Light primary backgrounds, tinted cards |
| `--nv-accent-500` | `#eda83e` | Warm accent — taglines, badges, callout highlights |
| `--nv-accent-600` | `#d98c2e` | Accent hover, severity badges |
| `--nv-neutral-900` | `#111827` | Headings (dark text on light backgrounds) |
| `--nv-neutral-400` | `#9ca3af` | Body text, muted descriptions |
| `--nv-neutral-000` | `#ffffff` | Slide canvas |
| `--nv-neutral-050` | `#f9fafb` | Card and surface backgrounds |
| `--nv-neutral-100` | `#f3f4f6` | Secondary surface, dividers |
| `--nv-danger-600` | `#d92d2d` | High-severity alerts, warning elements |
| `--nv-success-600` | `#2d8c4a` | Safe indicators, positive stats |

### Typography

| Role | Font | Weight |
|---|---|---|
| Slide headings | Plus Jakarta Sans or Inter | Bold (700) |
| Body / descriptions | Inter | Regular (400) |
| Data labels / badges | Inter | Semibold (600) |

### Global Style Rules

- Light theme throughout (`--nv-neutral-000` canvas)
- Generous whitespace — max 3 concepts per slide
- Minimum text — numbers and icons do the heavy lifting
- No gradient noise — clean, clinical, spacious
- Card surfaces: `--nv-neutral-050` with `1px solid --nv-neutral-200` border
- Narrative arc: **Villain → Suffering → Weapon → Proof → Conquest**

---

## Slide Timing Summary

| # | Slide | Time | Cumulative |
|---|---|---|---|
| 1 | Title | 15s | 0:15 |
| 2 | The Pain (Story) | 30s | 0:45 |
| 3 | Cost of Inaction (Stats) | 20s | 1:05 |
| 4 | The Solution | 25s | 1:30 |
| 5 | Product Demo | 35s | 2:05 |
| 6 | How It Works | 25s | 2:30 |
| 7 | Market Opportunity | 25s | 2:55 |
| 8 | Business Model | 20s | 3:15 |
| 9 | Competitive Landscape | 20s | 3:35 |
| 10 | Roadmap | 30s | 4:05 |
| 11 | Team & Ask | 20s | 4:25 |
| 12 | Closing Vision | 15s | 4:40 |
| — | Buffer | 20s | 5:00 |

---

---

## Slide 1 — Title

**Timing:** 15 seconds | **Purpose:** Grab attention. Bold, disruptive first impression.

### Visual Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                                                                             │
│   [Full-bleed background: dark teal gradient — #0d3b3b → #1a7a7a]         │
│   [Subtle geometric overlay: interconnected hex/circuit nodes in primary]   │
│                                                                             │
│                                                                             │
│                    ┌──────────────────────────┐                            │
│                    │   [HEALYNX LOGO]          │                            │
│                    └──────────────────────────┘                            │
│                                                                             │
│              ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━                     │
│                                                                             │
│                           HEALYNX                                           │
│                  [Font: Plus Jakarta Sans Bold, 72px]                      │
│                  [Color: --nv-neutral-000 #ffffff]                         │
│                                                                             │
│              ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━                     │
│                                                                             │
│        The first integrated AI-powered preventive                           │
│        healthcare platform in Malaysia                                       │
│        [Font: Inter Regular, 22px]                                          │
│        [Color: --nv-accent-500 #eda83e]                                    │
│                                                                             │
│                                                                             │
│   [Bottom-left: National Deep Tech Challenge 2026 · Universiti Malaya]     │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

### Speaker Note — Opening Hook

> "What if I told you that right now, across Malaysia's clinics, there's a patient taking five supplements alongside three prescription drugs — and their doctor has absolutely no idea? This isn't rare. This is every consultation. We built Healynx to fix that."

### Key Content

- **Product Name:** Healynx
- **Tagline:** "The first integrated AI-powered preventive healthcare platform in Malaysia that combines real-time supplement intelligence with secure, AI-enhanced medical records management."
- **Sub-context:** National Deep Tech Challenge 2026 · Universiti Malaya

---

## Slide 2 — The Pain (Story-Driven)

**Timing:** 30 seconds | **Purpose:** Make the problem visceral. Humanise with a real persona.

### Visual Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│  [Background: --nv-neutral-050 with subtle warm undertone on left panel]   │
│                                                                             │
│  ┌───────────────────────────┐  ┌─────────────────────────────────────┐    │
│  │                           │  │                                     │    │
│  │  [Illustration:           │  │  DR. AMINAH                         │    │
│  │   Doctor silhouette at    │  │  General Practitioner               │    │
│  │   desk, surrounded by     │  │  40–50 patients · every day         │    │
│  │   fragmented paper        │  │                                     │    │
│  │   records, phone scans,   │  │  ─────────────────────────────────  │    │
│  │   pill bottles]           │  │                                     │    │
│  │                           │  │  🚫  Patients never disclose        │    │
│  │  [Muted dark palette]     │  │      supplement use                 │    │
│  │                           │  │      → Hidden drug interactions     │    │
│  │                           │  │                                     │    │
│  │                           │  │  📄  Records scattered across       │    │
│  │                           │  │      clinics, paper & phone scans   │    │
│  │                           │  │      → Zero AI, not searchable      │    │
│  │                           │  │                                     │    │
│  │                           │  │  ⚖️  No way to cross-check          │    │
│  │                           │  │      drug–supplement interactions   │    │
│  │                           │  │      → Medico-legal liability       │    │
│  │                           │  │                                     │    │
│  └───────────────────────────┘  └─────────────────────────────────────┘    │
│                                                                             │
│  [Footer quote — italic, --nv-neutral-400]                                 │
│  "Opening a consultation without knowing the patient's jamu prep           │
│   can react with their antihypertensive — that risk is invisible today."   │
└─────────────────────────────────────────────────────────────────────────────┘
```

### Key Content

**Persona:** Dr. Aminah — General Practitioner, private clinic, Klang Valley
- Sees **40–50 patients per day**; ~60% have at least one chronic condition
- **Pain 1:** Patients routinely fail to disclose supplement, herbal, and traditional remedy use — creating dangerous drug-interaction blind spots (PRD §2, Persona 1)
- **Pain 2:** Medical records fragmented across clinics, paper files, and phone-camera scans — non-searchable, no AI to extract meaning (PRD §2, Persona 1)
- **Pain 3:** No automated, localised way to cross-check drug–supplement interactions at point of care — medico-legal risk from undocumented supplement use (PRD §2, Persona 1)

**Secondary persona (bottom card or spoken):** Mr. Rajan — 58 years old, Type 2 diabetes + hypertension, takes 3 prescription meds + 4 supplements including garlic extract and a traditional jamu preparation. Records scattered across two clinics and printed lab reports. Worried about interactions but no one checks. (PRD §2, Persona 2)

### Speaker Note

> "Imagine Dr. Aminah — a GP in a busy Klang Valley clinic. She chose medicine to heal. But every day she's trapped in a labyrinth of invisible supplement risks and fragmented records she can't search. Meanwhile her patient Mr. Rajan is silently taking a jamu preparation that could amplify his blood pressure medication. Nobody knows."

---

## Slide 3 — The Cost of Inaction (Statistics)

**Timing:** 20 seconds | **Purpose:** Quantify the pain with hard numbers.

### Visual Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│  [Background: --nv-neutral-000]                                             │
│                                                                             │
│  THE PROBLEM IN NUMBERS                                                     │
│  [Section label: --nv-primary-600, uppercase, tracking wide]                │
│                                                                             │
│  ┌──────────────────┐  ┌──────────────────┐  ┌──────────────────────────┐  │
│  │                  │  │                  │  │                          │  │
│  │     4.2 / 5      │  │    15–20%        │  │    RM 1.75B              │  │
│  │                  │  │                  │  │                          │  │
│  │  Composite Pain  │  │  of clinician    │  │  Total Addressable       │  │
│  │  Score           │  │  consult time    │  │  Market (Malaysia)       │  │
│  │                  │  │  wasted on       │  │                          │  │
│  │  Severe, frequent│  │  manual record   │  │  Private clinics,        │  │
│  │  costly, no      │  │  review          │  │  hospitals, telemedicine,│  │
│  │  solution exists │  │                  │  │  corporate wellness,     │  │
│  │                  │  │  (Pitch Deck §   │  │  insurance               │  │
│  │  (Pitch Deck §   │  │   2.5)           │  │                          │  │
│  │   2.5)           │  │                  │  │  (Pitch Deck §2.2)       │  │
│  │                  │  │                  │  │                          │  │
│  │  [color:         │  │  [color:         │  │  [color:                 │  │
│  │  --nv-danger-600]│  │  --nv-accent-500]│  │  --nv-primary-600]       │  │
│  └──────────────────┘  └──────────────────┘  └──────────────────────────┘  │
│                                                                             │
│  ┌─────────────────────────────────────────────────────────────────────┐    │
│  │  PAIN DIMENSIONS              Frequency ████████ 4/5               │    │
│  │  (Healynx Pain Score)         Intensity █████████ 5/5              │    │
│  │                               Cost      ████████ 4/5               │    │
│  │  All five dimensions ≥4/5     Urgency   ████████ 4/5               │    │
│  │  — structural, daily,         Reach     ████████ 4/5               │    │
│  │    high-stakes problem.                                             │    │
│  └─────────────────────────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────────────────────┘
```

### Key Content — Sourced from `Healynx_Pitch_Deck.md` §2.5 and §2.2

**Pain Score Dimensions (§2.5):**
| Dimension | Score | Justification |
|---|---|---|
| Frequency | 4/5 | 60% of GP caseload is chronic disease; supplement non-disclosure at every visit |
| Intensity | 5/5 | Drug-supplement interactions can cause hospitalisation, organ damage, or death |
| Cost | 4/5 | 15–20% of consult time wasted on manual record review; days lost on audit prep |
| Urgency | 4/5 | Rising chronic disease burden; digital health mandates active; no Malaysian solution |
| Reachability | 4/5 | Buyers concentrated in Klang Valley, Penang, Johor Bahru; accessible via associations |

**Composite Pain Score: 4.2 / 5** — "A severe, frequent, costly problem with reachable buyers and no existing solution." (Pitch Deck §2.5)

**Market Size (§2.2):** TAM ~RM 1.75B annually across private clinics (~8,000), private hospitals (~220), telemedicine (25+ platforms), corporate employers (1.1M+ establishments), and insurance companies (40+)

### Speaker Note

> "This isn't a minor inconvenience. Drug-supplement interactions cause hospitalisation and death. And right now, there is no Malaysian solution that detects them. This is a 4.2-out-of-5 problem — and a RM 1.75B market waiting to be served."

---

## Slide 4 — The Solution (Story-Driven)

**Timing:** 25 seconds | **Purpose:** Unveil Healynx. Call back Slide 2's persona story.

### Visual Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│  [Split screen with glow transition at center]                              │
│                                                                             │
│  ┌──────────────────────────────┐ ✨ ┌──────────────────────────────────┐  │
│  │  BEFORE HEALYNX              │   │  WITH HEALYNX                    │  │
│  │  [Dark background: #0d3b3b]  │   │  [Light background: --nv-primary-050] │
│  │                              │   │                                  │  │
│  │  🔴 Fragmented records       │ → │  🟢 Unified AI-powered platform  │  │
│  │     across clinics, paper,   │   │     — one encrypted, searchable  │  │
│  │     phone scans              │   │     record vault                 │  │
│  │                              │   │                                  │  │
│  │  🔴 Supplement risks         │ → │  🟢 Real-time supplement         │  │
│  │     invisible to doctors     │   │     intelligence — AI flags      │  │
│  │                              │   │     interactions in <3 seconds   │  │
│  │                              │   │                                  │  │
│  │  🔴 Reactive, episodic care  │ → │  🟢 Preventive closed-loop care  │  │
│  │     — problems found too     │   │     — risks detected before      │  │
│  │     late                     │   │     they become events           │  │
│  └──────────────────────────────┘   └──────────────────────────────────┘  │
│                                                                             │
│  ─────────────────────────────────────────────────────────────────────────  │
│                                                                             │
│  Healynx merges two Universiti Malaya patents into one platform:            │
│  • IP 2 (UI 2025002953) — AI-powered supplement intelligence               │
│  • IP 1 (PI 2025007955) — Secure AI-enhanced medical records               │
│                                                                             │
│  [Safety disclaimer, small: AI outputs are decision-support only.          │
│   All recommendations require clinician verification.]                     │
└─────────────────────────────────────────────────────────────────────────────┘
```

### Key Content — Sourced from `Healynx_Pitch_Deck.md` §1.2–1.4

**Core Value Proposition (§1.3):**
> "The first integrated AI-powered preventive healthcare platform in Malaysia that combines real-time supplement intelligence with secure, AI-enhanced medical records management — enabling clinicians to detect risks, patients to manage their health, and organisations to scale preventive care."

**IP Integration Table (§1.4):**

| Layer | IP 1 Contribution | IP 2 Contribution | Integrated Value |
|---|---|---|---|
| Data Ingestion | OCR digitisation of existing records | Structured supplement intake survey | All health data (clinical + behavioural) in one system |
| Intelligence | AI record summarisation | AI interaction detection + risk scoring | Holistic patient risk profile |
| Security | RBAC, encrypted storage, time-limited sharing | Access-controlled recommendation outputs | Enterprise-grade security for every insight |
| Emergency | Emergency access override mode | Automated red-flag alerts | Safety net for data access and clinical risk |

### Speaker Note

> "Now imagine Dr. Aminah opens Healynx before her next patient. In one tap she sees every supplement risk, every AI summary, every interaction alert — before the patient even sits down. That's not the future. We built that. This weekend."

---

## Slide 5 — Product Demo: Mobile + Web

**Timing:** 35 seconds | **Purpose:** Show, don't tell. Prove the product exists and works.

### Visual Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│  [Background: --nv-neutral-050]                                             │
│                                                                             │
│  ┌─────────────────────────────────┐   ┌──────────────────────────────────┐ │
│  │  [Mobile phone frame]           │   │  [Laptop/browser frame]          │ │
│  │                                 │   │                                  │ │
│  │  ┌─────────────────────┐        │   │  ┌──────────────────────────────┐│ │
│  │  │ Healynx    👤 Rajan │        │   │  │ Healynx Doctor Dashboard     ││ │
│  │  ├─────────────────────┤        │   │  ├──────────────────────────────┤│ │
│  │  │ YOUR WELLNESS SCORE │        │   │  │ ⚠️ 3 Unacknowledged Alerts   ││ │
│  │  │     ┌───────┐       │        │   │  │                              ││ │
│  │  │     │  72   │ ⬆+14  │        │   │  │ Patient: Mr. Rajan           ││ │
│  │  │     └───────┘       │        │   │  │ Wellness Score: 72 ▲         ││ │
│  │  │  Good — improving   │        │   │  │ Last Survey: Today           ││ │
│  │  │                     │        │   │  │                              ││ │
│  │  │ SUPPLEMENT TRACKER  │        │   │  │ 🔴 HIGH: Jamu prep may       ││ │
│  │  │ 🟢 Vitamin D  Safe  │        │   │  │    amplify amlodipine        ││ │
│  │  │ 🟡 Garlic Ext Review│        │   │  │    [Acknowledge]             ││ │
│  │  │ 🔴 Jamu Herbal WARN │        │   │  │                              ││ │
│  │  │                     │        │   │  │ 🟡 MOD: Garlic + atorvastatin││ │
│  │  │ [Share with Doctor] │◄──┐    │   │  │    interaction — monitor     ││ │
│  │  │                     │   │    │   │  │                              ││ │
│  │  └─────────────────────┘   │    │   │  │ AI SUMMARY ─────────────────││ │
│  │                             │    │   │  │ "58M, T2DM + HTN. Garlic    ││ │
│  │  [Callout arrow ①]          │    │   │  │  extract + jamu presents    ││ │
│  │  One-tap supplement risk    │    │   │  │  BP-lowering interaction    ││ │
│  │  check — takes 3 min        │    │   │  │  risk. HbA1c stable."       ││ │
│  │                             │    │   │  │  [Confidence: 91%]          ││ │
│  │                         share    │   │  └──────────────────────────────┘│ │
│  └─────────────────────────────│───┘   └──────────────────────────────────┘ │
│                                └──────────────────────────────────────────── │
│  [Callout ①] One-tap supplement check    [Callout ②] AI interaction cards   │
│  [Callout ③] Pre-filled "Share with Dr"  [Callout ④] High-severity ACK modal│
└─────────────────────────────────────────────────────────────────────────────┘
```

### Prototype Screen References (from `Ideas/Healynx_UX_Flow.md`)

**Mobile — Patient side:**
- `Flow 1, Step 1` — Patient Dashboard with wellness score (72/100), supplement tracker (Vitamin D 🟢, Garlic Extract 🟡, Jamu Herbal 🔴), upcoming reminders (Metformin 500mg — 8:00 AM), and "Update Supplement Intake" CTA
- `Flow 1, Step 5` — Results screen: wellness score ⬆+14, colour-coded supplement cards with severity badges, "Share with Doctor" and "Done" actions

**Web — Clinician side:**
- `Flow 2, Step 1` — Doctor Dashboard: patient list with columns (name, age, last visit, wellness score, alerts by severity, one-line AI summary), 3 unacknowledged high-severity alerts banner
- `Flow 2, Patient Profile` — Patient record view with AI-generated summary (confidence score 91%), drug-supplement interaction cards (severity: High/Moderate/Low), Acknowledge button triggering `POST /medication-conflicts/{conflictId}/acknowledge`

### Feature Callouts

| # | Callout | UX Location |
|---|---|---|
| ① | One-tap supplement intake — pre-populated from last survey | Patient Dashboard → Survey Screen 1 |
| ② | AI interaction cards with severity badges (🔴/🟡/🟢) | Patient Results + Doctor Dashboard |
| ③ | "Share with Doctor" — pre-filled secure share flow | Patient Results Screen |
| ④ | High-severity acknowledgement modal — clinician must confirm | Doctor Dashboard → Alert card |

---

## Slide 6 — How It Works (3-Step Journey)

**Timing:** 25 seconds | **Purpose:** Simplify complexity. Make the architecture legible in one glance.

### Visual Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│  [Background: --nv-neutral-000]                                             │
│                                                                             │
│  HOW HEALYNX WORKS                                                          │
│                                                                             │
│  ┌───────────────┐        ┌───────────────┐        ┌───────────────┐       │
│  │               │  ───►  │               │  ───►  │               │       │
│  │       01      │        │       02      │        │       03      │       │
│  │               │        │               │        │               │       │
│  │  [📋 ICON]    │        │  [🤖 ICON]    │        │  [🔒 ICON]    │       │
│  │               │        │               │        │               │       │
│  │  CAPTURE      │        │  ANALYSE      │        │  ACT          │       │
│  │               │        │               │        │               │       │
│  │  Patient      │        │  Dual-engine  │        │  Results land │       │
│  │  completes    │        │  AI processes │        │  in the       │       │
│  │  structured   │        │  responses:   │        │  patient's    │       │
│  │  supplement   │        │  interaction  │        │  encrypted    │       │
│  │  survey on    │        │  detection +  │        │  record &     │       │
│  │  mobile in    │        │  statistical  │        │  doctor's     │       │
│  │  <3 minutes   │        │  risk scoring │        │  dashboard    │       │
│  │               │        │  in <3 seconds│        │  simultaneously│      │
│  │               │        │               │        │               │       │
│  │  [Mini proto: │        │  [Mini proto: │        │  [Mini proto: │       │
│  │  Survey       │        │  Loading/     │        │  Doctor dash  │       │
│  │  Screen 1]    │        │  Analysis     │        │  with alert]  │       │
│  │               │        │  animation]   │        │               │       │
│  └───────────────┘        └───────────────┘        └───────────────┘       │
│                                                                             │
│  [Card borders: 1px solid --nv-neutral-200 | Step numbers: --nv-primary-600]│
│                                                                             │
│  Powered by IP UI 2025002953 (Supplement AI) + PI 2025007955 (Secure Records)│
└─────────────────────────────────────────────────────────────────────────────┘
```

### Key Content — Sourced from `Healynx_Pitch_Deck.md` §1.2

**Step 1 — CAPTURE:**
- Patient completes structured supplement intake survey via mobile or web
- Survey captures: supplement/product name, dosage, frequency, duration, health condition being addressed, existing medications, allergies, comorbidities
- Takes under 3 minutes; renders correctly from 320px mobile to 1920px desktop
- Pre-populated from last completed survey for returning patients

**Step 2 — ANALYSE:**
- Dual-engine AI processes survey responses (IP 2 — UI 2025002953):
  - **AI Algorithm** — trained on expert-verified, pre-labelled Malaysian medical data; detects drug-supplement and supplement-supplement interactions
  - **Statistical Scoring Model** — evaluates patient variables (dosage, frequency, comorbidities) to generate numeric risk score (0–100) and categorical benefit level
- Results returned in **under 3 seconds** (NFR-05)
- Target accuracy: ≥85% sensitivity against clinical pharmacist review

**Step 3 — ACT:**
- Risk scores, interaction alerts, and personalised recommendations stored in patient's AES-256 encrypted medical record (IP 1 — PI 2025007955)
- Patient receives wellness score + plain-language recommendations on mobile dashboard
- Clinician receives high/moderate/low severity alert cards on clinical dashboard within 10 seconds
- Emergency access override available 24/7 with automatic audit logging

---

## Slide 7 — Market Opportunity

**Timing:** 25 seconds | **Purpose:** Prove the size of the prize.

### Visual Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│  [Background: --nv-neutral-050]                                             │
│                                                                             │
│  THE MARKET OPPORTUNITY                                                     │
│                                                                             │
│  ┌──────────────────────────────────────┐  ┌───────────────────────────┐   │
│  │                                      │  │                           │   │
│  │    [Concentric circle / funnel]      │  │  KEY MARKET DRIVERS       │   │
│  │                                      │  │                           │   │
│  │  ┌──────────────────────────────┐    │  │  📈 Supplement            │   │
│  │  │           TAM                │    │  │     consumption growth    │   │
│  │  │        ~RM 1.75B             │    │  │     (aging population,    │   │
│  │  │        annually              │    │  │      post-pandemic health  │   │
│  │  │                              │    │  │      consciousness)        │   │
│  │  │  ┌──────────────────────┐    │    │  │                           │   │
│  │  │  │        SAM           │    │    │  │  🏥 Digital health        │   │
│  │  │  │     ~RM 350M         │    │    │  │     expansion (MOH        │   │
│  │  │  │     annually         │    │    │  │     mandates, telemedicine │   │
│  │  │  │                      │    │    │  │     reforms)              │   │
│  │  │  │  ┌────────────────┐  │    │    │  │                           │   │
│  │  │  │  │     SOM        │  │    │    │  │  💊 Chronic disease       │   │
│  │  │  │  │  ~RM 780K ARR  │  │    │    │  │     burden rising         │   │
│  │  │  │  │   (Year 3)     │  │    │    │  │     (diabetes, HTN,       │   │
│  │  │  │  └────────────────┘  │    │    │  │      cardiovascular)      │   │
│  │  │  └──────────────────────┘    │    │  │                           │   │
│  │  └──────────────────────────────┘    │  │  🔒 PDPA enforcement      │   │
│  │                                      │  │     turning compliance    │   │
│  │  [TAM: --nv-primary-100 fill]        │  │     into competitive      │   │
│  │  [SAM: --nv-primary-300 fill]        │  │     advantage             │   │
│  │  [SOM: --nv-primary-600 fill]        │  │                           │   │
│  └──────────────────────────────────────┘  └───────────────────────────┘   │
│                                                                             │
│  Urban focus: ~3,500 private clinics · 15 telemedicine platforms            │
│  · 800 mid-to-large corporate employers (Klang Valley, Penang, JB)         │
└─────────────────────────────────────────────────────────────────────────────┘
```

### Key Content — Sourced from `Healynx_Pitch_Deck.md` §2.2–2.3

**TAM — Total Addressable Market (§2.2):**
| Segment | Count | Annual Digital Health Spend |
|---|---|---|
| Private clinics | ~8,000 | RM 400M+ |
| Private hospitals | ~220 | RM 600M+ |
| Telemedicine platforms | 25+ | RM 150M+ |
| Corporate employers | 1.1M+ establishments | RM 300M+ |
| Insurance companies | 40+ | RM 200M+ |
| Pharmacies | ~3,000 | RM 100M+ |
| **Total TAM** | | **~RM 1.75B annually** |

**SAM — Serviceable Addressable Market (§2.2):**
- ~3,500 private clinics in Klang Valley, Penang, Johor Bahru
- ~15 active telemedicine platforms
- ~800 mid-to-large corporate employers with formal wellness programs
- **SAM: ~RM 350M annually**

**SOM — Year 3 Target (§2.2):**
| Segment | Target Accounts | ARPU/yr | Year 3 Revenue |
|---|---|---|---|
| Private clinics | 80 | RM 5,000 | RM 400,000 |
| Multi-branch clinic groups | 5 | RM 30,000 | RM 150,000 |
| Telemedicine platforms | 2 | RM 40,000 | RM 80,000 |
| Corporate wellness | 8 | RM 15,000 | RM 120,000 |
| API licensing | 3 | RM 10,000 | RM 30,000 |
| **Total SOM (Year 3)** | **98 accounts** | | **~RM 780K ARR** |

---

## Slide 8 — Business Model

**Timing:** 20 seconds | **Purpose:** Show how this becomes a real company.

### Visual Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│  [Background: --nv-neutral-000]                                             │
│                                                                             │
│  HOW WE MAKE MONEY                                                          │
│                                                                             │
│  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐        │
│  │    💼    │ │    🔌    │ │    🏢    │ │    👥    │ │    📊    │        │
│  │          │ │          │ │          │ │          │ │          │        │
│  │Healthcare│ │   API    │ │Enterprise│ │Corporate │ │  Data    │        │
│  │Provider  │ │Licensing │ │  Custom  │ │ Wellness │ │Analytics │        │
│  │  SaaS    │ │          │ │          │ │          │ │          │        │
│  │          │ │          │ │          │ │          │ │          │        │
│  │  55%     │ │  15%     │ │  10%     │ │  15%     │ │   5%     │        │
│  │recurring │ │usage-    │ │one-time  │ │per-      │ │project/  │        │
│  │          │ │based     │ │+maint    │ │employee  │ │annual    │        │
│  └──────────┘ └──────────┘ └──────────┘ └──────────┘ └──────────┘        │
│                                                                             │
│  ─────────────────────────────────────────────────────────────────         │
│                                                                             │
│  PRICING (Clinic Tier)              UNIT ECONOMICS                          │
│  Essentials   RM 350/mo             ARPU: RM 5,000/yr                      │
│  Professional RM 700/mo             Gross Margin: 55–65% (early)           │
│  Enterprise   Custom                             65–75% (steady state)     │
│                                     Break-Even: Month 30–36                │
│  API Starter  RM 1,200/mo           LTV:CAC Ratio: 5:1 to 8:1             │
│  Corporate    RM 10–14/employee/yr                                          │
└─────────────────────────────────────────────────────────────────────────────┘
```

### Key Content — Sourced from `Healynx_Pitch_Deck.md` §4.2–4.4

**Revenue Streams (§4.2):**
1. **Healthcare Provider Subscriptions** — Monthly/annual SaaS for clinics, hospitals, telemedicine (55% contribution target)
2. **API Licensing** — Supplement Intelligence Engine + AI Summarisation API, usage-based tiers (15%)
3. **Enterprise Customisation** — Custom dashboards, bespoke integrations, advanced security layers (10%)
4. **Corporate Wellness Solutions** — Per-employee preventive health portal + aggregate insights (15%)
5. **Privacy-Compliant Data Analytics** — Anonymised supplement trends for researchers and public health agencies (5%)

**Pricing Tiers (§4.3):**

*Clinic Tier:*
| Plan | Monthly | Annual | Seats |
|---|---|---|---|
| Essentials | RM 350 | RM 3,500 | Up to 3 clinician accounts, 500 patient records |
| Professional | RM 700 | RM 7,000 | Up to 10 clinician accounts, 2,000 records, chatbot |
| Enterprise | Custom | Custom | Unlimited, white-label, SLA, dedicated CSM |

*Telemedicine / Platform:*
| Plan | Monthly | Includes |
|---|---|---|
| API Starter | RM 1,200 | 5,000 API calls/month, supplement intelligence endpoint |
| API Growth | RM 2,800 | 25,000 API calls, both AI endpoints, webhook, SLA |

*Corporate Wellness:*
| Plan | Annual | Per Employee |
|---|---|---|
| Starter | RM 7,200 | RM 14 (up to 500 employees) |
| Growth | RM 20,000 | RM 10 (up to 2,500 employees) |

**Gross Margin & Break-Even (§4.4):**
- Gross margin: 55–65% (early years) → 65–75% (steady state)
- Break-even: Month 30–36 (mid-to-late Year 3)
- >75% of revenue recurring at steady state

---

## Slide 9 — Competitive Landscape

**Timing:** 20 seconds | **Purpose:** Prove defensibility and show Healynx's unique position.

### Visual Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│  [Background: --nv-neutral-050]                                             │
│                                                                             │
│              High ↑ Feature Depth / AI Capability                          │
│              │                                                              │
│              │                         ⭐ HEALYNX                           │
│              │                         [Circle: --nv-primary-600, bold]     │
│              │                         "Only platform with AI supplement    │
│              │                          intelligence + secure records"      │
│              │                                                              │
│  Traditional │                                                              │
│  EMR         │              Telemedicine                                    │
│  (Caresys,   │              Platforms                                       │
│  KMS,        │              (DoctorOnCall,                                  │
│  Mediviron)  │              Doctor Anywhere,                                │
│              │              BookDoc)                                        │
│  ─────────── │ ─────────────────────────────────────────────→              │
│  Low         │              High                                            │
│              │              Ease of Use / Deployment Speed                  │
│  Generic     │                                                              │
│  Health Apps │                                                              │
│  (HealthifyMe│                                                              │
│  MyFitnessPal│                                                              │
│  Samsung     │                                                              │
│  Health)     │                                                              │
│              │                                                              │
│  [Gap annotation: "No competitor combines supplement AI + secure records"]  │
└─────────────────────────────────────────────────────────────────────────────┘
```

### Key Content — Sourced from `Healynx_Pitch_Deck.md` §6.1–6.4

**Competitor Categories (§6.1):**
| Category | Malaysia Examples | Primary Weakness vs Healynx |
|---|---|---|
| Traditional EMR | Caresys, KMS Healthcare, Mediviron | No AI, no supplement intelligence, weak preventive health |
| Telemedicine Platforms | DoctorOnCall, Doctor Anywhere, BookDoc | Consultation-centric, no longitudinal records, no supplement analysis |
| Generic Health Apps | HealthifyMe, MyFitnessPal, Samsung Health | Not medical-grade, no PDPA compliance, no clinical integration |
| Hospital HIS | iHIS (MOH), Cerner | Heavy, expensive, not for clinic/telemedicine segment, no supplement AI |

**Capability Gap (§6.2) — Healynx leads in every AI-powered dimension:**
| Capability | Traditional EMR | Telemedicine | Generic Apps | Healynx |
|---|---|---|---|---|
| Supplement Intake Assessment | ❌ | ❌ | ⚠️ Manual | ✅ **AI-Powered** |
| Drug-Supplement Interaction Detection | ❌ | ❌ | ❌ | ✅ **Expert-Validated AI** |
| AI Record Summarisation | ⚠️ Limited | ⚠️ Limited | ❌ | ✅ **Advanced** |
| Time-Limited Secure Sharing | ❌ | ❌ | ❌ | ✅ **Advanced** |
| Emergency Access Mode | ❌ | ❌ | ❌ | ✅ **Built-In** |

**UVP (§6.3):**
> "Healynx is the only platform in Malaysia that unifies AI-driven supplement intelligence, secure medical records management, and preventive healthcare analytics into a single cloud-native ecosystem."

**Competitive Moats (§6.4):**
- Dual UM patent protection (UI 2025002953 + PI 2025007955)
- Proprietary expert-labelled Malaysian supplement dataset (years to replicate)
- Regulatory first-mover: PDPA, ISO 27001, AI governance documentation
- Data network effects: more patients → larger dataset → better AI → more adoption
- High switching costs once clinic records are digitised and AI-enriched

---

## Slide 10 — Roadmap

**Timing:** 30 seconds | **Purpose:** Show ambition and a credible execution plan.

### Visual Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│  [Background: --nv-neutral-000]                                             │
│                                                                             │
│  FROM PROTOTYPE TO REGIONAL PLATFORM                                        │
│                                                                             │
│  ──────────────────────────────────────────────────────────────────────     │
│  │           │             │               │              │               │ │
│  ▼           ▼             ▼               ▼              ▼               │ │
│                                                                             │
│  Phase 1      Phase 2       Phase 3        Phase 4                          │
│  VALIDATE     PARTNER &     COMMERCIAL     ASEAN                            │
│               COMPLIANCE    LAUNCH         SCALE                            │
│                                                                             │
│  Months 0–9   Months 6–15   Months 15–24   Years 3–5                        │
│  TRL 5 → 6    TRL 6 → 7     Revenue Gen    200–380 accounts                │
│                                                                             │
│  ✦ Pilot at   ✦ 3–5 clinic  ✦ B2B SaaS     ✦ Singapore (Y2–3)             │
│    UM Medical   agreements    goes live     ✦ Indonesia (Y3–4)              │
│    Centre     ✦ 2 telemedi- ✦ 3–5 person   ✦ Thailand (Y3–4)              │
│  ✦ AI target:   cine APIs    sales team    ✦ Vietnam (Y4–5)               │
│    ≥85%       ✦ PDPA +      ✦ 20 paying    ✦ Philippines (Y4–5)           │
│    sensitivity  ISO 27001     accounts     ✦ 100s of clinics              │
│  ✦ 5 pilot      compliance    by Mth 24   ✦ ASEAN supplement DB           │
│    sites                    ✦ MRR target:                                 │
│  ✦ Security                   RM 32K by                                   │
│    pen test                   Month 24                                     │
│    passed                                                                   │
│                                                                             │
│  ── ── ── ── ── ── ── ── ── ── ── ── ── ── ── ── ── ── ── ── ── ──         │
│  ★ YOU ARE HERE                                                             │
│  [Marker on Phase 1 start]                                                  │
└─────────────────────────────────────────────────────────────────────────────┘
```

### Key Content — Sourced from `Healynx_PRD.md` §9.2–9.5 and `Healynx_Pitch_Deck.md` §3.1

**Phase 1 — Foundation & Validation (Months 0–9):**
- Deploy at UM Medical Centre; integrate both IPs into unified prototype
- Build: supplement survey (FR-01), AI interaction detection (FR-02, FR-03), real-time recommendations (FR-04), clinician dashboard (FR-18), encrypted records (FR-07), RBAC (FR-10), audit logs (FR-21)
- **Exit Gate:** 5 pilot sites, 30 clinicians, 500 patients, AI sensitivity ≥85%, zero critical security findings, penetration test passed (PRD §9.2)

**Phase 2 — Partnerships & Regulatory Readiness (Months 6–15):**
- Sign 3–5 pilot clinic agreements (KPJ, Columbia Asia target); 2 telemedicine integrations (DoctorOnCall target)
- PDPA compliance programme initiated; MDA regulatory pathway determined
- Secure sharing via time-limited links (FR-11, FR-12); OCR document digitisation (FR-08)
- **Exit Gate:** 15 paying clinic accounts, 2 telemedicine integrations, RM 5K MRR (PRD §9.3)

**Phase 3 — Commercial Launch (Months 15–24):**
- B2B SaaS platform goes live; direct sales team of 3–5 people
- Target 20 paying accounts by Month 24; RM 32K MRR target by Month 24
- **Exit Gate:** 40+ accounts, break-even on horizon (Month 30–36) (PRD §9.4)

**Phase 4 — ASEAN Scale (Years 3–5):**
- Singapore (Year 2–3) → Indonesia (Year 3–4) → Thailand (Year 3–4) → Vietnam/Philippines (Year 4–5)
- Build ASEAN-wide supplement database covering Jamu, TCM, Ayurveda
- Target: 200–380 paying accounts, RM 1.8M–3.5M ARR (PRD §9.5, §9.6)

---

## Slide 11 — Team & Ask

**Timing:** 20 seconds | **Purpose:** Credibility + clear funding request.

### Visual Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│  [Background: --nv-neutral-050]                                             │
│                                                                             │
│  ┌────────────────────────────────────┐  ┌───────────────────────────────┐  │
│  │  THE TEAM                          │  │  THE ASK                      │  │
│  │                                    │  │                               │  │
│  │  CORE FOUNDERS                     │  │  RM 1.5M – 2.5M               │  │
│  │  ─────────────────────             │  │  [Color: --nv-primary-600]    │  │
│  │  Saiket Das         Co-Founder     │  │  Seed / Commercialisation     │  │
│  │  Irtiza Aziz        Co-Founder     │  │  Funding                      │  │
│  │  Khan Safwan Hasan  Co-Founder     │  │                               │  │
│  │  Saif Abdullah      Co-Founder     │  │  24-month path to revenue     │  │
│  │  Md Musa Al Kafi    Co-Founder     │  │                               │  │
│  │                                    │  │  ─────────────────────────    │  │
│  │  5 Universiti Malaya CS founders   │  │  USE OF FUNDS                 │  │
│  │  Hackathon-proven builders         │  │  AI Development    22–23%     │  │
│  │  Embedded in UM ecosystem          │  │  Product Dev       19–20%     │  │
│  │                                    │  │  Clinical Valid.   12–14%     │  │
│  │  KEY ADVISORS                      │  │  Security Infra    12–13%     │  │
│  │  ─────────────────────             │  │  Regulatory        9–10%      │  │
│  │  Dr. Lee Ching Shya                │  │  Sales & Marketing 9–10%      │  │
│  │  IP Inventor (UI 2025002953)       │  │  Cloud Infra       7%         │  │
│  │  RTTP, Universiti Malaya           │  │  Ops & Contingency 4%         │  │
│  │                                    │  │                               │  │
│  │  Dr. Nurul Fauzani Jamaluddin      │  │  Lean team (5 pax, Mths 1–12) │  │
│  │  IP Inventor (PI 2025007955)       │  │  Scale to 8 post-pilot        │  │
│  │  RTTP, Universiti Malaya           │  │  Break-even: Month 30–36      │  │
│  │                                    │  │                               │  │
│  └────────────────────────────────────┘  └───────────────────────────────┘  │
│                                                                             │
│  [No team photos — names and roles only. Product and data do the talking.] │
└─────────────────────────────────────────────────────────────────────────────┘
```

### Key Content — Sourced from `Healynx_Pitch_Deck.md` §9 and §8.1–8.3

**Core Team (§9):**
| Name | Role |
|---|---|
| Saiket Das | Co-Founder |
| Irtiza Aziz | Co-Founder |
| Khan Safwan Hasan | Co-Founder |
| Saif Abdullah | Co-Founder |
| Md Musa Al Kafi | Co-Founder |

5 Universiti Malaya computer science students. Hackathon-proven builders. Embedded in the same university that owns the IP, employs the inventors, and operates UM Medical Centre — the pilot deployment site. (Pitch Deck §9)

**Key Advisors / IP Inventors (§9):**
- **Dr. Lee Ching Shya** — IP Inventor (UI 2025002953), Registered Technology Transfer Professional (RTTP), Universiti Malaya. Contact: leecs@um.edu.my
- **Dr. Nurul Fauzani Jamaluddin** — IP Inventor (PI 2025007955), Registered Technology Transfer Professional (RTTP), Universiti Malaya. Contact: nfauzani@um.edu.my

**The Ask (§8.1):**
Seeking **RM 1.5M – RM 2.5M** in Seed / Commercialisation Funding. Covers 24-month path from integrated prototype to revenue-generating platform.

**Fund Allocation (§8.2):**
| Area | Allocation (RM) | % |
|---|---|---|
| AI Model Development & Training | RM 350K – 550K | 22–23% |
| Product Development (mobile, web, API) | RM 280K – 500K | 19–20% |
| Clinical Validation (pilots, UX research) | RM 180K – 350K | 12–14% |
| Security Infrastructure (pen test, ISO 27001 alignment) | RM 180K – 320K | 12–13% |
| Regulatory Compliance (PDPA, MDA, legal) | RM 130K – 250K | 9–10% |
| Sales & Marketing (B2B team, conferences) | RM 130K – 250K | 9–10% |
| Cloud Infrastructure (AWS MY region) | RM 100K – 180K | 7% |
| Operations & Contingency | RM 50K – 100K | 4% |
| **TOTAL** | **RM 1.5M – 2.5M** | **100%** |

**Burn Rate & Runway (§8.3):**
- Lean team (5 people): RM 55K/month → **27 months on RM 1.5M**
- **Recommendation:** Start lean (5-person, Months 1–12), scale to 8 post-pilot at Month 12 → stretches RM 1.5M to 22-month runway

**Contingency — Zero External Funding:**
- Path 1: Apply to 5 Malaysian grants simultaneously by Month 2 (CREST, MDEC GAH, UMCIE, MIDA, MTDC) — even 2 wins give RM 275K bridge
- Path 2: Partner pre-commitments from KPJ/Columbia Asia (RM 30–60K) + DoctorOnCall (RM 40–50K)
- Path 3: Self-funded lean MVP on AWS Activate credits (~RM 75–100K) + UM infrastructure
- Bootstrap Year 3 ARR target: RM 450K (vs RM 780K funded path)

---

## Slide 12 — Closing Vision

**Timing:** 15 seconds | **Purpose:** End on inspiration, not logistics.

### Visual Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                                                                             │
│  [Full-bleed background: dark teal gradient #0d3b3b → #1a7a7a]            │
│  [Same family as Slide 1 — bookends the presentation visually]             │
│                                                                             │
│                                                                             │
│                                                                             │
│                                                                             │
│       "To become the regional AI preventive healthcare                      │
│        intelligence infrastructure — the intelligence layer                 │
│        that sits between patients, providers, payers,                       │
│        and policymakers across ASEAN, making preventive                     │
│        healthcare data-driven, personalised,                                │
│        and accessible to all."                                              │
│                                                                             │
│        [Font: Plus Jakarta Sans Bold, 28–32px]                             │
│        [Color: --nv-neutral-000 #ffffff]                                   │
│                                                                             │
│                                                                             │
│                    ┌──────────────────────────┐                            │
│                    │      [HEALYNX LOGO]       │                            │
│                    └──────────────────────────┘                            │
│                                                                             │
│        umcie@um.edu.my  ·  leecs@um.edu.my  ·  nfauzani@um.edu.my         │
│        Tel: +603-79677351                                                   │
│                                                                             │
│        [QR Code to prototype — if available]                               │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

### Key Content — Sourced from `Healynx_Pitch_Deck.md` §7.3

**Long-Term Vision Statement (§7.3):**
> "To become the regional AI preventive healthcare intelligence infrastructure — the intelligence layer that sits between patients, providers, payers, and policymakers across ASEAN, making preventive healthcare data-driven, personalised, and accessible to all."

**Vision Evolution (§7.3):**
| Stage | What Healynx Becomes |
|---|---|
| Today (Year 0–2) | AI-powered supplement safety + secure records for Malaysian clinics |
| Growth (Year 2–4) | Multi-country preventive health SaaS integrated with wearables, insurance, telemedicine |
| Scale (Year 4–7) | Regional AI healthcare intelligence infrastructure across ASEAN |
| Moonshot (Year 7+) | The intelligence backbone for preventive healthcare in emerging markets |

**Contact:**
- UMCIE: umcie@um.edu.my · +603-79677351
- Dr. Lee Ching Shya (IP Inventor): leecs@um.edu.my
- Dr. Nurul Fauzani Jamaluddin (IP Inventor): nfauzani@um.edu.my

### Speaker Note

> "Healthcare's future isn't coming — it's already here. And we built it this weekend. Two UM patents. One unified platform. A RM 1.75 billion market with no existing solution. Let's bring it to the world."

---

---

## Appendix A — Data Source Cross-Reference

All data points are sourced directly from project documents. No statistics were invented.

| Slide | Claim | Source | Section |
|---|---|---|---|
| 1 | Product name, tagline | `Ideas/Healynx_Pitch_Deck.md` | §1.3 |
| 2 | Persona: Dr. Aminah (40–50 patients/day) | `Ideas/Healynx_PRD.md` | §2, Persona 1 |
| 2 | Persona: Mr. Rajan (3 meds + 4 supplements) | `Ideas/Healynx_PRD.md` | §2, Persona 2 |
| 3 | Pain Score 4.2/5 (5 dimensions) | `Ideas/Healynx_Pitch_Deck.md` | §2.5 |
| 3 | 15–20% consult time on record review | `Ideas/Healynx_Pitch_Deck.md` | §2.5 (Cost dimension) |
| 3 | TAM ~RM 1.75B | `Ideas/Healynx_Pitch_Deck.md` | §2.2 |
| 4 | IP numbers (UI 2025002953, PI 2025007955) | `Ideas/Healynx_PRD.md` | Cover, §1 |
| 4 | Integration architecture table | `Ideas/Healynx_Pitch_Deck.md` | §1.4 |
| 5 | Patient wellness score (72, ⬆+14) | `Ideas/Healynx_UX_Flow.md` | Flow 1, Step 1 & Step 5 |
| 5 | Supplement severity (🟢/🟡/🔴) | `Ideas/Healynx_UX_Flow.md` | Flow 1, Step 1 |
| 5 | API endpoint for acknowledgement | `Ideas/Healynx_UX_Flow.md` | Flow 2 |
| 6 | AI results in <3 seconds | `Ideas/Healynx_PRD.md` | NFR-05 |
| 6 | AI sensitivity ≥85% | `Ideas/Healynx_PRD.md` | FR-02, §7 KPIs |
| 7 | TAM/SAM/SOM breakdown | `Ideas/Healynx_Pitch_Deck.md` | §2.2 |
| 7 | Year 3 SOM ~RM 780K ARR (98 accounts) | `Ideas/Healynx_Pitch_Deck.md` | §2.2 |
| 8 | Revenue streams + % contributions | `Ideas/Healynx_Pitch_Deck.md` | §4.2 |
| 8 | Pricing tiers (RM 350/mo Essentials etc.) | `Ideas/Healynx_Pitch_Deck.md` | §4.3 |
| 8 | Gross margin 55–65% → 65–75% | `Ideas/Healynx_Pitch_Deck.md` | §4.4 |
| 8 | Break-even Month 30–36 | `Ideas/Healynx_Pitch_Deck.md` | §4.4, §8.4 |
| 9 | Competitor categories + weaknesses | `Ideas/Healynx_Pitch_Deck.md` | §6.1–6.2 |
| 9 | UVP statement | `Ideas/Healynx_Pitch_Deck.md` | §6.3 |
| 9 | Competitive moats | `Ideas/Healynx_Pitch_Deck.md` | §6.4 |
| 10 | Phase 1 exit gate (5 sites, 30 clinicians, 500 patients) | `Ideas/Healynx_PRD.md` | §9.2 |
| 10 | Phase 3 MRR target RM 32K by Month 24 | `Ideas/Healynx_Pitch_Deck.md` | §5.3 |
| 10 | ASEAN expansion waves | `Ideas/Healynx_Pitch_Deck.md` | §7.1 |
| 11 | Core team (5 names) | `Ideas/Healynx_Pitch_Deck.md` | §9 |
| 11 | IP inventors + contacts | `Ideas/Healynx_Pitch_Deck.md` | §3.2, §9 |
| 11 | Ask RM 1.5M–2.5M | `Ideas/Healynx_Pitch_Deck.md` | §8.1 |
| 11 | Fund allocation table | `Ideas/Healynx_Pitch_Deck.md` | §8.2 |
| 11 | Bootstrap contingency paths | `Ideas/Healynx_Pitch_Deck.md` | §8.6 |
| 12 | Long-term vision statement | `Ideas/Healynx_Pitch_Deck.md` | §7.3 |

---

## Appendix B — Prototype Screen Checklist

| Slide | Screen | UX Source | Status |
|---|---|---|---|
| 5 | Patient Dashboard (wellness score + supplement tracker) | `Ideas/Healynx_UX_Flow.md` — Flow 1, Step 1 | Ready to render |
| 5 | Survey Results screen (score ⬆+14, severity cards, Share with Doctor) | `Ideas/Healynx_UX_Flow.md` — Flow 1, Step 5 | Ready to render |
| 5 | Doctor Dashboard (patient list, 3 unacknowledged alerts) | `Ideas/Healynx_UX_Flow.md` — Flow 2, Step 1 | Ready to render |
| 5 | Patient Profile / AI Summary (confidence score, interaction cards) | `Ideas/Healynx_UX_Flow.md` — Flow 2, Patient Profile view | Ready to render |
| 6 | Survey Screen 1 (supplement entry, pre-populated) | `Ideas/Healynx_UX_Flow.md` — Flow 1, Step 2 | Ready to render |
| 6 | Loading / AI analysis animation | `Ideas/Healynx_UX_Flow.md` — Flow 1, Step 4 | Ready to render |
| 6 | Doctor alert card (severity badge, Acknowledge button) | `Ideas/Healynx_UX_Flow.md` — Flow 2 | Ready to render |

---

## Appendix C — Visual Assets Checklist

| Slide | Asset Type | Description |
|---|---|---|
| 1 | Background illustration | Dark teal gradient (#0d3b3b → #1a7a7a) with geometric medical-tech nodes |
| 2 | Character illustration | Doctor at desk, surrounded by paper records and phone scans (muted dark palette) |
| 3 | Horizontal bar chart | Pain Score dimensions (Frequency/Intensity/Cost/Urgency/Reachability) each 4–5/5 |
| 7 | Concentric circles / funnel | TAM (RM 1.75B) → SAM (RM 350M) → SOM (RM 780K) in primary color scale |
| 8 | Pie chart — revenue mix | 5 segments with % labels (55/15/10/15/5) in --nv-primary and --nv-accent palette |
| 9 | 2×2 matrix chart | X: Ease of Use, Y: AI/Feature Depth; Healynx top-right in --nv-primary-600 |
| 10 | Horizontal timeline | 4 phases with "You Are Here" marker on Phase 1; milestones below each phase |
| 11 | Pie chart — use of funds | 8 segments matching fund allocation table; warm and primary palette |
| 12 | Background | Same dark teal gradient as Slide 1 (bookend visual coherence) |

---

_Healynx — Pitch Deck Prompt Document_  
_National Deep Tech Challenge 2026 · Universiti Malaya_  
_All data sourced from: `Ideas/Healynx_Pitch_Deck.md`, `Ideas/Healynx_PRD.md`, `Ideas/Healynx_UX_Flow.md`, `UI_Prompt/doctor_UI.md`_
