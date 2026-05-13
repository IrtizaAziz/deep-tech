# Healynx — Product Requirement Document (PRD)

**Document Version:** 1.0  
**Date:** 13 May 2026  
**Author:** Product Management  
**Classification:** Confidential — For Design & Development Handoff  
**Source IPs:**
| IP | Patent No. | TRL | Owner |
|---|---|---|---|
| Interactive Patient Supplement Intake & Disclosure Practice Assessment Method | UI 2025002953 | 3 | Universiti Malaya |
| Computer-Implemented Method for Secure Management of Medical Records in an AI-Enhanced System | PI 2025007955 | 5 | Universiti Malaya |

---

## 1. Product Overview

### Product Name
**Healynx**

### Elevator Pitch
Healynx is the first integrated AI-powered preventive healthcare platform in Malaysia that combines real-time supplement intelligence with secure, AI-enhanced medical records management — enabling clinicians to detect hidden risks, patients to manage their health proactively, and healthcare organisations to deliver preventive care at scale.

### Mission Statement
To close the clinical blind spot between what patients consume and what doctors know, by making every supplement intake visible, every medical record intelligent, and every health decision preventive.

### Problem Statement
- **Fragmented records:** Patient medical histories are siloed across clinics, paper files, and phone-camera scans, with no unified, searchable digital record and no AI to extract clinical meaning.
- **Undisclosed supplement risks:** Patients routinely fail to disclose supplement, herbal, and traditional remedy use to doctors, creating dangerous drug-supplement interaction blind spots that go undetected until adverse events occur.
- **Lack of preventive health intelligence:** Existing healthcare tools focus on reactive record-keeping; no Malaysian platform combines structured behavioural intake data with clinical records to generate real-time, personalised preventive health recommendations.

### Solution Summary
Healynx merges two Universiti Malaya IPs into a single cloud-native platform: patients complete a structured supplement intake survey (IP 2 — UI 2025002953), which is processed by an AI algorithm and statistical scoring model to detect risks and generate personalised recommendations. Results are then stored, summarised, and surfaced within a secure, encrypted medical record (IP 1 — PI 2025007955) accessible via role-based access control. Clinicians view AI-generated summaries, drug-supplement interaction alerts, and patient wellness scores through a unified dashboard, while secure time-limited sharing enables collaboration with specialists, insurers, and telemedicine providers — forming a closed-loop preventive healthcare system.

### Scope

**In Scope (v1 MVP):**
- Structured supplement intake survey instrument
- AI-driven supplement risk/benefit analysis and personalised recommendations
- Encrypted cloud storage for medical documents (PDF, image, scan)
- AI-generated medical record summaries
- Role-based access control (patient, doctor, admin)
- Patient-facing mobile/web dashboard with health summary and supplement tracker
- Clinician dashboard with patient list, AI summaries, and interaction alerts
- Emergency access override mode with audit trail
- Audit logging for all access and sharing events
- PDPA-compliant consent management
- AES-256 / TLS 1.3 encryption

**Out of Scope (v1):**
- OCR digitisation of scanned documents (planned v2)
- Time-limited secure record sharing via external links (planned v2)
- EMR/EHR integration with third-party systems (planned v2)
- Insurance integration and wellness scoring API (planned v3)
- Wearable device data ingestion (planned v3)
- ASEAN multi-language localisation (planned v3)
- In-pharmacy kiosk mode
- Corporate wellness aggregate dashboards

---

## 2. User Personas

### Persona 1 — Dr. Amira, General Practitioner (Primary)
| Attribute | Detail |
|---|---|
| **Role** | General practitioner at a private clinic in Klang Valley. Sees 40–50 patients/day; ~60% have at least one chronic condition. |
| **Goals** | Reduce time spent reviewing fragmented records; detect risky supplement combinations before prescribing; provide preventive health guidance during 10-minute consultations. |
| **Pain Points** | Patients do not disclose supplement/herbal use; paper records and phone-scanned PDFs are non-searchable; no automated way to cross-check drug-supplement interactions during consultation; medico-legal risk from undocumented supplement use. |
| **How Healynx Solves** | Before consultation, the patient completes the supplement intake survey in the waiting room on a tablet. The Supplement Intelligence Engine (IP 2) flags dangerous interactions and generates a risk score. The Secure Medical Records Module (IP 1) presents an AI-summarised patient history in under 3 seconds. Dr. Amira sees all alerts on her clinician dashboard, discusses them with the patient, and saves the updated record securely — all within the consultation window. |

### Persona 2 — Mr. Rajan, Patient (Primary)
| Attribute | Detail |
|---|---|
| **Role** | 58-year-old with Type 2 diabetes and hypertension. Takes 3 prescription medications (metformin, amlodipine, atorvastatin) and 4 supplements (vitamin D, fish oil, garlic extract, a traditional jamu preparation). Visits 2 different clinics and occasionally buys supplements online. |
| **Goals** | Understand whether his supplements are safe with his medications; keep all his health records in one place; share records securely when seeing a specialist; receive reminders so he doesn't miss doses. |
| **Pain Points** | Cannot remember all supplement names/dosages during doctor visits; has records scattered across two clinics plus printed lab reports; worried about supplement interactions but no one systematically checks; forgets to take medications on weekends. |
| **How Healynx Solves** | Mr. Rajan completes the structured supplement survey once, and it becomes part of his permanent encrypted record (IP 1). The AI (IP 2) flags that garlic extract may amplify his amlodipine's blood-pressure-lowering effect and that his jamu contains undeclared ingredients with potential liver interaction risk. He receives a personal wellness score and plain-language recommendations on his mobile dashboard. He sets medication/supplement reminders. When referred to a cardiologist, his GP shares a time-limited secure link to his records. |

### Persona 3 — Ms. Linda, Clinic Administrator (Secondary)
| Attribute | Detail |
|---|---|
| **Role** | Clinic manager at a 5-doctor private clinic. Responsible for operations, compliance, patient intake flow, and audit preparation. |
| **Goals** | Digitise patient intake to reduce paper forms and waiting room clutter; ensure PDPA compliance for all patient data handling; demonstrate audit readiness for MOH inspections; reduce staff time spent on manual record filing and retrieval. |
| **Pain Points** | Paper intake forms are illegible, incomplete, or missing supplement disclosure entirely; preparing for audits requires days of manual record pulling; staff spend 15–20% of their time on filing and retrieval; no systematic access control — any staff member can theoretically open any physical file. |
| **How Healynx Solves** | The structured digital survey (IP 2) eliminates paper intake forms and ensures complete supplement disclosure data. Role-based access control (IP 1) restricts record access by role — receptionists can initiate surveys but cannot view clinical records; doctors have full access; administrators manage accounts. Audit logs (IP 1) are automatically generated for every access, sharing, and data modification event, making audit preparation near-instant. Staff time on record management drops by 60%. |

