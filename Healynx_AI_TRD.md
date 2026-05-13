# Healynx — Technical Requirement Document (TRD)

**Document Version:** 1.0  
**Date:** 13 May 2026  
**Author:** Solutions Architecture  
**Classification:** Confidential — For Engineering & DevOps Handoff  
**Reference PRD:** Healynx_AI_PRD.md v1.0  
**Source IPs:**
| IP | Patent No. | TRL | Core Technical Capability |
|---|---|---|---|
| Interactive Patient Supplement Intake & Disclosure Practice Assessment Method | UI 2025002953 | 3 | Dual-engine AI algorithm + statistical scoring model for supplement risk/benefit analysis |
| Computer-Implemented Method for Secure Management of Medical Records in an AI-Enhanced System | PI 2025007955 | 5 | Encrypted cloud storage, RBAC, AI summarisation, secure sharing, emergency access, audit trail |

---

## 1. Technology Stack

### 1.1 Technology Selection Overview

All decisions are driven by five architectural principles derived from the two UM IPs:

1. **AI-readiness** — the stack must natively support model serving, NLP pipelines, and real-time inference (IP 2's dual-engine architecture + IP 1's AI summarisation)
2. **Security-first** — encryption, RBAC, and audit must be baked into every layer (IP 1's core method)
3. **API-first** — all features must be accessible programmatically for telemedicine and EMR integration (IP 1's platform integration capability)
4. **Cloud-native scalability** — multi-tenant SaaS from Day 1 with ASEAN expansion path (PRD scalability constraint)
5. **Malaysia data residency** — all patient data within Malaysian cloud regions (PDPA compliance)

### 1.2 Layer-by-Layer Stack

#### Frontend

| Component | Technology | Version | Rationale |
|---|---|---|---|
| **Framework** | React 18+ with Next.js 14+ (App Router) | 18.x / 14.x | Server-side rendering for SEO-optional pages (landing), client-side rendering for dashboards; React's component model maps cleanly to the patient/clinician/administrator role-based views; Next.js API routes serve as lightweight BFF (Backend-for-Frontend) without requiring a separate server. |
| **State Management** | React Query (TanStack Query) + Zustand | TanStack Query 5.x, Zustand 4.x | React Query handles server-state caching, background refetch, and optimistic updates for survey submissions and record loading. Zustand manages client-only UI state (sidebar collapsed, active tab, secure viewer session) — minimal boilerplate, no Redux overhead. |
| **Responsive Strategy** | Tailwind CSS + Radix UI primitives | Tailwind 3.x, Radix latest | Mobile-first utility CSS ensures 320px–1920px responsiveness per NFR-06. Radix UI provides accessible, unstyled primitives for modals, dropdowns, and accordions that meet WCAG 2.1 AA. No heavy component library — keeps bundle under 200KB gzipped. |
| **Charts** | Recharts | 2.x | Lightweight React charting for health trend visualisations (FR-16) — wellness score trend lines, supplement count bar charts, alert count timelines. SVG-based, no DOM-manipulation libraries. |
| **PWA** | Workbox (via next-pwa) | 6.x | Offline survey capability for clinics with intermittent connectivity; service worker caches survey instrument and previously viewed records. |
| **i18n** | next-intl | 3.x | ICU message format; prepared for English + Bahasa Malaysia in v2 with no re-architecture. |

#### Backend

| Component | Technology | Version | Rationale |
|---|---|---|---|
| **Language** | Python 3.12 | 3.12 | De facto standard for AI/ML workloads. Both UM IPs have Python-based research codebases. Single language across API and AI services reduces context-switching and enables shared type definitions. |
| **API Framework** | FastAPI | 0.111+ | Native async support, automatic OpenAPI 3.0 generation (NFR-07), Pydantic v2 for request/response validation with JSON Schema, built-in dependency injection for auth and DB sessions. Performance benchmarks comparable to Node.js Express for I/O-bound healthcare workloads. |
| **API Architecture** | REST (primary); GraphQL (future) | — | REST chosen for v1 — simpler caching semantics for survey results and record retrieval, easier client integration for telemedicine partners, well-understood security model (per-endpoint authz). GraphQL deferred to v2/v3 when telemedicine partners need flexible field selection in patient queries. |
| **Async Task Queue** | Celery with Redis broker | Celery 5.x, Redis 7.x | Decouples long-running AI inference (supplement analysis, record summarisation, OCR) from the HTTP request/response cycle. Enables the 3-second SLA (NFR-05) — the API returns a 202 Accepted with a task ID, the client polls or receives a webhook callback on completion. |
| **API Gateway** | Kong (self-hosted) or AWS API Gateway | Kong 3.x or AWS API Gateway v2 | Rate limiting per API key tenant, request/response logging, TLS termination, authentication plugin (OAuth 2.0 / JWT validation). Kong preferred for multi-cloud portability; AWS API Gateway acceptable if AWS is the chosen cloud provider. |
| **WebSocket** | FastAPI WebSocket (Starlette) | — | Real-time push of interaction alerts to clinician dashboard (FR-05 — alert within 10 seconds). Falls back to polling if WebSocket blocked by clinic firewall. |
| **FHIR Server** | HAPI FHIR (JVM sidecar) or custom FastAPI FHIR module | HAPI 7.x or custom | v2 feature. If HAPI FHIR, deployed as a sidecar container translating between Healynx's internal data model and FHIR R4 resources. If custom, FastAPI endpoints implementing FHIR Patient, Observation, Condition, DocumentReference resources. |

#### Database

| Component | Technology | Version | Rationale |
|---|---|---|---|
| **Primary DB** | PostgreSQL 16 | 16.x | ACID compliance for medical records (data integrity non-negotiable), JSONB for flexible survey response storage (FR-01), full-text search via `tsvector` for clinician patient search (FR-18), row-level security for multi-tenancy, mature encryption options (pgcrypto). |
| **Caching Layer** | Redis 7 | 7.x | Session store, Celery broker, and application cache. Caches AI-generated patient summaries (FR-09) with 5-minute TTL to avoid recomputation; caches RBAC permission lookups per session; stores rate-limit counters. |
| **Search Engine** | PostgreSQL `tsvector` (v1); Elasticsearch (v2 deferred) | — | PostgreSQL full-text search suffices for v1's 500–5,000 patient panel per clinician. Elasticsearch introduced in v2 when multi-clinic federated search and natural-language record querying (FR-19 clinician chatbot) require relevance-ranked results across millions of documents. |
| **Object Storage** | AWS S3 (Malaysia Region) or Azure Blob Storage (Malaysia West) | — | Encrypted medical document blobs (PDF, JPEG, PNG, TIFF — FR-07). Server-side encryption with KMS-managed keys. Lifecycle policies for archival after configurable retention period. Pre-signed URLs for secure viewer access (FR-12). |
| **Audit Log Store** | PostgreSQL (separate logical database in same cluster — v1); AWS CloudWatch Logs / Azure Monitor Logs (v2) | — | v1: separate PostgreSQL schema with `UNLOGGED` tables for write performance and continuous archival to object storage. v2: cloud-native immutable log store with WORM (write-once-read-many) semantics. |

#### AI/ML

| Component | Technology | Version | Rationale |
|---|---|---|---|
| **Model Serving** | BentoML or FastAPI with ONNX Runtime | BentoML 1.x / ONNX Runtime 1.18 | IP 2's AI algorithm and statistical scoring model are packaged as a BentoML service (standardised API, model versioning, adaptive batching). If the UM research codebase exports to ONNX, ONNX Runtime provides hardware-agnostic inference with lower cold-start. |
| **NLP Summarisation** | Fine-tuned BART or PEGASUS on UM clinical data | HuggingFace Transformers 4.x | IP 1's AI-powered record summarisation (FR-09). BART (Bidirectional Auto-Regressive Transformer) is the established baseline for abstractive summarisation of clinical text. Fine-tuned on the UM expert-labelled dataset (Dependency D2). Deployed behind the Celery async pipeline with GPU-accelerated inference. |
| **OCR Engine** | Tesseract 5 with LSTM models + AWS Textract (fallback) | Tesseract 5.x | IP 1's OCR digitisation (FR-08, v2). Tesseract for on-premise/offline scenarios; AWS Textract for cloud-based high-accuracy extraction of structured fields (diagnosis, medications, lab values) from complex clinical forms. Tesseract models fine-tuned on Malaysian clinical document layouts. |
| **LLM (Chatbot)** | Mistral 7B or Llama 3 8B (self-hosted) with RAG | via vLLM or llama.cpp | IP 1's chatbot interface (FR-17 patient, FR-19 clinician). Self-hosted open-weight model to maintain data sovereignty (no patient data sent to external LLM APIs). RAG (Retrieval-Augmented Generation) using patient records as context, with guardrails: (a) responses constrained to retrieved documents, (b) medical disclaimer injected, (c) PII filter on output. |
| **Vector Database** | pgvector (PostgreSQL extension) | 0.7.x | Embedding storage for RAG pipeline. Patient record chunks embedded via `sentence-transformers` (all-MiniLM-L6-v2 for English, paraphrase-multilingual for Bahasa Malaysia). Avoids a separate vector DB dependency in v1. |
| **Interaction Knowledge Base** | Neo4j Community Edition | 5.x | IP 2's drug-supplement interaction detection (FR-02). Graph database modelling medications, supplements, conditions, and their interaction edges. Enables Cypher queries like "find all supplements interacting with metformin in hypertensive patients." UM expert-labelled data is imported as the initial graph. Statistical scoring model weights are stored as edge properties. |

#### Cloud

| Component | Technology | Rationale |
|---|---|---|
| **Primary Provider** | AWS (Asia Pacific — Malaysia Region) | First-mover with Malaysia Region (ap-southeast-6). Mature healthcare compliance tooling (AWS Artifact, HIPAA-eligible services). If Malaysia Region is not GA at launch, fallback to AWS Asia Pacific (Singapore) with contractual PDPA addendum for data residency waiver. |
| **Secondary Provider** | Azure (Malaysia West) | Contingency if Azure establishes Malaysia West before AWS. Azure Health Data Services provides FHIR-native integration. |
| **Container Runtime** | Docker with containerd | Industry standard. Images built multi-arch (amd64/arm64) for flexibility across cloud and on-premise edge deployments. |
| **Orchestration** | Kubernetes (AWS EKS or Azure AKS) | Multi-service deployment: API pods, AI inference pods (GPU node group), Celery workers, Redis, Neo4j. Horizontal Pod Autoscaler (HPA) scales API pods on CPU/memory; KEDA scales Celery workers on Redis queue depth. |
| **Serverless (supplementary)** | AWS Lambda / Azure Functions | Lightweight, stateless operations: PDF thumbnail generation, notification dispatch (email/SMS/push), webhook delivery to telemedicine platforms. Keeps the Kubernetes cluster focused on stateful and latency-sensitive workloads. |
| **CDN** | CloudFront (AWS) or Azure CDN | Static assets (React bundle, survey instrument JSON, health education content). Edge caching at Singapore POP for sub-100ms latency to Klang Valley. |
| **DNS** | Route 53 (AWS) or Azure DNS | Health-check-based failover between primary and DR regions. |

#### Security

| Component | Technology | Rationale |
|---|---|---|
| **Encryption Library** | AWS KMS / Azure Key Vault + python-cryptography (Fernet) | AES-256-GCM for record blob encryption (FR-07). Envelope encryption: per-record data encryption key (DEK) generated via Fernet, DEK encrypted with KMS-managed customer master key (CMK). Automatic key rotation per NFR-01. |
| **Authentication** | OAuth 2.0 + OpenID Connect (OIDC) via Keycloak | Self-hosted Keycloak for tenant-local identity. Supports: (a) username/password with TOTP MFA, (b) social login deferred to v2, (c) SAML for hospital SSO integration (v2). JWT access tokens (15-min expiry) + refresh tokens (7-day expiry with rotation). |
| **Authorisation** | Open Policy Agent (OPA) + FastAPI dependency | RBAC policy as Rego code: `allow { input.role == "doctor"; input.action == "read"; input.patient_id == input.assigned_patients[_] }`. Policies version-controlled alongside application code. FastAPI dependency `Depends(require_role("doctor"))` enforces at endpoint level. |
| **Web Application Firewall** | AWS WAF / Azure WAF v2 | OWASP Top 10 rule set. Rate-based rules for brute-force and credential-stuffing. Geo-blocking to Malaysian IP ranges during pilot phase; expanded as ASEAN regions go live. |
| **Secret Management** | AWS Secrets Manager / Azure Key Vault | Database credentials, API keys, KMS key ARNs, third-party integration secrets. No secrets in environment variables, code, or config files. |

---

## 2. System Architecture

### 2.1 High-Level Architecture (Text Diagram)

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                              CLIENT LAYER                                     │
│  ┌──────────────────┐  ┌──────────────────┐  ┌──────────────────────────┐   │
│  │ Patient Web/     │  │ Clinician Web    │  │ Telemedicine Platform    │   │
│  │ Mobile (React)   │  │ Dashboard (React)│  │ (REST API Consumer)      │   │
│  └────────┬─────────┘  └────────┬─────────┘  └────────────┬─────────────┘   │
│           │                     │                          │                  │
│           └─────────────────────┼──────────────────────────┘                  │
│                                 │ TLS 1.3                                     │
│                                 ▼                                             │
├─────────────────────────────────────────────────────────────────────────────┤
│                            API GATEWAY LAYER                                  │
│  ┌───────────────────────────────────────────────────────────────────────┐   │
│  │  Kong / AWS API Gateway                                               │   │
│  │  • Rate Limiting  • JWT Validation  • TLS Termination                 │   │
│  │  • Request Logging  • API Key Management  • CORS                      │   │
│  └──────────────────────────────┬────────────────────────────────────────┘   │
│                                 │                                             │
├─────────────────────────────────┼─────────────────────────────────────────────┤
│                          SERVICE LAYER                                        │
│                                 │                                             │
│     ┌───────────────────────────┼───────────────────────────────────┐        │
│     │                    FastAPI Application                         │        │
│     │                                                                │        │
│     │  ┌─────────────────┐  ┌──────────────┐  ┌──────────────────┐  │        │
│     │  │ Auth Service     │  │ Supplement   │  │ Records Engine   │  │        │
│     │  │ (Keycloak + OPA) │  │ Engine       │  │ (Upload, Summary │  │        │
│     │  │                  │  │ (Survey, AI, │  │  Share, Access)  │  │        │
│     │  │ • Login/Logout   │  │  Scoring)    │  │                  │  │        │
│     │  │ • Token Refresh  │  │              │  │ • Document CRUD  │  │        │
│     │  │ • RBAC Enforce   │  │ • Survey API │  │ • AI Summary     │  │        │
│     │  │ • MFA            │  │ • AI Infer   │  │ • Secure Viewer  │  │        │
│     │  └─────────────────┘  │ • Scoring     │  │ • Emergency Acc  │  │        │
│     │                        └──────┬───────┘  └────────┬─────────┘  │        │
│     │                               │                    │            │        │
│     │  ┌─────────────────┐  ┌───────┴───────┐  ┌────────┴─────────┐  │        │
│     │  │ Notification    │  │ AI Pipeline   │  │ Audit Service    │  │        │
│     │  │ Service         │  │ (Celery)      │  │                  │  │        │
│     │  │                 │  │               │  │ • Event Capture  │  │        │
│     │  │ • Push/SMS/Email│  │ • Task Queue  │  │ • Log Query      │  │        │
│     │  │ • Reminders     │  │ • Workers     │  │ • Immutability   │  │        │
│     │  │ • Alert Dispatch│  │ • GPU Nodes   │  │ • Export         │  │        │
│     │  └─────────────────┘  └───────┬───────┘  └──────────────────┘  │        │
│     │                               │                                 │        │
│     └───────────────────────────────┼─────────────────────────────────┘        │
│                                     │                                          │
├─────────────────────────────────────┼──────────────────────────────────────────┤
│                              DATA LAYER                                        │
│                                     │                                          │
│     ┌───────────────────────────────┼───────────────────────────────────┐      │
│     │                               │                                   │      │
│     │  ┌──────────┐  ┌──────────┐   │   ┌──────────┐  ┌──────────┐    │      │
│     │  │PostgreSQL│  │  Redis   │   │   │ Neo4j    │  │  AWS S3  │    │      │
│     │  │ (Primary)│  │  Cache / │   │   │ (Graph)  │  │ (Blobs)  │    │      │
│     │  │          │  │  Broker  │   │   │          │  │          │    │      │
│     │  │ • Patient│  │ • Session│   │   │ • Drug   │  │ • PDF    │    │      │
│     │  │ • Records│  │ • Cache  │   │   │ • Supp   │  │ • Images │    │      │
│     │  │ • Survey │  │ • Queue  │   │   │ • Inter. │  │ • Scans  │    │      │
│     │  │ • Audit  │  │ • Rate L.│   │   │ • Cond.  │  │ • OCR txt│    │      │
│     │  └──────────┘  └──────────┘   │   └──────────┘  └──────────┘    │      │
│     │                                │                                  │      │
│     └────────────────────────────────┴──────────────────────────────────┘      │
│                                                                                │
└────────────────────────────────────────────────────────────────────────────────┘
```

### 2.2 Service Decomposition

| Service | Responsibility | IP Traceability | Communication |
|---|---|---|---|
| **Auth Service** | Identity management, authentication (JWT issue/verify), MFA, token refresh, RBAC policy evaluation via OPA | IP 1 — RBAC method | Synchronous REST. Called by API Gateway on every request for token validation (cached in Redis for 5 min). |
| **Supplement Engine** | Survey instrument delivery, response ingestion, triggering AI analysis pipeline, serving results to patient and clinician dashboards | IP 2 — Core survey method + dual-engine architecture | Synchronous REST for survey CRUD. Async (Celery task) for AI analysis dispatch. Results notified via WebSocket to clinician dashboard. |
| **Records Engine** | Medical document upload/encrypt/store, record retrieval, AI summarisation triggering, share link generation, secure viewer rendering, emergency access activation | IP 1 — Core secure records method | Synchronous REST for CRUD and viewer. Async (Celery task) for OCR and summarisation. S3 pre-signed URLs for secure blob access. |
| **AI Pipeline** | Celery workers consuming tasks: supplement interaction analysis (IP 2), record summarisation (IP 1), OCR extraction (IP 1), chatbot RAG inference (IP 1). GPU node group for LLM inference. | IP 1 + IP 2 — all AI methods | Async via Redis message broker. Results written to PostgreSQL and object storage. Notification dispatched on completion. |
| **Notification Service** | Push notification (FCM/APNs), SMS (Twilio or local gateway), email (Amazon SES). Medication/supplement reminders (FR-15), interaction alert dispatch (FR-05), emergency access notification (FR-13), share link delivery. | IP 1 + IP 2 — alert and notification methods | Async. Consumed by Celery workers triggered by application events. |
| **Audit Service** | Capture, store, and query audit events (FR-21). Immutable append-only write. Searchable read interface for clinic administrators. | IP 1 — Audit logging method | Internal synchronous write (must not fail silently — audit loss is a compliance violation). Async read API for querying. |

### 2.3 Communication Patterns

| Pattern | Use Case | Rationale |
|---|---|---|
| **Synchronous REST** | User authentication, survey submission, record retrieval, RBAC checks, audit log queries, dashboard data loading | Immediate request-response needed. Latency SLA ≤500ms for CRUD, ≤3s for AI results retrieval (cached). |
| **Async (Celery + Redis)** | AI supplement analysis (FR-02/03/04), AI record summarisation (FR-09), OCR digitisation (FR-08), panel-wide alert scanning (FR-20), notification dispatch | Long-running (5–30s) or batch operations. Decouples from HTTP request cycle to meet SLA. Client receives task ID → polls GET /tasks/{id} for status → retrieves result when complete. |
| **WebSocket** | Real-time clinician dashboard alerts (FR-05), patient chatbot streaming responses (FR-17) | Push-based updates eliminate polling overhead. Fallback to 10-second polling if WebSocket blocked by clinic network. |
| **Event-Driven (Future — v2)** | Cross-service events: "PatientRecordUpdated" → triggers re-summarisation; "HighSeverityAlertGenerated" → triggers notification dispatch | v1 uses direct Celery task chaining. v2 introduces SNS/SQS or Redis Streams for looser coupling as service count grows. |

### 2.4 Multi-Tenancy Model

**Chosen approach: Shared database, row-level security (RLS) + tenant_id column.**

| Approach Considered | Verdict | Reasoning |
|---|---|---|
| **Database-per-tenant** | Rejected for v1 | Operational overhead of managing hundreds of databases for clinics. Connection pooling complexity. Cross-tenant data analytics impossible without ETL. |
| **Schema-per-tenant** | Rejected | PostgreSQL schema-per-tenant provides isolation but complicates migrations (must run per schema), connection pooling, and cross-tenant queries for telemedicine platforms serving multiple clinics. |
| **Shared DB with RLS** | **Selected** | Every table carrying patient data has a `tenant_id` column. PostgreSQL RLS policy: `USING (tenant_id = current_setting('app.current_tenant_id'))`. Application sets `app.current_tenant_id` at session start after JWT validation. Isolation verified via integration tests that deliberately attempt cross-tenant access. |

**Tenant isolation enforcement:**
- `tenant_id` is a UUID generated at clinic/organisation registration
- API Gateway extracts tenant from JWT claim `tenant_id` and injects into `X-Tenant-ID` header
- FastAPI middleware sets PostgreSQL runtime parameter before each request
- RLS policy enforced at database level — even if application logic fails, Postgres blocks cross-tenant reads
- Audit Service logs all tenant-context switches for compliance

---

## 3. Data Model

### 3.1 Entity-Relationship Overview

```
┌──────────┐       ┌────────────────┐       ┌─────────────────────┐
│  Tenant  │1────*│     User       │1────*│   PatientProfile     │
│ (Clinic) │       │ (id, role,     │       │ (health_profile,     │
│          │       │  tenant_id)    │       │  wellness_score)     │
└──────────┘       └───────┬────────┘       └──────────┬──────────┘
                           │                            │
                           │ 1                          │ 1
                           │                            │
                           ▼                            ▼
                  ┌────────────────┐          ┌─────────────────────┐
                  │   Permission   │          │ SupplementAssessment │
                  │ (role, action, │          │ (survey_json,        │
                  │  resource)     │          │  ai_recommendation,  │
                  └────────────────┘          │  risk_score)         │
                                              └──────────┬──────────┘
                                                         │ *
                                                         │
                                              ┌──────────▼──────────┐
                                              │ SupplementInteraction│
                                              │ (drug, supplement,   │
                                              │  severity, evidence) │
                                              └─────────────────────┘

┌────────────────┐       ┌────────────────┐       ┌─────────────────────┐
│ MedicalRecord  │*────1│    Patient     │1────*│    AccessGrant       │
│ (type, blob_ref,│       │                │       │ (grantor, grantee,   │
│  ocr_text,      │       │                │       │  permissions, expiry)│
│  ai_summary)    │       │                │       └──────────┬──────────┘
└────────┬───────┘       └────────────────┘                  │
         │ *                                                  │ *
         │                                                    │
┌────────▼───────┐                                   ┌───────▼─────────────┐
│ RecordVersion  │                                   │     AccessLog       │
│ (version_no,   │                                   │ (grant_id, accessor, │
│  change_diff,  │                                   │  timestamp, outcome) │
│  modified_by)  │                                   └─────────────────────┘
└────────────────┘

┌───────────────────────────────────────────────────────────────────┐
│                          AuditEntry                                │
│ (actor_id, action, target_type, target_id, details_json,           │
│  ip_address, user_agent, outcome, tenant_id)                       │
└───────────────────────────────────────────────────────────────────┘
                          │
                          │ * (references all above entities as targets)
                          │
┌────────────────┐       ┌─────────────────────┐
│  Reminder      │       │   Alert             │
│ (patient_id,   │       │ (patient_id,         │
│  type, time,   │       │  severity, message,  │
│  recurrence)   │       │  acknowledged_by,    │
└────────────────┘       │  acknowledged_at)    │
                         └─────────────────────┘
```

### 3.2 Core Entities

#### Tenant
```
Tenant {
    id:              UUID (PK)
    name:            VARCHAR(255)           — Clinic/organisation name
    type:            ENUM('clinic','hospital','telemedicine','corporate','research')
    contact_email:   VARCHAR(255)
    subscription_tier: ENUM('essentials','professional','enterprise','api_starter','api_growth')
    is_active:       BOOLEAN
    created_at:      TIMESTAMPTZ
}
```

#### User
```
User {
    id:              UUID (PK)
    tenant_id:       UUID (FK → Tenant.id)
    email:           VARCHAR(255) UNIQUE
    password_hash:   VARCHAR(255)
    role:            ENUM('patient','doctor','clinic_admin','emergency_access','super_admin')
    mfa_enabled:     BOOLEAN DEFAULT false
    mfa_secret:      VARCHAR(64) NULL
    is_active:       BOOLEAN DEFAULT true
    last_login_at:   TIMESTAMPTZ
    created_at:      TIMESTAMPTZ
}
```

#### PatientProfile
```
PatientProfile {
    id:              UUID (PK)
    user_id:         UUID (FK → User.id, UNIQUE)
    tenant_id:       UUID (FK → Tenant.id)
    full_name:       VARCHAR(255)
    ic_number:       VARCHAR(20) ENCRYPTED      — Malaysian NRIC, encrypted at column level
    date_of_birth:   DATE
    sex:             ENUM('male','female','other')
    blood_type:      ENUM('A+','A-','B+','B-','AB+','AB-','O+','O-','unknown')
    health_profile:  JSONB                      — { conditions: [...], medications: [...], allergies: [...], ... }
    wellness_score:  INTEGER CHECK (0 <= wellness_score <= 100)
    wellness_score_updated_at: TIMESTAMPTZ
    survey_updated_at: TIMESTAMPTZ              — last supplement survey submission
    primary_doctor_id: UUID (FK → User.id)
    created_at:      TIMESTAMPTZ
    updated_at:      TIMESTAMPTZ

    INDEX: idx_patient_tenant (tenant_id)
    INDEX: idx_patient_doctor (primary_doctor_id)
}
```

#### MedicalRecord
```
MedicalRecord {
    id:              UUID (PK)
    patient_id:      UUID (FK → PatientProfile.id)
    tenant_id:       UUID (FK → Tenant.id)
    record_type:     ENUM('lab_report','prescription','discharge_summary','scan','clinical_note','vaccination','other')
    title:            VARCHAR(500)
    description:      TEXT
    blob_ref:         VARCHAR(1024)              — S3 object key, encrypted blob storage path
    blob_encryption_key_id: VARCHAR(255)         — KMS key ID used for envelope encryption
    blob_size_bytes:  BIGINT
    mime_type:        VARCHAR(100)
    ocr_text:         TEXT NULL                  — extracted via OCR pipeline (v2); NULL in v1
    ai_summary:       TEXT NULL                  — AI-generated summary (FR-09)
    ai_summary_confidence: FLOAT NULL CHECK (0 <= confidence <= 1)
    ai_summary_generated_at: TIMESTAMPTZ
    ai_summary_version: INTEGER DEFAULT 1
    uploaded_by:      UUID (FK → User.id)
    is_latest_version: BOOLEAN DEFAULT true
    created_at:       TIMESTAMPTZ
    updated_at:       TIMESTAMPTZ

    INDEX: idx_record_patient (patient_id)
    INDEX: idx_record_tenant (tenant_id)
    INDEX: idx_record_type (record_type)
}
```

#### RecordVersion
```
RecordVersion {
    id:              UUID (PK)
    record_id:       UUID (FK → MedicalRecord.id)
    version_number:  INTEGER
    blob_ref:        VARCHAR(1024)              — previous blob version
    change_diff:     JSONB                      — structured diff of field changes
    modified_by:     UUID (FK → User.id)
    modified_at:     TIMESTAMPTZ
}
```

#### SupplementAssessment
```
SupplementAssessment {
    id:              UUID (PK)
    patient_id:      UUID (FK → PatientProfile.id)
    tenant_id:       UUID (FK → Tenant.id)
    survey_responses: JSONB NOT NULL             — FR-01: structured survey instrument output
                                                 — { supplements: [{ name, dosage, unit, frequency, duration, condition }],
                                                 —   health_variables: { age, sex, conditions, medications, allergies } }
    ai_recommendation: JSONB NULL                — FR-04: AI output
                                                 — { safe: [...], review_needed: [...], warnings: [...], wellness_tips: [...] }
    risk_score:      INTEGER NULL CHECK(0 <= risk_score <= 100)  — FR-03
    benefit_level:   ENUM('low','moderate','high') NULL          — FR-03
    patient_facing_output: JSONB NULL            — plain-language version for patient dashboard
    clinician_facing_output: JSONB NULL           — clinical terminology version for doctor dashboard
    model_version:   VARCHAR(50)                 — AI model version used for this assessment
    scoring_model_version: VARCHAR(50)           — statistical scoring model version
    task_id:         UUID NULL                   — Celery task ID for async processing
    status:          ENUM('pending','processing','completed','failed')
    error_message:   TEXT NULL
    completed_at:    TIMESTAMPTZ
    created_at:      TIMESTAMPTZ

    INDEX: idx_assessment_patient (patient_id)
    INDEX: idx_assessment_status (status)
}
```

#### SupplementInteraction
```
SupplementInteraction {
    id:              UUID (PK)
    assessment_id:   UUID (FK → SupplementAssessment.id)
    supplement_name: VARCHAR(255)
    interacting_drug_or_supplement: VARCHAR(255)
    interaction_type: ENUM('drug-supplement','supplement-supplement','condition-contraindication')
    severity:        ENUM('low','moderate','high')
    evidence_source: VARCHAR(500)                — link to expert-labelled data or literature reference
    recommendation:  TEXT
    created_at:      TIMESTAMPTZ
}
```

#### AccessGrant
```
AccessGrant {
    id:              UUID (PK)
    record_id:       UUID (FK → MedicalRecord.id)
    grantor_id:      UUID (FK → User.id)         — who created the grant
    grantee_email:   VARCHAR(255)                — external recipient email (for share links)
    grantee_user_id: UUID (FK → User.id) NULL     — internal recipient (for doctor-to-doctor sharing)
    permissions:     JSONB                       — { view: true, download: false, print: false, share: false }
    access_token:    VARCHAR(128) UNIQUE          — cryptographically random, ≥128 bits entropy
    access_code:     VARCHAR(6) NULL              — optional PIN for additional security
    expiry_at:       TIMESTAMPTZ
    is_revoked:      BOOLEAN DEFAULT false
    revoked_by:      UUID (FK → User.id) NULL
    revoked_at:      TIMESTAMPTZ NULL
    created_at:      TIMESTAMPTZ
}
```

#### AccessLog
```
AccessLog {
    id:              UUID (PK)
    grant_id:        UUID (FK → AccessGrant.id)
    accessor_email:  VARCHAR(255)
    accessor_ip:     INET
    accessor_user_agent: VARCHAR(500)
    outcome:         ENUM('success','expired','revoked','invalid_token','blocked')
    accessed_at:     TIMESTAMPTZ
}
```

#### AuditEntry
```
AuditEntry {
    id:              UUID (PK)
    tenant_id:       UUID (FK → Tenant.id)
    actor_id:        UUID (FK → User.id)
    action:          ENUM('record_view','record_create','record_update','record_delete',
                          'assessment_create','assessment_view',
                          'share_grant_create','share_grant_revoke','share_view',
                          'emergency_access_activate','emergency_access_deactivate',
                          'alert_acknowledge','user_login','user_logout',
                          'permission_change','data_export')
    target_type:     VARCHAR(50)                  — 'MedicalRecord','SupplementAssessment','AccessGrant','PatientProfile','User'
    target_id:       UUID
    details:         JSONB                        — action-specific details (e.g., before/after diff for updates)
    ip_address:      INET
    user_agent:      VARCHAR(500)
    outcome:         ENUM('success','failure')
    created_at:      TIMESTAMPTZ

    INDEX: idx_audit_tenant_time (tenant_id, created_at DESC)
    INDEX: idx_audit_actor (actor_id, created_at DESC)
    INDEX: idx_audit_target (target_type, target_id)
}

-- Audit table is in a SEPARATE PostgreSQL schema ('audit') with:
-- 1. Application DB user has INSERT-only permission (no UPDATE, DELETE, TRUNCATE, DROP)
-- 2. A separate read-only user for audit queries
-- 3. WAL (Write-Ahead Log) archiving enabled for point-in-time recovery
```

#### Alert
```
Alert {
    id:              UUID (PK)
    patient_id:      UUID (FK → PatientProfile.id)
    doctor_id:       UUID (FK → User.id)
    tenant_id:       UUID (FK → Tenant.id)
    alert_type:      ENUM('high_severity_interaction','multiple_moderate','wellness_score_drop','survey_overdue')
    severity:        ENUM('low','moderate','high')
    message:         TEXT
    acknowledged_by: UUID (FK → User.id) NULL
    acknowledged_at: TIMESTAMPTZ NULL
    generated_at:    TIMESTAMPTZ
    expires_at:      TIMESTAMPTZ                  — NULL if permanent until acknowledged

    INDEX: idx_alert_doctor_unack (doctor_id, acknowledged_at) WHERE acknowledged_at IS NULL
}
```

#### Reminder
```
Reminder {
    id:              UUID (PK)
    patient_id:      UUID (FK → PatientProfile.id)
    tenant_id:       UUID (FK → Tenant.id)
    reminder_type:   ENUM('medication','supplement','survey','appointment')
    item_name:       VARCHAR(255)
    dosage:          VARCHAR(100) NULL
    scheduled_time:  TIME
    days_of_week:    INTEGER[]                    — [0=Mon,...,6=Sun], empty = daily
    notification_channel: ENUM('push','sms','email')
    is_active:       BOOLEAN DEFAULT true
    created_at:      TIMESTAMPTZ
}
```

---

## 4. API Specification

All endpoints prefixed with `/api/v1`. All request/response bodies in JSON. Authentication via `Authorization: Bearer <JWT>` header. Tenant context via `X-Tenant-ID` header (verified against JWT claim).

### 4.1 Supplement Assessment Endpoints

#### `POST /assessments` — Submit supplement survey

| Attribute | Detail |
|---|---|
| **IP Traceability** | IP 2 (UI 2025002953) — Survey instrument data collection |
| **Auth** | Patient role (or Doctor on behalf of patient with consent) |
| **Rate Limit** | 5 requests per patient per hour |

**Request:**
```json
{
  "patient_id": "uuid",
  "survey_responses": {
    "supplements": [
      {
        "name": "Garlic Extract",
        "dosage": 1000,
        "unit": "mg",
        "frequency": "daily",
        "duration_months": 12,
        "health_condition_addressed": "blood_pressure"
      }
    ],
    "health_variables": {
      "age": 58,
      "sex": "male",
      "conditions": ["type_2_diabetes", "hypertension"],
      "medications": [
        { "name": "Metformin", "dosage": 500, "unit": "mg", "frequency": "twice_daily" },
        { "name": "Amlodipine", "dosage": 5, "unit": "mg", "frequency": "daily" }
      ],
      "allergies": ["penicillin"]
    }
  }
}
```

**Response (202 Accepted — async processing):**
```json
{
  "assessment_id": "uuid",
  "task_id": "uuid",
  "status": "pending",
  "status_url": "/api/v1/tasks/{task_id}",
  "estimated_completion_seconds": 3
}
```

#### `GET /assessments/{id}` — Retrieve AI results

| Attribute | Detail |
|---|---|
| **IP Traceability** | IP 2 — Personalised recommendations output |
| **Auth** | Patient (own), Doctor (assigned patients), Admin (audit only) |

**Response (200 — when task completes):**
```json
{
  "assessment_id": "uuid",
  "patient_id": "uuid",
  "status": "completed",
  "risk_score": 65,
  "benefit_level": "moderate",
  "patient_facing_output": {
    "safe_supplements": [{ "name": "Vitamin D", "note": "Safe at current dosage" }],
    "review_needed": [{ "name": "Garlic Extract", "note": "May enhance blood pressure medication effect. Discuss with your doctor." }],
    "warnings": [{ "name": "Jamu Herbal", "note": "Contains undeclared ingredients. Stop use and consult doctor." }],
    "wellness_tips": ["Consider reducing sodium intake given hypertension diagnosis."]
  },
  "clinician_facing_output": {
    "interactions": [
      {
        "supplement": "Garlic Extract",
        "interacts_with": "Amlodipine",
        "type": "drug-supplement",
        "severity": "moderate",
        "mechanism": "Additive hypotensive effect",
        "evidence": "PMID: 12345678",
        "recommendation": "Monitor BP. Consider reducing garlic extract dose or switching to aged garlic extract."
      }
    ],
    "risk_score": 65,
    "model_version": "supp_v2.1.0",
    "scoring_model_version": "scoring_v1.3.0"
  },
  "wellness_score": 58,
  "completed_at": "2026-09-15T10:30:00+08:00"
}
```

#### `GET /patients/{patient_id}/assessments` — List assessment history

| Attribute | Detail |
|---|---|
| **Auth** | Patient (own), Doctor (assigned) |
| **Query Params** | `from`, `to`, `page`, `per_page` (default 20, max 100) |

**Response (200):**
```json
{
  "assessments": [
    {
      "id": "uuid",
      "risk_score": 65,
      "wellness_score": 58,
      "status": "completed",
      "completed_at": "2026-09-15T10:30:00+08:00"
    }
  ],
  "total": 12,
  "page": 1,
  "per_page": 20
}
```

### 4.2 Medical Records Endpoints

#### `POST /records` — Upload medical document

| Attribute | Detail |
|---|---|
| **IP Traceability** | IP 1 (PI 2025007955) — Encrypted cloud storage upload |
| **Auth** | Doctor, Patient (own records) |
| **Content-Type** | `multipart/form-data` |

**Request:**
```
POST /api/v1/records
Content-Type: multipart/form-data

patient_id: <uuid>
record_type: lab_report
title: HbA1c Test Results
description: Quarterly diabetes monitoring
file: <binary> (PDF/JPEG/PNG/TIFF, max 25MB)
```

**Response (201 Created):**
```json
{
  "record_id": "uuid",
  "blob_ref": "s3://Healynx-records-ap-se-6/tenant_uuid/patient_uuid/record_uuid/encrypted_blob.enc",
  "blob_encryption_status": "AES-256-GCM",
  "status": "stored",
  "created_at": "2026-09-15T10:35:00+08:00"
}
```

#### `GET /records/{id}` — Retrieve record metadata

| Attribute | Detail |
|---|---|
| **Auth** | Doctor (assigned patients), Patient (own), Emergency Access |

**Response (200):**
```json
{
  "record_id": "uuid",
  "patient_id": "uuid",
  "record_type": "lab_report",
  "title": "HbA1c Test Results",
  "description": "Quarterly diabetes monitoring",
  "mime_type": "application/pdf",
  "blob_size_bytes": 245760,
  "ocr_text": null,
  "ai_summary": "HbA1c: 7.2% (down from 7.8% 3 months ago). Indicates improved glycemic control. Continue current metformin regimen.",
  "ai_summary_confidence": 0.89,
  "uploaded_by": { "id": "uuid", "name": "Dr. Amira" },
  "created_at": "2026-09-15T10:35:00+08:00"
}
```

#### `GET /records/{id}/summary` — Get AI-generated summary

| Attribute | Detail |
|---|---|
| **IP Traceability** | IP 1 — AI-powered record summarisation (FR-09) |
| **Auth** | Doctor, Patient (own) |
| **Cache** | 5-minute Redis cache, invalidated on summary regeneration |

**Response (200):**
```json
{
  "record_id": "uuid",
  "summary": "Patient: Mr. Rajan (58M). Document: HbA1c lab report dated 12 Sep 2026. HbA1c: 7.2% (target <7.0%). Trend: improving from 7.8% three months prior. Current medication: Metformin 500mg BID. Assessment: Adequate glycemic control with room for improvement. No evidence of diabetic complications in this report. Next HbA1c recommended in 3 months.",
  "confidence": 0.89,
  "generated_at": "2026-09-15T10:40:00+08:00",
  "version": 1
}
```

#### `POST /records/{id}/share` — Generate time-limited access link (v2)

| Attribute | Detail |
|---|---|
| **IP Traceability** | IP 1 — Time-limited, permission-controlled record sharing (FR-11) |
| **Auth** | Doctor, Patient (own records) |
| **v1 Status** | Deferred to v2 per PRD scope |

**Request:**
```json
{
  "grantee_email": "cardiologist@hospital.com",
  "permissions": { "view": true, "download": false, "print": false },
  "expiry_hours": 72,
  "access_code": "123456"
}
```

**Response (201):**
```json
{
  "grant_id": "uuid",
  "share_url": "https://Healynx.ai/share/aB3xK9mW7qR2tY8v",
  "access_token": "aB3xK9mW7qR2tY8v",
  "expires_at": "2026-09-18T10:45:00+08:00"
}
```

#### `GET /share/{token}` — Secure viewer (view-only)

| Attribute | Detail |
|---|---|
| **IP Traceability** | IP 1 — Secure viewer with view-only restrictions (FR-12) |
| **Auth** | None (token-based access) |
| **v1 Status** | Deferred to v2 per PRD scope |

**Response (200):** Renders the secure viewer HTML page. The page enforces:
- Watermark overlay with `grantee_email` and access timestamp
- `Content-Security-Policy: default-src 'self'; script-src 'self'; style-src 'self'` to prevent XSS-based exfiltration
- `X-Frame-Options: DENY` to prevent clickjacking
- JavaScript event handlers blocking: `contextmenu`, `keydown` (Ctrl+C/Ctrl+S/Ctrl+P), `dragstart`
- CSS `@media print { body { display: none; } }` or renders watermark-only
- Session timeout: 15 minutes of inactivity

### 4.3 Emergency Access Endpoints

#### `POST /emergency-access` — Activate emergency override

| Attribute | Detail |
|---|---|
| **IP Traceability** | IP 1 — Emergency access override mode (FR-13) |
| **Auth** | Doctor, Clinic Admin |

**Request:**
```json
{
  "patient_id": "uuid",
  "reason": "Patient unconscious, suspected drug interaction. Need full medication history.",
  "justification_free_text": "ER admission, no next-of-kin available for consent."
}
```

**Response (201):**
```json
{
  "emergency_session_id": "uuid",
  "patient_id": "uuid",
  "doctor_id": "uuid",
  "access_token": "<short-lived JWT with emergency scope, 30-min expiry>",
  "expires_at": "2026-09-15T11:15:00+08:00",
  "notification_sent_to": ["patient_email", "clinic_admin_email"]
}
```

### 4.4 Audit Endpoints

#### `GET /audit-logs` — Query audit trail

| Attribute | Detail |
|---|---|
| **IP Traceability** | IP 1 — Audit logging for all events (FR-21) |
| **Auth** | Clinic Admin, Super Admin |

**Query Parameters:**
| Param | Type | Description |
|---|---|---|
| `from` | ISO 8601 | Start of range (required) |
| `to` | ISO 8601 | End of range (required) |
| `actor_id` | UUID | Filter by specific user |
| `action` | String | e.g., `record_view`, `emergency_access_activate` |
| `target_type` | String | e.g., `MedicalRecord` |
| `target_id` | UUID | Filter by specific resource |
| `page` | Integer | Default 1 |
| `per_page` | Integer | Default 50, max 200 |

**Response (200):**
```json
{
  "entries": [
    {
      "id": "uuid",
      "actor": { "id": "uuid", "name": "Dr. Amira", "role": "doctor" },
      "action": "emergency_access_activate",
      "target_type": "PatientProfile",
      "target_id": "uuid",
      "details": { "reason": "Patient unconscious, suspected drug interaction." },
      "ip_address": "203.106.12.45",
      "outcome": "success",
      "created_at": "2026-09-15T10:45:00+08:00"
    }
  ],
  "total": 1,
  "page": 1,
  "per_page": 50
}
```

### 4.5 Notification & Task Endpoints

#### `GET /tasks/{task_id}` — Poll async task status

**Response (200):**
```json
{
  "task_id": "uuid",
  "status": "completed",
  "result_resource_url": "/api/v1/assessments/{assessment_id}",
  "progress_percent": 100,
  "created_at": "2026-09-15T10:30:00+08:00",
  "completed_at": "2026-09-15T10:30:03+08:00"
}
```

#### `POST /reminders` — Create medication/supplement reminder

| Attribute | Detail |
|---|---|
| **Auth** | Patient |

**Request:**
```json
{
  "reminder_type": "medication",
  "item_name": "Metformin",
  "dosage": "500mg",
  "scheduled_time": "08:00",
  "days_of_week": [0, 1, 2, 3, 4, 5, 6],
  "notification_channel": "push"
}
```

**Response (201):**
```json
{
  "reminder_id": "uuid",
  "next_reminder_at": "2026-09-16T08:00:00+08:00"
}
```

---

## 5. AI Pipeline Architecture

### 5.1 Supplement Analysis Pipeline (IP 2 — UI 2025002953)

This pipeline implements the dual-engine architecture specified in IP 2: an AI Algorithm trained on expert-verified medical data + a Statistical Scoring Model evaluating patient variables.

```
                       ┌──────────────────────────┐
                       │  POST /assessments        │
                       │  (Survey JSON submitted)  │
                       └────────────┬─────────────┘
                                    │
                                    ▼
                       ┌──────────────────────────┐
                       │  Step 1: Data Validation  │
                       │  • Pydantic schema check  │
                       │  • Supplement name ↦      │
                       │    canonical DB lookup    │
                       │  • Dosage parsing + unit  │
                       │    normalisation          │
                       └────────────┬─────────────┘
                                    │
                                    ▼
                       ┌──────────────────────────┐
                       │  Step 2: Interaction      │
                       │  Detection (AI Engine)    │
                       │                           │
                       │  Neo4j Cypher Query:      │
                       │  MATCH (s:Supplement)-    │
                       │  [r:INTERACTS_WITH]->      │
                       │  (m:Medication)           │
                       │  WHERE s.name IN [...]    │
                       │  AND m.name IN [...]      │
                       │  RETURN s, r, m, r.severity│
                       │                           │
                       │  + Condition check:       │
                       │  MATCH (s:Supplement)-    │
                       │  [r:CONTRAINDICATED_IN]-> │
                       │  (c:Condition)            │
                       └────────────┬─────────────┘
                                    │
                                    ▼
                       ┌──────────────────────────┐
                       │  Step 3: Statistical      │
                       │  Scoring Model            │
                       │                           │
                       │  Risk Score = f(          │
                       │    interaction_count,     │
                       │    max_interaction_severity│
                       │    dosage_vs_rda_ratio,   │
                       │    comorbidity_multiplier,│
                       │    polypharmacy_factor    │
                       │  )                        │
                       │                           │
                       │  Benefit Level = g(       │
                       │    supplement_evidence,   │
                       │    condition_match,       │
                       │    dosage_appropriateness │
                       │  )                        │
                       └────────────┬─────────────┘
                                    │
                                    ▼
                       ┌──────────────────────────┐
                       │  Step 4: Recommendation   │
                       │  Generation               │
                       │                           │
                       │  Template-based + Rules:  │
                       │  • High severity → STOP   │
                       │  • Moderate → MONITOR     │
                       │  • Low → INFO             │
                       │                           │
                       │  LLM pass (optional):     │
                       │  Generate patient-facing  │
                       │  plain-language text      │
                       │  from structured output   │
                       │  (with guardrails)        │
                       └────────────┬─────────────┘
                                    │
                                    ▼
                       ┌──────────────────────────┐
                       │  Step 5: Wellness Score   │
                       │  Calculation (FR-06)       │
                       │                           │
                       │  Score = 100 -            │
                       │    (risk_penalty *         │
                       │     30) -                 │
                       │    (condition_penalty *    │
                       │     20) +                 │
                       │    (adherence_bonus *      │
                       │     15)                   │
                       └────────────┬─────────────┘
                                    │
                                    ▼
                       ┌──────────────────────────┐
                       │  Step 6: Output Assembly  │
                       │  & Persistence              │
                       │                           │
                       │  • Write Supplement-      │
                       │    Assessment row         │
                       │  • Write Supplement-      │
                       │    Interaction rows       │
                       │  • Update PatientProfile. │
                       │    wellness_score         │
                       │  • Generate Alert if      │
                       │    severity == high       │
                       │  • Dispatch notification  │
                       │  • Log audit entry        │
                       └──────────────────────────┘
```

**Implementation details:**
- The AI Algorithm component (IP 2) is realised as: (a) a Neo4j graph database containing the expert-labelled interaction knowledge base, and (b) a set of Cypher queries that are the production encoding of the expert-labelled rules. If the UM research codebase includes a trained classifier (e.g., XGBoost, Random Forest) for interaction detection, the model is served via BentoML and called in addition to the graph query.
- The Statistical Scoring Model (IP 2) is realised as a Python function with versioned weights. The formula is derived from the IP 2 scoring methodology and validated during Phase 1 clinical pilot against pharmacist review.
- Total pipeline latency target: ≤3 seconds from survey submission to completed assessment (NFR-05).

### 5.2 Record Summarisation Pipeline (IP 1 — PI 2025007955)

```
                       ┌──────────────────────────┐
                       │  POST /records            │
                       │  (Document uploaded)      │
                       └────────────┬─────────────┘
                                    │
                                    ▼
                       ┌──────────────────────────┐
                       │  Step 1: Encryption &     │
                       │  Storage                   │
                       │  • Generate DEK (Fernet)  │
                       │  • Encrypt blob with DEK  │
                       │  • Encrypt DEK with KMS   │
                       │  • Store encrypted blob   │
                       │    in S3                  │
                       │  • Store metadata in PG   │
                       └────────────┬─────────────┘
                                    │
                                    ▼
                       ┌──────────────────────────┐
                       │  Step 2: OCR Extraction   │
                       │  (v2 — deferred)          │
                       │                           │
                       │  • Tesseract 5 LSTM       │
                       │  • AWS Textract (fallback)│
                       │  • Output: raw text +     │
                       │    structured fields      │
                       │    (diagnosis, meds, labs)│
                       └────────────┬─────────────┘
                                    │
                                    ▼
                       ┌──────────────────────────┐
                       │  Step 3: Text              │
                       │  Preprocessing            │
                       │  • Document chunking      │
                       │    (max 512 tokens)       │
                       │  • De-identification      │
                       │    (Regex-based for NRIC, │
                       │     phone numbers)        │
                       │  • Section classification │
                       │    (Diagnosis, Meds,      │
                       │     Labs, Plan)           │
                       └────────────┬─────────────┘
                                    │
                                    ▼
                       ┌──────────────────────────┐
                       │  Step 4: NLP              │
                       │  Summarisation Model      │
                       │                           │
                       │  • Model: BART-large      │
                       │    fine-tuned on UM       │
                       │    clinical text dataset  │
                       │  • Input: concatenated    │
                       │    text sections          │
                       │  • Output: ≤500 word      │
                       │    abstractive summary    │
                       │  • Confidence: model      │
                       │    log-probability score  │
                       │                           │
                       │  GPU inference via ONNX   │
                       │  Runtime or PyTorch       │
                       └────────────┬─────────────┘
                                    │
                                    ▼
                       ┌──────────────────────────┐
                       │  Step 5: Structured       │
                       │  Output & Persistence      │
                       │  • Store summary in       │
                       │    MedicalRecord.ai_summary│
                       │  • Store confidence score │
                       │  • Version summary        │
                       │  • Log audit entry        │
                       │  • Invalidate cache key   │
                       │    in Redis               │
                       └──────────────────────────┘
```

**v1 note:** In v1, document upload stores the encrypted blob and metadata. AI summarisation runs on the OCR text if the uploaded document is already text-based (PDF with embedded text, or text pasted directly into a clinical note field). Full OCR pipeline (Step 2) is deferred to v2 per PRD scope.

### 5.3 Chatbot Query Pipeline (IP 1 — PI 2025007955)

```
  User: "What were my HbA1c results
         over the last year?"
                │
                ▼
  ┌──────────────────────────┐
  │ Step 1: Intent Detection │
  │ • Classify query type:   │
  │   - Record lookup        │
  │   - Supplement question  │
  │   - General health info  │
  │ • Extract entities:      │
  │   - "HbA1c" (lab test)  │
  │   - "last year" (time)  │
  └────────────┬─────────────┘
               │
               ▼
  ┌──────────────────────────┐
  │ Step 2: Context Retrieval│
  │ (RAG)                    │
  │ • Embed query via        │
  │   sentence-transformers  │
  │ • pgvector similarity    │
  │   search against patient's│
  │   record chunks          │
  │ • Retrieve top-k (k=5)   │
  │   relevant chunks        │
  │ • Filter by RBAC: only   │
  │   chunks from this       │
  │   patient's records      │
  └────────────┬─────────────┘
               │
               ▼
  ┌──────────────────────────┐
  │ Step 3: LLM Generation   │
  │ • Model: Mistral 7B or   │
  │   Llama 3 8B (self-hosted)│
  │ • Prompt:                 │
  │   "You are a medical      │
  │   assistant. Using ONLY   │
  │   the following patient   │
  │   records, answer the     │
  │   question. If the        │
  │   information is not in   │
  │   the records, say so.    │
  │   NEVER fabricate data.   │
  │   Include: [disclaimer]"  │
  │ • Context: [retrieved     │
  │   chunks]                 │
  └────────────┬─────────────┘
               │
               ▼
  ┌──────────────────────────┐
  │ Step 4: Output Guardrails│
  │ • PII filter: scan output│
  │   for NRIC, phone, email │
  │ • Medical disclaimer      │
  │   injection               │
  │ • Check output against    │
  │   retrieved chunks for    │
  │   hallucination (ROUGE-L) │
  │ • If hallucination        │
  │   detected → flag for     │
  │   human review, return    │
  │   "I cannot answer"       │
  └──────────────────────────┘
```

### 5.4 Model Training & Validation Approach

| Aspect | Approach |
|---|---|
| **Training Data** | UM expert-labelled dataset transferred under IP licensing (Dependency D2). Dataset contains: (a) labelled supplement-drug interaction pairs with severity, (b) clinical document-summary pairs for summarisation fine-tuning, (c) expert-annotated supplement risk scores for scoring model calibration. |
| **Supplement Interaction Model** | If UM provides a trained classifier: retrained with expanded dataset during Phase 1 validation. If rule-based (Neo4j graph): graph populated from expert-labelled data; new interactions added via clinician feedback during pilot. |
| **Summarisation Model** | Baseline BART-large fine-tuned on UM clinical document-summary pairs. Evaluation: ROUGE-L against held-out test set ≥0.40. Human evaluation: 3 clinicians rate summary quality (relevance, completeness, conciseness) on 1–5 scale; target ≥4.0 average. |
| **Continuous Improvement** | Clinician "flag as incorrect" feedback (FR-09) stored in a `ModelFeedback` table. Quarterly retraining batch processes flagged cases. New model versions deployed via BentoML with canary rollout (10% traffic → monitor → full rollout). |
| **Bias Testing** | Model evaluated across demographic subgroups (ethnicity: Malay, Chinese, Indian; sex: male, female; age groups: <40, 40–60, >60). Performance parity requirement: no subgroup may have accuracy ≥10 percentage points below the best-performing subgroup. |

---

## 6. Security Architecture

### 6.1 Encryption Strategy

```
┌─────────────────────────────────────────────────────────────────┐
│                      ENCRYPTION LAYERS                           │
│                                                                  │
│  ┌──────────────────────┐    ┌──────────────────────────────┐   │
│  │  ENCRYPTION IN       │    │  ENCRYPTION AT REST           │   │
│  │  TRANSIT             │    │                               │   │
│  │                      │    │  Layer 1: Storage-level       │   │
│  │  TLS 1.3 (minimum)   │    │  • AWS S3 SSE-KMS / Azure     │   │
│  │  • Client → CDN      │    │    Storage Service Encryption │   │
│  │  • CDN → ALB         │    │  • PostgreSQL TDE via         │   │
│  │  • ALB → EKS         │    │    cloud provider KMS        │   │
│  │  • EKS → RDS         │    │                               │   │
│  │  • EKS → ElastiCache │    │  Layer 2: Application-level  │   │
│  │  • EKS → Neo4j       │    │  • Envelope encryption for   │   │
│  │                      │    │    record blobs (DEK + CMK)  │   │
│  │  Mutual TLS for      │    │  • Column-level encryption   │   │
│  │  service-to-service  │    │    for NRIC, phone (pgcrypto)│   │
│  │  (v2 with service     │    │  • JSONB encryption for     │   │
│  │   mesh)              │    │    survey responses containing│   │
│  │                      │    │    PII                       │   │
│  └──────────────────────┘    └──────────────────────────────┘   │
│                                                                  │
│  Key Management:                                                 │
│  ┌──────────────────────────────────────────────────────────┐   │
│  │  AWS KMS / Azure Key Vault                                │   │
│  │  • CMK auto-rotated every 90 days                         │   │
│  │  • DEKs generated per record (Fernet → AES-256-GCM)       │   │
│  │  • DEK stored alongside encrypted blob, encrypted with CMK│   │
│  │  • HSM-backed keys in production (CloudHSM for AWS)       │   │
│  └──────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────┘
```

### 6.2 Authentication Flow

```
┌──────────┐     ┌──────────┐     ┌──────────┐     ┌──────────┐
│  Client  │     │  API GW  │     │ Keycloak │     │ FastAPI  │
└────┬─────┘     └────┬─────┘     └────┬─────┘     └────┬─────┘
     │                │               │                 │
     │ POST /auth/login               │                 │
     │ {email, pass}  │               │                 │
     │───────────────►│               │                 │
     │                │──────────────►│                 │
     │                │               │ Validate creds  │
     │                │               │ ( + TOTP if MFA)│
     │                │◄──────────────│                 │
     │                │ access_token  │                 │
     │                │ refresh_token │                 │
     │◄───────────────│               │                 │
     │ {access_token, │               │                 │
     │  refresh_token} │               │                 │
     │                │               │                 │
     │ GET /api/v1/patients          │                 │
     │ Authorization: │               │                 │
     │ Bearer <access>│               │                 │
     │───────────────►│               │                 │
     │                │ Verify JWT    │                 │
     │                │ (signature,   │                 │
     │                │  expiry,      │                 │
     │                │  claims)      │                 │
     │                │───────────────┼────────────────►│
     │                │               │  X-User-ID      │
     │                │               │  X-Role         │
     │                │               │  X-Tenant-ID    │
     │                │               │                 │
     │                │               │  OPA evaluates   │
     │                │               │  Rego policy     │
     │                │               │                 │
     │                │               │◄────────────────│
     │                │               │  200 + data     │
     │◄───────────────│               │                 │
```

**Token specifications:**
| Token | Format | Expiry | Storage (Client) |
|---|---|---|---|
| Access Token | JWT (RS256), Keycloak-issued | 15 minutes | Memory only |
| Refresh Token | Opaque token (UUID), stored in Keycloak | 7 days, rotation on use | HttpOnly Secure cookie or secure mobile storage |
| Emergency Access Token | JWT with `scope: emergency_access` claim | 30 minutes | Memory only |

### 6.3 Authorisation — RBAC Policy Matrix

| Action | Patient | Doctor | Clinic Admin | Emergency Access | Super Admin |
|---|---|---|---|---|---|
| View own records | ✅ | — | — | — | — |
| View assigned patient records | — | ✅ (assigned only) | ❌ | ✅ (any, temp) | ✅ |
| Edit/create records for assigned patients | — | ✅ | ❌ | ❌ | ✅ |
| Submit supplement survey | ✅ (own) | ✅ (on behalf) | ❌ | ❌ | ❌ |
| View own AI results | ✅ | — | — | — | — |
| View assigned patient AI results | — | ✅ | ❌ | ✅ (temp) | ✅ |
| Acknowledge alerts | — | ✅ (assigned) | ❌ | ❌ | ❌ |
| Generate share links | — | ✅ (assigned) | ❌ | ❌ | ❌ |
| Activate emergency access | — | ✅ | ✅ | — | ✅ |
| View audit logs | ❌ | ❌ | ✅ (own tenant) | ❌ | ✅ |
| Manage users/roles | ❌ | ❌ | ✅ (own tenant) | ❌ | ✅ |
| Export data | ✅ (own) | ❌ | ✅ (own tenant, aggregate only) | ❌ | ✅ |
| Delete records | ❌ | ❌ | ❌ | ❌ | ✅ (supervised) |

**OPA Rego policy example:**
```rego
package Healynx.authz

default allow = false

# Doctor can view records of assigned patients
allow {
    input.role == "doctor"
    input.action == "record_view"
    input.patient_id == input.assigned_patients[_]
}

# Clinic admin can view audit logs of own tenant only
allow {
    input.role == "clinic_admin"
    input.action == "audit_view"
    input.tenant_id == input.user_tenant_id
}

# Emergency access — requires emergency scope in JWT
allow {
    input.role == "emergency_access"
    input.action == "record_view"
    input.token_scope == "emergency_access"
    time.now_ns() < input.emergency_expiry_ns
}
```

### 6.4 Secure Viewer Technical Implementation

| Mechanism | Implementation |
|---|---|
| **No Copy** | `document.addEventListener('copy', e => e.preventDefault())` and `document.addEventListener('cut', e => e.preventDefault())`. Selection disabled via CSS `user-select: none` on the viewer container. |
| **No Download** | Record content served as server-rendered HTML (not a PDF/URL that can be downloaded). The actual blob is never exposed to the browser — the backend renders the document as `<canvas>` elements (PDF.js) or `<img>` elements (images), watermarked server-side. |
| **No Print** | `@media print { body * { display: none !important; } .watermark-overlay { display: block !important; } }`. Additionally, `window.onbeforeprint = () => { /* show warning */ }`. |
| **No Screenshot** | Not fully preventable at browser level. Mitigations: (a) visible dynamic watermark with user identity deters casual screenshots, (b) session timeout at 15 minutes limits exposure, (c) Mobile: Android FLAG_SECURE / iOS `capture` detection (limited effectiveness), (d) Legal deterrence via terms of service with audit trail of every access. |
| **Watermark** | Server-side rendered overlay with: `{grantee_email} | {access_datetime} | {access_ip}`. Applied as repeating semi-transparent text across the document. |
| **Content-Security-Policy** | Strict CSP: `default-src 'none'; script-src 'self'; style-src 'self'; img-src 'self' data:; connect-src 'self'; frame-ancestors 'none'; base-uri 'self'; form-action 'none'`. |
| **Session Management** | Secure viewer session token is separate from the main application JWT. 15-minute idle timeout. Server-side session invalidation on tab close (via `beforeunload` event — best effort). |

### 6.5 PDPA Compliance Implementation

| PDPA Principle | Technical Implementation |
|---|---|
| **Consent** | Consent collection screen before first survey submission. Consent record stored in `ConsentRecord` table with versioning. Patient can view and revoke consent at any time. Revocation triggers data processing freeze (not deletion, which is managed separately). |
| **Data Subject Access** | `GET /patients/{id}/data-export` endpoint returns all patient data in machine-readable JSON. Includes: profile, all assessments, all records metadata, all sharing history. Fulfilment within PDPA-required timeline (typically 30 days, targeted within 48 hours). |
| **Data Deletion** | `POST /patients/{id}/request-deletion` triggers a workflow: (a) soft-delete (anonymise PII, retain anonymised records for clinical audit obligation — 7 years), (b) hard-delete of uploaded blobs from S3 after retention period, (c) Redis cache invalidation. Admin approval required before execution. |
| **Data Residency** | All patient data (PostgreSQL, S3 blobs, Redis, Neo4j) deployed within `ap-southeast-6` (AWS Malaysia) or equivalent Malaysian region. No cross-border data transfer without explicit consent and PDPA-compliant transfer mechanism. CDN edge caches contain only static assets, never patient data. |
| **Breach Notification** | Automated monitoring for anomalous access patterns (e.g., bulk record downloads, off-hours access, unusual IP geolocation). If breach detected: (a) automated incident response runbook triggered, (b) notification to Data Protection Officer within 1 hour, (c) PDPA-mandated notification to authorities and affected data subjects within prescribed timeline. |
| **Data Minimisation** | API responses return only fields necessary for the requesting role. Analytics/reporting endpoints use anonymised aggregate data only. LLM prompts include only the minimum context chunks needed to answer the query. |

### 6.6 Audit Trail Technical Architecture

```
┌────────────────────┐
│ Application Code   │
│ (FastAPI)          │
│                    │
│ audit_log(          │
│   action, target,  │
│   details)         │
└────────┬───────────┘
         │
         ▼
┌────────────────────┐
│ Audit Middleware    │
│ • Auto-captures:   │
│   - actor_id (JWT) │
│   - ip_address     │
│   - user_agent     │
│   - tenant_id      │
│   - timestamp      │
│ • Enriches with    │
│   action-specific  │
│   details          │
└────────┬───────────┘
         │
         ▼
┌────────────────────┐
│ PostgreSQL         │
│ 'audit' schema     │
│                    │
│ App user: INSERT   │
│ only (no UPDATE,   │
│ DELETE, TRUNCATE)  │
│                    │
│ Read user: SELECT  │
│ only (for queries) │
│                    │
│ WAL archived to    │
│ S3 for PITR        │
└────────────────────┘
```

- Audit entries are written **synchronously** — if the audit write fails, the triggering action fails. This prevents unlogged access.
- Audit log retention: 7 years minimum (clinical record keeping requirement under Malaysian law). Older logs archived to S3 Glacier Deep Archive.
- Log integrity verification: nightly job computes hash chain over the day's audit entries. Any tampering detected via hash mismatch triggers immediate security alert.

---

## 7. Infrastructure & DevOps

### 7.1 Cloud Architecture (AWS Primary)

```
┌──────────────────────────────────────────────────────────────────────────┐
│                        AWS ap-southeast-6 (Malaysia)                      │
│                                                                           │
│  ┌─────────────────────────────────────────────────────────────────────┐ │
│  │                           VPC (10.0.0.0/16)                          │ │
│  │                                                                      │ │
│  │  ┌───────────────────┐  ┌───────────────────┐  ┌──────────────────┐ │ │
│  │  │ Public Subnet A   │  │ Public Subnet B   │  │ Public Subnet C  │ │ │
│  │  │ (ap-se-6a)        │  │ (ap-se-6b)        │  │ (ap-se-6c)       │ │ │
│  │  │                   │  │                   │  │                  │ │ │
│  │  │ ┌───────────────┐ │  │ ┌───────────────┐ │  │ ┌──────────────┐ │ │ │
│  │  │ │ Application   │ │  │ │ Application   │ │  │ │ Application  │ │ │ │
│  │  │ │ Load Balancer │ │  │ │ Load Balancer │ │  │ │ Load Balancer│ │ │ │
│  │  │ │ (ALB)         │ │  │ │ (ALB)         │ │  │ │ (ALB)        │ │ │ │
│  │  │ └───────┬───────┘ │  │ └───────┬───────┘ │  │ └──────┬───────┘ │ │ │
│  │  └─────────┼─────────┘  └─────────┼─────────┘  └────────┼─────────┘ │ │
│  │            │                      │                     │           │ │
│  │  ┌─────────┼──────────────────────┼─────────────────────┼─────────┐ │ │
│  │  │         ▼                      ▼                     ▼         │ │ │
│  │  │              Private Subnets (App Tier)                        │ │ │
│  │  │                                                                │ │ │
│  │  │  ┌──────────────────────────────────────────────────────┐     │ │ │
│  │  │  │                  EKS Cluster                          │     │ │ │
│  │  │  │                                                       │     │ │ │
│  │  │  │  ┌─────────┐ ┌──────────┐ ┌────────┐ ┌───────────┐  │     │ │ │
│  │  │  │  │API Pods │ │AI/GPU    │ │Celery  │ │WebSocket  │  │     │ │ │
│  │  │  │  │(FastAPI)│ │Pods      │ │Workers │ │Pods       │  │     │ │ │
│  │  │  │  │x3 (min) │ │(G5.xlarge│ │x5 (min)│ │x2 (min)   │  │     │ │ │
│  │  │  │  │         │ │ or equiv)│ │        │ │           │  │     │ │ │
│  │  │  │  └─────────┘ └──────────┘ └────────┘ └───────────┘  │     │ │ │
│  │  │  │                                                       │     │ │ │
│  │  │  │  ┌─────────┐ ┌──────────┐                            │     │ │ │
│  │  │  │  │Keycloak │ │Neo4j     │                            │     │ │ │
│  │  │  │  │Pod      │ │Pod       │                            │     │ │ │
│  │  │  │  └─────────┘ └──────────┘                            │     │ │ │
│  │  │  └──────────────────────────────────────────────────────┘     │ │ │
│  │  │                                                                │ │ │
│  │  │  ┌────────────────┐  ┌──────────────┐  ┌──────────────────┐  │ │ │
│  │  │  │ RDS PostgreSQL │  │ ElastiCache  │  │ S3 (Encrypted)   │  │ │ │
│  │  │  │ Multi-AZ       │  │ Redis Cluster│  │ • Record Blobs   │  │ │ │
│  │  │  │ (db.r6g.xlarge)│  │ (cache.r6g)  │  │ • Audit Archives │  │ │ │
│  │  │  └────────────────┘  └──────────────┘  │ • Model Artifacts│  │ │ │
│  │  │                                        └──────────────────┘  │ │ │
│  │  └────────────────────────────────────────────────────────────────┘ │ │
│  └──────────────────────────────────────────────────────────────────────┘ │
│                                                                           │
│  Edge Services:                                                           │
│  ┌────────────┐ ┌──────────┐ ┌──────────┐ ┌─────────────┐                │
│  │ CloudFront │ │ AWS WAF  │ │Route 53  │ │AWS KMS      │                │
│  │ (CDN)      │ │          │ │(DNS +    │ │(Key Mgmt)   │                │
│  │            │ │          │ │ Health)  │ │             │                │
│  └────────────┘ └──────────┘ └──────────┘ └─────────────┘                │
└──────────────────────────────────────────────────────────────────────────┘
```

**Key infrastructure decisions:**
- **Multi-AZ deployment** across 3 availability zones for high availability (NFR-04: 99.9% uptime).
- **EKS with managed node groups** — API pods on `t3.medium` (CPU), AI inference pods on `g5.xlarge` (GPU — NVIDIA A10G). KEDA scales Celery workers based on Redis queue depth.
- **RDS PostgreSQL Multi-AZ** with 1 primary + 1 standby replica in different AZs. Automated backups every 6 hours with 30-day retention. Point-in-time recovery enabled.
- **S3** for record blobs with versioning enabled, SSE-KMS encryption, and lifecycle policy: transition to S3 Standard-IA after 90 days, Glacier Deep Archive after 1 year, delete after 7 years + retention hold.
- **No direct internet access** to data tier — RDS, ElastiCache, Neo4j, S3 accessible only from private subnets via VPC endpoints.

### 7.2 CI/CD Pipeline

```
┌──────────┐    ┌──────────┐    ┌──────────┐    ┌──────────┐    ┌──────────┐
│  Git     │    │  Build   │    │  Test    │    │ Security │    │  Deploy  │
│  Push    │───►│  & Image │───►│  Suite   │───►│  Scan    │───►│  to EKS  │
│ (GitHub) │    │          │    │          │    │          │    │          │
└──────────┘    └──────────┘    └──────────┘    └──────────┘    └──────────┘
                    │               │               │               │
                    ▼               ▼               ▼               ▼
              ┌──────────┐    ┌──────────┐    ┌──────────┐    ┌──────────┐
              │ Docker   │    │ Backend: │    │ Trivy    │    │ ArgoCD   │
              │ Build    │    │ - pytest │    │ (CVE     │    │ (GitOps) │
              │ (multi-  │    │ - API    │    │  scan)   │    │          │
              │  arch)   │    │   tests  │    │          │    │ Canary:  │
              │          │    │ - AI pipe│    │ Bandit   │    │ 10% →    │
              │ Frontend:│    │   tests  │    │ (SAST    │    │ monitor  │
              │ - next   │    │          │    │  Python) │    │ 10min →  │
              │   build  │    │ Frontend:│    │          │    │ 100%     │
              │ - vitest │    │ - vitest │    │ npm audit│    │          │
              │          │    │ - Play-  │    │          │    │ Rollback:│
              │          │    │   wright │    │ OPA      │    │ automatic│
              │          │    │   E2E    │    │ policy   │    │ if error │
              │          │    │          │    │ check    │    │ rate >1% │
              └──────────┘    └──────────┘    └──────────┘    └──────────┘
```

**Pipeline stages:**
1. **Build:** GitHub Actions. Docker images built for linux/amd64 and linux/arm64. Frontend: `next build` with static analysis (`next lint`). Images pushed to ECR.
2. **Test:** Backend unit tests (`pytest`), API integration tests (FastAPI TestClient), AI pipeline tests (mock Neo4j + test fixtures), Frontend unit (`vitest`), E2E (Playwright against staging environment).
3. **Security Scan:** Trivy for container image CVEs, Bandit for Python SAST, npm audit for frontend dependencies, OPA policy unit tests, dependency-review for supply chain.
4. **Deploy:** ArgoCD watches the deployment repo. Canary deployment: new version receives 10% traffic for 10 minutes with Datadog monitors watching error rate, latency p95. If healthy, promote to 100%. If error rate >1% or latency p95 > 2x baseline, automatic rollback.

### 7.3 Monitoring & Observability

| Layer | Tool | Metrics |
|---|---|---|
| **APM** | Datadog APM / Grafana + Tempo (OSS alternative) | Request latency (p50/p95/p99), error rate, throughput per endpoint, DB query performance, Celery task duration. |
| **Infrastructure** | Datadog / Grafana + Prometheus | CPU, memory, GPU utilisation per pod; Redis hit/miss ratio; RDS connection count, IOPS; S3 request latency. |
| **Logging** | Structured JSON logs → Fluent Bit (DaemonSet) → OpenSearch (or Datadog Logs) | Application logs, access logs (ALB), EKS control plane logs, RDS slow query logs. Log retention: 90 days hot, 7 years archived to S3. |
| **Alerting** | Datadog Monitors / Grafana Alertmanager | **Critical (P1 — page on-call):** Error rate >5%, API latency p95 >5s, DB connection exhaustion, audit log write failure, encryption key access failure. **Warning (P2 — Slack/Teams):** Error rate >2%, latency p95 >3s, disk >80%, Redis memory >80%, Celery queue depth >1000. |
| **Synthetic Monitoring** | Datadog Synthetic / Uptime Robot | Health check endpoint every 60 seconds from 3 geographic locations. Simulated user flows: login → survey submission → results retrieval every 15 minutes. |
| **Dashboard** | Grafana (per tenant operational dashboard for clinic admins showing uptime, record count, active users) | |

### 7.4 Backup & Disaster Recovery

| Resource | RPO | RTO | Backup Strategy |
|---|---|---|---|
| **RDS PostgreSQL** | 15 minutes | 1 hour | Automated backups every 6 hours with 30-day retention. Continuous WAL archiving to S3 enabling PITR to within 5 minutes. Cross-region (Singapore) backup copy for region-level disaster. |
| **S3 (Record Blobs)** | 1 hour | 4 hours | Cross-region replication (SRR) to S3 in ap-southeast-1 (Singapore). Versioning enabled for protection against accidental deletion. Object Lock in compliance mode for 7-year retention records. |
| **Neo4j (Graph DB)** | 1 hour | 2 hours | `neo4j-admin backup` to S3 every hour. Graph can be rebuilt from PostgreSQL event log if needed (full replay). |
| **Redis (Cache + Broker)** | N/A (cache) | 15 minutes | Cache is ephemeral — no backup. Celery task state is transient — lost tasks re-queued by application on Redis failure detection. Session data: users re-authenticate. |
| **EKS Cluster Config** | Config change | 1 hour | All infrastructure defined as Terraform (IaC). Cluster state stored in S3 backend with versioning. ArgoCD re-syncs applications on new cluster. |

**DR scenario: ap-southeast-6 region failure:**
1. Route 53 health check detects ALB endpoint failure.
2. DNS failover to ap-southeast-1 (Singapore) standby region (pre-provisioned EKS cluster, RDS read replica promoted to primary).
3. S3 Cross-Region Replication ensures record blobs available.
4. Total RTO: 1 hour (DNS propagation + DB promotion). RPO: 15 minutes (max data loss: last WAL archive interval).

---

## 8. Integration Patterns

### 8.1 EMR/EHR Integration (HL7 FHIR R4) — v2

```
┌─────────────────┐                    ┌──────────────────────┐
│  External EMR    │                    │  Healynx       │
│  (e.g., Caresys, │                    │  FHIR Adapter        │
│   KMS Healthcare)│                    │                      │
└────────┬────────┘                    └──────────┬───────────┘
         │                                        │
         │  FHIR R4 (REST)                        │
         │  GET /Patient/{id}                     │
         │──────────────────────────────────────►│
         │                                        │
         │                            ┌───────────▼───────────┐
         │                            │ FHIR → Internal Model │
         │                            │ • Patient →            │
         │                            │   PatientProfile       │
         │                            │ • Observation →        │
         │                            │   MedicalRecord        │
         │                            │ • MedicationStatement →│
         │                            │   health_profile JSONB │
         │                            │ • DocumentReference →  │
         │                            │   MedicalRecord        │
         │                            └───────────┬───────────┘
         │                                        │
         │◄───────────────────────────────────────│
         │  FHIR Bundle (JSON)                    │
         │                                        │
```

**FHIR Resource Mapping:**

| FHIR Resource | Healynx Entity | Direction |
|---|---|---|
| `Patient` | `PatientProfile` | Bidirectional |
| `Observation` (lab results) | `MedicalRecord` (record_type: lab_report) | Bidirectional |
| `Condition` | `PatientProfile.health_profile.conditions` | Bidirectional |
| `MedicationStatement` | `PatientProfile.health_profile.medications` + `SupplementAssessment.survey_responses.supplements` | Bidirectional |
| `DocumentReference` | `MedicalRecord` (PDF/scan blobs) | Bidirectional |
| `AllergyIntolerance` | `PatientProfile.health_profile.allergies` | Bidirectional |

**Implementation options:**
1. **HAPI FHIR JVM sidecar** — Deploy as a companion container in the EKS pod. Configured with FHIR R4 validation, Healynx custom interceptors for authentication. Pros: Full FHIR compliance out-of-the-box. Cons: JVM overhead.
2. **Custom FastAPI FHIR module** — Lightweight FHIR R4 subset implementation. Handles only the resources listed above. Pros: No JVM dependency, smaller footprint. Cons: Must maintain FHIR compliance manually.

Recommended: HAPI FHIR sidecar for production (v2); custom FastAPI module for v2 prototype/demo.

### 8.2 Telemedicine Platform Integration — v2

```
┌──────────────────────┐         ┌──────────────────────┐
│  Telemedicine        │         │  Healynx       │
│  Platform            │         │                      │
│  (e.g., DoctorOnCall)│         │                      │
└──────────┬───────────┘         └──────────┬───────────┘
           │                                │
           │  Step 1: API Key Auth          │
           │  POST /partner/assessments     │
           │  X-API-Key: <partner_key>      │
           │  X-Partner-Tenant-ID: <uuid>   │
           │──────────────────────────────►│
           │                                │
           │◄───────────────────────────────│
           │  202 + task_id                 │
           │                                │
           │  Step 2: Webhook Callback      │
           │  (or poll GET /tasks/{id})     │
           │                                │
           │◄───────────────────────────────│
           │  POST {webhook_url}            │
           │  { assessment_id, status,      │
           │    risk_score }                │
           │                                │
           │  Step 3: Retrieve Results      │
           │  GET /assessments/{id}         │
           │──────────────────────────────►│
           │◄───────────────────────────────│
           │  Full AI recommendation +      │
           │  patient-facing output         │
           │                                │
```

**Integration workflow:**
1. Telemedicine platform registers as a partner tenant and receives an API key (with configurable rate limits per tier).
2. Before each teleconsultation, the platform embeds the supplement survey (via Healynx's embeddable React component or a direct API call).
3. Platform calls `POST /partner/assessments` with the patient's survey responses. Healynx processes and returns results.
4. Platform can register a webhook URL to receive push callbacks when AI analysis completes.
5. Platform retrieves the full AI output via `GET /assessments/{id}` and displays it in the consultant's pre-consultation view.

**Rate limits (per API tier):**

| Tier | Requests/Minute | Burst | Concurrent Connections |
|---|---|---|---|
| API Starter | 60 | 10 | 5 |
| API Growth | 300 | 50 | 20 |
| API Enterprise | Custom | Custom | Custom |

### 8.3 Wearable Device Data Ingestion — v3

```
┌──────────────┐    ┌──────────────┐    ┌──────────────────┐
│ Apple Health │    │ Google Fit   │    │ Healynx       │
│ / Garmin     │    │ / Samsung    │    │ Health Data       │
│              │    │   Health     │    │ Ingestion API     │
└──────┬───────┘    └──────┬───────┘    └────────┬─────────┘
       │                   │                     │
       │  Apple HealthKit  │  Google Fit REST    │
       │  Export (XML)     │  API (JSON)         │
       │                   │                     │
       ▼                   ▼                     │
┌──────────────────────────────────┐            │
│  Healynx Mobile App           │            │
│  (Health Data Aggregator)        │            │
│  • Reads Apple Health / Google   │            │
│    Fit data with user permission │            │
│  • Maps to standard schema       │            │
│  • Batch uploads periodically    │            │
└──────────────────┬───────────────┘            │
                   │                            │
                   │  POST /patients/{id}/      │
                   │  wearable-data              │
                   │────────────────────────────►│
                   │                            │
                   │  Standardised schema:       │
                   │  {                          │
                   │    "source": "apple_health",│
                   │    "data": [                │
                   │      {                      │
                   │        "metric": "heart_rate│
                   │        "value": 72,         │
                   │        "unit": "bpm",       │
                   │        "timestamp": "..."   │
                   │      },                     │
                   │      ...                    │
                   │    ]                        │
                   │  }                          │
                   │                            │
```

**Standardised health data format** maps to `Observation` FHIR resources internally. Supported metrics in v3: heart rate, blood pressure, blood glucose (from CGM), step count, sleep duration, weight. Data enriches the wellness score calculation (FR-06) and health trend visualisations (FR-16).

---

## 9. Performance & Scalability Benchmarks

### 9.1 Concurrent User Targets

| Metric | v1 Pilot (Phase 1) | v1 Launch (End Phase 1) | v2 (Month 18) | v3 (Month 24) |
|---|---|---|---|---|
| **Active clinics (tenants)** | 5 | 15 | 50 | 100+ |
| **Active clinicians (concurrent)** | 30 | 75 | 250 | 600 |
| **Active patients (MAU)** | 500 | 2,000 | 8,000 | 25,000 |
| **Concurrent API requests** | 50 | 200 | 750 | 2,000 |
| **Surveys submitted per day** | 100 | 500 | 2,000 | 6,000 |
| **Records uploaded per day** | 50 | 200 | 800 | 2,500 |
| **Share links generated per day** | N/A (v2) | N/A (v2) | 300 | 1,000 |

### 9.2 Latency Targets

| Operation | p50 | p95 | p99 | Max Acceptable |
|---|---|---|---|---|
| **Survey submission → 202 response** | 100ms | 250ms | 500ms | 1s |
| **AI supplement analysis (survey → result ready)** | 1.5s | 3s | 5s | 5s (SLA: 3s p95) |
| **Record metadata retrieval (GET /records/{id})** | 50ms | 200ms | 500ms | 1s |
| **AI summarisation (record → summary ready)** | 3s | 10s | 15s | 15s (SLA: 10s p95) |
| **Chatbot query response** | 2s | 5s | 8s | 8s (SLA: 5s p95) |
| **Patient dashboard load** | 500ms | 2s | 3s | 3s |
| **Clinician dashboard load (500 patients)** | 800ms | 3s | 5s | 5s |
| **Audit log query (50 entries)** | 200ms | 1s | 2s | 3s |
| **Emergency access activation** | 100ms | 300ms | 500ms | 2s |

### 9.3 Storage Growth Projections

| Data Type | Per Patient (Avg) | Per Clinic (100 patients) | 100 Clinics (10,000 patients) |
|---|---|---|---|
| **Record blobs (S3)** | 50 MB | 5 GB | 500 GB |
| **Survey JSON (PostgreSQL JSONB)** | 50 KB | 5 MB | 500 MB |
| **AI summaries (PostgreSQL TEXT)** | 100 KB | 10 MB | 1 GB |
| **Audit logs (PostgreSQL)** | 200 KB | 20 MB | 2 GB |
| **Total (1 year)** | ~51 MB | ~5.1 GB | ~510 GB |

**Growth rate:** 30% year-over-year as patients accumulate records. S3 lifecycle policy (transition to IA/Glacier) keeps hot storage manageable.

### 9.4 Scaling Strategy

| Component | Scaling Mechanism | Trigger |
|---|---|---|
| **API Pods (FastAPI)** | HPA (Horizontal Pod Autoscaler) on CPU > 70% AND request rate > 200 req/s per pod | Min: 3, Max: 20 |
| **AI Inference Pods (GPU)** | KEDA on Redis queue depth > 50 tasks | Min: 1, Max: 8 |
| **Celery Workers** | KEDA on Redis queue depth > 100 tasks | Min: 3, Max: 30 |
| **RDS PostgreSQL** | Vertical scaling (instance size up) at 80% CPU or 70% connection utilisation. Read replicas added at >500 concurrent connections. | `db.r6g.large` → `db.r6g.xlarge` → `db.r6g.2xlarge` |
| **Redis** | Cluster mode with auto-sharding when memory > 60% | Starts at 1 shard, scales to 5 |
| **Neo4j** | Vertical scaling. Read replicas in v2 for query fan-out. | `r6g.xlarge` → `r6g.2xlarge` |

### 9.5 ASEAN Multi-Region Deployment (v3)

```
┌───────────────────────────────┐     ┌───────────────────────────────┐
│  ap-southeast-6 (Malaysia)   │     │  ap-southeast-1 (Singapore)   │
│  ┌─────────────────────────┐ │     │  ┌─────────────────────────┐ │
│  │ Primary Deployment      │ │     │  │ DR Standby (v1-v2)     │ │
│  │ • All services active   │ │     │  │ → Active Region (v3)   │ │
│  │ • Malaysian tenants     │ │     │  │ • Singapore tenants    │ │
│  └─────────────────────────┘ │     │  └─────────────────────────┘ │
└───────────────────────────────┘     └───────────────────────────────┘

┌───────────────────────────────┐     ┌───────────────────────────────┐
│  ap-southeast-3 (Jakarta)    │     │  ap-southeast-7 (Bangkok)     │
│  ┌─────────────────────────┐ │     │  ┌─────────────────────────┐ │
│  │ • Indonesian tenants    │ │     │  │ • Thai tenants          │ │
│  │ • Bahasa Indonesia UI   │ │     │  │ • Thai language UI      │ │
│  │ • Jamu supplement DB    │ │     │  │ • TCM supplement DB     │ │
│  └─────────────────────────┘ │     │  └─────────────────────────┘ │
└───────────────────────────────┘     └───────────────────────────────┘
```

Each region is a self-contained deployment with its own data store. Tenant data never crosses regions (PDPA data residency). Global services: Route 53 latency-based routing, shared CI/CD pipeline, centralised monitoring dashboard, federated admin console.

---

## 10. Technical Risks & Mitigations

| # | Technical Risk | Probability | Impact | IP Affected | Mitigation | Contingency |
|---|---|---|---|---|---|---|
| **TR-1** | **AI accuracy on local population:** Supplement interaction model trained on UM dataset underperforms on diverse Malaysian population (varying ethnicity, traditional remedies, polypharmacy patterns). | Medium | High | IP 2 | Expand training dataset during Phase 1 validation at UM Medical Centre + partner clinics to capture demographic diversity. Continuous learning pipeline: clinician feedback (flag incorrect) fed into quarterly retraining. Confidence scores displayed so clinicians calibrate trust. | If accuracy <80% sensitivity at v1 launch gate, scope v1 to "assisted review" mode (AI flags for clinician verification, no autonomous alerts). Delay automated alerting to v2 after retraining. |
| **TR-2** | **Real-time AI latency exceeds SLA:** Supplement analysis takes >3 seconds or summarisation >10 seconds at scale, violating NFR-05. | Medium | High | IP 1 + IP 2 | Model optimisation: ONNX export and quantisation (FP16/INT8) for faster inference. GPU node group for LLM and summarisation workloads. Redis caching of common supplement interaction queries (hot path). Async task queue with WebSocket push (user doesn't block on result). | If optimisation insufficient: (a) Increase p95 SLA from 3s to 5s for pilot launch, (b) pre-compute supplement interaction graph offline and serve from materialised cache, (c) summarisation model → smaller model (DistilBART) with acceptable quality tradeoff. |
| **TR-3** | **Multi-tenant data isolation breach:** A bug or misconfiguration allows Tenant A to access Tenant B's patient data. | Low | Catastrophic | IP 1 | PostgreSQL Row-Level Security as defence-in-depth. Integration test suite that deliberately attempts cross-tenant access on every PR. Penetration test before each release targeting tenant isolation. Audit log anomaly detection (cross-tenant access pattern alerting). | If breach suspected: immediate tenant isolation (revoke DB access for affected tenant), forensic analysis, PDPA breach notification protocol. Insurance coverage for regulatory penalties. |
| **TR-4** | **Legacy EMR incompatibility:** Partner clinics use proprietary, non-FHIR EMR systems that cannot integrate without custom connectors. | Medium | Medium | IP 1 | Architecture designed with an FHIR adapter abstraction layer. For non-FHIR systems, build custom ETL connectors as billable professional services (not core product). Prioritise FHIR-only for standard product; custom work is separate revenue stream. | If key partner clinic has critical legacy EMR: scope a one-off HL7 v2 → FHIR bridge as a consulting engagement. Document as a reference integration to inform future adapter development. |
| **TR-5** | **LLM hallucination in chatbot:** Patient or clinician chatbot generates incorrect medical information, creating clinical risk. | Medium | Critical | IP 1 | RAG constrains LLM to retrieved document context only. Output guardrails: PII filter, hallucination detector (ROUGE-L vs context chunks), mandatory medical disclaimer, clinician-in-the-loop for all clinical chatbot features. Log every chatbot interaction for audit. Clinician chatbot queries are logged and can be reviewed by administrators. | If hallucination rate >5% in testing: replace open-ended LLM with template-based response system (deterministic, no hallucination risk). Reserve LLM for non-clinical queries only. |
| **TR-6** | **Neo4j graph database performance:** Drug-supplement interaction graph queries slow as supplement product database grows (500 → 5,000+ products). | Low | Medium | IP 2 | Neo4j indexes on supplement name and medication name. Query optimisation: limit traversal depth, use parameterised Cypher. Pre-compute and cache interaction lookup table for the 500 most common supplement-drug pairs in Redis (hot path). Graph partitioning by therapeutic category if needed. | If graph queries exceed 500ms: migrate interaction lookup to PostgreSQL materialised view (denormalised, indexed). Retain Neo4j only for the product database and analytics, not for the real-time query path. |
| **TR-7** | **GPU availability in Malaysian cloud region:** AWS ap-southeast-6 does not launch with GPU instance types, blocking self-hosted LLM inference. | Low | High | IP 1 | Architecture abstracts AI inference behind a service interface — can run on CPU (slower), GPU in Singapore region (with PDPA-compliant data handling), or external GPU cloud (e.g., Lambda Labs). Model quantisation reduces GPU requirement (INT8 can run on CPU if needed). | Fallback: Deploy AI inference in ap-southeast-1 (Singapore) with a Data Processing Agreement ensuring data in transit is encrypted and processed in memory only. Return results to Malaysian region for persistence. |
| **TR-8** | **Key management failure:** KMS outage or key loss prevents decryption of patient records. | Very Low | Catastrophic | IP 1 | Multi-region KMS key replication. Offline key backup in hardware security module (HSM) stored in secure physical location (UM or bank vault). Key ceremony documented and tested quarterly. CloudHSM for production. | Disaster recovery runbook: KMS failover to Singapore region. Key recovery from offline HSM. Full restoration test conducted quarterly. |
| **TR-9** | **WebSocket blocked by clinic firewalls:** Clinics in Malaysia often have restrictive network policies blocking WebSocket connections. | Medium | Low | IP 1 + IP 2 | Polyfill: automatic fallback to HTTP long-polling (10-second interval) if WebSocket handshake fails within 3 seconds. Connection health monitoring per tenant to identify clinics needing firewall whitelist documentation. | Provide clinic IT administrators with a one-page firewall configuration document: allow outbound TLS 1.3 on port 443 to `*.Healynx.ai`. If impossible: polling-only mode with configurable interval. |
| **TR-10** | **UM IP codebase quality:** The research code from UM inventors is not production-grade (no tests, no error handling, hardcoded paths, Python 2/3 incompatibility). | High | Medium | IP 1 + IP 2 | Knowledge transfer (Dependency D2) must include code walkthroughs, not just code handover. Plan for code reimplementation (port logic, not copy code) in the production stack. Allocate engineering time specifically for "IP translation" in the project plan — treat the research code as a reference implementation, not the foundation. | If IP code is unusable: reimplement the AI algorithm and scoring model from the patent specification and published methodology. UM inventors serve as domain expert reviewers, not code contributors. |

---

## Appendix A: Technical Decision Records (TDR) Index

Key architectural decisions, each with context, options considered, decision, and rationale:

| TDR # | Decision | Section |
|---|---|---|
| TDR-01 | PostgreSQL as primary database (not MongoDB, not MySQL) | 1.2 — Database |
| TDR-02 | Shared DB with RLS for multi-tenancy (not DB-per-tenant, not schema-per-tenant) | 2.4 |
| TDR-03 | FastAPI + Celery async pattern (not synchronous Django, not Node.js) | 1.2 — Backend |
| TDR-04 | Self-hosted LLM for chatbot (not OpenAI/Anthropic API) | 1.2 — AI/ML |
| TDR-05 | REST API for v1, GraphQL deferred to v2 | 1.2 — Backend |
| TDR-06 | Neo4j for supplement interaction graph (not PostgreSQL recursive CTE, not RedisGraph) | 1.2 — AI/ML |
| TDR-07 | OPA for RBAC policy (not hardcoded decorators, not Casbin) | 1.2 — Security |
| TDR-08 | Keycloak for Auth (not Auth0, not AWS Cognito, not custom) | 1.2 — Security |
| TDR-09 | Kubernetes on EKS (not ECS Fargate, not raw EC2, not Lambda-only) | 7.1 — Cloud Architecture |
| TDR-10 | Canary deployment via ArgoCD (not blue-green, not rolling update only) | 7.2 — CI/CD |

---

## Appendix B: Glossary of Technical Terms

| Term | Definition |
|---|---|
| **BentoML** | Open-source model serving framework for packaging and deploying ML models as standardised services. |
| **Celery** | Distributed task queue for Python, using message brokers (Redis/RabbitMQ) to dispatch async work to worker processes. |
| **Cypher** | Neo4j's declarative graph query language, equivalent to SQL for relational databases. |
| **DEK / CMK** | Data Encryption Key / Customer Master Key — envelope encryption pattern where a per-record key (DEK) encrypts data, and a master key (CMK) in KMS encrypts the DEK. |
| **EKS / AKS / GKE** | Managed Kubernetes services from AWS, Azure, and GCP respectively. |
| **FHIR R4** | Fast Healthcare Interoperability Resources Release 4 — HL7 standard for electronic health information exchange. |
| **HPA / KEDA** | Horizontal Pod Autoscaler (Kubernetes native, CPU/memory-based) and Kubernetes Event-Driven Autoscaling (scales on external metrics like queue depth). |
| **JWT** | JSON Web Token — compact, URL-safe token for transmitting claims between parties. Used for authentication and authorisation. |
| **KMS** | Key Management Service — cloud service for creating, managing, and rotating cryptographic keys. |
| **Neo4j** | Graph database management system, used here for drug-supplement interaction knowledge base. |
| **OPA** | Open Policy Agent — general-purpose policy engine that evaluates Rego policies for authorisation decisions. |
| **pgvector** | PostgreSQL extension for vector similarity search, enabling RAG without a separate vector database. |
| **RAG** | Retrieval-Augmented Generation — technique where an LLM's responses are grounded in retrieved documents rather than relying solely on training data. |
| **RBAC** | Role-Based Access Control — restricting system access based on user roles. |
| **RLS** | Row-Level Security — PostgreSQL feature that restricts which rows a database user can access based on policy expressions. |
| **ROUGE-L** | Recall-Oriented Understudy for Gisting Evaluation — a metric for evaluating text summarisation by measuring longest common subsequence. |
| **RPO / RTO** | Recovery Point Objective (max data loss measured in time) / Recovery Time Objective (max time to restore service). |

---

*This TRD is the single source of technical truth for Healynx v1 through v3. All engineering sprints, architecture reviews, and infrastructure provisioning shall reference decisions and specifications in this document. Deviations require a formal Architecture Decision Record (ADR) approved by the solutions architect.*
