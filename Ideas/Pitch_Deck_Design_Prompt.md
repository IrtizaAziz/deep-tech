# Pitch Deck Design Prompt — Healynx

## IT × Health | Hackathon Edition | 12 Slides | 5-Minute Runtime

---

## ⚠️ Data Sourcing Instructions

**Before designing any slide, fetch data from these project files:**

| Data Needed                                 | Source File             | Section                                    |
| ------------------------------------------- | ----------------------- | ------------------------------------------ |
| Product name, tagline, UVP, IP details      | `Healynx_Pitch_Deck.md` | Executive Summary, §1, §6.3                |
| Market stats (TAM/SAM/SOM, growth, drivers) | `Healynx_Pitch_Deck.md` | §2.2–2.5                                   |
| Personas (names, roles, pain points)        | `Healynx_PRD.md`        | §2, §3                                     |
| Pricing tiers, revenue streams              | `Healynx_Pitch_Deck.md` | §4.2–4.3                                   |
| Competitor matrix, UVP                      | `Healynx_Pitch_Deck.md` | §6.1–6.4                                   |
| Roadmap phases & milestones                 | `Healynx_PRD.md`        | §9.2–9.5 (or `Healynx_Pitch_Deck.md` §3.1) |
| Team names & roles                          | `Healynx_Pitch_Deck.md` | §9                                         |
| Prototype screen layouts & flows            | `Healynx_UX_Flow.md`    | Flow 1–Flow 5                              |
| Persona stories & user journeys             | `Healynx_UX_Flow.md`    | Screens with persona data                  |
| Ask amount & fund allocation                | `Healynx_Pitch_Deck.md` | §8.1–8.3                                   |

**Rule:** Do not invent numbers. Pull all statistics, names, prices, and milestones from the source files above. If a data point is missing from the source files, use the placeholder `[MISSING DATA: description]` rather than guessing.

---

## Embedded Source Excerpts (inlined from project docs)

- **Product Name:** Healynx
- **Core Value Proposition / Tagline:** "The first integrated AI-powered preventive healthcare platform in Malaysia that combines real-time supplement intelligence with secure, AI-enhanced medical records management — enabling clinicians to detect risks, patients to manage their health, and organisations to scale preventive care."
- **Representative Personas (from PRD):**
  - **Dr. Aminah** — General Practitioner, sees 40–50 patients/day; pain: patients do not disclose supplements; Healynx solution: pre-consult supplement intake + AI flags to reduce review time.
  - **Mr. Rajan** — 58-year-old patient with Type 2 diabetes and hypertension taking prescription meds + supplements; pain: scattered records and unclear supplement safety; Healynx solution: single encrypted record and AI interaction alerts.
- **Addressable Market (from Healynx Pitch Deck §2.2):**
  - **TAM (Malaysia, combined segments):** ~RM 1.75B annually
  - **SAM (urban private clinics, telemedicine, corporate wellness focus):** ~RM 350M annually
  - **SOM (Year 3 target):** ~RM 780K ARR (detailed Year 3 target in pitch materials)
- **The Ask (funding):** Seeking **RM 1.5M – RM 2.5M** in Seed / Commercialisation Funding (covers 24-month path to revenue; see Healynx_Pitch_Deck.md §10).
- **Prototype screen sources:** Use `Healynx_UX_Flow.md` — Flow 1 (Patient Supplement Assessment) and Flow 2 (Doctor Clinical Review) as the canonical prototype references for Slide 5 screenshots.

---

## Global Design Notes

### Visual Identity (Light Theme)

| Attribute         | Recommendation                                                                                                                                   |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Primary color** | Use Healynx light-theme token `--nv-primary-600` (see `UI_Prompt/doctor_UI.md` `:root` for values).                                              |
| **Accent color**  | Use Healynx accent token `--nv-accent-500` (fallback coral `#EDA83E`).                                                                           |
| **Background**    | Light / white background: use `--nv-neutral-000` for canvas; use `--nv-neutral-050` / `--nv-neutral-100` for cards and surfaces.                 |
| **Text**          | Headings: `--nv-neutral-900` (dark); Body: `--nv-neutral-400` (muted gray) for readable contrast.                                                |
| **Typography**    | Headings: **Inter Bold** or **Space Grotesk**. Body: **Inter Regular**                                                                           |
| **General style** | Clean, clinical, spacious — minimal text per slide, generous whitespace, large typography. Use `doctor_UI.md` light-theme tokens for all colors. |

