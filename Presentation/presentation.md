# Healynx Pitch Deck / Presentation Slides — Generation Prompt

Use this prompt to create a complete investor-ready pitch deck or presentation slide deck (e.g., for Canva, PowerPoint, Google Slides, or Keynote) for **Healynx**.

## Source Documents (Read These First)

All data, facts, numbers, and claims below are derived from these four files. You must cross-reference and stay faithful to them:

1. **Healynx_PRD.md** — Product requirements, functional/non-functional requirements, user stories, KPIs, roadmap, risks
2. **Healynx_TRD.md** — Technical architecture, stack decisions, data models, AI pipeline, security, infrastructure
3. **Healynx_Pitch_Deck.md** — Market sizing, business model, pricing, GTM, competitive analysis, financial projections, team
4. **doctor_UI.md** — Doctor/admin desktop app feature set, clinical workflow UX, emergency access, secure sharing
5. **patient_UI.md** — Patient mobile-first app feature set, supplement survey, AI analysis results, records vault, share flows

---

## Core Narrative Arc — "The Story"

The presentation must follow a **single, cohesive story arc** from start to finish. Do not present features in isolation. Every slide must advance the narrative.

**The Story Arc:**

> **"Malaysian healthcare has a blind spot: patients take supplements their doctors don't know about, using records scattered across clinics and paper files. This blind spot causes preventable harm, wastes clinical time, and blocks preventive healthcare at scale. Healynx is the first platform to close this gap — combining AI-powered supplement intelligence with secure medical records in a single, cloud-native system. Here's how we do it, why the market needs it now, and how we'll scale from Malaysia to ASEAN."**

**Slide-by-slide narrative flow:**

1. **Hook** — A striking, relatable healthcare moment
2. **The Problem** — The blind spot, with real numbers
3. **The Consequence** — What this costs patients, doctors, and the system
4. **The Solution** — Healynx in one clear sentence
5. **How It Works** — The product story (survey → AI → records → alerts → action)
6. **Why Now** — Market timing and driving forces
7. **Market Opportunity** — TAM → SAM → SOM with numbers
8. **Product Deep Dive** — Key features (not all, just the game-changers)
9. **Technology & IP Moat** — What makes this hard to copy
10. **Business Model** — Revenue streams, pricing, unit economics
11. **Go-To-Market Plan** — Phased, cost-conscious, partnership-driven
12. **Competitive Landscape** — The gap no one fills
13. **Scaling Potential** — Malaysia → ASEAN → regional intelligence layer
14. **Traction & Validation** — What's done, what's in progress
15. **The Ask** — Funding, allocation, use of proceeds
16. **Vision Close** — Where we're going

---

## Slide-by-Slide Requirements

### Slide 1: Title Slide

| Element              | Content                                                                                                                                                                                                                                                                                               |
| -------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Tagline**          | "Closing the blind spot between what patients take and what doctors know."                                                                                                                                                                                                                            |
| **Product Name**     | Healynx                                                                                                                                                                                                                                                                                               |
| **One-liner**        | AI-powered preventive healthcare platform combining supplement intelligence with secure medical records                                                                                                                                                                                               |
| **Badges**           | Powered by 2 Universiti Malaya patents • TRL 5 • National Deep Tech Challenge 2026                                                                                                                                                                                                                    |
| **Visual direction** | Clean, clinical, teal-primary design system (Healynx brand: `--nv-primary-600` teal). Split-screen: left side with product name and tagline, right side with an abstract medical/graph network visual or a photo of a doctor-patient consultation with a glowing supplement/drug interaction overlay. |

---

### Slide 2: The Hook — "Meet Mr. Rajan"

| Element      | Content                                                                                                                                                                                                                                                                                                                                                   |
| ------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Headline** | "Meet Mr. Rajan. He's 58. He takes 3 prescription meds, 4 supplements — and his doctor knows almost nothing about 4 of them."                                                                                                                                                                                                                             |
| **Subtext**  | He has Type 2 diabetes and hypertension. He visits 2 different clinics. He buys supplements online. His records are scattered across paper files, phone photos, and two separate clinics. No single system connects the dots.                                                                                                                             |
| **Visual**   | A simple infographic showing Mr. Rajan as a central figure. Lines radiating outward: "Metformin (Dr. A's clinic)", "Amlodipine (Dr. B's clinic)", "Vitamin D (online)", "Fish oil (pharmacy)", "Garlic extract (friend recommended)", "Jamu herbal (traditional shop)". A large red question mark or blind-spot icon in the center overlaid on his chest. |
| **Source**   | Pitch Deck §2.1 (Persona 2 — Mr. Rajan)                                                                                                                                                                                                                                                                                                                   |

---

### Slide 3: The Problem — "The Clinical Blind Spot"

| Element                                           | Content                                                                                                                                                                                                                                                                                                                                                     |
| ------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Headline**                                      | "Three interconnected failures in Malaysian healthcare"                                                                                                                                                                                                                                                                                                     |
| **Problem 1 — Fragmented Records**                | Patient medical histories are siloed across clinics, paper files, and phone-camera scans. No unified, searchable, AI-enriched digital record exists. EMR adoption in private clinics remains low. **Effect:** Clinicians waste 15–20% of consultation time on manual record review.                                                                         |
| **Problem 2 — Undisclosed Supplement Risks**      | Patients routinely fail to disclose supplement, herbal, and traditional remedy use to doctors. 60% of GP caseload is chronic disease — patients with chronic conditions are the highest users of supplements AND the highest risk for drug-supplement interactions. **Effect:** Dangerous interaction blind spots go undetected until adverse events occur. |
| **Problem 3 — No Preventive Health Intelligence** | Existing tools focus on reactive record-keeping. No Malaysian platform combines structured behavioural intake data (what patients consume) with clinical records to generate real-time, personalised preventive health recommendations. **Effect:** Healthcare remains episodic and reactive, not preventive.                                               |
| **Visual**                                        | Three columns (or a triangle diagram), each with an icon (scattered papers for records, crossed pills for supplements, a flatline for lack of prevention). Below each, a stark number or statistic.                                                                                                                                                         |
| **Numbers to use**                                | 60% of GP caseload is chronic disease; 15–20% of consultation time lost to record review; supplement market in Malaysia growing rapidly post-pandemic with no clinical oversight layer                                                                                                                                                                      |
| **Source**                                        | PRD §1 (Problem Statement), Pitch Deck §2.4 (Problem Being Solved & Demand Gap), Pitch Deck §2.5 (Pain Score)                                                                                                                                                                                                                                               |

