# Pitch Deck Design Prompt — Healynx
## IT × Health | Hackathon Edition | 12 Slides | 5-Minute Runtime

---

## ⚠️ Data Sourcing Instructions

**Before designing any slide, fetch data from these project files:**

| Data Needed | Source File | Section |
|---|---|---|
| Product name, tagline, UVP, IP details | `Healynx_Pitch_Deck.md` | Executive Summary, §1, §6.3 |
| Market stats (TAM/SAM/SOM, growth, drivers) | `Healynx_Pitch_Deck.md` | §2.2–2.5 |
| Personas (names, roles, pain points) | `Healynx_PRD.md` | §2, §3 |
| Pricing tiers, revenue streams | `Healynx_Pitch_Deck.md` | §4.2–4.3 |
| Competitor matrix, UVP | `Healynx_Pitch_Deck.md` | §6.1–6.4 |
| Roadmap phases & milestones | `Healynx_PRD.md` | §9.2–9.5 (or `Healynx_Pitch_Deck.md` §3.1) |
| Team names & roles | `Healynx_Pitch_Deck.md` | §9 |
| Prototype screen layouts & flows | `Healynx_UX_Flow.md` | Flow 1–Flow 5 |
| Persona stories & user journeys | `Healynx_UX_Flow.md` | Screens with persona data |
| Ask amount & fund allocation | `Healynx_Pitch_Deck.md` | §8.1–8.3 |

**Rule:** Do not invent numbers. Pull all statistics, names, prices, and milestones from the source files above. If a data point is missing from the source files, use the placeholder `[MISSING DATA: description]` rather than guessing.

---

## Global Design Notes

### Visual Identity
| Attribute | Recommendation |
|---|---|
| **Primary color** | Deep teal `#0D9488` or electric blue `#2563EB` |
| **Accent color** | Vibrant coral `#FF6B6B` or neon green `#10B981` |
| **Background** | Dark mode preferred (`#0F172A` slate-900) for bold/disruptive tone |
| **Text** | White `#F8FAFC` for headings, light gray `#CBD5E1` for body |
| **Typography** | Headings: **Inter Bold** or **Space Grotesk**. Body: **Inter Regular** |
| **General style** | High contrast, minimal text per slide, large typography, no clutter |

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
Meet [PERSONA NAME — from Healynx_PRD.md §2], a [ROLE].

Every day, [PERSONA] faces:

[ICON: clock]  [PAIN 1 — from PRD persona pain points]
[ICON: error]  [PAIN 2 — from PRD persona pain points]
[ICON: broken] [PAIN 3 — from PRD persona pain points]

[Quote or stat — from PRD pain description]
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
[STAT: from §2.2 TAM]    [STAT: from §2.5 Pain Score]    [STAT: from §2.3 Market Drivers]
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
    [PROTO: mobile, Patient Dashboard — UX Flow §1.0, Step 1]
    [PROTO: mobile, Survey Screen — UX Flow §1.1, Step 2]

          ↘ Feature callout 1
          ↘ Feature callout 2

    [PROTO: web, Clinician Dashboard — UX Flow §2, Step 1]
    [PROTO: web, Patient Profile — UX Flow §2, Step 2+]
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
         │  TAM          │  [RM amount from §2.2]
         │  ┌─────────┐  │
         │  │  SAM    │  │  [RM amount from §2.2]
         │  │ ┌─────┐ │  │
         │  │ │ SOM │ │  │  [RM amount from §2.2]
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
[5 Revenue Streams — from §4.2]
[Pricing Tier Summary — from §4.3]
[Gross Margin / Break-Even — from §4.4]
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
    Competitor A    │    ⭐ Healynx
                    │
  ──────────────────┼──────────────────→
                    │
    Competitor C    │    Competitor B
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
Phase 1          Phase 2           Phase 3            Phase 4
Validation       Partnerships      Commercial         ASEAN Scale
(Months 1–9)     (Months 6–15)     (Months 15–24)     (Years 3–5)
   ●─────────────────●─────────────────●─────────────────●
   │                 │                 │                 │
[Key milestones] [Key milestones] [Key milestones] [Key milestones]
   ▲
YOU ARE HERE
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
TEAM                       │     THE ASK
                           │
[NAME 1] — [ROLE]         │  $[AMOUNT — from §8.1]
[NAME 2] — [ROLE]         │  
[NAME 3] — [ROLE]         │  [USE OF FUNDS — from §8.2]
[NAME 4] — [ROLE]         │  • [Top allocation area]
[NAME 5] — [ROLE]         │  • [2nd allocation area]
                           │  • [3rd allocation area]
Advisors:                  │
[ADVISOR 1 NAME]           │  [CHART: pie, use of funds — from §8.2 %]
[ADVISOR 2 NAME]           │
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

| Slide | Seconds | Cumulative |
|---|---|---|
| 1 — Title | 15s | 0:15 |
| 2 — The Pain (Story) | 30s | 0:45 |
| 3 — Cost of Inaction (Stats) | 20s | 1:05 |
| 4 — The Solution (Story) | 25s | 1:30 |
| 5 — Product Demo (Prototypes) | 35s | 2:05 |
| 6 — How It Works | 25s | 2:30 |
| 7 — Market Opportunity (Stats) | 25s | 2:55 |
| 8 — Business Model | 20s | 3:15 |
| 9 — Competitive Landscape | 20s | 3:35 |
| 10 — Roadmap | 30s | 4:05 |
| 11 — Team & Ask | 20s | 4:25 |
| 12 — Closing/Vision | 15s | 4:40 |
| **Buffer** | 20s | 5:00 |

---

## Appendix: Data Source Cross-Reference Checklist

Each data point in the deck must trace back to one of these files:

| Slide | Data Needed | Source File | Section |
|---|---|---|---|
| 1 | Product name, tagline | `Healynx_Pitch_Deck.md` | Title, §1.3 |
| 2 | Persona names, pain points | `Healynx_PRD.md` | §2 |
| 3 | Market stats, pain score | `Healynx_Pitch_Deck.md` | §2.2, §2.5 |
| 4 | Solution description, UVP | `Healynx_Pitch_Deck.md` | §1.2, §1.4, §6.3 |
| 5 | Prototype screens | `Healynx_UX_Flow.md` | Flow 1, Flow 2 |
| 6 | How it works (3 steps) | `Healynx_Pitch_Deck.md` | §1.2 |
| 7 | TAM/SAM/SOM numbers | `Healynx_Pitch_Deck.md` | §2.2 |
| 8 | Pricing, revenue streams | `Healynx_Pitch_Deck.md` | §4.2–4.4 |
| 9 | Competitor matrix | `Healynx_Pitch_Deck.md` | §6.1–6.4 |
| 10 | Roadmap phases | `Healynx_PRD.md` | §9.2–9.5 |
| 11 | Team names, ask amount | `Healynx_Pitch_Deck.md` | §9, §8.1–8.3 |
| 12 | Vision statement | `Healynx_Pitch_Deck.md` | §7.3 |

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

*End of Design Prompt — Healynx Pitch Deck*