**Note:** When exporting slides, reference the canonical `--nv-` tokens from `UI_Prompt/doctor_UI.md`. If your slide tooling cannot consume CSS tokens, fall back to the corresponding hex values listed in `doctor_UI.md`'s `:root` block.

### Placeholder Conventions

- `[CHART: type, metric]` — Replace with a data visualization
- `[PROTO: mobile/web, screen name]` — Replace with a prototype screenshot from UX Flow
- `[STAT: number + description]` — Replace with a key metric (sourced from project .md files)
- `[ICON: concept]` — Replace with an icon
- `[IMAGE: description]` — Replace with a photograph or illustration

### Narrative Arc

The deck follows a **hero's journey in reverse**: villain (problem) → suffering (pain stories) → weapon (solution) → proving it works (demo + stats) → charting the conquest (roadmap + ask).

---

## Slide 1 — Title Slide

**Purpose:** Grab attention. Set the bold, disruptive tone.

**Timing:** 15 seconds

**📂 Data Source:** `Healynx_Pitch_Deck.md` — Executive Summary, §1.3

### Visual Layout

- Full-bleed dark background with a subtle gradient or abstract medical-tech geometric overlay
- Large product name centered, bold, white
- Tagline beneath in accent color
- Brand logo top-left or centered above the title

### Content

```
[PRODUCT NAME — from Healynx_Pitch_Deck.md]
─────────────────────────────────────────
[One-line bold tagline / value prop — from Healynx_Pitch_Deck.md §1.3]
```

### Narrative Note

Open with a provocative question or a startling stat pulled from the docs. E.g., "What if I told you most supplement risks go undetected until it's too late?"

---

## Slide 2 — The Pain (Story-Driven)

**Purpose:** Make the problem visceral. Use a persona story from the PRD to humanize the pain.

**Timing:** 30 seconds

**📂 Data Source:** `Healynx_PRD.md` §2 (Persona 1: Dr. Amira, Persona 2: Mr. Rajan)

### Visual Layout

- Left half: Character illustration/silhouette of the persona
- Right half: Pain points as a cascading stack of cards
- Each pain point: icon + 3-5 words + optional stat
- Background: dark with subtle red/warm undertone

### Content — Persona Story

```
Meet Dr. Aminah — a busy General Practitioner in a private clinic who sees 40–50 patients per day.

Every day, Dr. Aminah faces:

[ICON: clock]  Patients routinely fail to disclose supplement, herbal and traditional remedy use during 10‑minute consultations, creating hidden clinical blind spots.
[ICON: error]  Medical records are fragmented across clinics, paper files and phone-camera scans and are not searchable, increasing review time.
[ICON: broken] There is no automated, localised way to cross-check drug–supplement interactions during the visit, creating medico-legal and patient-safety risk.

"Imagine opening a consultation and not knowing the patient is taking an herbal prep that can interact with their antihypertensive — that risk is invisible today." (PRD persona summary)
```

### Narrative Note

Tell a micro-story. "Imagine Dr. Amira, a GP in a busy Klang Valley clinic. She chose medicine to heal — but instead she's trapped in a labyrinth of fragmented records and invisible supplement risks..."

---

## Slide 3 — The Cost of Inaction (Statistics)

**Purpose:** Quantify the pain with hard numbers.

**Timing:** 20 seconds

**📂 Data Source:** `Healynx_Pitch_Deck.md` §2.2–2.5

### Visual Layout

- Three large stat cards in a row or a bold infographic
- Each card: oversized number + short description
- Could use a split-screen: "The Problem in Numbers" top, "The Ripple Effect" bottom

### Content

```
[STAT: TAM — ~RM 1.75B annually (Pitch Deck §2.2)]    [STAT: Composite Pain Score — 4.2 / 5 (Pitch Deck §2.5)]    [STAT: Key Drivers — Rapid supplement consumption growth; digital health expansion; rising chronic disease burden (Pitch Deck §2.3)]
```

