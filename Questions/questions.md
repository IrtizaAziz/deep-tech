# Healynx AI — Judge Q&A Preparation
## National Deep Tech Challenge 2026 | Universiti Malaya

---

## PROBLEM & MARKET

**Q1. What exactly is the "clinical blind spot" you're solving, and how big is it really?**

The blind spot is the gap between what patients consume — supplements, herbal remedies, traditional jamu preparations — and what their doctors actually know about. Today, patients routinely fail to disclose supplement use during consultations, and no clinical system in Malaysia checks for drug-supplement interactions automatically. 60% of GP caseload is chronic disease patients who are typically on multiple medications and supplements simultaneously. Our persona, Mr. Rajan — 58 years old, Type 2 diabetes and hypertension — takes 3 prescription meds and 4 supplements, and his doctor knows nothing about those supplements. This isn't an edge case; it's the norm across Malaysian clinics every day.

---

**Q2. Where does the 4.2/5 pain score come from? Is it self-reported or independently validated?**

The pain score is a composite across five structured dimensions: Intensity (5/5 — drug-supplement interactions can cause hospitalisation, organ damage, or death), Frequency (4/5 — supplement non-disclosure happens at nearly every consultation for chronic patients), Cost (4/5 — 15–20% of clinical consultation time is wasted on manual record review), Urgency (4/5 — rising chronic disease burden and active MOH digital health mandates with no Malaysian solution), and Reachability (4/5 — buyers concentrated in urban hubs accessible via medical associations). The methodology is based on standard startup pain scoring frameworks applied to data sourced from our PRD and market research. We acknowledge it is an internal framework, but each dimension is grounded in quantifiable evidence.

---

**Q3. How did you arrive at the RM 1.75 billion TAM figure?**

The TAM represents the total annual digital health spend across all addressable segments in Malaysia: approximately 8,000 private clinics (RM 400M+), 220 private hospitals (RM 600M+), 25+ telemedicine platforms (RM 150M+), 1.1M+ corporate employers (RM 300M+), 40+ insurance companies (RM 200M+), and around 3,000 pharmacies (RM 100M+). Our SAM of RM 350M focuses on the 3,500 private clinics in urban hubs plus 15 active telemedicine platforms and 800 mid-to-large corporate employers. Our Year 3 SOM target is RM 780K ARR from 98 accounts — a very conservative 0.22% of SAM — which makes it a credible and achievable near-term target.

---

**Q4. Why is supplement non-disclosure such a persistent problem? Can't doctors just ask patients?**

They do ask — and patients still don't disclose, for several reasons. Many patients don't consider supplements or traditional remedies to be "medicine," so they don't mention them. Others feel embarrassed or worry the doctor will disapprove of their traditional practices. The consultation time pressure (40–50 patients per day for a typical GP) means there's rarely time for a thorough supplement intake conversation. And even when a doctor asks, there's no structured system to capture, store, or cross-check those answers against the patient's prescription list. Healynx solves this at the system level rather than relying on patient honesty or clinician memory.

---

**Q5. Is drug-supplement interaction detection a solved problem globally? What makes this hard?**

Globally, some clinical decision support tools exist (Drugs.com, Lexicomp, Clinical Pharmacology), but they focus almost entirely on pharmaceutical drug-drug interactions and Western supplement databases. The Malaysian context introduces unique complexity: traditional Jamu preparations, TCM herbs, and Ayurvedic compounds that are commonly consumed but largely absent from Western databases. There is no expert-labelled Malaysian clinical dataset for these interactions. That's the core technical moat we're building — a proprietary dataset trained on Malaysian supplement usage patterns, verified by clinical pharmacists.

---

## TECHNOLOGY & AI

**Q6. How does your AI supplement interaction detection actually work?**

We use a dual-engine architecture. The first engine is an AI classification model trained on an expert-verified, pre-labelled Malaysian supplement interaction dataset. It detects drug-supplement and supplement-supplement interactions by pattern-matching the patient's reported supplements against their known medications and health conditions. The second engine is a statistical risk scoring model that evaluates patient-specific variables — dosage, frequency, duration, comorbidities — to produce a numeric risk score from 0 to 100 and a categorical severity level (High, Moderate, Low). The system uses a Neo4j graph database engine for complex multi-hop interaction detection (e.g., Supplement A interacts with Drug B, which affects Condition C), and ONNX-optimised inference for sub-3-second response times.