---

### Slide 4: The Pain Score — Quantifying the Urgency

| Element                  | Content                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| ------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Headline**             | "How severe is this problem? A 5-dimension assessment."                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| **Pain Score Framework** | Display 5 dimensions as horizontal bars or a radar chart: **Frequency** (4/5), **Intensity** (5/5), **Cost** (4/5), **Urgency** (4/5), **Reachability** (4/5)                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| **Composite**            | **Pain Score: 4.2 / 5** — A severe, frequent, costly problem with reachable buyers and no existing solution.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| **Detail for each**      | **Frequency:** 60% of GP caseload is chronic disease; supplements are an issue at every visit. **Intensity:** Drug-supplement interactions can cause hospitalisation, organ damage, or death. Medico-legal liability for clinicians. **Cost:** Clinicians waste 15–20% of consult time on manual record review. Clinics spend days preparing for audits. **Urgency:** Chronic disease burden is rising; digital health mandates are active; supplement consumption is accelerating. **Reachability:** 3,500+ private clinics in urban Malaysia; buyers identifiable through medical associations. |
| **Source**               | Pitch Deck §2.5 (The Pain Score)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |

---

### Slide 5: The Solution — Healynx

| Element               | Content                                                                                                                                                                                                                                                                                                                                                                                                           |
| --------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Headline**          | "Healynx — The first integrated AI-powered preventive healthcare platform in Malaysia"                                                                                                                                                                                                                                                                                                                            |
| **One-line solution** | Combining AI-driven supplement intelligence with secure, encrypted medical records management — enabling clinicians to detect hidden risks, patients to manage their health proactively, and healthcare organisations to deliver preventive care at scale.                                                                                                                                                        |
| **Three-part UVP**    | **See what's missed** — AI flags supplement-drug interactions that are currently invisible because patients don't disclose and systems don't check. **Secure what's scattered** — Fragmented records become a unified, encrypted, AI-searchable patient history. **Prevent what's predictable** — Real-time risk scoring and personalised recommendations transform reactive treatment into proactive prevention. |
| **Source IPs**        | **IP 1 (PI 2025007955):** Computer-Implemented Method for Secure Management of Medical Records in an AI-Enhanced System (TRL 5) • **IP 2 (UI 2025002953):** Interactive Patient Supplement Intake & Disclosure Practice Assessment Method (TRL 3 → 5 in integration)                                                                                                                                              |
| **Visual**            | A central "Healynx" logo. To the left: "Patient Intake (Survey + Records)" with an arrow pointing to the center. From the center, arrows radiate to: "AI Engine (Interactions + Scoring)", "Clinician Dashboard (Alerts + Summaries)", "Patient Dashboard (Wellness + Recommendations)". Below: a security shield icon with "AES-256 • TLS 1.3 • RBAC • PDPA Compliant".                                          |
| **Source**            | PRD §1 (Solution Summary), Pitch Deck §1.3–1.4                                                                                                                                                                                                                                                                                                                                                                    |

---

### Slide 6: How It Works — The Closed-Loop System

| Element                        | Content                                                                                                                                                                                                                                                                                                                    |
| ------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Headline**                   | "A closed-loop preventive healthcare system in 5 steps"                                                                                                                                                                                                                                                                    |
| **Step 1 — Patient Intake**    | Patient completes a structured supplement intake survey on their phone (or tablet in the waiting room). Captures: supplement names, dosages, frequency, health conditions, current medications, allergies. **FR-01** • **Time:** ~3 minutes                                                                                |
| **Step 2 — AI Analysis**       | Dual-engine architecture processes the data: (1) **AI Algorithm** — trained on expert-verified, pre-labelled medical data detects drug-supplement interactions. (2) **Statistical Scoring Model** — evaluates patient variables to produce risk scores (0–100) and benefit levels. **FR-02, FR-03** • **Time:** ≤3 seconds |
| **Step 3 — Alerts & Insights** | High/moderate/low severity interaction alerts surface on the clinician's dashboard within 10 seconds. High-severity alerts (red) require clinician acknowledgement before dismissal. **FR-05** • **SLA:** 10 seconds                                                                                                       |
| **Step 4 — Secure Storage**    | All data encrypted at rest (AES-256) and in transit (TLS 1.3), stored in Malaysian cloud regions, governed by RBAC. Patients own their data. Audit trail immutable. **FR-07, FR-10, FR-21**                                                                                                                                |
| **Step 5 — Action**            | Clinician discusses flagged risks with patient during consultation. AI-generated summary saves review time. Recommendations are personalised and evidence-linked. Patient receives wellness score and plain-language guidance. Records shareable securely with specialists. **FR-04, FR-06, FR-09, FR-11**                 |
| **Visual**                     | A circular flow diagram. Each step is a node on the circle, connected by arrows showing the loop. Center of circle: "Closed-Loop Preventive Healthcare". Label each node with step number and key metric (e.g., "≤3 seconds", "AES-256", "10 seconds SLA").                                                                |
| **Source**                     | PRD §3 (Functional Requirements), TRD §5.1 (Supplement Analysis Pipeline), Pitch Deck §1.4 (Integration)                                                                                                                                                                                                                   |