### Persona 4 — Mr. Hafiz, Telemedicine Platform Operator (Secondary)
| Attribute | Detail |
|---|---|
| **Role** | Product and operations lead at a Malaysian telemedicine platform handling 50,000+ virtual consultations per month. Responsible for platform features, partner clinic onboarding, and clinical quality. |
| **Goals** | Add value-added AI features to differentiate the platform; ensure consultants have complete patient data before virtual consultations; reduce clinical risk from incomplete patient histories; offer a secure records feature as a retention tool. |
| **Pain Points** | Consultants work with only what the patient types into a chat box — no structured supplement history, no longitudinal records; high rate of incomplete consultations requiring follow-up; platform lacks a secure, integrated medical records module; supplement use is a complete blind spot for telemedicine. |
| **How Healynx Solves** | Healynx integrates via API. Before a teleconsultation, the patient completes the supplement intake survey within the telemedicine app (IP 2). The consultant sees AI-generated interaction flags and a risk summary before the video call begins. Encrypted records (IP 1) are stored and accessible across consultations, creating longitudinal patient histories. The platform differentiates with AI-powered supplement safety and secure records, increasing consultation quality and reducing clinical risk. |

---

## 3. Functional Requirements

All requirements are tagged with the originating UM IP. Requirements are testable, unambiguous, and written for design and development handoff.

### A. Supplement Intelligence Module
*Derived from UM IP UI 2025002953 — Interactive Patient Supplement Intake & Disclosure Practice Assessment Method*

| ID | Requirement | IP Source | Acceptance Criteria |
|---|---|---|---|
| **FR-01** | The system shall present a structured survey instrument that captures: supplement/product name, dosage (amount + unit), frequency (daily/weekly/monthly/as-needed), duration of use, health condition being addressed, and relevant health variables (age, sex, known conditions, current medications, allergies). | IP 2 | Survey completion rate ≥95% in usability testing; all mandatory fields validated before submission; survey renders correctly on mobile (320px width) and desktop (1920px width). |
| **FR-02** | The system shall process survey responses through an AI algorithm trained on expert-verified, pre-labelled medical data to detect drug-supplement interactions, supplement-supplement interactions, and contraindications with existing health conditions. | IP 2 | AI sensitivity (detection of true interactions) ≥85% when benchmarked against clinical pharmacist review on a test set of ≥200 cases; results returned within 3 seconds of survey submission. |
| **FR-03** | The system shall apply a statistical scoring model that evaluates patient variables (dosage, frequency, health profile, comorbidities) to produce a numeric risk score (0–100) and a categorical benefit level (Low/Moderate/High) for each supplement in the patient's regimen. | IP 2 | Risk score output is deterministic for identical inputs; score ranges are documented with clinical rationale for each threshold; scoring model version is logged with each assessment. |
| **FR-04** | Upon survey submission, the system shall generate and display real-time personalised recommendations including: (a) supplements flagged as safe to continue, (b) supplements flagged for clinician review with suggested alternatives, (c) supplements flagged with interaction warnings requiring immediate attention, and (d) general wellness recommendations based on the patient's profile. | IP 2 | Recommendations rendered on-screen within 5 seconds of submission; each recommendation links to its supporting evidence source; both patient-facing (plain language) and clinician-facing (clinical terminology) versions are generated. |
| **FR-05** | The system shall flag drug-supplement interaction alerts to both the patient record and the assigned doctor's dashboard, with severity levels: Low (informational), Moderate (recommend monitoring), High (recommend discontinuation or immediate review). | IP 2 | Alert appears on clinician dashboard within 10 seconds of survey completion; alert is linked to the specific patient record; alert severity is colour-coded (green/amber/red); clinician must acknowledge High-severity alerts before the alert can be dismissed. |
| **FR-06** | The system shall calculate and display a Patient Wellness Score (0–100) derived from: supplement safety profile, health condition risk factors, medication adherence (when available), and lifestyle indicators captured in the survey. The score shall be recalculated upon each new survey submission. | IP 2 | Score formula is documented and versioned; score updates within 5 seconds of survey submission; historical scores are graphed on the patient dashboard; score interpretation key is displayed alongside the score. |

### B. Secure Medical Records Module
*Derived from UM IP PI 2025007955 — Computer-Implemented Method for Secure Management of Medical Records in an AI-Enhanced System*

| ID | Requirement | IP Source | Acceptance Criteria |
|---|---|---|---|
| **FR-07** | The system shall allow authorised users to upload medical documents in PDF, JPEG, PNG, and TIFF formats to an encrypted cloud storage bucket. Uploaded files shall be encrypted at rest using AES-256 before storage. | IP 1 | Upload succeeds for files up to 25MB; file integrity verified via checksum post-upload; encrypted storage confirmed via infrastructure audit; upload progress indicator shown to user. |
| **FR-08** | The system shall perform optical character recognition (OCR) on uploaded scanned documents and images, extract text content, and structure the output into standardised record fields (diagnosis, date, provider, findings, medications). | IP 1 | OCR accuracy ≥90% for typed English text on clean scans at 300 DPI; extracted fields are presented for user review and correction before final storage; OCR processing completes within 30 seconds for a 5-page document. |
| **FR-09** | The system shall generate an AI-powered medical record summary for each patient that includes: chronological medical history, active conditions, current medications, recent lab results (if available), and supplement profile with detected risks. The summary shall be tagged with a confidence score. | IP 1 | Summary generated within 10 seconds of request; summary length does not exceed 500 words; confidence score is displayed; summary includes a "last updated" timestamp; clinician can flag summary errors for model improvement. |
| **FR-10** | The system shall enforce role-based access control (RBAC) with the following roles: Patient (view own records, complete surveys, manage sharing permissions), Doctor (view/edit assigned patient records, view AI insights, acknowledge alerts), Clinic Administrator (manage user accounts, view audit logs, access aggregate reports — no clinical record access), Emergency Access (temporary full read access to a specific patient record, logged with justification). | IP 1 | Each role can only perform actions defined in its permission matrix; unauthorised access attempts are logged and alert administrators; role assignment is auditable; a user can hold only one role per session. |
| **FR-11** | The system shall enable patients and doctors to generate time-limited, permission-controlled secure sharing links for a specific patient record or record subset. Sharing parameters shall include: expiry time (default 72 hours, max 30 days), access type (view-only default), and optional access code. | IP 1 | Shared link is a cryptographically random URL (≥128 bits entropy); link expires and becomes inaccessible after the set duration; access via link is logged in audit trail; recipient cannot share or forward the link (link is tied to recipient identity where possible). |
| **FR-12** | The system shall render shared records in a secure viewer that enforces: view-only mode (no download, no print, no copy/paste, no screenshot detection where technically feasible). The viewer shall include a visible watermark with the recipient's name/email and access timestamp. | IP 1 | Right-click disabled; keyboard shortcuts for copy/save disabled; print CSS renders blank or watermarked-only; watermark is clearly visible and contains identifying information; viewer session times out after 15 minutes of inactivity. |
| **FR-13** | The system shall provide an Emergency Access Override mode that allows an authorised clinician to bypass standard permission restrictions and gain temporary read access to a patient record. Activation requires: reason selection from a predefined list or free-text justification, explicit confirmation, and automatic logging of the access event with timestamp, accessing clinician identity, accessed patient identity, and stated reason. | IP 1 | Emergency access is immediately logged in the audit trail; the patient and clinic administrator receive an automated notification within 5 minutes; emergency access session expires after 30 minutes; all actions during emergency access are separately logged. |