---

**Q7. What is your target AI accuracy and have you validated it yet?**

Our target is ≥85% sensitivity against clinical pharmacist review. This means that for every 100 true interactions, our system should flag at least 85. We are currently at TRL 5 — the system is validated in a relevant environment but has not yet undergone full clinical validation. Phase 1 of our roadmap (Months 1–9) is specifically focused on AI accuracy benchmarking at UM Medical Centre with 5 pilot sites and 200+ patients. The ≥85% sensitivity target is the Phase 1 exit gate before we move to commercial partnerships.

---

**Q8. Why Neo4j specifically? Couldn't you use a relational database for interaction detection?**

Drug-supplement interactions are inherently graph problems. A single patient may have 5 supplements interacting with 3 drugs across 2 chronic conditions — that's a multi-hop traversal that relational databases handle poorly at query speed. Neo4j's native graph structure allows us to traverse these relationships in milliseconds without expensive JOIN operations. For example, detecting that Garlic Extract lowers blood pressure AND that the patient is on Amlodipine (a calcium channel blocker that also lowers blood pressure), creating an additive hypotensive risk, requires a three-hop graph query. Neo4j handles this naturally.

---

**Q9. What models are you using in your AI pipeline?**

Our pipeline uses BART-large fine-tuned for clinical summarisation — it generates the AI-written patient summaries that appear on the clinician dashboard (e.g., "58M, T2DM + HTN. Garlic extract + jamu presents BP-lowering interaction risk. HbA1c stable"). For the supplement intelligence layer we use Mistral 7B with RAG (Retrieval-Augmented Generation) to ground responses in our proprietary Malaysian supplement dataset. ONNX runtime is used for optimised model inference to meet our ≤3 second SLA.

---

**Q10. What happens if the AI is wrong? What safeguards do you have?**

Every AI output carries a confidence score and is explicitly labelled as clinical decision support, not a clinical decision. The clinician must acknowledge every high-severity alert — we enforce this at the UI level with a mandatory acknowledgement modal before any alert is dismissed. Low and moderate alerts are surfaced for review but don't block workflow. All AI recommendations are evidence-linked to source literature. Our terms and privacy policy clearly state that all outputs require clinician verification. The AI is a risk-flagging tool, not a diagnostic or prescribing tool.

---

**Q11. How do you handle supplements that are not in your database?**

If a supplement is unrecognised, the system flags it as "Unknown — Clinical Review Recommended" rather than silently passing it. The clinician is notified. We also have a continuous data pipeline where unrecognised substances are flagged for expert review and addition to the database. Over time, this builds the network effect — more users report more supplements, which expands coverage and improves the model for everyone.

---

**Q12. What is your data source for training the AI? Is it Malaysian-specific?**

Yes. The proprietary dataset is built from expert-labelled Malaysian clinical data collected in partnership with Universiti Malaya. It covers drug-supplement interactions specific to common Malaysian supplement categories including traditional Jamu, TCM herbs, and Ayurvedic compounds that are absent from Western databases. This dataset is years in the making and is one of the primary competitive moats — it is not publicly available and is very hard to replicate.

---

## INTELLECTUAL PROPERTY & TECHNOLOGY MOAT

**Q13. You claim 2 UM patents — what exactly do they cover?**

Patent PI 2025007955 covers "Secure Medical Records Management in AI-Enhanced Systems" — this is the IP underpinning our encrypted medical records vault, RBAC governance, and AI-enhanced clinical record management. Patent UI 2025002953 covers "Interactive Patient Supplement Intake Assessment Method" — this is the IP for our structured supplement intake survey and AI interaction assessment methodology. Both are at TRL 5. Both are exclusively licensed from Universiti Malaya to Healynx, providing a legal barrier to entry. The inventors, Dr. Lee Ching Shya and Dr. Nurul Fauzani Jamaluddin, are our key advisors.

---

**Q14. What does "exclusively licensed from Universiti Malaya" mean practically? Can UM revoke the license?**

An exclusive licence means no other commercial entity can use these patents without our permission during the licence term. The licence agreement specifies terms, royalties, and conditions. Revocation would only occur if we materially breach the licence terms — typically failure to pay royalties or abandonment of commercialisation efforts. Our close relationship with UM (5 UM CS founders, IP inventors as advisors, UM Medical Centre as pilot site) significantly reduces this risk. The licence is a formal legal instrument, not an informal agreement.