---

### Slide 7: Why Now — Market Timing

| Element                                      | Content                                                                                                                                                                                                                                                                                                                                                             |
| -------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Headline**                                 | "Every trend lines up. The window is open."                                                                                                                                                                                                                                                                                                                         |
| **Driver 1 — Supplement Consumption Growth** | Malaysia's health supplement market is growing rapidly. Aging population, post-pandemic health consciousness, wellness trends. Consumers using vitamins, herbal products, functional foods, and traditional supplements at rising rates. **Direct effect:** More supplements = more interaction risk = more demand for AI clinical oversight.                       |
| **Driver 2 — Digital Health Expansion**      | Government digitalisation initiatives, telemedicine regulatory reforms, EMR implementation mandates, AI healthcare innovation. MOH actively pushing digital health. MDEC's GAH programme supports health-tech startups. **Direct effect:** Infrastructure readiness for a cloud-based, API-driven platform to plug into existing and emerging healthcare workflows. |
| **Driver 3 — Chronic Disease Burden**        | Malaysia faces rising rates of diabetes, hypertension, obesity, cardiovascular disease — all requiring long-term monitoring, patient engagement, and integrated health records. **Direct effect:** Chronic disease patients are the highest users of supplements AND highest risk for interactions. They are Healynx's core clinical use case.                      |
| **Driver 4 — Regulatory Push**               | PDPA enforcement strengthening. Healthcare cybersecurity requirements tightening. Growing patient concern about data misuse. **Direct effect:** Healynx's encryption-first, RBAC-governed, audit-logged architecture turns regulatory burden into competitive advantage.                                                                                            |
| **Driver 5 — No Existing Solution**          | No Malaysian platform combines AI supplement intelligence + secure records + preventive analytics. The gap is structural, not incremental. **Direct effect:** First-mover advantage in a defined, reachable market.                                                                                                                                                 |
| **Visual**                                   | 5 cards or columns, each with an icon (upward arrow for supplement growth, phone for digital health, heartbeat for chronic disease, shield for regulation, crossed-circle for no existing solution).                                                                                                                                                                |
| **Source**                                   | Pitch Deck §2.3 (Key Market Drivers)                                                                                                                                                                                                                                                                                                                                |

---

### Slide 8: Market Opportunity — TAM → SAM → SOM

| Element                                          | Content                                                                                                                                                                                                       |
| ------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Headline**                                     | "RM 1.75 Billion total addressable market — and we're starting with a focused beachhead."                                                                                                                     |
| **TAM — Total Addressable Market**               | **RM 1.75B annually** across private clinics (RM 400M+), private hospitals (RM 600M+), telemedicine platforms (RM 150M+), corporate employers (RM 300M+), insurance (RM 200M+), pharmacies (RM 100M+)         |
| **SAM — Serviceable Addressable Market**         | **RM 350M annually** — focused on 3,500 private clinics in Klang Valley, Penang, Johor Bahru + 15 active telemedicine platforms + 800 mid-to-large corporate employers with formal wellness programs          |
| **SOM — Serviceable Obtainable Market (Year 3)** | **~RM 780K ARR** — 98 accounts: 80 private clinics (RM 5K/yr avg), 5 multi-branch groups (RM 30K/yr), 2 telemedicine platforms (RM 40K/yr), 8 corporate wellness (RM 15K/yr), 3 API/data licenses (RM 10K/yr) |
| **ASEAN Expansion TAM**                          | Adding Singapore, Indonesia, Thailand, Vietnam, Philippines — combined population of 670M+, significantly expanding the addressable market beyond Malaysia                                                    |
| **Visual**                                       | Three nested circles or a funnel diagram. Largest circle: "TAM RM 1.75B". Middle: "SAM RM 350M". Smallest: "SOM RM 780K (Year 3)". Arrow from SOM pointing outward: "ASEAN: 670M population".                 |
| **Source**                                       | Pitch Deck §2.2 (Addressable Market)                                                                                                                                                                          |

---

### Slide 9: Product Deep Dive — The Game-Changing Features

| Element                                              | Content                                                                                                                                                                                                                                                                                                                                       |
| ---------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Headline**                                         | "Features that change how preventive healthcare works"                                                                                                                                                                                                                                                                                        |
| **Feature 1 — AI Supplement Interaction Detection**  | **FR-02, FR-05** • Trained on expert-verified, pre-labelled Malaysian clinical data • Detects drug-supplement, supplement-supplement, and condition-contraindication interactions • Sensitivity target: ≥85% (benchmarked against clinical pharmacist review) • Results in ≤3 seconds • Powered by Neo4j graph DB + statistical scoring model |
| **Feature 2 — Secure Medical Records Vault**         | **FR-07, FR-09, FR-11, FR-12** • AES-256 encrypted cloud storage in Malaysian region • AI-generated clinical summaries with confidence scores • Time-limited, permission-controlled secure sharing • Secure viewer with watermark, anti-copy, inactivity timeout • Emergency access override mode with full audit trail                       |
| **Feature 3 — Patient Wellness Score**               | **FR-06** • 0–100 score derived from supplement safety profile, health condition risk factors, medication adherence, and lifestyle indicators • Recalculated on each survey submission • Trend visualisation over time • Clinician-facing and patient-facing versions                                                                         |
| **Feature 4 — Clinician Dashboard with AI Insights** | **FR-18, FR-19, FR-20** • Searchable, filterable patient list with wellness scores and alert counts • AI-powered clinical record queries via chatbot • Automatic panel-wide scanning: high-severity interactions, score drops ≥15 points, surveys overdue ≥90 days • WebSocket real-time alert push                                           |
| **Feature 5 — Patient Mobile Dashboard**             | **FR-14, FR-15, FR-16, FR-17** • Wellness score trend line • Active supplement list with risk indicators • Medication/supplement reminders (push, SMS, email) • AI chatbot for natural-language health record queries • Supplement survey — structured, ~3 minute completion                                                                  |
| **Visual**                                           | 5 feature cards in a grid or horizontal scroll. Each card has: feature name, icon, 2–3 bullet points of key capabilities, 1 key number (e.g., "≤3 seconds", "AES-256", "≥85% sensitivity", "0–100 score").                                                                                                                                    |
| **Source**                                           | PRD §3 (FR-01 through FR-21), TRD §5.1–5.4 (AI Pipelines), doctor_UI.md, patient_UI.md                                                                                                                                                                                                                                                        |