### Placeholders

- `[CHART: bar, market waste/spend]` — from Pitch Deck §2.2
- `[CHART: area, problem growth trend]` — from Pitch Deck §2.3
- `[STAT: key metrics from §2.5 Pain Score dimensions]`

---

## Slide 4 — The Solution (Story-Driven)

**Purpose:** Unveil Healynx as the hero. Connect emotionally to Slide 2's pain story. Call back the persona.

**Timing:** 25 seconds

**📂 Data Source:** `Healynx_Pitch_Deck.md` §1.2–1.4 (Combined Product Vision, Value Prop, IP Integration)

### Visual Layout

- Split-screen: Left = "Before" (dark, fragmented — reminiscent of Slide 2)
- Right = "After" (bright, unified — showing Healynx)
- Center = a glow/transition effect bridging the two
- Product name and one-line value prop at the bottom

### Content

```
Before Healynx              →              With Healynx

[ICON: chaos] Fragmented records       [ICON: sync] Unified AI-powered platform
[ICON: clock] Undisclosed risks        [ICON: bolt] Real-time supplement intelligence
[ICON: error] Reactive care            [ICON: link] Preventive, closed-loop care
```

### Narrative Note

Bring back the persona: "Now imagine Dr. Amira opens Healynx. In one tap, she sees every supplement risk, every AI summary, every patient insight — before the patient even sits down."

### Safety Note

All AI outputs shown in the deck are decision-support only and require clinician verification. v1 includes supplement and drug–drug detection plus smart reminders; appointment booking and clinician scheduling are scoped for v2.

---

## Slide 5 — Product Demo: Mobile + Web (Prototype Screens)

**Purpose:** Show, don't tell. Prove the product exists and works beautifully.

**Timing:** 35 seconds

**📂 Data Source:** `Healynx_UX_Flow.md` — Flow 1 (Patient Supplement Assessment), Flow 2 (Doctor Clinical Review)

### Visual Layout

- Two large device mockups side-by-side:
  - Left: Mobile phone frame with prototype screenshot (Patient Dashboard or Survey screen)
  - Right: Laptop/browser frame with Clinician Dashboard screenshot
- 2-3 feature callouts with arrows pointing to specific UI elements

### Content

```
  Prototype references (use these exact locations in `Ideas/Healynx_UX_Flow.md`):
  - Mobile — Patient Dashboard (Flow 1: Patient — Supplement Assessment & Results, section "Step 1 — Dashboard: Entry Widget")
  - Mobile — Survey & Analysis Results (Flow 1: section "Step 4 — Review & Confirm" and "Step 5 — Results Screen")
  - Web — Clinician Dashboard (Flow 2: section "Step 1 — Doctor Dashboard")
  - Web — Patient Profile / AI Summary (Flow 2: "Patient Profile View" and the AI Results subsections)

  Feature callouts: one-tap supplement intake, AI interaction cards with severity badges, "Share with Doctor" prefilled share flow, clinician High-severity acknowledgement modal (API: `POST /medication-conflicts/{conflictId}/acknowledge`).
```

### Placeholders

- `[PROTO: mobile, screen 1]` — Patient Dashboard (UX Flow §1.0 Step 1)
- `[PROTO: mobile, screen 2]` — Survey Results (UX Flow §1.1 Step 5)
- `[PROTO: web, dashboard]` — Doctor Dashboard (UX Flow §2 Step 1)
- `[PROTO: web, screen 2]` — AI Results / Patient Profile

### Narrative Note

Tag each prototype with the real user value: "One-tap supplement risk check," not "Search bar feature." Keep walkthrough brisk.

---

## Slide 6 — How It Works (3-Step Journey)

**Purpose:** Simplify complexity. No one funds what they don't understand.

**Timing:** 25 seconds

**📂 Data Source:** `Healynx_Pitch_Deck.md` §1.2 (Combined Product Vision), `Healynx_PRD.md` §1 (Solution Summary)

### Visual Layout

- Horizontal 3-step flow with connecting arrows
- Each step: icon + bold title + one-line description
- Under each step: a small prototype screenshot relevant to that step

### Content