---

**Q15. What prevents a well-funded competitor from building the same thing without infringing your patents?**

The patents create a legal barrier but not an impenetrable wall. Our real moat is multi-layered: (1) the proprietary Malaysian supplement interaction dataset — years to collect and label, impossible to buy; (2) regulatory first-mover advantage — PDPA compliance, ISO 27001 alignment, and MDA regulatory pathway documentation takes 12–18 months to replicate; (3) data network effects — our dataset improves with every patient that uses the platform, widening the accuracy gap over time; (4) switching costs — once a clinic's patient records are digitised and AI-enriched on Healynx, migrating is costly and disruptive; (5) UM institutional relationships including the pilot at UM Medical Centre.

---

## PRODUCT & USER EXPERIENCE

**Q16. Walk us through how a patient uses Healynx from start to finish.**

A patient opens the Healynx mobile app and completes a structured supplement intake survey — roughly 3 minutes. The survey captures supplement name, dosage, frequency, duration, the health condition being addressed, existing medications, allergies, and comorbidities. This is pre-populated from the last survey for returning patients. Upon submission, the dual-engine AI processes the data in under 3 seconds. The patient receives their Wellness Score (0–100), colour-coded supplement safety ratings (green/yellow/red), and personalised recommendations. They can tap "Share with Doctor" to securely share their results before their next consultation.

---

**Q17. Walk us through the clinician experience.**

The clinician logs into the Healynx web dashboard. High-severity interaction alerts surface immediately via real-time WebSocket notifications — there's no need to refresh. The dashboard shows a patient list with name, age, last visit, wellness score, alert counts by severity, and a one-line AI-generated summary. Clicking into a patient shows the full AI-generated clinical summary (with confidence score), drug-supplement interaction cards by severity, and the full supplement history. The clinician must click "Acknowledge" on high-severity alerts. The AI chatbot allows natural-language queries of patient records — e.g., "Which of my patients on warfarin are taking fish oil?"

---

**Q18. What does the Patient Wellness Score measure and how is it calculated?**

The Patient Wellness Score is a dynamic 0–100 composite derived from three dimensions: safety profile (supplement-drug interaction severity and count), medication adherence (reported adherence to prescribed medications), and lifestyle factors (diet, exercise, sleep patterns from the intake survey). A score of 72 with a +14 improvement indicates improving safety and adherence over time. The score is designed to be actionable — it tells both the patient and clinician whether things are getting better or worse, and which factor is driving the change.

---

**Q19. Have you tested the product with real doctors or patients?**

We have live prototypes — both a Doctor & Admin Portal and a Patient App. The prototypes are functional and have been built to demonstrate the core user flows. We have not yet conducted formal clinical validation — that is the purpose of Phase 1 (Months 1–9) at UM Medical Centre with 5 pilot sites and 200+ patients. The pilot will also include UX research and iteration with real clinicians. Our close ties to UM Medical Centre give us privileged access to this validation environment.

---

**Q20. How long does the patient survey take, and will patients actually complete it?**

The survey takes approximately 3 minutes. We've designed it to be pre-populated for returning patients, so repeat visits take even less time. Survey completion is an acknowledged challenge in digital health. Our approach addresses it in two ways: (1) the survey is integrated into the clinical pathway — the doctor or clinic receptionist prompts patients to complete it before the consultation, so there's institutional encouragement; (2) the patient immediately receives their Wellness Score and supplement risk colour-coding, giving instant value that makes completion worthwhile.

---

## SECURITY & COMPLIANCE

**Q21. How do you ensure patient data security?**

We use AES-256-GCM envelope encryption for all data at rest, with AWS KMS key rotation. Data in transit is protected by TLS 1.3. Data is stored in AWS Malaysia cloud regions (data residency). Access is controlled via Role-Based Access Control (RBAC) — clinicians can only see their own patients, admins have configurable access, and all access is logged in immutable audit trails. Time-limited sharing links allow patients to share data with specific clinicians for a defined period. Emergency access override is available 24/7 with automatic audit logging. We are PDPA 2010 compliant and ISO 27001 aligned.

---

**Q22. What is PDPA 2010 and how does Healynx comply?**