---

### Slide 10: Technology & IP Moat — "Hard to Copy"

| Element                    | Content                                                                                                                                                                                                                                                                                                                                                                 |
| -------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Headline**               | "Protected by 2 UM patents. Built on architecture competitors can't replicate quickly."                                                                                                                                                                                                                                                                                 |
| **IP Protection**          | **IP 1 (PI 2025007955)** — Secure Medical Records Management (TRL 5) • **IP 2 (UI 2025002953)** — Supplement Intake Assessment Method (TRL 3 → 5) • **Both licensed exclusively from Universiti Malaya**                                                                                                                                                                |
| **Data Moat**              | Proprietary expert-labelled Malaysian supplement interaction dataset • Malaysian-market supplement and traditional remedy database • Years to replicate data quality • Continuous improvement via clinician feedback loop                                                                                                                                               |
| **Technical Architecture** | **Cloud-native (AWS Malaysia Region)** • **Multi-tenant SaaS** with Row-Level Security • **Microservices** on Kubernetes with GPU node group for AI • **PostgreSQL 16 + Redis 7 + Neo4j graph** • **FastAPI** with async Celery workers • **AES-256-GCM envelope encryption** with KMS key rotation every 90 days • **Keycloak** OIDC + OPA Rego policy engine for RBAC |
| **AI Pipeline**            | **BART-large** fine-tuned on UM clinical data for summarisation • **Neo4j Cypher queries** for interaction detection (expert-labelled rules engine) • **Statistical scoring model** with versioned weights • **Mistral 7B / Llama 3 8B** self-hosted for chatbot with RAG (pgvector) • **ONNX Runtime** for optimized inference                                         |
| **Compliance**             | PDPA 2010 compliant • AES-256 / TLS 1.3 • ISO 27001 aligned • Malaysian data residency • Audit trail immutability (append-only, separate schema, INSERT-only permissions)                                                                                                                                                                                               |
| **Visual**                 | A layered diagram. Bottom layer: "2 UM Patents" with patent numbers. Middle layer: "Data Moat — Expert-Labelled Malaysian Dataset". Top layer: "Technical Architecture — Cloud-Native, Multi-Tenant, AI-Powered". Right side: "Compliance — PDPA • ISO 27001 • AES-256". Arrows showing interconnection.                                                                |
| **Source**                 | TRD §1 (Technology Stack), TRD §6 (Security Architecture), Pitch Deck §6.4 (Competitive Moats)                                                                                                                                                                                                                                                                          |

---

### Slide 11: Business Model — "Recurring Revenue, Expanding Margins"

| Element                                  | Content                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| ---------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Headline**                             | "Multi-tier B2B SaaS with 75%+ recurring revenue at steady state"                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| **Revenue Streams**                      | **Healthcare Provider Subscriptions (55%)** — Monthly/annual SaaS for clinics, hospitals, telemedicine platforms • **API Licensing (15%)** — Supplement Intelligence Engine + AI Summarisation as standalone APIs, usage-based • **Enterprise Customisation (10%)** — Custom dashboards, AI workflow tailoring, bespoke integrations • **Corporate Wellness (15%)** — Per-employee/year assessment portal for employers • **Data Analytics (5%)** — Anonymised, aggregated health trends for researchers and public health |
| **Pricing — Clinic Tier**                | **Essentials:** RM 350/mo (3 clinicians, 500 patients, AI + supplement module) • **Professional:** RM 700/mo (10 clinicians, 2,000 patients, advanced AI + chatbot) • **Enterprise:** Custom (unlimited, white-label, dedicated CSM)                                                                                                                                                                                                                                                                                       |
| **Pricing — Telemedicine/Platform Tier** | **API Starter:** RM 1,200/mo (5K API calls, supplement endpoint) • **API Growth:** RM 2,800/mo (25K API calls, both AI endpoints, SLA) • **API Enterprise:** Custom                                                                                                                                                                                                                                                                                                                                                        |
| **Pricing — Corporate Wellness**         | **Starter:** RM 7,200/yr (up to 500 employees, RM 14/employee) • **Growth:** RM 20,000/yr (up to 2,500 employees, RM 10/employee) • **Enterprise:** Custom (5,000+ employees)                                                                                                                                                                                                                                                                                                                                              |
| **Unit Economics**                       | **ARPU:** ~RM 5,000/clinic/year • **Gross Margin:** 60% (Year 2) → 72% (Year 5) • **CAC:** RM 3,000–5,000 • **CAC Payback:** 10–17 months • **LTV/CAC Ratio:** 5:1 to 8:1 • **Break-even:** Month 30–36                                                                                                                                                                                                                                                                                                                    |
| **Visual**                               | Left side: 4 tier cards (Clinic, Telemedicine, Corporate, Enterprise) with pricing and features. Right side: A simple unit economics summary box showing ARPU, Gross Margin, CAC, LTV/CAC. Bottom: "75%+ recurring revenue at steady state".                                                                                                                                                                                                                                                                               |
| **Source**                               | Pitch Deck §4 (Business Model)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |

---

### Slide 12: Go-To-Market Plan — "Phased, Capital-Efficient, Partnership-Led"

| Element                                        | Content                                                                                                                                                                                                                                                                                                                                                                                                               |
| ---------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Headline**                                   | "Three phases from pilot to scale — with a bootstrap contingency"                                                                                                                                                                                                                                                                                                                                                     |
| **Phase 1 — Validation (Months 1–9)**          | **Target:** UM Medical Centre + 3–5 partner private clinics • **Focus:** Clinical validation, AI accuracy benchmarking, UX iteration • **Key metric:** AI sensitivity ≥85%, 200+ active patients, 5 pilot sites • **Cost:** Low (university infrastructure, startup cloud credits) • **TRL:** 5 → 6                                                                                                                   |
| **Phase 2 — Partnerships (Months 6–18)**       | **Target:** Private clinic groups (KPJ, Columbia Asia), telemedicine platforms (DoctorOnCall, Doctor Anywhere), pharmacy chains • **Focus:** API integrations, pilot-to-paid conversion, PDPA compliance • **Key metric:** 15 paying accounts, 2 telemedicine integrations, RM 5K MRR • **Cost:** Moderate (sales + compliance)                                                                                       |
| **Phase 3 — Commercial Launch (Months 15–24)** | **Target:** Scale to 40+ accounts across clinics, telemedicine, and corporate wellness • **Focus:** B2B sales team (3–5 people), conference marketing, insurance partnerships • **Key metric:** 40+ accounts, RM 32K MRR, break-even approaching • **TRL:** 6 → 7                                                                                                                                                     |
| **Buyer Decision Chain**                       | **Private clinic:** Clinic Owner (signs) → Senior Doctor (recommends) → Peer clinicians (influence) → IT lead (may block). **Telemedicine:** CTO/CPO (signs) → Product Lead (recommends) → Consultant network (influence) → CISO (may block). **Corporate:** HR Director/CFO (signs) → Wellness Manager (recommends) → Insurance broker (influence) → Legal (may block).                                              |
| **Bootstrap Contingency**                      | If seed funding delayed, 3-path strategy: **(1) Grant-first** — Apply to 5 programmes simultaneously (UMCIE, MDEC GAH, CREST, MIDA, MTDC). Target: RM 200–500K. **(2) Partner-funded** — Pre-commitments for development (RM 80–150K). **(3) Self-funded lean MVP** — 2 founders + 1 RA, RM 8–12K/month burn, startup cloud credits. Bootstrap path reaches same product milestones 6–12 months later, zero dilution. |
| **Visual**                                     | A timeline arrow from left (Month 0) to right (Month 24). Three segments color-coded: Phase 1 (blue), Phase 2 (teal), Phase 3 (green). Below the timeline, key milestones per phase. Below that, a smaller parallel track labeled "Bootstrap Contingency" showing the same phases shifted right by 6–12 months.                                                                                                       |
| **Source**                                     | Pitch Deck §5 (Go-To-Market Plan), PRD §9 (GTM Roadmap)                                                                                                                                                                                                                                                                                                                                                               |

---

### Slide 13: Competitive Landscape — "The Gap No One Fills"

| Element                                                             | Content                                                                                                                                                                                                                                                                                              |
| ------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Headline**                                                        | "No existing competitor combines AI supplement intelligence + secure records + preventive analytics"                                                                                                                                                                                                 |
| **Traditional EMR Providers** (Caresys, KMS Healthcare)             | **Strengths:** Clinical records, billing • **Weaknesses:** Limited or no AI, no supplement intelligence, weak preventive health • **Healynx advantage:** AI summaries + supplement detection + preventive scoring                                                                                    |
| **Telemedicine Platforms** (DoctorOnCall, Doctor Anywhere, BookDoc) | **Strengths:** Virtual consultations, appointment booking • **Weaknesses:** Records are consultation-centric, not longitudinal; minimal AI analytics; no supplement analysis • **Healynx advantage:** Longitudinal records + AI supplement safety + API integration                                  |
| **Generic Health Apps** (HealthifyMe, MyFitnessPal, Samsung Health) | **Strengths:** Calorie tracking, exercise logging • **Weaknesses:** Not medical-grade, no healthcare provider integration, weak security, no PDPA compliance • **Healynx advantage:** Medical-grade, clinician-integrated, PDPA-compliant, encrypted                                                 |
| **Pharmacy Systems** (Multicare, Pharmex)                           | **Strengths:** Dispensing, inventory • **Weaknesses:** Focus on pharmacy operations, not clinical intelligence • **Healynx advantage:** Clinical decision support + patient-facing preventive health                                                                                                 |
| **Hospital HIS** (iHIS, Cerner)                                     | **Strengths:** Large hospital operations • **Weaknesses:** Heavy, expensive, not for clinic/telemedicine/wellness segments, no supplement intelligence • **Healynx advantage:** Lightweight, affordable, supplement-aware, cloud-native                                                              |
| **Healynx's Unique Position**                                       | The only platform that sits at the intersection of: **(1)** AI-powered supplement safety, **(2)** Secure medical records, **(3)** Preventive health analytics, **(4)** Multi-tenant cloud-native SaaS, **(5)** Malaysian data residency & PDPA compliance                                            |
| **Visual**                                                          | A 2x2 matrix or a comparison table. If matrix: X-axis = "Clinical Depth (low→high)", Y-axis = "AI Intelligence (basic→advanced)". Plot competitors as dots. Healynx is a large star in the top-right quadrant (High Clinical Depth, High AI Intelligence). Include brief labels for each competitor. |
| **Source**                                                          | Pitch Deck §6 (Competitive Landscape)                                                                                                                                                                                                                                                                |