```
  01                   02                   03
[ICON: capture]    [ICON: process]     [ICON: deliver]
[STEP 1 TITLE]     [STEP 2 TITLE]      [STEP 3 TITLE]
One-line desc      One-line desc       One-line desc
[PROTO: step 1]    [PROTO: step 2]     [PROTO: step 3]
```

### Placeholders

- Steps should map to: **Capture** (survey intake) → **Analyze** (AI engine) → **Act** (secure records + clinician view)
- Source: `Healynx_Pitch_Deck.md` §1.2 integration table

---

## Slide 7 — Market Opportunity (Visual Statistics)

**Purpose:** Prove the size of the prize.

**Timing:** 25 seconds

**📂 Data Source:** `Healynx_Pitch_Deck.md` §2.2 — Addressable Market (TAM/SAM/SOM)

### Visual Layout

- TAM / SAM / SOM funnel or concentric circles
- Each layer labeled with the exact RM amounts from §2.2
- Right side or bottom: supporting stats (growth drivers from §2.3)

### Content

```
         ┌───────────────┐
         │  TAM          │  ~RM 1.75B annually (Pitch Deck §2.2)
         │  ┌─────────┐  │
         │  │  SAM    │  │  ~RM 350M annually (Pitch Deck §2.2)
         │  │ ┌─────┐ │  │
         │  │ │ SOM │ │  │  ~RM 780K ARR (Year 3 target — Pitch Deck §2.2)
         │  │ └─────┘ │  │
         │  └─────────┘  │
         └───────────────┘
```

### Placeholders

- `[CHART: funnel, TAM/SAM/SOM]` — exact numbers from `Healynx_Pitch_Deck.md` §2.2
- `[CHART: bar, market growth drivers]` — data from §2.3
- `[STAT: market size, growth rate, segment counts]`

---

## Slide 8 — Business Model

**Purpose:** Show how this becomes a real company.

**Timing:** 20 seconds

**📂 Data Source:** `Healynx_Pitch_Deck.md` §4 (Business Model)

### Visual Layout

- Clean revenue-stream breakdown: 5 columns or cards with icons
- Pricing tiers summarized simply
- Optional: unit economics callout

### Content

```
Revenue streams (Pitch Deck §4.2):
 1) Healthcare Provider Subscriptions — recurring SaaS (target ~55% contribution)
 2) API Licensing — Supplement Intelligence & AI Summarisation (usage-based, ~15%)
 3) Enterprise Customisation & Integrations (one-time + maintenance, ~10%)
 4) Corporate Wellness Solutions (per-employee recurring, ~15%)
 5) Privacy-compliant Data Analytics & Research Licences (project/annual, ~5%)

Pricing tier summary (Pitch Deck §4.3):
 - Clinic Essentials: RM 350 / month (or RM 3,500 / year) — up to 3 clinician accounts
 - Clinic Professional: RM 700 / month (or RM 7,000 / year) — up to 10 clinician accounts
 - Enterprise: Custom pricing (white-label, SLA, unlimited records)
 - Telemedicine API Starter: RM 1,200 / month (5,000 API calls/month)
 - Corporate Wellness: RM 14–RM 10 per employee/year depending on scale

Gross margin & break-even (Pitch Deck §4.4):
 - Gross margin expected: 55–65% in early years, improving to 65–75% at scale
 - Break-even estimated: Month 30–36 (mid-to-late Year 3)
```

### Placeholders

- `[STAT: projected ARR, break-even month]` — from §4.4, §8.4
- `[CHART: pie, revenue mix]` — from §4.2 contribution targets

---

## Slide 9 — Competitive Landscape

**Purpose:** Prove defensibility.

**Timing:** 20 seconds

**📂 Data Source:** `Healynx_Pitch_Deck.md` §6.1–6.4

### Visual Layout

- 2×2 matrix chart (axes: "Ease of Use" vs "Feature Depth" or similar)
- Healynx in the top-right quadrant, bold and highlighted
- Competitors placed as smaller dots
- OR: simplified feature comparison table from §6.2

### Content

```
      High ↑ Feature Depth
        │
    Traditional EMR    │    ⭐ Healynx
    (Caresys, KMS, Mediviron) │
  ──────────────────┼──────────────────→
        │
    Generic Health Apps │    Telemedicine Platforms
    (HealthifyMe, MyFitnessPal) │    (DoctorOnCall, Doctor Anywhere, BookDoc)
        │
      Low  ↓
```

