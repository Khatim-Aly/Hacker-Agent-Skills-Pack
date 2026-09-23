# Hacker Agent Skills Pack

**1,008 task-specific security assessment playbooks across 56 categories — designed for selective retrieval, not bulk loading.**

> **Honest note on scope:** These 1,008 entries are practical starting playbooks that cover a wide range of security checks. Many are structured starting points, not battle-tested expert capabilities. What matters more in real assessments is whether an agent can *choose the right check*, *use its tools reliably*, *verify evidence*, *recognize uncertainty*, and *explain impact*. That judgment comes from practice and evaluation across real test cases — a skill count cannot supply it.
>
> **Recommended usage:** Give an agent 50–80 carefully tested core skills as its always-on foundation, then retrieve 1–3 specialist skills per task from this pack. Keep the full 1,008-skill pack as a searchable reference library, not a prompt payload.

---

## Table of Contents

- [Overview](#overview)
- [How to Use This Pack](#how-to-use-this-pack)
- [Repository Structure](#repository-structure)
- [Skill Categories](#skill-categories)
- [Skill Format](#skill-format)
- [Schemas](#schemas)
- [Model Routing](#model-routing)
- [Execution Contract](#execution-contract)
- [Research Anchors](#research-anchors)
- [License](#license)

---

## Overview

This pack contains **1,008 independent `SKILL.md` playbooks** organized into **56 security categories**, covering the full lifecycle of a security engagement — from scope and asset discovery through active testing, adversary emulation, incident response, and reporting.

Each skill is a self-contained, structured assessment playbook that tells an agent:

- What question it is answering
- What preconditions and scope gates apply
- What method to follow
- What evidence and output to produce
- Which authoritative reference applies

The pack includes no live-target automation, exploit payloads, or credentials. It is a knowledge library and retrieval corpus; the host agent and its tooling handle execution.

---

## How to Use This Pack

### The core principle: selective retrieval

Loading all 1,008 skills into an agent's context window at once adds noise and wastes tokens. The skills are designed for on-demand retrieval.

```
┌─────────────────────────────────────┐
│  Agent context (always loaded)      │
│  ├── AGENT.md  (6 rules)            │
│  ├── CONTRACT.md  (execution gates) │
│  ├── Engagement scope JSON          │
│  └── ~50–80 core skills             │
└─────────────────────────────────────┘
           │  task arrives
           ▼
┌─────────────────────────────────────┐
│  Retrieval step                     │
│  pick_skill.py  <keywords>          │
│  → returns 1–3 SKILL.md paths       │
└─────────────────────────────────────┘
           │  load matching skill bodies
           ▼
┌─────────────────────────────────────┐
│  Execution                          │
│  Agent follows skill method,        │
│  validates scope, collects evidence │
│  → produces finding.json            │
└─────────────────────────────────────┘
```

### Step 1 — Pick a skill

```bash
python scripts/pick_skill.py jwt token expiry --limit 3
```

Returns ranked matches by name and category overlap:

```
 6 assess-jwt-expiry-and-revocation   skills/authentication-and-sessions/assess-jwt-expiry-and-revocation/SKILL.md
 4 assess-token-rotation-policy       skills/oauth-federation/assess-token-rotation-policy/SKILL.md
 3 review-session-fixation             skills/web-application/review-session-fixation/SKILL.md
```

### Step 2 — Load the skill

Read the returned `SKILL.md` file into the agent's context alongside the engagement scope. Do not load more than 1–3 skills per task.

### Step 3 — Follow the contract

Before any tool call, the agent verifies against `CONTRACT.md`:

- Is this target in scope?
- Is this method approved?
- Is the time window active?
- Does this action require explicit owner approval?

### Step 4 — Produce a finding

Output follows the `finding.example.json` schema: observed vs. hypothesized impact, confidence rating, evidence references, remediation, and retest criterion.

---

## Repository Structure

```
security-agent-skills/
├── AGENT.md                    # Six rules every agent must follow
├── CONTRACT.md                 # Execution contract and evidence rules
├── MODEL_ROUTING.md            # OpenRouter / Ollama dispatch guidance
├── RESEARCH.md                 # Source anchors and taxonomy rationale
├── LICENSE.txt
│
├── catalog.json                # Machine-readable index of all 1,008 skills
│                               # (name, description, category, path)
│
├── schemas/
│   ├── engagement.example.json # Engagement scope format
│   └── finding.example.json    # Finding output format
│
├── scripts/
│   ├── pick_skill.py           # Offline keyword search against catalog.json
│   └── validate_pack.py        # Integrity check for the skill tree
│
└── skills/
    ├── advanced-code-review/       (18 skills)
    ├── adversary-emulation/        (18 skills)
    ├── ai-agent-assurance/         (18 skills)
    ├── ai-and-llm-security/        (18 skills)
    ├── api-security/               (18 skills)
    ├── asset-discovery/            (18 skills)
    ├── authentication-and-sessions/(18 skills)
    ├── aws-specific/               (18 skills)
    ├── azure-specific/             (18 skills)
    ├── binary-analysis/            (18 skills)
    ├── blockchain-contracts/       (18 skills)
    ├── browser-client/             (18 skills)
    ├── bug-bounty-advanced/        (18 skills)
    ├── bug-bounty-workflow/        (18 skills)
    ├── ci-cd-advanced/             (18 skills)
    ├── cloud-forensics/            (18 skills)
    ├── cloud-platforms/            (18 skills)
    ├── containers-and-kubernetes/  (18 skills)
    ├── crypto-and-data-protection/ (18 skills)
    ├── crypto-protocols/           (18 skills)
    ├── database-storage/           (18 skills)
    ├── detection-engineering/      (18 skills)
    ├── dns-edge/                   (18 skills)
    ├── email-security/             (18 skills)
    ├── endpoint-and-operating-systems/ (18 skills)
    ├── enterprise-identity/        (18 skills)
    ├── gcp-specific/               (18 skills)
    ├── incident-response/          (18 skills)
    ├── iot-firmware/               (18 skills)
    ├── kubernetes-advanced/        (18 skills)
    ├── linux-unix/                 (18 skills)
    ├── malware-forensics/          (18 skills)
    ├── ml-ops-security/            (18 skills)
    ├── mobile-and-client-apps/     (18 skills)
    ├── mobile-android/             (18 skills)
    ├── mobile-ios/                 (18 skills)
    ├── network-and-infrastructure/ (18 skills)
    ├── oauth-federation/           (18 skills)
    ├── ot-iot-and-hardware/        (18 skills)
    ├── ot-protocols/               (18 skills)
    ├── payments-fintech/           (18 skills)
    ├── physical-security/          (18 skills)
    ├── privacy-dlp/                (18 skills)
    ├── professional-practice/      (18 skills)
    ├── scope-and-operations/       (18 skills)
    ├── secrets-pki/                (18 skills)
    ├── secure-development/         (18 skills)
    ├── siem-soc/                   (18 skills)
    ├── supply-chain-advanced/      (18 skills)
    ├── tabletop-recovery/          (18 skills)
    ├── threat-hunting/             (18 skills)
    ├── vulnerability-management/   (18 skills)
    ├── web-application/            (18 skills)
    ├── web-protocols/              (18 skills)
    ├── windows-ad/                 (18 skills)
    └── wireless-rf/                (18 skills)
```

---

## Skill Categories

| Category | Skills | Focus |
|---|---|---|
| `advanced-code-review` | 18 | Crypto API misuse, deserialization, race conditions, FFI, TOCTOU |
| `adversary-emulation` | 18 | Purple team, MITRE ATT&CK scenarios, detection gap measurement |
| `ai-agent-assurance` | 18 | Agent tool allowlists, prompt injection, memory poisoning, rollback |
| `ai-and-llm-security` | 18 | Model input/output, retrieval injection, supply chain, OWASP GenAI |
| `api-security` | 18 | REST/GraphQL authorization, rate limiting, BOLA, mass assignment |
| `asset-discovery` | 18 | Scope enumeration, shadow IT, subdomain mapping |
| `authentication-and-sessions` | 18 | MFA, session fixation, JWT, credential stuffing |
| `aws-specific` | 18 | IAM effective permissions, S3 policy, Lambda, CloudTrail |
| `azure-specific` | 18 | Entra ID, RBAC, storage SAS tokens, Defender coverage |
| `binary-analysis` | 18 | Static analysis, memory safety, stripped binaries, CFI |
| `blockchain-contracts` | 18 | Smart contract logic, reentrancy, oracle trust, OWASP SCSTG |
| `browser-client` | 18 | CSP, CORS, postMessage, extension permissions, WebAuthn |
| `bug-bounty-advanced` | 18 | Chaining low-severity findings, scope edge cases, impact escalation |
| `bug-bounty-workflow` | 18 | Triage, reproducibility, disclosure timelines, program rules |
| `ci-cd-advanced` | 18 | Pipeline poisoning, artifact integrity, secret scanning, SBOM |
| `cloud-forensics` | 18 | Log collection, chain of custody, volatile data, cloud provider APIs |
| `cloud-platforms` | 18 | Multi-cloud baseline, shared-responsibility boundary, posture review |
| `containers-and-kubernetes` | 18 | Image scanning, Pod Security Admission, network policy, RBAC |
| `crypto-and-data-protection` | 18 | Encryption at rest/transit, key management, tokenization |
| `crypto-protocols` | 18 | TLS configuration, cipher suites, certificate validation, HSTS |
| `database-storage` | 18 | Injection, privilege review, encryption, backup exposure |
| `detection-engineering` | 18 | Alert logic, false-positive review, coverage mapping to ATT&CK |
| `dns-edge` | 18 | Subdomain takeover, DNSSEC, resolver policy, CDN configuration |
| `email-security` | 18 | SPF/DKIM/DMARC, phishing simulation policy, gateway review |
| `endpoint-and-operating-systems` | 18 | Hardening baselines, EDR coverage, patch posture, local privilege |
| `enterprise-identity` | 18 | Directory tiering, privileged access, federation, lifecycle |
| `gcp-specific` | 18 | IAM bindings, org policy, VPC service controls, Cloud Armor |
| `incident-response` | 18 | NIST SP 800-61 lifecycle, containment, eradication, post-incident |
| `iot-firmware` | 18 | Firmware extraction, default credentials, update signing, UART |
| `kubernetes-advanced` | 18 | Admission controllers, etcd encryption, node isolation, supply chain |
| `linux-unix` | 18 | SUID/SGID, cron, capabilities, PAM, auditd |
| `malware-forensics` | 18 | Static and dynamic analysis, sandbox evasion, IOC extraction |
| `ml-ops-security` | 18 | Model pipeline, training data integrity, registry access, inference API |
| `mobile-and-client-apps` | 18 | Binary protections, deep link handling, shared storage, IPC |
| `mobile-android` | 18 | Manifest permissions, content providers, root detection, MASVS |
| `mobile-ios` | 18 | Keychain, entitlements, ATS, jailbreak detection, MASVS |
| `network-and-infrastructure` | 18 | Segmentation, firewall review, lateral movement paths, VLAN |
| `oauth-federation` | 18 | Token scope, redirect URI validation, PKCE, federation trust |
| `ot-iot-and-hardware` | 18 | Safety-first, passive enumeration, NIST SP 800-82 alignment |
| `ot-protocols` | 18 | Modbus, DNP3, OPC-UA — passive review and configuration checks |
| `payments-fintech` | 18 | PCI DSS scope, cardholder data flows, sandbox testing rules |
| `physical-security` | 18 | Access control review, tailgating, camera coverage, visitor policy |
| `privacy-dlp` | 18 | Data classification, DLP rule testing, retention, cross-border flows |
| `professional-practice` | 18 | Engagement setup, communication, ethics, documentation standards |
| `scope-and-operations` | 18 | Scope definition, rules of engagement, kick-off, out-of-scope handling |
| `secrets-pki` | 18 | Secret detection in code/CI, PKI hierarchy, certificate lifecycle |
| `secure-development` | 18 | SSDLC gates, threat modeling, NIST SP 800-218 (SSDF) alignment |
| `siem-soc` | 18 | Log source coverage, detection coverage, tuning, escalation paths |
| `supply-chain-advanced` | 18 | Dependency confusion, typosquatting, build provenance, SLSA |
| `tabletop-recovery` | 18 | BCP/DR exercises, RTO/RPO validation, stakeholder simulation |
| `threat-hunting` | 18 | Hypothesis formation, data source selection, anomaly triage |
| `vulnerability-management` | 18 | Scanner tuning, prioritization, SLA tracking, false-positive review |
| `web-application` | 18 | OWASP WSTG checks — injection, authz, business logic, file handling |
| `web-protocols` | 18 | HTTP semantics, WebSockets, HTTP/2, cache behavior, header policy |
| `windows-ad` | 18 | Kerberos, ACL review, GPO, privileged groups, LAPS |
| `wireless-rf` | 18 | Wi-Fi configuration, Bluetooth, RF signal review, rogue AP detection |

---

## Skill Format

Every `SKILL.md` follows this structure:

```markdown
---
name: <kebab-case-slug>
description: "<one line — what check this is and when to load it>"
---

# <Human-readable title>

## Preconditions
- Load CONTRACT.md and the engagement scope before any tool call.
- Impact class: <low-impact | medium-impact | high-impact | requires-approval>
- Specific scope gate or approval requirement.

## Method
1. Define the question: what observable behavior is being checked.
2. Use synthetic accounts and owned assets.
3. Compare at least two independent signals.
4. Record exact observation, reproduction path, counterexample, and uncertainty.
5. Identify fix, owner, rollback consideration, and retest criterion.

## Evidence and output
- Produce: <named artifact type>
- Include: UTC time, scoped asset, inputs, tool/source, observations, confidence.
- Avoid: live secrets, private records, harmful payloads, broad enumeration.

## Reference
- [Publisher reference](URL); verify current documentation at assessment time.
```

The `catalog.json` file contains a machine-readable index of all 1,008 skills with `name`, `description`, `category`, and `path` fields, usable for embedding-based or keyword retrieval.

---

## Schemas

### Engagement scope (`schemas/engagement.example.json`)

Required fields before any tool call:

| Field | Purpose |
|---|---|
| `owner` | Organization authorizing the assessment |
| `scope_reference` | Unique engagement identifier |
| `targets` | Explicit list of in-scope assets |
| `excluded_targets` | Explicit exclusions |
| `allowed_methods` | Approved test methods |
| `start_utc` / `end_utc` | Time window |
| `rate_limit` | Requests per minute, concurrency cap |
| `data_policy` | Whether external model providers are permitted |
| `approval_contact` | Who to call on scope uncertainty or incident |
| `stop_conditions` | Conditions that require immediate halt |

### Finding output (`schemas/finding.example.json`)

| Field | Purpose |
|---|---|
| `observed_behavior` | What was actually seen, using synthetic accounts |
| `expected_behavior` | What the control should have done |
| `evidence_references` | Pointers to stored, immutable evidence |
| `impact_observed` | Confirmed impact on synthetic/test data |
| `impact_hypothesized` | Extrapolation to production — marked as unverified |
| `confidence` | `high` / `medium` / `low` |
| `severity_rationale` | Context-dependent reasoning, not just a CVSS number |
| `remediation` | Concrete fix |
| `retest` | Specific criterion to verify the fix |

---

## Model Routing

`MODEL_ROUTING.md` covers how to dispatch tasks between remote and local models:

- **OpenRouter** — for public/internal data where the engagement policy permits external providers. Never send raw secrets, credentials, or customer data.
- **Ollama (local)** — for restricted data, offline environments, or when the engagement data policy prohibits third-party processing. Validate output carefully; local model capabilities vary.

Key rules:

1. Classify data before routing: `public` → OpenRouter permitted; `restricted` → local only.
2. Never hardcode API keys, model IDs, or vendor promises in skills or prompts.
3. Treat all model output as a **proposal**. Validate targets, commands, schema, and side effects before any tool execution.
4. The host application — not the model — holds the tool allowlist and executes approved calls.

---

## Execution Contract

`CONTRACT.md` defines the rules every agent must follow. Key requirements:

**Before every tool call:**
- A structured engagement scope must be loaded and verified.
- Target, time window, method, identity, and impact must all match the scope exactly.
- New discovered assets are candidates, not automatic permission.
- State-changing, high-volume, third-party, and OT actions require explicit owner approval.

**Evidence rules:**
- Keep originals immutable. Timestamp in UTC.
- Label every statement: `observed`, `inferred`, or `unverified`.
- Never claim a vulnerability from a version banner or scanner output alone.
- Use synthetic content and dedicated test accounts for proof of concept.
- Report findings privately through the owner's designated channel.

**Stop conditions:**
- Service instability
- Unexpected personal or sensitive data encountered
- Scope uncertainty
- Stop condition triggers must be logged and the engagement contact alerted.

---

## Research Anchors

The skills are original assessment playbooks informed by (not copied from) these authoritative sources. Verify current versions when performing real work.

| Source | Coverage |
|---|---|
| NIST SP 800-115 | Security testing methodology (planning → execution → reporting) |
| NIST SP 800-61 Rev. 3 | Incident response lifecycle |
| NIST SP 800-218 (SSDF) | Secure software development |
| NIST SP 800-82 Rev. 3 | Operational technology security |
| OWASP WSTG | Web application test techniques |
| OWASP ASVS | Verifiable application controls |
| OWASP API Security Top 10 | API-specific risks |
| OWASP MASVS | Mobile application security |
| OWASP Top 10 for LLM Applications | AI/LLM risks |
| OWASP Smart Contract Security Testing Guide | Blockchain/contract assessment |
| MITRE ATT&CK (Enterprise + ICS) | Adversary behavior vocabulary |
| MITRE D3FEND | Countermeasure graph |
| MITRE ATLAS | AI threat landscape |
| CIS Controls v8.1 | Critical security controls |
| AWS / Azure / GCP security pillars | Cloud-provider-specific guidance |
| Kubernetes Pod Security Admission | Container security |
| PCI SSC document library | Payments and cardholder data |

---

## What This Pack Is Not

- **Not a certification.** Skills are not mapped to numbered compliance requirements.
- **Not an exhaustive CVE index.** Coverage is by control family and assessment method, not by individual vulnerability ID.
- **Not a substitute for experience.** The judgment to choose the right check, interpret ambiguous evidence, and explain risk in context comes from practice and evaluation on real test cases.
- **Not a runner.** No live-target automation, exploit payloads, or credentials are included.

What will matter in real assessments is whether an agent can: choose the right skill for the task, use its tools reliably within scope, collect and label evidence correctly, recognize and communicate uncertainty, and produce findings that help owners fix things. That capability needs to be built and evaluated separately.

---

## License

Copyright (c) 2026. Permission is granted to use, copy, modify and distribute this original skill pack. Provided as-is, without warranty. Third-party tools and models have their own terms.
