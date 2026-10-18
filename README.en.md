# ind-safety-level-checker

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Domain Profile](https://img.shields.io/badge/domain-ind%20%7C%20domain%2F1.0-blue.svg)](domain.manifest.json)
[![Status: RESERVED](https://img.shields.io/badge/status-RESERVED%20%7C%20open%20claim-orange.svg)](domain.manifest.json)
[![Gate: five-level](https://img.shields.io/badge/gate-silent%2Fwarn%2Fconfirm%2Fblock%2Frequire--owner-8a2be2.svg)](gate-map.json)
[![Privacy: default-off](https://img.shields.io/badge/privacy-default--off-brightgreen.svg)](privacy.json)

> **Status: RESERVED (placeholder · open for claim)** — this repository ships the four-piece industry-access skeleton already built to the platform protocol, and waits for an operator with the proper domain qualification to claim it and fill in the real rules.

It turns "what risk grade is this production line, and are the licences and work permits in order" into verifiable attributes: the AI only surfaces what is missing — it does **not** issue a safety conclusion.

## What this domain covers

Industrial safety-production risk grading and special-equipment compliance. Risk-grading control, special-equipment inspection, and certified special operations all carry statutory rules; a gap in any can cause injury. The AI lays out the evidence and expiry dates; safety-condition determination stays with the enterprise and the regulator.

## Quick start

This repo is a **domain-profile skeleton** (four JSON files), not a runnable application. Validate the structure with standard JSON tooling:

```bash
# 1. clone
git clone https://github.com/SynomosAI/ind-safety-level-checker.git
cd ind-safety-level-checker

# 2. validate the four pieces are well-formed JSON
python -m json.tool domain.manifest.json > /dev/null
python -m json.tool gate-map.json        > /dev/null
python -m json.tool privacy.json         > /dev/null
python -m json.tool checker.io.json      > /dev/null
```

The checker (an MCP tool) input/output contract is in `checker.io.json`. A minimal call:

```json
{
  "subject_ref": "mpp:cn:7a3d2e1f",
  "attribute_keys": ["ind.safety_risk_level", "ind.special_equipment_status"],
  "evidence": ["tsa:2026-09-12#sha256:9c4d..."],
  "request_id": "req-20260912-0002"
}
```

> The checker has exactly one action — *show and verify*. It has **no "judge" option**: pass/fail, "may start production / may power on", and "meets safety conditions" conclusions are all on the `_forbidden_outputs` blacklist.

## Domain identity

| Item | Value |
|---|---|
| Domain ID | `ind` (globally unique; never reused) |
| Domain name | Industrial · Safety-production risk grading |
| Profile version | `domain/1.0` |
| Status | `RESERVED` |
| Placeholder since | 2026-09-12 |

## Attributes

| Key | Type | Meaning |
|---|---|---|
| `ind.safety_risk_level` | enum | Risk grade (red/orange/yellow/blue), assessed and logged by the enterprise · red/orange/yellow/blue/undetermined |
| `ind.safety_production_license` | enum | Safety-production licence state · licensed/unlicensed/exempt/pending |
| `ind.special_equipment_status` | enum | Special-equipment inspection validity · inspected_valid/due_inspection/overdue/not_applicable |
| `ind.work_permit` | enum | Hot-work / confined-space permit state · valid/missing/expired/not_applicable |
| `ind.ai_control_authority` | enum | AI authority boundary in process control, declared explicitly · advisory_only/auto_shutdown_enabled/process_control/none |

## Red lines (machine-readable)

1. Risk grade must be assessed and logged by the enterprise per standard; the AI must not grade on its behalf.
2. No "may start production / may power on / meets safety conditions" go-ahead wording — that follows the regulator and qualified bodies.
3. The AI must not issue shutdown or process-setpoint control commands unless independent safety confirmation exists.

Each red line has a corresponding blocking rule in `gate-map.json`.

## The four-piece access set

| File | Role |
|---|---|
| `domain.manifest.json` | Domain declaration: attribute list, issuer requirements, validity, red lines |
| `gate-map.json` | How much friction per action: silent / warn / confirm / block / require-owner |
| `privacy.json` | Privacy declaration: default-off, minimal, withdrawable, deletable |
| `checker.io.json` | Checker (MCP tool) IO contract — show & verify, never judge |

## Core principles

**The platform is a ring, not a shelf.** The checker only answers "is this claim verifiable, and which evidence is missing" — it does not answer "is this compliant / should it be done".

**Privacy is an access condition, not a remediation item.** Missing `privacy.json` or any non-conforming required field fails the conformance check outright.

## References

| — | Safety Production Law of the PRC; GB/T 33000 Basic norm for enterprise safety-production standardization; Regulations on Safety Supervision of Special Equipment. The references only explain the source of the attributes; they do not constitute legal advice.

## How to claim

1. Fork this repo and fill in `operator` and `checker_endpoint`.
2. Calibrate attribute values and red-line wording per currently effective rules.
3. Run the platform-side validator self-check (all five criteria must pass).
4. Open a PR with qualification evidence and the rule basis.

## License / 署名

Code and configurations are used under the MIT licence. Document copyright belongs to SynomosAI.

© 2026 SynomosAI. All rights reserved.