### C. Patient Engagement Module
*Powered by both IPs — survey data from IP 2 feeds the patient dashboard; secure record data from IP 1 powers the health summary and chatbot*

| ID | Requirement | IP Source | Acceptance Criteria |
|---|---|---|---|
| **FR-14** | The system shall provide a responsive mobile/web patient dashboard displaying: (a) wellness score with trend line, (b) active supplement list with risk status indicators, (c) upcoming medication/supplement reminders, (d) recent AI recommendations, (e) quick-access button to update supplement survey. | IP 1 + IP 2 | Dashboard loads within 3 seconds on 4G mobile connection; all data points are current (≤60 seconds staleness); dashboard adapts layout between mobile (single column) and desktop (grid). |
| **FR-15** | The system shall enable patients to set and manage medication and supplement reminders with configurable: time of day, days of week, dosage instructions, and notification type (push notification, SMS, or email). | IP 1 + IP 2 | Reminder fires within ±2 minutes of scheduled time; patient can snooze or dismiss; reminder log shows adherence history; notification delivery rate ≥95%. |
| **FR-16** | The system shall generate health trend visualisations over time for: wellness score, supplement count, interaction alert count, and self-reported outcomes. Visualisations shall support time ranges: 1 month, 3 months, 6 months, 1 year, and All. | IP 1 + IP 2 | Charts render using a standard charting library; data points are plotted with dates on x-axis; each chart includes a title, axis labels, and legend; export as PNG is available. |
| **FR-17** | The system shall include an AI chatbot interface that enables patients to ask natural-language questions about their health records, supplement recommendations, and general health information. The chatbot shall have access to the patient's own record data and the system's supplement knowledge base. Responses that constitute medical advice shall include a disclaimer recommending clinician consultation. | IP 1 + IP 2 | Chatbot responds within 5 seconds; response relevance rated ≥4/5 in user testing; chatbot does not hallucinate data not present in the patient's record; medical disclaimer is present on every response that could be construed as advice. |

### D. Clinician Dashboard Module
*Powered by both IPs — AI insights from IP 2 populate alerts and summaries; secure records from IP 1 power the patient list and audit logs*

| ID | Requirement | IP Source | Acceptance Criteria |
|---|---|---|---|
| **FR-18** | The system shall provide a clinician dashboard with a searchable, filterable patient list displaying for each patient: name, age, last visit date, wellness score, number of active supplement alerts (by severity), and an AI-generated one-line clinical summary. The list shall support sorting by any column and filtering by alert severity, wellness score range, and last visit date range. | IP 1 + IP 2 | Patient list loads within 3 seconds for up to 500 patients; search returns results within 1 second; filters apply correctly and can be combined; clinical summary is accurate against the full AI summary for each patient. |
| **FR-19** | The system shall provide a clinician-facing chatbot interface that supports natural-language queries across patient records, including: "Show me all patients on metformin taking supplements with potential interactions," "What was Mr. X's HbA1c trend over the last year?", and "Which of my patients have unacknowledged high-severity alerts?" | IP 1 + IP 2 | Query results return within 5 seconds; chatbot correctly interprets ≥90% of predefined test queries; results are restricted to the clinician's assigned patient panel; chatbot does not expose data from unassigned patients. |
| **FR-20** | The system shall automatically scan the clinician's entire assigned patient panel and generate alerts for: (a) any patient with ≥1 high-severity supplement interaction, (b) any patient with ≥3 moderate-severity interactions, (c) any patient whose wellness score dropped by ≥15 points since last assessment, (d) any patient who has not updated their supplement survey in ≥90 days. | IP 1 + IP 2 | Panel scan runs at least once every 24 hours; alerts are visible on the clinician dashboard home screen; alert list can be filtered by type and severity; alert generation is logged in audit trail. |
| **FR-21** | The system shall maintain immutable audit logs for every: record access (who accessed whose record, when, from which IP, for how long), record modification (what changed, before/after values, who made the change), sharing event (who shared what with whom, expiry time set), emergency access activation, and alert acknowledgement by clinicians. Audit logs shall be searchable by date range, user, patient, and event type. | IP 1 | Every event listed produces an audit log entry within 1 second; log entries are append-only and cannot be modified or deleted; log export available as CSV and PDF; log retention period is configurable (default 7 years). |

---

## 4. Non-Functional Requirements