---

### Slide 14: Scaling Potential — "Malaysia → ASEAN Intelligence Infrastructure"

| Element                   | Content                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| ------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Headline**              | "From RM 780K ARR (Year 3) to RM 3.5M ARR (Year 5) — and beyond"                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| **Financial Trajectory**  | **Year 1:** RM 0 (pilot, 0 paying, 500 patients, 30 clinicians) • **Year 2:** RM 150K ARR (20 accounts, 2,500 patients) • **Year 3:** RM 780K ARR (98 accounts, break-even) • **Year 4:** RM 1.8M ARR (200 accounts, ASEAN entry) • **Year 5:** RM 3.5M ARR (380 accounts, 3 ASEAN markets)                                                                                                                                                                                                                                                        |
| **ASEAN Expansion Waves** | **Wave 1 (Year 2–3):** Singapore — highest healthcare spend per capita in ASEAN, regulatory alignment with Malaysia • **Wave 2 (Year 3–4):** Indonesia — 280M population, high supplement consumption (Jamu), rapidly growing digital health • **Wave 3 (Year 3–4):** Thailand — advanced medical tourism hub, strong traditional/herbal supplement culture • **Wave 4 (Year 4–5):** Vietnam — fast-growing economy, rising health consciousness • **Wave 5 (Year 4–5):** Philippines — large English-speaking market, high chronic disease burden |
| **Product Expansion**     | **Year 2–3:** Wearable integration (Apple Health, Google Fit, Garmin) — unlocks continuous monitoring tier (+20–30% ARPU) • **Year 3–4:** Insurance integration — wellness scoring API, risk-based premium models, opens RM 200M+ insurance analytics segment • **Year 3–5:** AI Predictive Healthcare — from reactive detection to predictive risk scoring for chronic disease • **Year 4–6:** National Healthcare Integration — MOH's MyHDW (Malaysian Health Data Warehouse), population health analytics                                       |
| **Long-Term Vision**      | "To become the regional AI preventive healthcare intelligence infrastructure — the intelligence layer between patients, providers, payers, and policymakers across ASEAN."                                                                                                                                                                                                                                                                                                                                                                         |
| **Visual**                | Bottom half: ASEAN map with numbered waves (1–5) marking country entries with year labels. Top half: A growth curve graph from Year 1–5 showing ARR climbing from RM 0 to RM 3.5M, with key milestone callouts (break-even, ASEAN entry, etc.).                                                                                                                                                                                                                                                                                                    |
| **Source**                | Pitch Deck §7 (Scaling Potential), Pitch Deck §9.6 (5-Year Financial Projection)                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |

---

### Slide 15: Traction & Validation — "What We've Built, What We're Proving"

| Element                   | Content                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| ------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Headline**              | "Two UM patents licensed. Research codebase transferred. Architecture designed. Building towards pilot."                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| **What's Done**           | • 2 UM patents filed and available for licensing (PI 2025007955, UI 2025002953) • Knowledge transfer contact established with both IP inventors (Dr. Lee Ching Shya, Dr. Nurul Fauzani Jamaluddin) • Research codebase accessed — UM's expert-labelled training dataset and algorithm documentation • Full technical architecture designed and documented (TRD v1.0) • Complete product specification authored (PRD v1.0 covering 21 functional requirements, 8 non-functional requirements) • Doctor and patient UI/UX fully designed with component specifications • 5-member founding team from UM Computer Science — hackathon-proven builders |
| **What's In Progress**    | • IP licensing agreement negotiation with UMCIE • Cloud infrastructure provisioning (AWS Malaysia Region) via startup programme • AI model accuracy benchmarking — targeting ≥85% sensitivity for drug-supplement interaction detection on 200+ test cases • Grant applications to CREST, MDEC GAH, UMCIE, MIDA, MTDC • Partner negotiation with UM Medical Centre for Phase 1 pilot                                                                                                                                                                                                                                                               |
| **Validation Benchmarks** | **AI Sensitivity:** ≥85% (target at v1 launch, benchmarked against clinical pharmacist review) • **AI Specificity:** ≥80% (minimise false-positive alert fatigue) • **Record Retrieval Time:** ≤3 seconds for any patient in clinician's panel • **User Onboarding:** ≤5 minutes for registration, consent, and first survey • **Platform Uptime:** 99.9% SLA                                                                                                                                                                                                                                                                                      |
| **Visual**                | Left column: "✅ Done" with 5–6 checkmark items. Right column: "🚧 In Progress" with 4–5 items showing progress indicators. Bottom: "🎯 Key Targets" with 3–4 benchmark numbers.                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| **Source**                | PRD §7 (Success Metrics), Pitch Deck §9 (Team)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |

---

### Slide 16: The Ask — "RM 1.5M–2.5M Seed / Commercialisation Funding"