The Personal Data Protection Act 2010 is Malaysia's primary data privacy legislation governing the collection, processing, and storage of personal data. Healynx complies by: obtaining explicit patient consent before collecting health data, storing data only in Malaysian cloud regions, providing patients with access, correction, and deletion rights, maintaining a Data Protection Officer role, implementing the technical safeguards required under PDPA (encryption, access controls, audit trails), and maintaining a breach notification process. We have also aligned with ISO 27001 information security standards as a higher-bar voluntary commitment.

---

**Q23. What happens if there's a data breach?**

Our breach response protocol includes: immediate incident detection via AWS GuardDuty and CloudTrail monitoring, containment within 1 hour, patient notification within 72 hours as required by PDPA, regulatory notification to the Personal Data Protection Commissioner, and a post-incident review. All access to patient data is logged immutably — we can reconstruct exactly who accessed what and when. Our AES-256-GCM encryption means that even if data is exfiltrated, it is unreadable without the KMS keys, which are never co-located with the data.

---

**Q24. You mention "time-limited sharing" — how does that work?**

A patient can tap "Share with Doctor" in the app, which generates a time-limited secure link (e.g., valid for 24 hours). The clinician receives the link and can view the patient's supplement profile and AI analysis. After the time limit expires, the link is revoked and the clinician no longer has access. This ensures that sharing is purposeful and temporary rather than permanent and uncontrolled. The sharing event is logged in the audit trail. Emergency override allows clinicians in acute care situations to access a patient's record without a pre-shared link, but this generates an automatic alert and full audit entry.

---

## BUSINESS MODEL & FINANCIALS

**Q25. Why a B2B SaaS model targeting clinics rather than going direct-to-consumer?**

Three reasons. First, trust and clinical authority: patients are far more likely to act on supplement risk information that comes through their doctor than through a consumer app. The clinical context makes the data meaningful and actionable. Second, willingness to pay: clinics and healthcare providers have institutional budgets, longer contract cycles, and higher LTV than individual consumers. Third, data quality: integrating into the clinical workflow means supplement data is captured at point of care with clinical context, making the AI more accurate and the product more defensible.

---

**Q26. Your Essentials tier is RM 350/month. How did you arrive at that price point?**

RM 350/month (RM 4,200/year) is positioned below the threshold where clinics need significant budget approval — most clinic managers can authorise this level of spend independently. Comparable medical software in Malaysia (practice management systems, EMRs) typically costs RM 300–800/month, so we're within the established price norm. At RM 350/month, a clinic with 500 active patients is paying roughly RM 0.70 per patient per month for AI-powered supplement safety — an easy ROI argument given the medico-legal liability it mitigates.

---

**Q27. How do you get to a 72% gross margin by Year 5?**

Our major costs are AI model inference (cloud compute), AWS infrastructure, and personnel. As we scale, the AI inference cost per query drops due to model optimisation (ONNX runtime) and better batch processing. Infrastructure costs grow sub-linearly relative to revenue because we're a SaaS platform with high software leverage. By Year 5, with 200+ accounts generating RM 3.5M ARR, fixed costs (infrastructure, core team) are spread across a much larger revenue base, driving margin expansion from our current early-stage 55% toward the 72% target.

---

**Q28. What is your customer acquisition strategy? How do you reach clinics?**

Our go-to-market is a combination of: (1) institutional anchoring — UM Medical Centre as the first pilot creates credibility and case studies; (2) medical association channels — Malaysian Medical Association (MMA) and Federation of Private Medical Practitioners' Associations (FPMPAM) are our primary B2B channels to reach private GPs; (3) direct outreach to clinic groups — KPJ and Columbia Asia are target early adopters for multi-branch deployment; (4) telemedicine platform partnerships — API integrations with platforms like DoctorOnCall embed Healynx into existing digital health ecosystems; (5) pharmacy chain partnerships in Phase 2 for cross-referral.

---

**Q29. Your LTV:CAC ratio is 5:1 to 8:1. How did you calculate this?**

ARPU is ~RM 5,000/year per clinic. Estimated average contract duration is 3–4 years (high switching cost once records are digitised). This gives an LTV of RM 15,000–20,000 per clinic. CAC for B2B clinic sales via direct outreach and association channels is estimated at RM 2,000–4,000 per account (sales team cost divided by accounts won). This yields LTV:CAC of 5:1 to 8:1. We'll refine these numbers through Phase 1 pilot data where we track actual churn, expansion, and sales cycle metrics.

