# Architecture Specification (PS 1 - Phase-1 MVP)
## 1. Purpose
Define the MVP system architecture for user-controlled financial identity verification, aligned to `requirements.md` (Phase-1 must-haves) and `threat-model.md` (MVP threat boundary).

## 2. MVP Architectural Goals
- Deliver one-time verification plus reusable credential flow (FR-01, FR-02, FR-03, FR-14, FR-15).
- Enforce consent-first selective disclosure (FR-10, FR-11, FR-12, FR-13).
- Detect deepfake/spoof and synthetic risk signals in real time (FR-06 to FR-09, FR-17 to FR-20).
- Preserve privacy and auditability under constrained networks (FR-21, FR-22, FR-24, FR-25; NFR-05, NFR-10).

## 3. Logical Components (MVP)
### 3.1 User Wallet App
- Onboarding UI, consent approval/revocation UI, credential presentation.
- Performs device-level authentication before consent approval (integrated with Supabase Auth).
- Supports low-bandwidth and multilingual UX fallback.

### 3.2 Verification Gateway (Edge Functions)
- Supabase Edge Functions for enrollment, verification, re-KYC, and consent operations.
- Validates institution requests and routes calls to internal logic.
- Applies policy checks and nonce validation via database lookups.

### 3.3 Credential & Consent Service (Database + RLS)
- Issues signed verifiable credentials via Edge Functions.
- Stores consent grants with purpose, scope, and expiry in PostgreSQL.
- Enforces selective disclosure and privacy policies using Supabase RLS.
- Supports consent revocation and time-bound access.

### 3.4 Biometric Verification Service (Edge Functions)
- Edge Functions for liveness, anti-spoof, and face match pipelines.
- Produces confidence scores and decision signals for policy engine.
- Flags likely replay/injection attempts for step-up or fail.

### 3.5 Risk and Anomaly Service (Edge Functions)
- Aggregates behavioral, device, and session signals.
- Computes real-time risk score and reason codes.
- Routes high-risk sessions to manual review queue.

### 3.6 Audit and Evidence Service (Database)
- Captures tamper-evident event logs in PostgreSQL.
- Stores minimal evidence references in Supabase Storage.
- Preserves traceability of model version and policy version used.

### 3.8 Integration Adapter Layer (MVP Stub-Ready)
- Adapter contracts for DigiLocker/AA-style verification patterns.
- MVP supports mock/stub adapters with same request-response interfaces.
- Live ecosystem connectors are post-MVP enhancements.

### 3.9 Analyst Review Console (Minimal)
- Displays high-risk sessions, evidence summary, and decision actions.
- Allows approve/reject/escalate outcomes with operator attribution.

## 4. Core Data Stores (Supabase / PostgreSQL)
- **Identity Vault**: PostgreSQL tables with RLS and column-level encryption.
- **Credential Store**: Metadata and status of issued credentials.
- **Consent Ledger**: Audit-compliant log of consent grants/revocations.
- **Risk Event Store**: Model outputs, triggers, and reason codes.
- **Audit Log Store**: Append-only operational and security events.
- **Biometric Storage**: Supabase Storage buckets for encrypted artifacts.

## 5. End-to-End MVP Flows
### 5.1 Enrollment and Credential Issuance
1. User starts onboarding in wallet app and submits required identity inputs.
2. Gateway invokes biometric verification (liveness + anti-spoof + face match).
3. Risk service computes onboarding risk score.
4. If policy passes, credential service issues verifiable credential.
5. Audit service records all decisions and evidence pointers.

### 5.2 Consent-Based Verification
1. Regulated entity sends verification request with requested attributes and purpose.
2. Consent service presents request to user and requires explicit approval.
3. Credential service returns selective disclosure proof/claims (minimum necessary data).
4. Gateway returns verification result and logs event trail.

### 5.3 Re-KYC Using Prior Credential
1. Regulated entity requests re-KYC with freshness requirement.
2. System validates prior credential status and requests freshness checks.
3. Biometric service performs lightweight re-verification.
4. Gateway returns re-KYC result with policy decision and audit trace.

### 5.4 High-Risk Escalation
1. Risk/anomaly score exceeds threshold or spoof signal is uncertain.
2. Session is blocked or moved to step-up/manual review.
3. Analyst reviews case evidence and records final disposition.
4. Final decision and rationale are logged immutably.

## 6. Security and Privacy Controls (MVP Baseline)
- TLS in transit; encryption at rest for sensitive stores.
- Nonce-bound verification to reduce replay risk.
- Least-privilege access across services and operator tools.
- Data minimization and selective disclosure by default.
- Tamper-evident logging for key security and compliance events.

## 7. Deployment View (Supabase)
- **Client tier**: React webapp hosted on Supabase Hosting or Vercel.
- **API/Logic tier**: Supabase Edge Functions (Deno).
- **Data tier**: Supabase PostgreSQL + RLS + Storage.
- **Ops tier**: Supabase Dashboard + custom analyst console (React).

## 8. Out of Scope for Phase-1 MVP
- Cross-institution global revocation registry.
- Full high-availability SLO hardening and multi-region failover.
- Fully live DigiLocker/AA integrations in all flows.
- Advanced SOC automation and enterprise fraud intelligence sharing.