| ID | Requirement | Target / Standard | Verification Method |
|---|---|---|---|
| **NFR-01** | All patient data shall be encrypted at rest using AES-256 and all data in transit shall be encrypted using TLS 1.3 (or higher). Encryption keys shall be managed via a cloud-native key management service (KMS) with automatic key rotation every 90 days. | AES-256, TLS 1.3 | Infrastructure penetration test; configuration audit; certificate validation. |
| **NFR-02** | The platform shall comply with the Malaysia Personal Data Protection Act 2010 (PDPA), including: explicit consent collection before data processing, data subject access request workflow, data retention and deletion policies, and cross-border data transfer restrictions (patient data stored exclusively in Malaysian cloud regions). | PDPA 2010 | Legal audit by PDPA-qualified consultant; consent flow UX review; data residency infrastructure verification. |
| **NFR-03** | The system shall align with healthcare cybersecurity standards including: ISO 27001 information security management principles, OWASP Top 10 vulnerability mitigation, and Malaysia's National Cyber Security Agency (NACSA) healthcare sector guidelines. | ISO 27001 (aligned), OWASP Top 10, NACSA guidelines | Third-party penetration test; static code analysis; dependency vulnerability scan in CI/CD pipeline. |
| **NFR-04** | The cloud-deployed platform shall maintain 99.9% uptime (excluding scheduled maintenance announced ≥48 hours in advance). Scheduled maintenance shall occur during off-peak hours (02:00–05:00 MYT). | 99.9% (≤8.76 hours downtime/year) | Uptime monitoring via independent service; SLA report published monthly. |
| **NFR-05** | AI analysis results (supplement interaction detection, risk scoring, recommendation generation) shall be returned within 3 seconds of survey submission for ≥95% of requests. AI record summarisation shall complete within 10 seconds for ≥95% of requests. | 3 seconds (analysis), 10 seconds (summarisation) | Load testing at 500 concurrent requests; 95th percentile response time measurement. |
| **NFR-06** | The patient and clinician web interfaces shall be mobile-responsive and render correctly on: iOS Safari (latest 2 versions), Android Chrome (latest 2 versions), Chrome Desktop (latest 2 versions), Firefox Desktop (latest version), and Safari Desktop (latest 2 versions). Minimum supported viewport width: 320px. | Responsive design | Cross-browser testing suite; manual QA on each target browser/device. |
| **NFR-07** | The platform shall expose functionality via a versioned RESTful API with JSON request/response bodies, OAuth 2.0 authentication, rate limiting (configurable per API key), and comprehensive OpenAPI 3.0 documentation. All features available in the UI shall be accessible via API. | API-first architecture | OpenAPI spec validation; API integration test suite; rate limit enforcement test. |
| **NFR-08** | Every record access, modification, sharing, and emergency override event shall generate an immutable audit trail entry containing: timestamp (ISO 8601, UTC), actor ID, target resource ID, action type, outcome (success/failure), source IP, and user agent. Audit log entries shall be stored in a separate, append-only data store with write-only permissions for the application layer. | Immutable audit trail | Audit log integrity verification; penetration test confirming immutability; storage separation confirmed via architecture review. |

---

## 5. System & Architecture Constraints

### Deployment Model
- **Cloud-based deployment** on AWS, Azure, or GCP with preference for cloud providers offering a Malaysian region (AWS Asia Pacific [Malaysia] Region, Azure Malaysia West, or equivalent).
- **Multi-tenant SaaS architecture** with logical tenant isolation at the database layer. Each clinic/organisation is a tenant. Tenant data shall never co-mingle in query results.
- **Containerised microservices** orchestrated via Kubernetes (or equivalent managed service, e.g., EKS/AKS/GKE) to enable independent scaling of the AI engine, OCR pipeline, API gateway, and web application servers.

### Data Architecture
- **Patient health data** stored in a dedicated, encrypted database cluster within Malaysian cloud region boundaries.
- **AI model artifacts and training data** stored separately from production patient data.
- **Audit logs** stored in an append-only log store (e.g., CloudWatch Logs, Azure Monitor, or ELK stack with immutability enabled).

### Integration Compatibility
- **EMR/EHR integration compatibility** via HL7 FHIR R4 standards. The platform shall support FHIR Patient, Observation, Condition, MedicationStatement, and DocumentReference resources for both inbound (import) and outbound (export) data exchange. In v1, FHIR compatibility is architectural readiness only; full integration is a v2 feature.
- **API-first design** as specified in NFR-07. Third-party telemedicine platforms, pharmacy systems, and insurance systems shall integrate via the versioned REST API.

### Scalability
- Architecture shall support scaling from 500 active patients (Phase 1) to 100,000+ active patients (Phase 3–4) without architectural re-platforming.
- Multi-language UI framework shall be implemented from Day 1 using i18n libraries, with initial support for English and Bahasa Malaysia. ASEAN language expansion (Bahasa Indonesia, Thai, Vietnamese) is a v3 feature.

### Access Control
- **Role-based access control (RBAC)** with at minimum four roles: Patient, Doctor, Clinic Administrator, Emergency Access.
- Permission matrix shall be configurable per tenant to accommodate different clinic policies.
- Emergency override shall be available 24/7 with automatic notification and audit logging.

---

## 6. User Stories (Top 10 Priority)

Priority assigned using MoSCoW method for v1 scope.

| # | User Story | Priority | Rationale |
|---|---|---|---|
| **US-01** | As a **patient**, I want to complete a structured supplement intake survey on my phone before my clinic visit, so that my doctor has a complete picture of everything I am taking without me having to remember during the consultation. | **Must Have** | Core IP 2 application. Without this, the supplement intelligence pipeline has no input. |
| **US-02** | As a **doctor**, I want to see AI-generated drug-supplement interaction alerts on my dashboard for each patient, so that I can identify and address risks during the consultation without manual cross-referencing. | **Must Have** | Core value proposition of IP 2. Directly solves the "undisclosed supplement risk" problem. |
| **US-03** | As a **patient**, I want all my medical records (lab reports, prescriptions, discharge summaries) stored securely in one place and accessible from my phone, so that I never have to carry paper files between clinics again. | **Must Have** | Core IP 1 application. The secure records module is the foundation. |
| **US-04** | As a **doctor**, I want to view an AI-generated summary of a patient's medical history before the consultation begins, so that I can reduce my record review time and focus on the patient. | **Must Have** | Highest-impact IP 1 feature for clinician workflow. Directly targets the "fragmented records" problem. |
| **US-05** | As a **clinic administrator**, I want role-based access control so that receptionists can register patients without seeing clinical records, and only authorised doctors can view sensitive data. | **Must Have** | Essential for PDPA compliance and security. Enables multi-role deployment within a single clinic. |
| **US-06** | As a **patient**, I want a personal wellness score and plain-language recommendations after completing my supplement survey, so that I can understand my health risks and take preventive action between doctor visits. | **Should Have** | Enhances patient engagement and self-management. Directly derived from IP 2's personalised output. Critical for patient retention. |
| **US-07** | As a **doctor**, I want an emergency access override to view a patient's full records without their prior consent during a medical emergency, so that I can make life-saving decisions with complete information. | **Must Have** | Safety-critical requirement from IP 1. Non-negotiable for clinical deployment. Must be logged and auditable. |
| **US-08** | As a **clinic administrator**, I want to view audit logs showing who accessed which patient records and when, so that I can demonstrate compliance during MOH audits and investigate any suspected unauthorised access. | **Must Have** | PDPA compliance requirement. Enables the audit readiness that clinic administrators need. |
| **US-09** | As a **telemedicine platform operator**, I want to integrate Healynx's supplement survey and interaction detection via API into my platform's pre-consultation flow, so that my consultants have complete patient data before virtual visits begin. | **Should Have** | Opens the telemedicine segment (Persona 4). API-first architecture is a v1 requirement even if telemedicine API integration is validated during v1 and full commercial integration occurs in v2. |
| **US-10** | As a **patient**, I want to set medication and supplement reminders on my phone, so that I maintain adherence and reduce the risk of missed doses or double-dosing. | **Could Have** | Valuable for patient engagement and data completeness (adherence data enriches wellness scoring), but not core to the supplement safety or secure records value propositions. Can be released as a v1.1 update. |