| Element                                              | Content                                                                                                                                                                                                                                                                                                                                                               |
| ---------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Headline**                                         | "RM 1.5M–2.5M to close the blind spot and launch Malaysia's first AI preventive healthcare platform"                                                                                                                                                                                                                                                                  |
| **Use of Proceeds**                                  |                                                                                                                                                                                                                                                                                                                                                                       |
| Category                                             | Allocation                                                                                                                                                                                                                                                                                                                                                            |
| AI Model Development & Training                      | RM 350K–550K (22–23%)                                                                                                                                                                                                                                                                                                                                                 |
| Security Infrastructure                              | RM 180K–320K (12–13%)                                                                                                                                                                                                                                                                                                                                                 |
| Product Development (Full-stack, API, QA)            | RM 280K–500K (19–20%)                                                                                                                                                                                                                                                                                                                                                 |
| Clinical Validation (Multi-site pilot studies)       | RM 180K–350K (12–14%)                                                                                                                                                                                                                                                                                                                                                 |
| Regulatory Compliance (PDPA, MDA pathway)            | RM 130K–250K (9–10%)                                                                                                                                                                                                                                                                                                                                                  |
| Sales & Marketing (B2B team, conferences, campaigns) | RM 130K–250K (9–10%)                                                                                                                                                                                                                                                                                                                                                  |
| Cloud Infrastructure (AWS Malaysia Region)           | RM 100K–180K (7%)                                                                                                                                                                                                                                                                                                                                                     |
| Operations & Contingency                             | RM 50K–100K (4%)                                                                                                                                                                                                                                                                                                                                                      |
| **TOTAL**                                            | **RM 1,500,000 – 2,500,000**                                                                                                                                                                                                                                                                                                                                          |
| **Burn Rate**                                        | **Lean (5 people):** RM 55K/mo → 27 months runway on RM 1.5M • **Mid (8 people):** RM 85K/mo → 18 months on RM 1.5M • **Recommendation:** Start lean, scale after pilot validation at Month 12                                                                                                                                                                        |
| **Return Trajectory**                                | **Year 3 ARR:** RM 780K • **Year 5 ARR:** RM 3.5M • **Break-even:** Month 30–36 • **Gross Margin:** 60% → 72% by Year 5 • **LTV/CAC:** 5:1 to 8:1                                                                                                                                                                                                                     |
| **Contingency**                                      | If seed not secured: **Bootstrap path** using RM 200–500K in grants + partner pre-commitments + startup cloud credits. Same product, 6–12 months slower, zero dilution.                                                                                                                                                                                               |
| **Visual**                                           | Either: (a) A pie chart showing the allocation breakdown, or (b) A horizontal stacked bar with each category colored differently. Below the chart: a simple funding summary box: "RM 1.5M–2.5M Seed • 27 months runway (lean) • Break-even Month 30–36 • RM 3.5M ARR by Year 5". Small note: "Bootstrap contingency available — grants + partners cover RM 200–500K". |
| **Source**                                           | Pitch Deck §10 (The Ask & Financial Model)                                                                                                                                                                                                                                                                                                                            |

---

### Slide 17: Vision Close — "The Intelligence Backbone for Preventive Healthcare in ASEAN"

| Element            | Content                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| ------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Headline**       | "From a blind spot to a backbone — making preventive healthcare data-driven, personalised, and accessible across ASEAN."                                                                                                                                                                                                                                                                                                                                                      |
| **The Evolution**  | **Today (Year 0–2):** AI-powered supplement safety + secure records for Malaysian clinics • **Growth (Year 2–4):** Multi-country preventive health SaaS integrated with wearables, insurance, and telemedicine • **Scale (Year 4–7):** Regional AI healthcare intelligence layer serving clinics, hospitals, insurers, employers, researchers, and governments across ASEAN • **Moonshot (Year 7+):** The intelligence backbone for preventive healthcare in emerging markets |
| **The Team**       | 5 UM Computer Science founders • Hackathon-proven builders • Embedded in UM ecosystem (physical proximity to IP, inventors, Medical Centre) • Backed by IP inventors Dr. Lee Ching Shya (RTTP) and Dr. Nurul Fauzani Jamaluddin (RTTP)                                                                                                                                                                                                                                        |
| **Final Tagline**  | "Healynx — See what's missed. Secure what's scattered. Prevent what's predictable."                                                                                                                                                                                                                                                                                                                                                                                           |
| **Call to Action** | "Join us in closing Malaysia's clinical blind spot — and building ASEAN's preventive healthcare intelligence infrastructure."                                                                                                                                                                                                                                                                                                                                                 |
| **Visual**         | Full-bleed background (dark teal or deep blue gradient). Center: Healynx logo large. Below: the three-part tagline. Bottom-right: "National Deep Tech Challenge 2026 • Universiti Malaya". Optionally, subtle ASEAN map silhouette in the background.                                                                                                                                                                                                                         |
| **Source**         | Pitch Deck §7.3 (Long-Term Vision), §9 (Team)                                                                                                                                                                                                                                                                                                                                                                                                                                 |

---

## Design & Visual Guidelines

### Brand System

| Element       | Value                                                                               |
| ------------- | ----------------------------------------------------------------------------------- |
| **Primary**   | Teal (`--nv-primary-600: #1e8c8c` in light mode)                                    |
| **Secondary** | Blue (`--nv-secondary-600: #2563b8`)                                                |
| **Accent**    | Amber (`--nv-accent-500: #eda83e`)                                                  |
| **Danger**    | Red (`--nv-danger-600: #d92d2d`)                                                    |
| **Success**   | Green (`--nv-success-600: #2d8c4a`)                                                 |
| **Neutral**   | Slate gray scale (`--nv-neutral-900` through `--nv-neutral-050`)                    |
| **Fonts**     | Headings: Plus Jakarta Sans • Body: Inter                                           |
| **Tone**      | Professional, clinical, confident. No playful language. Direct operational wording. |

### Slide Layout Principles

- Maximum 40–50 words of text per slide (not counting numbers/sources)
- One key message per slide
- Numbers should be large, bold, and visually prominent
- Use icons and illustrations, not generic stock photography
- Dark mode recommended for presentation delivery (teal/dark background, white text)
- Light mode acceptable for print handouts
- Consistent footer: slide number, "Healynx | Confidential" (except title and close slides)

### Data Visualization Rules