### Placeholders

- `[CHART: matrix, competitive positioning]` — using data from §6.2 comparison table
- `[COMPETITOR NAMES]` — from §6.1 categories

---

## Slide 10 — Roadmap

**Purpose:** Show ambition and execution plan.

**Timing:** 30 seconds

**📂 Data Source:** `Healynx_PRD.md` §9.2–9.5 (Phase 1–4 roadmap), OR `Healynx_Pitch_Deck.md` §3.1

### Visual Layout

- Horizontal timeline with 4 phases / milestones
- Current position highlighted with "You Are Here" marker
- Each milestone: phase label, icon, short description, key deliverables
- Roadmap takes up 70% of the slide

### Content

```
Phase 1 — Validation (Months 1–9): pilot at UM Medical Centre; integrate both IPs into prototype; achieve AI accuracy target (>85% sensitivity); 5 pilot sites; TRL 5→6

Phase 2 — Partnerships & Regulatory Readiness (Months 6–15): sign 3–5 pilot clinic agreements; 2 telemedicine integrations; PDPA/compliance work; penetration test; first commercial pilots

Phase 3 — Commercial Launch (Months 15–24): B2B SaaS go‑live; direct sales team (3–5); target 20 paying accounts; MRR ramp (target RM 32K by Month 24); prepare ASEAN entry

Phase 4 — ASEAN Scale (Years 3–5): Singapore → Indonesia → Thailand → Vietnam; build ASEAN supplement DB; enterprise & insurance integrations; scale to 100s of clinics

Key milestones and exit gates are detailed in `Ideas/Healynx_PRD.md` §9 (pilot validation, AI benchmark, security certification, paying accounts, break-even Month 30–36).
```

```

### Placeholders

- `[ROADMAP: timeline, 4 phases]` — milestones from `Healynx_PRD.md` §9.2–9.5
- `[MILESTONE DESCRIPTIONS]` — pulled from PRD phase objectives and exit gates

---

## Slide 11 — Team & Ask (Minimal)

**Purpose:** Credibility + clear request. Keep this slide lean — no photos.

**Timing:** 20 seconds

**📂 Data Source:** `Healynx_Pitch_Deck.md` §9 (Team), §8 (The Ask)

### Visual Layout

- Left 60%: Team names listed cleanly (no photos, no bios — just name + role)
- Right 40%: "The Ask" box with funding amount, use of funds breakdown

### Content

```

TEAM (summary — `Ideas/Healynx_Pitch_Deck.md` §9):

- Core founding team: 5 UM CS founders (technical & product leads) — recommended lean start (5-person team for Months 1–12)
- Advisors / IP contacts: Dr. Lee Ching Shya; Dr. Nurul Fauzani Jamaluddin; UMCIE liaison

THE ASK:

- Seeking **RM 1.5M – RM 2.5M** in Seed / Commercialisation Funding (Pitch Deck §10)

Use of Funds (high-level):

- Product & Engineering (cloud infra, AI compute, development)
- Sales & Marketing (B2B outreach, conferences, partner development)
- Regulatory, Security & Compliance (PDPA, penetration testing, legal)
- Data labelling & clinical validation (expert-labelled training data expansion)
- Operations & G&A

For a line-item % allocation table, see `Ideas/Healynx_Pitch_Deck.md` §8 — request extraction if you want a pie chart rendered.

```

### Placeholders

- `[TEAM NAMES + ROLES]` — from `Healynx_Pitch_Deck.md` §9 (5 co-founders, 2 advisors)
- `[ASK AMOUNT]` — RM 1.5M–2.5M from §8.1
- `[CHART: pie, use of funds]` — from §8.2 allocation table

### Note

**No team photos.** Names and roles only. The team is 5 UM CS sophomores — let the product and data do the talking. If the audience wants bios, those live in the appendix or spoken narrative, not on this slide.

---

## Slide 12 — Closing / Vision

**Purpose:** End on inspiration, not logistics.

**Timing:** 15 seconds

**📂 Data Source:** `Healynx_Pitch_Deck.md` §7.3 (Long-Term Vision)

### Visual Layout

- Full-bleed aspirational image or dark gradient background
- One bold, memorable closing line centered
- Product logo + contact info at bottom
- Optionally: QR code to prototype

### Content

```