### MoSCoW Summary
- **Must Have (v1):** US-01, US-02, US-03, US-04, US-05, US-07, US-08
- **Should Have (v1):** US-06, US-09
- **Could Have (v1 backlog):** US-10
- **Won't Have (v1):** Secure record sharing via external links, OCR document processing, EMR/EHR integration, wearable data.

---

## 7. Success Metrics (KPIs)

| KPI | Target | Measurement Method | Timeframe |
|---|---|---|---|
| **AI Recommendation Accuracy (Sensitivity)** | ≥85% sensitivity for drug-supplement interaction detection benchmarked against clinical pharmacist review | Controlled test set of ≥200 cases; quarterly re-benchmarking | At v1 launch and quarterly thereafter |
| **AI Recommendation Accuracy (Specificity)** | ≥80% specificity (minimise false-positive alerts that cause alert fatigue) | Same test set as sensitivity measurement | At v1 launch and quarterly thereafter |
| **User Onboarding Time** | ≤5 minutes for a new patient to complete registration, consent, and first supplement survey | Timed usability test with 30 new users | At v1 launch |
| **Record Retrieval Time** | ≤3 seconds from search query to full record display for any patient in the clinician's panel | Synthetic load test with 1,000 patient records per clinician | At v1 launch |
| **Monthly Active Users (MAU)** | 500 MAU (patients) and 30 MAU (clinicians) | Platform analytics dashboard | 6 months post-launch |
| **Doctor Workflow Time Reduction** | ≥30% reduction in time spent on record review (pre-consultation preparation) compared to pre-Healynx baseline | Time-motion study with 15 clinicians over 2 weeks | Within 3 months of pilot deployment |
| **Patient Supplement Disclosure Rate** | ≥90% of patients complete the supplement survey and disclose ≥1 supplement (among patients who self-report supplement use in baseline interview) | Compare survey completion data against baseline patient interview at pilot clinic | Within 3 months of pilot deployment |
| **High-Severity Alert Acknowledgment Rate** | ≥95% of high-severity supplement interaction alerts acknowledged by a clinician within 72 hours | Audit log analysis | Ongoing from v1 launch |
| **Platform Uptime** | ≥99.9% (≤8.76 hours downtime per year, excluding scheduled maintenance) | Independent uptime monitoring service | Ongoing from v1 launch |
| **API Response Time (p95)** | ≤3 seconds for AI analysis endpoint, ≤500ms for CRUD endpoints | API gateway metrics | Ongoing from v1 launch |

---

## 8. Assumptions, Risks & Dependencies

### Assumptions
| # | Assumption | Impact if Invalid |
|---|---|---|
| A1 | Target private clinics in Klang Valley, Penang, and Johor Bahru have reliable internet connectivity (≥10 Mbps) sufficient for cloud-based application access. | Offline-first architecture or local caching layer would be required, increasing v1 scope. |
| A2 | Patients have basic digital literacy sufficient to complete a mobile survey (reading level equivalent to primary 6 / UPSR). | Survey instrument would require voice-guided mode or assisted-completion workflow, delaying v1. |
| A3 | Clinicians are willing to adapt their workflow to include a pre-consultation digital survey for supplement disclosure. | Clinician adoption would require additional change-management effort, user training, and possibly incentive structures. |
| A4 | The expert-labelled training dataset for the AI algorithm (IP 2) is transferable from UM as part of the IP licensing agreement, with sufficient coverage of Malaysian-market supplements and traditional remedies. | Additional data labelling effort (3–6 months) would be required before achieving target AI accuracy. |
| A5 | PDPA compliance and healthcare cybersecurity certification can be achieved within the 9–18 month regulatory readiness window. | Commercial launch timeline would extend; pilot deployments would be limited to UM-affiliated facilities with research ethics approval. |
| A6 | The combined platform does not trigger Medical Device Authority (MDA) classification as a Software as a Medical Device (SaMD) in v1, as it provides decision support (not diagnosis or treatment recommendations without clinician review). | If SaMD classification is required, additional regulatory preparation (6–12 months) and certification costs would apply. |

### Risks
| # | Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|---|
| R1 | **Regulatory delay:** PDPA certification or MDA classification process takes longer than planned, delaying commercial launch. | Medium | High | Begin regulatory engagement in Month 1; engage a healthcare regulatory consultant; design platform to be compliance-ready so certification is confirmatory, not corrective. |
| R2 | **AI accuracy on local population:** The AI model trained on UM's expert-labelled data underperforms when deployed on a broader, more diverse Malaysian patient population (ethnicity, supplement types, traditional remedies not in the training set). | Medium | High | Expand training dataset during Phase 1 validation to include diverse patient demographics; continuous learning pipeline to incorporate clinician feedback and corrections; transparent confidence scores so clinicians can calibrate trust. |
| R3 | **Clinician adoption resistance:** Doctors perceive AI alerts as adding to cognitive load rather than reducing it, or distrust AI-generated recommendations. | Medium | Medium | Human-in-the-loop design (AI suggests, clinician decides); involve clinicians in UX co-design during Phase 1; publish clinical validation study results; provide "explainability" — each alert shows its evidence source. |
| R4 | **Data privacy breach:** A security vulnerability exposes patient health data, damaging trust and triggering PDPA penalties. | Low | Critical | Penetration testing before each release; encrypted at rest and in transit from Day 1; third-party security audit; bug bounty program post-launch; cyber insurance coverage. |
| R5 | **IP licensing terms unfavourable:** UM licensing terms (royalty rate, exclusivity scope, equity requirements) make the unit economics unviable or restrict commercial flexibility. | Low | High | Negotiate licensing terms during Phase 1 before significant development investment; model financial scenarios under different royalty rates (3%, 5%, 8%); explore grant-based commercialisation pathways that cap royalty obligations. |
| R6 | **Competitor response:** An established EMR vendor or well-funded health-tech startup launches a similar AI supplement module, eroding first-mover advantage. | Medium | Medium | Accelerate patent-protected feature development; focus on building the expert-labelled Malaysian supplement database as a data moat; secure reference customers and clinical validation publications as credibility moats. |
| R7 | **Seed funding gap:** Seed funding (D7) is delayed or not secured, leaving the team without the RM 1.5M–2.5M budget assumed for the funded development path. | Medium | High | Execute the 3-path bootstrap contingency: (1) apply to 5 Malaysian grants simultaneously by Month 2, (2) secure 2 partner pre-commitments by Month 6, (3) leverage UM infrastructure and startup cloud credits to operate at RM 8–12K/month burn. The bootstrap path reaches the same product milestones 6–12 months later with zero dilution. |