---

**Q30. When do you break even, and what assumptions underlie that?**

Break-even is projected at Month 30–36. Key assumptions: RM 1.5M seed funding at Month 0, lean team of 5 (RM 55K/month burn), scaling to 8 people at Month 12 (RM 85K/month), 40+ paying accounts by Month 24 at average RM 700/month ARPU, RM 32K MRR target by Month 24. Revenue ramp follows the Phase 1 pilot (free/subsidised) → Phase 2 first paying accounts (15 accounts, RM 5K MRR) → Phase 3 commercial scale. If funding is delayed, our bootstrap contingency via government grants (MDEC, CREST) provides a 6–12 month delay with 0% dilution.

---

## COMPETITION & DEFENSIBILITY

**Q31. DoctorOnCall and other telemedicine platforms already have millions of users. Why can't they just add supplement tracking?**

They could add a basic supplement logging feature, but that's not what we've built. Healynx's clinical value is in the AI interaction detection, which requires: (1) a proprietary expert-labelled dataset of Malaysian supplement interactions — this doesn't exist off the shelf; (2) a graph database architecture for multi-hop interaction detection; (3) clinical validation and AI accuracy benchmarking; (4) PDPA-compliant medical record integration. Adding this to a telemedicine platform would take 18–24 months minimum and require them to build a capability that is not their core competency. Meanwhile, we can partner with telemedicine platforms via API licensing — making us a complement, not just a competitor.

---

**Q32. What about traditional EMR systems like Caresys or Mediviron? Can't they add AI?**

Traditional EMRs are built on legacy architectures optimised for administrative functions — billing, scheduling, basic record storage. Adding AI interaction detection requires a fundamentally different data model (graph-based), inference infrastructure, and a supplement dataset that these vendors don't possess. They would need to build what we've already built, while we're already deploying at UM Medical Centre. Furthermore, traditional EMRs are not mobile-first and don't have the patient-facing wellness layer that drives supplement disclosure in the first place.

---

**Q33. What if a large tech company (Google Health, Microsoft Azure Health) enters this space?**

Large tech companies face significant localisation challenges in Malaysian healthcare. PDPA 2010 requires Malaysian data residency — which limits the ability to use hyperscaler global AI models on patient data. The Malaysian supplement interaction dataset requires local clinical expertise and UM partnerships that cannot be acquired quickly. Our TRL 5 patents and UM institutional alignment create regulatory and IP barriers. And large tech companies typically partner with local health-tech players rather than building from scratch in emerging markets — which is actually an acquisition opportunity for us in the long term.

---

## ROADMAP & EXECUTION

**Q34. What specifically happens in Phase 1, and what does success look like?**

Phase 1 (Months 1–9) is clinical validation at UM Medical Centre. Specific deliverables: 5 pilot sites deployed, 30 clinicians onboarded, 200+ patients surveyed, AI sensitivity benchmarked against clinical pharmacist review. Success criteria (exit gate): AI sensitivity ≥85%, zero critical security findings from penetration testing, positive clinician UX feedback, and regulatory pathway for MDA determined. This phase is primarily about proving the AI accuracy claim and collecting the clinical evidence needed to sell to paying customers in Phase 2.

---

**Q35. Why 9 months for Phase 1? That seems long for a team that has live prototypes.**

The prototypes demonstrate the technical feasibility of the product. Phase 1 is about clinical validation, which has a different timeline driven by: ethics committee approval at UM (typically 2–3 months), patient recruitment and onboarding (ongoing through Month 9), iterative UX cycles with real clinicians who have specific workflow requirements, AI accuracy benchmarking against a clinical pharmacist gold standard (requires a meaningful patient dataset), and penetration testing and security audit (typically 1–2 months for a thorough engagement). The 9-month timeline is realistic for regulated healthcare software.

---

**Q36. What is your Plan B if seed funding doesn't come through?**

We have a concrete bootstrap contingency: apply simultaneously to 5 Malaysian government grants by Month 2 — CREST, MDEC GAH, UMCIE, MIDA, and MTDC. Even securing 2 of these gives us RM 275K+ bridge funding. We have partner pre-commitment conversations with KPJ/Columbia Asia (RM 30–60K) and DoctorOnCall (RM 40–50K). AWS Activate provides cloud credits worth RM 75–100K. UM provides infrastructure and the pilot environment. This path achieves the same product milestones 6–12 months later with 0% dilution. Bootstrap Year 3 ARR target would be RM 450K (vs RM 780K on the funded path).

