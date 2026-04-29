# Plan: User-Controlled Financial Identity Verification System (PS 1)

## Context

This project addresses a critical need in India's financial services landscape: creating a user-controlled identity verification system that tackles deepfake impersonation, synthetic identity fraud, and credential theft while aligning with RBI's regulatory framework (DigiLocker, Account Aggregator).

## Solution Overview

Build a **Verifiable Identity Wallet** — a mobile-first system where users own portable, tamper-proof identity credentials they can share with financial institutions on consent, without exposing raw PII.

---

## Architecture: Modular Microservices

### Core Components

1. **Identity Wallet Service** (User-facing)
   - Mobile app / PWA for credential management
   - Biometric enrollment (fingerprint, face)
   - Local encrypted credential storage
   - Selective disclosure engine

2. **Verification Gateway** (Institution-facing)
   - REST API for financial institutions
   - Consent management layer
   - Credential validation
   - Integration with AA ecosystem

3. **Deepfake & Spoofing Detection Engine**
   - Liveness detection for video KYC
   - Face anti-spoofing (2D/3D mask detection)
   - Injection attack detection (deepfake video)
   - Real-time biometric matching

4. **Anomaly Detection Service**
   - Synthetic identity pattern detection
   - Behavioral biometrics
   - Device fingerprinting
   - Flagging suspicious onboarding

5. **Privacy-Preserving Credential Layer**
   - ZK-proof generation for age/income verification
   - Selective disclosure (reveal only required attributes)
   - BBS+ signatures for unlinkable proofs

6. **Accessibility Layer**
   - Low-bandwidth video compression
   - Multi-language voice-first UI
   - Simple iconography
   - Offline capability

---

## Implementation Plan (Prototype - 12 Sprints)

### Sprint 1-2: Core Identity Wallet
- Express server with user registration + biometric enrollment
- Credential issuance flow (linking to DigiLocker)
- Local encrypted storage (AES-256)
- Basic consent management UI

### Sprint 3-4: Verification Infrastructure
- REST API for institution queries
- Consent verification engine
- Credential presentation/validation
- AA framework integration stubs

### Sprint 5-6: Deepfake Detection 
- Liveness detection API 
- Face anti-spoofing endpoint 
- Video KYC workflow
- Injection detection 

### Sprint 7-8: Anomaly Detection 
- Behavioral biometrics capture 
- Synthetic identity heuristics
- Risk scoring engine
- Alert dashboard for institutions

### Sprint 9-10: Privacy Layer 
- ZK-proof verification endpoint 
- BBS+ signature verification
- Selective disclosure API
- Re-KYC using prior credentials

### Sprint 11-12: Accessibility & Polish
- Voice-first UI in Hindi/English
- Low-bandwidth video encoding config
- Simple onboarding flow
- Final integration testing

---

## File Structure

```
/home/hkk/
├── plan.md                          # This implementation plan
├── solution.txt                     # Simple explanation for stakeholders
├── frameworks.txt                   # Tech stack documentation
├── SPEC.md                          # Detailed technical specification
├── docker-compose.yml               # Local dev environment
├── backend/
│   ├── src/
│   │   ├── index.js                 # Express entry point
│   │   ├── routes/                  # API routes
│   │   ├── services/                 # Business logic 
│   │   ├── models/                   # Data models
│   │   └── utils/                    # Encryption, ZK 
│   └── package.json
├── frontend/
│   └── webapp/                     # React webapp
│       ├── src/
│       │   ├── App.jsx
│       │   ├── pages/
│       │   ├── components/
│       │   └── services/
│       ├── package.json
│       └── vite.config.js
└── docs/
    └── api-spec.yaml                 # OpenAPI spec
```

---

## Tech Stack

- **Backend**: Node.js + Express (fast prototyping)
- **ML/AI**: Python (mocked for prototype) with TensorFlow/PyTorch stubs
- **Frontend**: React (webapp with Vite)
- **Database**: SQLite (dev), PostgreSQL (prod)
- **ZK Proofs**: Mocked implementation with BLS signature stubs
- **Biometrics**: Face recognition lib stubs for prototype
- **Encryption**: Node.js crypto (AES-256-GCM)
- **Containerization**: Docker Compose (local dev)

---

## Verification Approach

1. **Unit Tests**: Each service has 80%+ test coverage
2. **Integration Tests**: API contract tests between services
3. **E2E Tests**: Complete user flow from enrollment to credential sharing
4. **Security Audit**: Third-party penetration testing
5. **Regulatory Compliance**: RBI guideline adherence review

---

## Key Dependencies to Research

- DigiLocker API integration
- AA framework (Account Aggregator) API specs
- RBI KYC guidelines (2024)
- NIST-biometric standards
- OpenID Connect / Verifiable Credentials W3C standard