[CLOSING LINE — from §7.3 vision statement]

            [LOGO: Healynx]

     [Website] · [Email] · [LinkedIn]
           [QR to prototype]

```

### Narrative Note

"Healthcare's future isn't coming — it's already here. And we built it this weekend. Let's bring it to the world."

---

## Slide Timing Summary

| Slide                          | Seconds | Cumulative |
| ------------------------------ | ------- | ---------- |
| 1 — Title                      | 15s     | 0:15       |
| 2 — The Pain (Story)           | 30s     | 0:45       |
| 3 — Cost of Inaction (Stats)   | 20s     | 1:05       |
| 4 — The Solution (Story)       | 25s     | 1:30       |
| 5 — Product Demo (Prototypes)  | 35s     | 2:05       |
| 6 — How It Works               | 25s     | 2:30       |
| 7 — Market Opportunity (Stats) | 25s     | 2:55       |
| 8 — Business Model             | 20s     | 3:15       |
| 9 — Competitive Landscape      | 20s     | 3:35       |
| 10 — Roadmap                   | 30s     | 4:05       |
| 11 — Team & Ask                | 20s     | 4:25       |
| 12 — Closing/Vision            | 15s     | 4:40       |
| **Buffer**                     | 20s     | 5:00       |

---

## Appendix: Data Source Cross-Reference Checklist

Each data point in the deck must trace back to one of these files:

| Slide | Data Needed                | Source File             | Section          |
| ----- | -------------------------- | ----------------------- | ---------------- |
| 1     | Product name, tagline      | `Healynx_Pitch_Deck.md` | Title, §1.3      |
| 2     | Persona names, pain points | `Healynx_PRD.md`        | §2               |
| 3     | Market stats, pain score   | `Healynx_Pitch_Deck.md` | §2.2, §2.5       |
| 4     | Solution description, UVP  | `Healynx_Pitch_Deck.md` | §1.2, §1.4, §6.3 |
| 5     | Prototype screens          | `Healynx_UX_Flow.md`    | Flow 1, Flow 2   |
| 6     | How it works (3 steps)     | `Healynx_Pitch_Deck.md` | §1.2             |
| 7     | TAM/SAM/SOM numbers        | `Healynx_Pitch_Deck.md` | §2.2             |
| 8     | Pricing, revenue streams   | `Healynx_Pitch_Deck.md` | §4.2–4.4         |
| 9     | Competitor matrix          | `Healynx_Pitch_Deck.md` | §6.1–6.4         |
| 10    | Roadmap phases             | `Healynx_PRD.md`        | §9.2–9.5         |
| 11    | Team names, ask amount     | `Healynx_Pitch_Deck.md` | §9, §8.1–8.3     |
| 12    | Vision statement           | `Healynx_Pitch_Deck.md` | §7.3             |

### Prototype Screens Needed

- [ ] Slide 5: Patient Dashboard (`UX_Flow.md` §1.0 Step 1)
- [ ] Slide 5: Survey Screen or Results (`UX_Flow.md` §1.1 Steps 2–5)
- [ ] Slide 5: Doctor Dashboard (`UX_Flow.md` §2 Step 1)
- [ ] Slide 5: Patient Profile / AI Summary view (`UX_Flow.md` §2 Step 2+)
- [ ] Slide 6: Small thumbnails for each of 3 steps

### Visual Statistics Needed

- [ ] Slide 3: `[CHART: bar/area, pain metrics]`
- [ ] Slide 7: `[CHART: funnel, TAM/SAM/SOM]`
- [ ] Slide 7: `[CHART: bar, market growth]`
- [ ] Slide 8: `[CHART: pie, revenue mix]`
- [ ] Slide 9: `[CHART: matrix, competitive positioning]`
- [ ] Slide 10: `[ROADMAP: timeline, 4 phases]`
- [ ] Slide 11: `[CHART: pie, use of funds]`

---

_End of Design Prompt — Healynx Pitch Deck_
```