- Bar charts for comparisons
- Line charts for growth trajectories (Year 1–5 projections)
- Funnel for TAM/SAM/SOM
- Radar for pain score dimensions
- Circular flow for closed-loop system diagram
- Tables for competitive comparison (keep to 5 columns max)
- Pie or horizontal stacked bar for funding allocation

---

## Content Rules & Guardrails

### What to ALWAYS Include

- The two UM patent numbers prominently (PI 2025007955, UI 2025002953)
- The three-part UVP (See what's missed / Secure what's scattered / Prevent what's predictable)
- Malaysian data residency and PDPA compliance
- The 3-second AI analysis SLA
- The bootstrap contingency (investors want to know founders have a Plan B)
- The 5-person founding team's UM CS background + IP inventor endorsements
- Specific numbers: RM 1.75B TAM, RM 350M SAM, RM 780K Year 3 ARR, RM 3.5M Year 5 ARR
- Break-even at Month 30–36

### What to NEVER Include

- Overpromising on AI accuracy (always say "≥85% sensitivity target" not "99.9% accurate")
- Claiming medical device or diagnostic status (Healynx is decision support, not SaMD in v1)
- Suggesting competitor disparagement (frame as "they don't do this" not "they're bad")
- Overloading slides with text (≤50 words max per slide)
- Generic wellness language (Healynx is clinical software, not a fitness app)
- Omitting risks / challenges (investors respect honesty — mention regulatory timing and AI accuracy risks, then discuss mitigation)

### Key Numbers to Feature Prominently

| Number       | Where to Use               |
| ------------ | -------------------------- |
| RM 1.75B     | TAM slide                  |
| RM 350M      | SAM slide                  |
| RM 780K      | SOM / Financial trajectory |
| RM 3.5M      | Year 5 ARR / The Ask       |
| Month 30–36  | Break-even timing          |
| 3 seconds    | AI analysis SLA            |
| ≥85%         | AI sensitivity target      |
| AES-256      | Security                   |
| 99.9%        | Uptime SLA                 |
| 98 accounts  | Year 3 target              |
| 380 accounts | Year 5 target              |
| 5:1 to 8:1   | LTV/CAC ratio              |
| 60% → 72%    | Gross margin trajectory    |

---

## Presentation Delivery Notes

### Suggested Deck Structure (17 Slides)

| #         | Slide Title                       | Duration (min)  |
| --------- | --------------------------------- | --------------- |
| 1         | Title Slide                       | 0:30            |
| 2         | The Hook — "Meet Mr. Rajan"       | 1:00            |
| 3         | The Problem — Clinical Blind Spot | 1:30            |
| 4         | Pain Score — Quantified Urgency   | 1:00            |
| 5         | The Solution — Healynx            | 1:30            |
| 6         | How It Works — Closed-Loop System | 2:00            |
| 7         | Why Now — Market Timing           | 1:00            |
| 8         | Market Opportunity — TAM/SAM/SOM  | 1:30            |
| 9         | Product Deep Dive — Key Features  | 2:00            |
| 10        | Technology & IP Moat              | 1:30            |
| 11        | Business Model                    | 2:00            |
| 12        | Go-To-Market Plan                 | 1:30            |
| 13        | Competitive Landscape             | 1:00            |
| 14        | Scaling Potential                 | 1:30            |
| 15        | Traction & Validation             | 1:00            |
| 16        | The Ask                           | 2:00            |
| 17        | Vision Close                      | 1:00            |
| **Total** |                                   | **~23 minutes** |

### Speaker Notes Guidelines

For each slide, the presenter should be able to:

1. State the headline (what this slide is about) — 10 seconds
2. Explain the key insight (why this matters) — 30 seconds
3. Give one concrete example or number — 15 seconds
4. Bridge to the next slide — 5 seconds

### Q&A Anticipation

Be prepared for these questions:

- "How is your AI different from existing drug interaction checkers?" — Answer: Malaysian-specific, expert-labelled, covers traditional remedies, combined with statistical scoring, not just a database lookup.
- "What's your customer acquisition cost and how do you get clinics to sign up?" — Answer: CAC RM 3K–5K, B2B healthcare sales via medical associations, conference demos, and the UM network as a credibility anchor.
- "Why would a clinic switch from their existing EMR?" — Answer: Healynx doesn't replace EMRs — it layers AI supplement intelligence + secure records ON TOP. API-first design integrates with existing systems.
- "How do you handle the regulatory risk around AI in healthcare?" — Answer: Decision support (not diagnosis), human-in-the-loop, transparent confidence scores, explainable outputs, early MDA engagement.
- "What happens if you don't raise the full RM 1.5M?" — Answer: Bootstrap path — 5 grants simultaneously, partner pre-commitments, lean 2-person team. Slower but viable. Zero dilution.

---

## Tools & Format

If creating slides:

- **Canva:** Use clean, minimal templates. Apply Healynx teal/dark palette. Export as PDF + PPTX.
- **Google Slides:** Use the same approach. Use "Google Slides" template with minimal distractions.
- **PowerPoint:** Use the "Minimal" or "Ion" theme as a starting point. Customize with Healynx colors.
- **Keynote:** Use the "Clean" theme. Healynx dark mode works well for presentation projectors.

**Output format:** Each slide should be described with:

- Headline text
- Body content (bullet points, numbers, key messages)
- Visual description / diagram specification
- Speaker note (what to say)
- Source reference (which document and section)

---

## Final Instruction

Generate a complete 17-slide pitch deck for Healynx based on all the content rules above. Each slide must include: full text content, data visualisation description, visual layout guidance, and speaker notes. The deck must tell a coherent story from problem through solution through market through ask, with numbers and business logic driving every slide. Do not skip any of the 17 slides. Ensure every number used can be traced back to the source documents (PRD, TRD, Pitch Deck, doctor_UI, patient_UI).
