# Requirements Specification (PS 1)
## 1. Scope
Define the implementation requirements for a user-controlled financial identity verification platform that supports secure onboarding, reusable credentials, consent-based sharing, and fraud defense against deepfake and synthetic identity threats.

## 1.1 MVP Boundary
This document is explicitly split into:
- Phase-1 MVP must-haves required for pilot launch.
- Future enhancements planned for post-MVP phases.

Allowed in Phase-1:
- Controlled stubs/mocks for external ecosystem integrations.
- Baseline heuristics for anomaly detection where advanced models are not yet production-ready.

Out of Phase-1:
- Full production scale hardening.
- Exhaustive regulator evidence automation.
- Advanced analyst automation and enterprise operating controls.

## 1.2 Phase Definitions
- Phase-1 MVP Must-Haves: required for end-to-end pilot readiness.
- Future Enhancements: important, but can be delivered after MVP without blocking pilot.

## 2. Actors
- User: Individual who owns and shares verified identity credentials.
- Regulated Entity (RE): Bank, NBFC, or financial institution performing KYC/re-KYC.
- Credential Issuer: Trusted source that attests identity attributes.
- Verification Gateway: Platform API that validates credentials and proof artifacts.
- Fraud/Risk Operations: Team that investigates flagged sessions.
- Auditor/Compliance: Team validating policy and regulatory adherence.

## 3. Phase-1 MVP Must-Haves (Build Now)
### 3.1 Functional Must-Haves
- FR-01: The system shall support first-time enrollment with identity proofing and biometric capture.
- FR-02: The system shall issue a verifiable credential after successful KYC checks.
- FR-03: The credential shall be user-controlled, portable, and retrievable by the user for reuse.
- FR-06: The system shall perform liveness checks during video KYC.
- FR-07: The system shall detect replay attacks, virtual camera injection, and presentation attacks (photo/video/mask) at baseline operational quality.
- FR-08: The system shall perform biometric matching against enrolled references with configurable confidence thresholds.
- FR-09: The system shall block or step-up verification when anti-spoof confidence falls below policy thresholds.
- FR-10: The system shall require explicit, informed user consent before sharing identity attributes.
- FR-11: Consent requests shall include requester identity, purpose, attributes requested, and retention period.
- FR-12: The system shall support selective disclosure so institutions receive only requested attributes or proofs.
- FR-13: The user shall be able to view, revoke, and time-limit active consents.
- FR-14: The system shall support re-KYC using prior verified credentials plus freshness checks.
- FR-15: The system shall support cross-institution onboarding using portable credentials within the pilot network without exposing raw documents by default.
- FR-17: The system shall compute real-time risk scores for onboarding sessions.
- FR-18: The system shall detect synthetic identity indicators (attribute inconsistency, unusual linkage patterns, velocity anomalies) using baseline rules/models.
- FR-19: The system shall detect suspicious behavior (device changes, geo-velocity anomalies, repeated failed liveness attempts).
- FR-20: High-risk sessions shall be routed for manual review with case evidence.
- FR-21: The system shall produce tamper-evident audit logs for verification, consent, and credential events.
- FR-22: Logs shall support regulator/auditor review without exposing unnecessary raw PII.
- FR-24: The system shall support low-bandwidth onboarding modes with adaptive quality and fallback flows.
- FR-25: The system shall support multilingual, low-literacy UX (voice prompts, icon-guided steps, simplified language).

### 3.2 Non-Functional Must-Haves
- NFR-01: All sensitive data in transit shall use TLS 1.2+ (enforced by Supabase).
- NFR-02: Sensitive data at rest shall be encrypted (Supabase managed encryption + column-level AES-256 for PII).
- NFR-03: Credential presentation and verification shall be nonce-based to prevent replay.
- NFR-04: Administrative access shall enforce least privilege via Supabase RBAC and 2FA.
- NFR-05: Data minimization shall be default; only required attributes are processed and disclosed.
- NFR-06: Biometric templates and raw PII shall be isolated in encrypted Storage buckets with strict RLS.
- NFR-07: Proof-based verification shall be preferred over raw document transfer wherever feasible.
- NFR-09: Risk scoring and anti-spoof checks shall return within onboarding-compatible latency budgets.
- NFR-10: The platform shall degrade gracefully under network constraints without disabling critical security checks.
- NFR-12: Consent and data sharing shall be policy-driven (PostgreSQL RLS) and auditable.
- NFR-13: Every model-assisted decision path shall support versioning and decision traceability via Edge Function logs.

## 4. Future Enhancements (Post-MVP)
### 4.1 Functional Enhancements
- FR-04: The system shall support full credential revocation, suspension, and re-issuance workflows across institutions.
- FR-05: The system shall maintain a multi-issuer trust metadata registry for credential verification at scale.
- FR-16: The system shall support production-grade proof verification flows with live DigiLocker and Account Aggregator integrations.
- FR-23: The system shall provide richer explainable risk decision summaries and analyst-facing decision narratives.
- FR-26: The system shall support assisted onboarding flows for users with limited digital literacy, including partner-assisted workflows.

### 4.2 Non-Functional Enhancements
- NFR-08: Core verification APIs shall meet production high-availability and fault-tolerance SLOs.
- NFR-11: Controls shall be fully traceable to applicable RBI KYC expectations and ecosystem obligations, with regulator-ready evidence packs.

## 5. Phase-1 Acceptance Criteria (MVP Gate)
- AC-01: A user can complete one-time verification and receive a reusable credential.
- AC-02: A regulated entity can verify required claims through selective disclosure with explicit user consent.
- AC-03: Re-KYC can be completed using prior credentials and freshness checks with reduced onboarding friction.
- AC-04: Known deepfake/replay test scenarios are detected and blocked or escalated.
- AC-05: Synthetic identity test cases produce risk alerts and analyst-review routing.
- AC-06: Low-bandwidth onboarding remains operable without bypassing anti-spoof protections.
- AC-07: Audit artifacts are generated for consent, verification, and risk decisions.

## 6. Future Enhancement Acceptance Signals
- AC-FUT-01: Multi-issuer trust and revocation workflows operate reliably across participating institutions.
- AC-FUT-02: Live DigiLocker/AA integrations pass end-to-end operational and compliance checks.
- AC-FUT-03: Production SLOs for availability and resilience are met under load testing.
- AC-FUT-04: Compliance traceability and evidence packs are exportable for regulator review.

## 7. Open Requirement Items (To Finalize)
- Final latency and throughput targets by channel and risk tier.
- Formal attribute schema and proof format for selective disclosure.
- Institution-specific policy packs (risk thresholds, escalation rules, retention periods).
- Compliance control mapping and evidence templates.


## 8. MVP Exit Criteria
- Core user journey works end-to-end for enrollment, consent, and verification.
- Anti-spoof and synthetic-risk baselines pass internal adversarial test thresholds.
- Security controls for transport, storage, and replay resistance are operational.
- Audit records are queryable for verification, consent, and risk decisions.
- Deferred production features are explicitly tracked with owners and target phases.