---

**Q37. What are the biggest risks to your plan and how are you mitigating them?**

Five key risks: (1) AI accuracy — mitigated by our proprietary dataset and ≥85% sensitivity gate before any commercial deployment; (2) clinical adoption resistance — mitigated by piloting at UM Medical Centre with IP inventors as advisors, using institutional credibility rather than cold outreach; (3) regulatory uncertainty around AI in clinical settings — mitigated by early MDA regulatory pathway determination in Phase 1 and ISO 27001 alignment; (4) data breach — mitigated by AES-256-GCM encryption, AWS Malaysia residency, immutable audit trails, and penetration testing; (5) funding delay — mitigated by the detailed bootstrap contingency plan above.

---

## TEAM

**Q38. You're a team of 5 computer science students. Do you have medical expertise?**

We're technology builders partnered with domain experts. Dr. Lee Ching Shya and Dr. Nurul Fauzani Jamaluddin — the IP inventors and our key advisors — are registered Universiti Malaya academics with clinical and research expertise. They provide the medical domain validation for our AI models and clinical methodology. Our pilot at UM Medical Centre embeds us in a clinical environment with access to GPs and clinical pharmacists for ongoing validation. We are not building this as pure technologists — we are building the technology layer that domain experts have validated and will continue to advise.

---

**Q39. Why are you the right team to solve this problem?**

Four reasons specific to us: (1) IP access — we have exclusive licence to both UM patents; other teams cannot legally use these inventions; (2) institutional embedding — we are UM CS students, giving us privileged access to UM Medical Centre, UM faculty advisors, and UM's institutional credibility; (3) hackathon-proven execution — we have demonstrated the ability to build and ship live prototypes under time pressure; (4) founder-market fit — we identified this problem from observing the Malaysian healthcare system firsthand and we understand the nuances of supplement culture in Southeast Asia that a foreign team would not.

---

**Q40. What are each founder's specific roles?**

Our team of 5 co-founders from Universiti Malaya's Computer Science programme covers the full product stack: full-stack development (web and mobile), AI/ML engineering, backend architecture and security infrastructure, product design and UX, and business development. Specific role assignments are being formalised as we move toward Phase 1. The IP inventors (Dr. Lee and Dr. Nurul Fauzani) serve as clinical and academic advisors guiding the AI validation methodology and regulatory strategy.

---

## VISION & IMPACT

**Q41. What is the long-term vision beyond Malaysia?**

Healynx's long-term vision is to become the regional AI preventive healthcare intelligence infrastructure across ASEAN — the intelligence layer connecting patients, providers, payers, and policymakers. The expansion roadmap is: Singapore (Year 2–3), Indonesia and Thailand (Year 3–4), Vietnam and Philippines (Year 4–5). ASEAN represents 670M+ additional addressable population. The strategic rationale for this sequence: Singapore for regulatory sophistication and regional credibility, Indonesia for scale (270M population with high supplement consumption), Thailand for medical tourism anchor clinics. By Year 5, we're targeting 200–380 paying accounts and RM 3.5M ARR.

---

**Q42. How does Healynx fit into Malaysia's national digital health agenda?**

We are directly aligned with the MOH's digital health mandates and EMR implementation directives. Malaysia is actively pushing healthcare digitalisation, which creates the infrastructure demand and regulatory environment for cloud-based clinical platforms. PDPA enforcement — which became stricter in recent years — turns compliance into a competitive advantage for early movers like Healynx. Our AWS Malaysia region deployment and PDPA 2010 compliance positions us as a model for what compliant health-tech in Malaysia looks like, which we believe will be attractive to MOH partnerships in Phase 2 and beyond.

---

**Q43. Is there a public health angle to the data you're collecting?**

Yes, and it's a significant long-term revenue and impact opportunity. With patient consent and full anonymisation, aggregate supplement consumption patterns across a large Malaysian patient population represent valuable public health intelligence. Which supplements are most commonly taken alongside which medications? Which combination presents the highest population-level risk? This anonymised data can be licensed to public health researchers, insurance actuaries, and pharmaceutical companies for supplement safety studies. This is the "Data Analytics" revenue stream (5% initially, growing over time) in our business model.

---