### Dependencies
| # | Dependency | Owner | Required By | Status |
|---|---|---|---|---|
| D1 | UM IP licensing agreement signed (both IPs: UI 2025002953 and PI 2025007955) | UMCIE / Founding Team | Month 3 (pre-development) | Not yet initiated |
| D2 | Knowledge transfer from IP inventors (Dr. Lee Ching Shya, Dr. Nurul Fauzani Jamaluddin) — access to research codebase, expert-labelled training dataset, and algorithm documentation | UM Inventors | Month 3 (development start) | Contact established |
| D3 | PDPA compliance and data protection officer appointment | Founding Team / Legal | Month 18 (commercial launch) | Not yet initiated |
| D4 | Pilot clinic agreements (UM Medical Centre + 3–5 private clinics) for Phase 1 validation | Business Development | Month 6 (pilot start) | Not yet initiated |
| D5 | Cloud infrastructure provisioning (AWS Malaysia Region or equivalent) with healthcare-appropriate security configuration | Engineering | Month 3 (development environment) | Not yet initiated |
| D6 | Healthcare cybersecurity penetration test by MY-certs or equivalent accredited firm | Third-Party Vendor | Month 12 (pre-pilot security review) | Not yet initiated |
| D7 | Seed/commercialisation funding (RM 1.5M–2.5M) secured | Founding Team / Investors | Month 3 (to fund development and validation) | Not yet secured |
| **D8** | **Bootstrap contingency — at least 1 grant secured (UMCIE, MDEC GAH, or CREST) as bridge funding if D7 fails** | **Founding Team** | **Month 3 (to begin lean development)** | **Not yet initiated** |
| **D9** | **Bootstrap contingency — at least 1 partner pre-commitment (clinic group, telemedicine platform, or supplement manufacturer) funding RM 30–60K in development costs** | **Business Development** | **Month 6 (to sustain operations if D7 fails)** | **Not yet initiated** |
| **D10** | **Startup cloud programme activation (AWS Activate, Azure for Startups, or GCP for Startups) for RM 50–100K in cloud credits** | **Engineering** | **Month 1 (to eliminate Year 1 infrastructure costs)** | **Not yet initiated** |

---

## 9. Go-to-Market & Commercialisation Roadmap

Healynx follows a 4-phase GTM roadmap from foundation through ASEAN scale. Two parallel paths are presented: a **seed-funded path** (RM 1.5M–2.5M, 5–8 person team) and a **bootstrap path** (grants + partner pre-commitments, 2–4 person team). Both paths deliver the same product; the bootstrap path runs 6–12 months behind the funded path with reduced commercial capacity.

### 9.1 Dual-Path Overview

| Dimension | Funded Path | Bootstrap Path |
|---|---|---|
| **Team Size (Year 1)** | 5–8 full-time | 2 founders + 1–2 RAs |
| **First Revenue** | Month 15–18 | Month 18–24 |
| **Break-Even** | Month 30–36 | Month 36–42 |
| **Year 3 ARR** | RM 780K | RM 450K |
| **Year 5 ARR** | RM 3.5M | RM 1.8M |
| **Dilution** | 15–25% | Zero |
| **Primary Risk** | Investor dependency | Founder burnout; slower execution |

---

### 9.2 Phase 1 — Foundation & Validation (Funded: Months 0–9 | Bootstrap: Months 0–12)

**Objective:** TRL 5→6, working MVP, pilot at UM Medical Centre. Core product features built and validated.

#### Product Delivery

| Feature | Source IP | Priority | Funded | Bootstrap |
|---|---|---|---|---|
| Structured supplement intake survey (FR-01) | IP 2 | Must Have | Month 4–6 | Month 5–8 |
| AI supplement interaction detection + risk scoring (FR-02, FR-03) | IP 2 | Must Have | Month 6–8 | Month 7–9 |
| Real-time personalised recommendations (FR-04) | IP 2 | Must Have | Month 6–8 | Month 7–9 |
| Interaction alerts → clinician dashboard (FR-05) | IP 2 | Must Have | Month 7–8 | Month 8–9 |
| Patient wellness score (FR-06) | IP 2 | Must Have | Month 8–9 | Deferred to v1.1 |
| Encrypted cloud upload of medical documents (FR-07) | IP 1 | Must Have | Month 5–7 | Month 6–8 |
| AI-generated medical record summaries (FR-09) | IP 1 | Must Have | Month 7–9 | Month 8–10 |
| Role-based access control (FR-10) | IP 1 | Must Have | Month 5–7 | Month 6–8 |
| Emergency access override with audit trail (FR-13) | IP 1 | Must Have | Month 8–9 | Month 9–10 |
| Patient mobile/web dashboard (FR-14) | IP 1 + IP 2 | Must Have | Month 7–9 | Month 8–10 |
| Clinician dashboard with patient list, AI summaries, alerts (FR-18, FR-20) | IP 1 + IP 2 | Must Have | Month 7–9 | Month 8–10 |
| Audit logs for access and sharing events (FR-21) | IP 1 | Must Have | Month 6–8 | Month 7–9 |
| All NFRs (NFR-01 through NFR-08) | Both IPs | Must Have | Month 4–9 | Month 5–10 |

**Deferred to next phase (Bootstrap only):** Patient wellness score (FR-06), AI chatbot (FR-17), medication reminders (FR-15), health trend visualisation (FR-16).

#### Business Milestones

| Milestone | Funded | Bootstrap | Dependency |
|---|---|---|---|
| UM IP licensing signed (both IPs) | Month 2–3 | Month 2–3 | UMCIE negotiation |
| Knowledge transfer from UM inventors | Month 3 | Month 3 | Dr Lee, Dr Nurul Fauzani |
| Cloud infrastructure provisioned (AWS MY Region) | Month 3 | Month 1–3 | AWS Activate / Azure for Startups |
| Seed funding closed | Month 3–4 | N/A | Investor(s) |
| Grant applications submitted (all 5 programmes) | Month 2 | Month 1–2 | CREST, MDEC, UMCIE, MIDA, MTDC |
| First grant received | N/A | Month 3–6 | RM 75–200K from UMCIE + MDEC |
| Partner pre-commitments secured | Month 4–6 | Month 3–6 | RM 80–150K from clinic/telemedicine partners |
| Pilot deployed at UM Medical Centre | Month 8–9 | Month 9–12 | UM ethics approval |
| AI accuracy benchmarked (≥85% sensitivity) | Month 9 | Month 12 | 200+ test cases |
| Security penetration test passed | Month 9 | Month 12 | MY-certs accredited firm |

