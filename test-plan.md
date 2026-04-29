# Test Plan (PS 1 - Phase-1 MVP)
## 1. Purpose
Define how the MVP is validated against Phase-1 requirements and MVP threat scenarios before pilot launch.

## 2. Test Objectives
- Verify core onboarding, consent, and re-KYC flows work end-to-end.
- Validate anti-spoof and synthetic-risk controls against realistic attack scenarios.
- Confirm privacy, auditability, and accessibility baselines are met for MVP.

## 3. Scope and Traceability
Primary traceability targets:
- Requirements: FR-01, FR-02, FR-03, FR-06 to FR-15, FR-17 to FR-22, FR-24, FR-25.
- NFRs: NFR-01 to NFR-07, NFR-09, NFR-10, NFR-12, NFR-13.
- Threat scenarios: TM-01 to TM-07.

## 4. Test Environments
- Local development environment using Supabase CLI and local PostgreSQL.
- Staging Supabase project with production-like RLS and Edge Function configuration.
- Controlled low-bandwidth test profiles for degraded network simulation.
- Attack simulation environment for adversarial exercises.

## 5. Test Suites
### 5.1 Unit Tests
- Edge Function logic for consent rules, credential status, and nonce handling.
- PostgreSQL RLS policy verification for data isolation.
- Risk rules and score aggregation logic in Deno/TS.
- Audit event generation and schema validation.

### 5.2 API and Integration Tests
- Enrollment, verification, consent, and re-KYC endpoint contracts.
- Error paths: expired consent, revoked credentials, invalid nonce, malformed proofs.
- Adapter contract tests for DigiLocker/AA-style stub integrations.

### 5.3 End-to-End User Journey Tests
- New user onboarding to credential issuance.
- Consent request, user approval, selective disclosure response.
- Re-KYC with prior credential and freshness re-check.
- High-risk session escalation to manual review and final disposition.

### 5.4 Adversarial Anti-Spoof Tests
- Replay attack samples (screen/video playback).
- Presentation attack samples (printed photo/mask).
- Injection-style deepfake or virtual camera scenarios.
- Challenge-response bypass attempts under unstable networks.

### 5.5 Synthetic Identity and Anomaly Tests
- Stitched identity attributes with partial inconsistencies.
- Velocity abuse patterns (rapid retries, repeated device/account linkage).
- Geo/device anomaly patterns and suspicious session sequencing.
- Verify reason codes and analyst routing for high-risk outcomes.

### 5.6 Security and Privacy Tests
- TLS enforcement and encrypted storage validation.
- Access-control tests for sensitive data and admin endpoints.
- Replay resistance tests for nonce-bound verification.
- Data minimization checks for selective disclosure outputs.

### 5.7 Accessibility and Inclusion Tests
- Multilingual prompt and simplified-flow verification.
- Low-bandwidth onboarding behavior without bypassing anti-spoof checks.
- Assisted flow usability checks for low-digital-literacy users.

## 6. Data Strategy for Testing
- Use anonymized/synthetic datasets for fraud and attack scenarios where possible.
- Restrict access to biometric test artifacts; log usage and retention.
- Keep separate datasets for dev validation and release gate testing.

## 7. Defect Severity and Release Gates
- Blocker: Security/privacy bypass, spoof false accept in critical path, broken consent enforcement.
- High: Major onboarding/re-KYC break, audit evidence missing for critical decisions.
- Medium: Non-critical UX or resilience gaps with available workaround.

MVP release gate requires:
- All blocker and high defects closed or formally risk-accepted by owners.
- Phase-1 acceptance criteria in `requirements.md` satisfied.
- Red-team/adversarial scenarios completed with documented outcomes.

## 8. Test Evidence Artifacts
- Test run reports by suite and environment.
- Requirement and threat traceability checklist with pass/fail status.
- Adversarial test summary with attack type, detection outcome, and disposition.
- Audit log sample pack for verification, consent, and risk decisions.

## 9. Post-MVP Test Expansion
- Full-scale performance and resilience testing for production SLOs.
- Continuous attack simulation pipelines and drift-triggered regression suites.
- Extended compliance evidence automation and external assurance readiness.