**Q44. How do you think about the ethical implications of AI in clinical decision-making?**

We hold a firm principle: AI is decision support, not decision-making. Every AI output is confidence-scored and explicitly labelled as requiring clinician verification. High-severity alerts require mandatory clinician acknowledgement — the system will not dismiss an alert without clinician sign-off. We maintain full audit trails so that every AI recommendation and every clinician action is traceable. We are designing for AI-assisted clinical workflows, not AI-replacement of clinical judgement. Our advisors (both clinical academics) actively shape the ethical guardrails in the product design.

---

**Q45. What does "TRL 5" mean and what does it tell us about where you are technically?**

TRL stands for Technology Readiness Level, a standard scale from 1 (basic principles) to 9 (fully operational in real-world deployment). TRL 5 means the technology has been validated in a relevant environment — i.e., the core functions work and have been tested in a setting representative of real use, but not yet in an operational clinical environment at scale. For Healynx, TRL 5 means both patents are validated technically, live prototypes are functional, and the system is ready for Phase 1 clinical validation pilots. TRL 6 (validation in operational environment) is the Phase 1 exit gate.

---

**Q46. What would you do differently if you were starting over?**

We would begin building the clinical advisory board and seeking ethics committee approval earlier — these are long-lead-time items in healthcare that we now understand better. We would also have initiated the MDA regulatory pathway enquiry from Day 1 to avoid discovering late-stage regulatory requirements. On the technical side, starting with the proprietary dataset from the earliest possible point (rather than building the AI first and sourcing data second) would compress Phase 1 timelines. These lessons are already incorporated into our Phase 1 plan.

---

**Q47. What's your moat if your AI sensitivity target of 85% turns out to be achievable by competitors?**

The 85% sensitivity threshold is not our moat — it's a minimum bar for clinical acceptability. Our actual moat is: (1) the proprietary Malaysian dataset that took years to collect and label, which keeps improving as more patients use the platform; (2) the integration depth — once a clinic has 2,000 patient records, AI summaries, and supplement histories on Healynx, switching to a competitor means losing all of that enriched data; (3) the dual patent protection covering the methodology, not just the implementation; (4) the regulatory compliance posture that takes competitors 12–18 months to replicate. The AI accuracy is the entry ticket; the moat is everything built around it.

---

**Q48. How do you handle the cold-start problem — your AI needs data to be accurate, but you need accuracy to get data?**

This is a real challenge we've thought carefully about. Our approach: (1) we begin with the existing UM research dataset from the IP inventors, which provides a non-zero starting corpus of expert-labelled Malaysian supplement interactions; (2) Phase 1 pilot at UM Medical Centre is subsidised/free for participating clinics, allowing us to collect clinical validation data before we need commercial-grade accuracy; (3) we use the Neo4j + RAG architecture to supplement the proprietary dataset with curated public literature (with clear provenance labelling), filling coverage gaps while we collect more Malaysian-specific data; (4) we are transparent with early-adopter clinics that the system is in clinical validation mode, framing their participation as contributing to a national health infrastructure asset.

---

**Q49. What is the regulatory pathway for an AI clinical decision support tool in Malaysia?**

The Medical Device Authority (MDA) under MOH Malaysia regulates software as a medical device. AI clinical decision support tools that are purely informational (the clinician retains full decision-making authority) may fall under the lower-risk classification requiring notification rather than full registration. Tools that make or substantially influence clinical decisions require higher-level registration. Healynx is deliberately positioned as decision support — AI outputs are advisory, clinicians acknowledge and decide — which we believe places us in the lower-risk regulatory category. We are confirming the exact classification with MDA as a Phase 1 activity. Our ISO 27001 alignment and PDPA compliance position us well for regulatory engagement.

---

**Q50. What do you need from judges and the ecosystem beyond funding?**

Three things: (1) clinical network connections — introductions to GP clinic group operators (KPJ, Columbia Asia, KPMC) and telemedicine platform CEOs (DoctorOnCall) would compress our Phase 2 partnership timeline significantly; (2) regulatory guidance — connections to MDA or MOH digital health offices who can advise on the AI clinical decision support classification process; (3) ecosystem credibility — winning or placing in National Deep Tech Challenge 2026 provides the institutional signal that accelerates trust-building with hospital systems and corporate wellness buyers who want to see competition-validated startups. We are looking for strategic partners, not just capital.
