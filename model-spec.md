# Model Specification (PS 1 - Phase-1 MVP)
## 1. Purpose
Define the MVP model and scoring layer for deepfake/spoof defense and synthetic identity risk detection used in onboarding and re-KYC workflows.

## 2. MVP Model Portfolio
### M1: Liveness and Deepfake Detection
- Goal: Detect non-live, replayed, injected, or AI-generated video artifacts.
- Primary use: Video KYC and high-risk re-KYC sessions (Edge Function: `verify/liveness`).

### M2: Face Match Verification
- Goal: Match live capture against enrolled reference with confidence scoring.
- Primary use: Enrollment and re-KYC freshness (Edge Function: `verify/face-match`).

### M3: Synthetic Identity Risk Scoring
- Goal: Identify suspicious identity composition and behavior patterns.
- Primary use: Real-time session risk scoring (Edge Function: `risk/synthetic`).

## 3. Inference Inputs and Outputs
### 3.1 M1 (Liveness/Deepfake) Contract
Inputs:
- Session video frames or short challenge-response clips.
- Device/session metadata (capture mode, camera source hints, timestamp integrity).
- Optional audio features for voice-video consistency checks.

Outputs:
- `liveness_score` (0.0 to 1.0)
- `spoof_probability` (0.0 to 1.0)
- `attack_type` (none, replay, presentation, injection, deepfake_suspected)
- `quality_flags` (low_light, blur, unstable_network, frame_drop)
- `model_version`

### 3.2 M2 (Face Match) Contract
Inputs:
- Enrolled biometric reference embedding/template.
- Live capture embedding/template.
- Capture quality metadata.

Outputs:
- `match_score` (0.0 to 1.0)
- `decision_hint` (pass, step_up, fail)
- `quality_flags`
- `model_version`

### 3.3 M3 (Synthetic Risk) Contract
Inputs:
- Identity attribute consistency features.
- Device and velocity features (repeated attempts, geo/device shifts).
- Behavioral sequence features (timing patterns, retry signatures).
- Historical event linkage features within policy constraints.

Outputs:
- `risk_score` (0 to 100)
- `risk_tier` (low, medium, high)
- `reason_codes` (top contributing indicators)
- `model_version` or `ruleset_version`

## 4. MVP Decision Policy (Initial)
- Auto-pass candidate:
  - `liveness_score >= 0.90`
  - `spoof_probability <= 0.15`
  - `match_score >= 0.88`
  - `risk_score < 55`
- Step-up/manual review candidate:
  - Any uncertain signal or medium risk tier.
- Auto-fail candidate:
  - Strong spoof/deepfake signal, severe mismatch, or high risk tier.

Note: Final thresholds are calibrated with pilot data and approved by fraud and compliance owners.

## 5. Data and Labeling Strategy (MVP)
- Combine:
  - Public anti-spoof/deepfake datasets.
  - Synthetic attack simulations for replay/presentation/injection.
  - Pilot onboarding samples with explicit consent and privacy controls.
- Label categories:
  - bona_fide, replay, presentation, injection, deepfake_suspected.
- Maintain strict separation between training, validation, and adversarial holdout sets.

## 6. Performance Targets (MVP Baseline)
- Liveness/deepfake:
  - High detection recall on known spoof scenarios used in pilot tests.
  - Low false reject impact on genuine users under normal capture conditions.
- Face match:
  - Stable match confidence under moderate lighting/device variance.
- Synthetic risk:
  - Actionable precision for high-risk routing to analyst queue.
- Latency:
  - Scores returned within onboarding-compatible response budgets (aligned to NFR-09).

## 7. Monitoring and Drift Controls
- Track score distributions by channel, device class, and network condition.
- Monitor false accept/false reject trends and analyst override rates.
- Alert on drift signals, threshold instability, and sudden attack-pattern shifts.
- Log model/ruleset version for every automated decision.

## 8. Fallback and Safety Behavior
- If model quality flags indicate unreliable input:
  - trigger recapture workflow or assisted path.
- If model service is degraded:
  - fail safe to step-up/manual review, not silent pass.
- If confidence is borderline:
  - prefer user-safe and fraud-safe step-up path.

## 9. Human-in-the-Loop Requirements
- High-risk or uncertain outcomes must be reviewable with reason codes.
- Analyst decisions must be captured with operator ID and timestamp.
- Disposition outcomes should feed periodic threshold and ruleset calibration.

## 10. Post-MVP Enhancements
- Advanced multimodal anti-deepfake ensembles.
- Continuous learning and risk graph intelligence.
- Model fairness and subgroup robustness audits at production depth.
- Federated or privacy-enhancing training approaches where feasible.