**Phase 1 Exit Gate (Funded):** 5 pilot clinics, 30 clinicians, 500 patients, AI sensitivity ≥85%, 0 critical security findings.

**Phase 1 Exit Gate (Bootstrap):** 1 pilot site (UMMC), 15 clinicians, 200 patients, AI sensitivity ≥85%, at least 1 grant renewal secured.

---

### 9.3 Phase 2 — Partnership & Regulatory Readiness (Funded: Months 9–18 | Bootstrap: Months 12–24)

**Objective:** Commercial partnerships, PDPA compliance, secure sharing features, first revenue. Target TRL 6→7.

#### Product Delivery

| Feature | Source IP | Priority | Funded | Bootstrap |
|---|---|---|---|---|
| Time-limited secure record sharing via links (FR-11) | IP 1 | Must Have | Month 14–16 | Month 20–22 |
| Secure viewer with anti-copy restrictions (FR-12) | IP 1 | Must Have | Month 14–16 | Month 20–22 |
| OCR digitisation of scanned documents (FR-08) | IP 1 | Must Have | Month 15–18 | Month 22–24 |
| FHIR R4 API endpoints for EMR/EHR integration | IP 1 | Should Have | Month 16–18 | Month 24 |
| Telemedicine platform API integration (US-09) | IP 1 + IP 2 | Should Have | Month 15–17 | Month 20–22 |
| Smart clinician chatbot for patient record querying (FR-19) | IP 1 + IP 2 | Should Have | Month 14–16 | Month 22–24 |
| Multi-language UI framework (Bahasa Malaysia) | Both IPs | Should Have | Month 16–18 | Month 22–24 |
| Patient wellness score (FR-06) | IP 2 | Must Have | Already in v1 | Month 12–14 |
| AI chatbot for patients (FR-17) | IP 1 + IP 2 | Should Have | Month 14–16 | Month 14–16 |
| Medication and supplement reminders (FR-15) | IP 1 + IP 2 | Could Have | Month 16–18 | Month 14–16 |
| Health trend visualisation (FR-16) | IP 1 + IP 2 | Could Have | Month 16–18 | Month 14–16 |

#### Business Milestones

| Milestone | Funded | Bootstrap | Dependency |
|---|---|---|---|
| 3–5 private clinic pilots active | Month 12 | Month 15 | Partner pre-commitments |
| PDPA compliance programme initiated (DPO appointed) | Month 9 | Month 12 | Legal / DPO |
| MDA regulatory pathway determined | Month 12 | Month 15 | SaMD classification confirmation |
| AI model v2 (≥90% sensitivity) | Month 14 | Month 16 | Expanded training data from pilots |
| ISO 27001 alignment audit | Month 18 | Month 24 | Third-party auditor |
| B2B sales capability established | Month 15 | Month 20 | 1–2 sales hires |
| First paying accounts | Month 15–18 | Month 18–24 | 6–12 month B2B sales cycle |
| 2 telemedicine integrations live | Month 18 | Month 24 | API contracts signed |

**Phase 2 Exit Gate (Funded):** 15 paying clinic accounts, 2 telemedicine integrations, OCR ≥90%, RM 5K MRR, secure sharing passes penetration test.

**Phase 2 Exit Gate (Bootstrap):** 8–12 paying accounts, RM 5–8K MRR, OCR ≥90%, first telemedicine integration, 3 pilot sites converted to paid.

---

### 9.4 Phase 3 — Commercial Launch & Revenue Scale (Funded: Months 15–30 | Bootstrap: Months 18–36)

**Objective:** Revenue generation, market penetration, break-even approach. Target TRL 7→8.

#### Product Delivery

| Feature | Source IP | Priority | Funded | Bootstrap |
|---|---|---|---|---|
| Insurance wellness scoring API and integration | IP 2 | Should Have | Month 20–24 | Month 28–32 |
| Wearable device data ingestion (Apple Health, Google Fit) | Both IPs | Could Have | Month 22–26 | Month 30–34 |
| ASEAN multi-language localisation (Bahasa Indonesia, Thai, Vietnamese) | Both IPs | Should Have | Month 22–28 | Month 32–36 |
| Multi-region cloud deployment (Singapore region) | Both IPs | Should Have | Month 22–24 | Month 30–32 |
| Corporate wellness aggregate dashboard | IP 1 + IP 2 | Could Have | Month 20–24 | Month 28–32 |
| ASEAN supplement product database expansion | IP 2 | Should Have | Month 22–28 | Month 30–36 |

#### Business Milestones

| Milestone | Funded | Bootstrap |
|---|---|---|
| B2B sales team active | Month 15 (3–5 people) | Month 24 (1–2 people) |
| 20 paying accounts | Month 18 | Month 24 |
| Conference marketing begins | Month 18 | Month 24 |
| Corporate wellness pilot (1–2 employers) | Month 20 | Month 28 |
| First ASEAN deployment (Singapore) | Month 24–30 | Month 32–36 |
| 40+ paying accounts, RM 32K MRR | Month 24 | Month 30 |
| 98 paying accounts | Month 36 | Month 36–42 |
| **Break-Even** | **Month 30–36** | **Month 36–42** |

---

### 9.5 Phase 4 — ASEAN Expansion (Years 3–5)

**Objective:** Multi-country, multi-language regional scale. 200–380 paying accounts, RM 1.8M–3.5M ARR.

| Wave | Country | Funded Entry | Bootstrap Entry | Strategy |
|---|---|---|---|---|
| Wave 1 | Singapore | Year 2–3 | Year 3–4 | Local subsidiary; HSA regulatory; telemedicine partners |
| Wave 2 | Indonesia | Year 3–4 | Year 4–5 | JV with Indonesian healthcare group; Bahasa Indonesia; Jamu database |
| Wave 3 | Thailand | Year 3–4 | Year 5 | Private hospital partnerships; FDA Thailand |
| Wave 4 | Vietnam | Year 4–5 | Year 5+ | Health-tech startup partners |
| Wave 5 | Philippines | Year 4–5 | Year 5+ | Direct English deployment; PhilHealth integration |

---

### 9.6 5-Year Financial Projection

