# Threat Model (PS 1)
## 1. Method and Objectives
This threat model uses STRIDE-style analysis plus fraud kill-chain thinking for onboarding and re-KYC flows. Objectives are to:
- Prevent AI-driven impersonation and spoofing success.
- Prevent creation or reuse of synthetic identities.
- Protect credential ownership and consent integrity.
- Minimize raw PII exposure while preserving verifiability.
## 1.1 MVP Threat Boundary
This threat model prioritizes highest-risk onboarding and credential abuse threats for MVP.
- Primary focus: deepfake injection, replay/presentation spoofing, synthetic identity signals, credential replay, consent abuse, API abuse, insider misuse.
- MVP assumption: controls are layered but not yet complete to production-grade depth.
- Deferred to later phases: comprehensive third-party assurance, mature SOC playbooks, and full cross-institution fraud-intel sharing.

## 2. Protected Assets
- Verified credential artifacts and their cryptographic proofs.
- Biometric references/templates and liveness telemetry.
- Consent records and authorization tokens.
- Identity attributes and associated PII.
- Risk signals, model outputs, and fraud-case evidence.
- Signing/encryption keys and trust registries.
- Audit logs used for compliance and investigation.

## 3. Trust Boundaries
- User device boundary (mobile wallet, local storage, device integrity).
- Supabase platform boundary (Auth, Edge Functions, PostgreSQL, Storage).
- Network boundary (device to Supabase, Supabase to institution/ecosystem).
- External ecosystem boundary (DigiLocker/AA connectors, issuer systems).
- Operations boundary (Supabase dashboard, analyst console, log access).

## 4. Threat Actors
- External fraudsters using deepfake generation and replay tooling.
- Organized synthetic identity rings using stitched/stolen attributes.
- Malware operators targeting user devices and session tokens.
- Malicious or compromised integration clients abusing APIs.
- Insider threats with privileged but misused access.

## 5. Priority Threat Scenarios
### TM-01: Real-Time Deepfake Injection in Video KYC
Attack: Adversary streams AI-generated face/voice to pass video KYC.
Impact: Fraudulent account onboarding, downstream financial abuse.
Mitigations:
- Active challenge-response with randomized prompts.
- Passive liveness and deepfake classifier ensemble.
- Frame-level injection and replay detection.
- Policy-driven step-up verification on uncertainty.

### TM-02: Presentation and Replay Spoofing
Attack: Printed photo, screen replay, mask, or recorded video used to bypass liveness.
Impact: Account takeover or fraudulent new identity onboarding.
Mitigations:
- Anti-spoof models for presentation attack detection.
- Temporal consistency checks and motion-depth cues.
- Session nonces and proof-of-fresh-capture controls.

### TM-03: Synthetic Identity Assembly
Attack: Fraudster combines partial real and fabricated identity attributes.
Impact: Hard-to-detect, long-lived fraud accounts.
Mitigations:
- Cross-attribute consistency checks.
- Velocity and linkage analytics (device, contact, behavior patterns).
- Risk scoring and mandatory analyst review for high-risk clusters.

### TM-04: Credential Theft and Replay
Attack: Credential artifacts or session tokens are exfiltrated and reused.
Impact: Unauthorized verification, impersonation across institutions.
Mitigations:
- Device-bound credentials and secure enclave/keystore usage.
- Supabase Auth session management, short-lived tokens, and nonce-bound proofs.
- PostgreSQL RLS to prevent cross-tenant/cross-user data exfiltration.
- Credential revocation and suspicious-use alerting.

### TM-05: Consent Phishing or Coercion
Attack: User is tricked into approving excessive attribute sharing.
Impact: Privacy breach and unauthorized data propagation.
Mitigations:
- Strong consent UX with clear requester/purpose/data scope.
- Granular, time-bound, revocable consent grants.
- Out-of-band confirmation for sensitive requests.

### TM-06: API Abuse and Automation Attacks
Attack: Bot-driven onboarding attempts, credential stuffing, endpoint abuse.
Impact: Fraud volume spike, service degradation, model probing.
Mitigations:
- Strong client authentication and request signing via Supabase SDK.
- Supabase-managed rate limiting, abuse detection, and IP reputation.
- WAF rules and anomaly-driven throttling.

### TM-07: Insider Misuse of Sensitive Data
Attack: Privileged user accesses raw PII or biometrics without valid need.
Impact: Data breach, regulatory non-compliance.
Mitigations:
- Least-privilege Supabase RBAC and segregation of duties.
- Immutable PostgreSQL audit logs and continuous access auditing.
- Just-in-time privileged access and periodic entitlement review.

## 6. Baseline Security Controls
- Cryptographic integrity for credentials, logs, and consent events.
- Strong encryption for sensitive data at rest and in transit.
- Secure software supply chain and signed deployment artifacts.
- Model governance: versioning, drift monitoring, rollback paths.
- Continuous fraud monitoring pipeline with alert triage.

## 7. Detection and Response Requirements
- DR-01: Every high-risk onboarding decision must include machine and rule-based evidence.
- DR-02: Alert workflows must support analyst disposition (approve, reject, escalate, watchlist).
- DR-03: Compromised credential response must support immediate revocation and propagation.
- DR-04: Incident forensics must preserve verifiable event timelines and model versions used.

## 8. Security Validation Requirements
- SV-01: Deepfake and replay red-team tests across varied lighting/network conditions.
- SV-02: Synthetic identity simulation using stitched attributes and velocity bursts.
- SV-03: Consent abuse simulations for deceptive request patterns.
- SV-04: API abuse tests for automated high-volume attack behavior.
- SV-05: Insider-access scenario testing for unauthorized data retrieval.

## 9. Residual Risks and Assumptions
- No anti-spoof system is perfect; layered controls and step-up flows are mandatory.
- Model performance can drift; periodic retraining and recalibration are assumed.
- External ecosystem connectors may introduce dependency risk; fallback procedures are required.
- Accessibility constraints can increase fraud risk if fallback flows are weak; inclusion controls must remain security-equivalent.
- MVP controls reduce risk but do not eliminate all attack paths; residual risk acceptance is required before pilot launch.