| Metric | Year 1 | Year 2 | Year 3 | Year 4 | Year 5 |
|---|---|---|---|---|---|
| **Paying Accounts (Funded)** | 0 | 20 | 98 | 200 | 380 |
| **Paying Accounts (Bootstrap)** | 0 | 8 | 40 | 90 | 180 |
| **ARR Funded (RM)** | 0 | 150K | 780K | 1.8M | 3.5M |
| **ARR Bootstrap (RM)** | 0 | 50K | 400K | 900K | 1.8M |
| **Gross Margin** | — | 60% | 65% | 68% | 72% |
| **Net Margin (Funded)** | Negative | Negative | ~5% | ~15% | ~22% |
| **Cash-Flow Positive (Funded)** | No | No | Month 30–36 | Yes | Yes |
| **Cash-Flow Positive (Bootstrap)** | No | No | Month 36–42 | Yes | Yes |

---

### 9.7 Critical Path Dependencies

| # | Dependency | Required By | Status |
|---|---|---|---|
| D1 | UM IP licensing signed (both IPs: UI 2025002953, PI 2025007955) | Month 3 | Not yet initiated |
| D2 | Knowledge transfer from IP inventors (Dr. Lee, Dr. Nurul Fauzani) | Month 3 | Contact established |
| D3 | PDPA compliance + Data Protection Officer appointment | Month 9 (Funded) / Month 12 (Bootstrap) | Not yet initiated |
| D4 | Pilot clinic agreements (UMMC + 3–5 private clinics) | Month 6 (Funded) / Month 9 (Bootstrap) | Not yet initiated |
| D5 | Cloud infrastructure (AWS Malaysia Region or equivalent) | Month 3 | Not yet initiated |
| D6 | Healthcare cybersecurity penetration test (MY-certs) | Month 12 | Not yet initiated |
| D7 | Seed/commercialisation funding secured (Funded Path) | Month 3 | Not yet secured |
| D8 | Bootstrap contingency: ≥1 grant secured (UMCIE, MDEC GAH, CREST) | Month 3–6 | Not yet initiated |
| D9 | Bootstrap contingency: ≥1 partner pre-commitment (RM 30–60K) | Month 6 | Not yet initiated |
| D10 | Startup cloud programme activation (RM 50–100K credits) | Month 1 | Not yet initiated |

---

### 9.8 Top Risks to Roadmap

| # | Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|---|
| R1 | **Regulatory delay** — PDPA/MDA takes 12+ months longer than planned | Medium | High | Engage healthcare regulatory consultant Month 1; design compliance-ready from Day 1 |
| R2 | **AI underperforms in diverse population** — <85% sensitivity on real Malaysian patients | Medium | High | Expand training dataset during Phase 1 validation; continuous learning pipeline with clinician feedback |
| R3 | **Clinician adoption failure** — doctors ignore or distrust AI alerts | Medium | Medium | Human-in-the-loop design; clinicians in UX co-design; publish validation study; show evidence sources per alert |
| R4 | **Seed funding gap** — D7 delayed or not secured | Medium | High | Bootstrap contingency: grants + partner pre-commitments execute same product, 6–12 months slower |
| R5 | **IP licensing terms unfavourable** — royalty/equity terms make unit economics unviable | Low | High | Negotiate during Phase 1 before significant investment; model at 3/5/8% royalty scenarios; explore grant-based pathways |

---

## Appendix A: IP Application Traceability Matrix

Every feature is traceable to a specific UM IP to demonstrate meaningful IP application.

| Feature | FR ID(s) | UM IP | IP Contribution |
|---|---|---|---|
| Supplement intake survey | FR-01 | IP 2 (UI 2025002953) | Core survey instrument method |
| AI interaction detection | FR-02, FR-05 | IP 2 (UI 2025002953) | Dual-engine AI + statistical model |
| Statistical risk scoring | FR-03 | IP 2 (UI 2025002953) | Scoring model evaluating patient variables |
| Personalised recommendations | FR-04 | IP 2 (UI 2025002953) | Real-time personalised output |
| Wellness scoring | FR-06 | IP 2 (UI 2025002953) | Derived from supplement + health profile analysis |
| Encrypted cloud storage | FR-07 | IP 1 (PI 2025007955) | Core secure storage method |
| OCR digitisation | FR-08 | IP 1 (PI 2025007955) | Document digitisation pipeline |
| AI record summarisation | FR-09 | IP 1 (PI 2025007955) | AI-powered clinical summarisation |
| Role-based access control | FR-10 | IP 1 (PI 2025007955) | Granular permission framework |
| Secure record sharing | FR-11, FR-12 | IP 1 (PI 2025007955) | Time-limited, permission-controlled sharing + secure viewer |
| Emergency access override | FR-13 | IP 1 (PI 2025007955) | Emergency access mode with audit |
| Patient dashboard | FR-14 | IP 1 + IP 2 | Combined survey data + secure records |
| Medication reminders | FR-15 | IP 1 + IP 2 | Built on patient record + supplement profile |
| Health visualisation | FR-16 | IP 1 + IP 2 | Trends from supplement surveys + records |
| AI chatbot (patient) | FR-17 | IP 1 (PI 2025007955) | Chatbot interface for record navigation |
| Clinician dashboard | FR-18, FR-20 | IP 1 + IP 2 | AI summaries (IP 1) + alerts (IP 2) |
| Clinician chatbot | FR-19 | IP 1 (PI 2025007955) | Smart querying of patient records |
| Audit logs | FR-21 | IP 1 (PI 2025007955) | Core security infrastructure |

---

## Appendix B: Glossary

| Term | Definition |
|---|---|
| **IP** | Intellectual Property — a patent or proprietary method owned by Universiti Malaya. |
| **TRL** | Technology Readiness Level — a scale from 1 (basic principles observed) to 9 (proven in operational environment). |
| **MR** | Market Readiness — an assessment of commercial readiness; scale 1–9. |
| **RBAC** | Role-Based Access Control — a method of restricting system access to authorised users based on their role. |
| **PDPA** | Personal Data Protection Act 2010 — Malaysia's primary data protection legislation. |
| **FHIR** | Fast Healthcare Interoperability Resources — an HL7 standard for electronic health information exchange. |
| **EMR / EHR** | Electronic Medical Records / Electronic Health Records — digital versions of a patient's medical history. |
| **SaaS** | Software as a Service — a software licensing and delivery model where software is hosted centrally and accessed via the internet. |
| **SLA** | Service Level Agreement — a commitment between a service provider and a client regarding service quality, availability, and responsibilities. |
| **OCR** | Optical Character Recognition — technology that converts images of typed or printed text into machine-encoded text. |
| **KMS** | Key Management Service — a cloud service for creating and managing cryptographic keys. |
| **MoSCoW** | A prioritisation framework: Must have, Should have, Could have, Won't have (for this release). |

---

*This PRD is the single source of truth for Healynx v1 scope. All feature decisions, design work, development sprints, and QA test plans shall reference requirements by their FR-XX or NFR-XX identifiers. Any deviation from this document requires a formal change request reviewed by the product manager.*
