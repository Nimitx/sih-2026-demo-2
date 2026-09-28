# CryptoAtlas-E

### Enterprise Cryptographic Discovery & Quantum Migration Intelligence

**Smart India Hackathon 2026**

**Problem Statement ID:** SIH26164  
**Problem Statement:** Enterprise Cryptographic Discovery & Analysis Tool (ECDAT)  
**Organization:** National Technical Research Organisation (NTRO)  
**Category:** Software  
**Theme:** Blockchain & Cybersecurity  
**Team:** Rangers101

---

## Overview

**CryptoAtlas-E** is an evidence-centered cryptographic discovery and migration-planning platform designed to help organisations answer:

> **What cryptography exists, where is it used, what depends on it, what is quantum-vulnerable, and what should be migrated first?**

Modern enterprise cryptography is distributed across many environments, including:

- source code;
- software dependencies;
- binaries;
- container images;
- certificates;
- TLS services;
- SSH and VPN infrastructure;
- key-management services;
- HSMs;
- legacy applications.

Post-quantum migration therefore cannot begin reliably with a simple search for algorithm names.

An organisation must first establish a **trustworthy cryptographic inventory backed by evidence**.

---

## The Problem

Finding a cryptographic component does not automatically prove how it is being used.

For example:

```text
OpenSSL Installed
```

proves that cryptographic capabilities are available.

It does **not** automatically prove:

```text
The application actively uses RSA.
```

Similarly:

```text
Certificate found on disk
```

does not automatically prove:

```text
That certificate is deployed on a production TLS endpoint.
```

CryptoAtlas-E therefore separates:

```text
What was observed
        ↓
What that evidence actually supports
        ↓
What remains unknown
        ↓
What migration action is justified
```

---

## Proposed Architecture

```text
Authorized Scope
        ↓
Discovery Workers
        ↓
Observations + Errors + Coverage
        ↓
Normalization
        ↓
Identity Resolution
        ↓
Assertion Ledger
        ↓
Versioned Cryptographic Inventory
        ↓
 ┌──────────────┬────────────┬────────────┬────────────┐
 │ Knowledge    │ CBOM       │ Quantum    │ Drift      │
 │ Graph        │ Export     │ Assessment │ Analysis   │
 └──────────────┴────────────┴────────────┴────────────┘
        ↓
Role-Aware Migration Planner
        ↓
Conditional Migration Impact
        ↓
Deployment Validation / Rescan
```

The inventory acts as the core source of truth.

The graph, CBOM, risk assessment and migration views are derived from the evidence-backed inventory rather than independently inventing facts.

---

## Discovery Surfaces

CryptoAtlas-E is designed to support multiple cryptographic discovery surfaces.

### Source Code

Possible findings include:

- cryptographic API calls;
- algorithm parameters;
- imports;
- aliases;
- configuration references;
- file and line locations.

---

### Dependencies

Dependency analysis identifies:

- cryptographic packages;
- package versions;
- component relationships;
- known provider capabilities.

A dependency being installed does not automatically prove that a particular algorithm is actively used.

---

### Binary Analysis

Static binary inspection may examine:

- linked libraries;
- imported symbols;
- metadata;
- hashes;
- strings.

The scanner does not execute untrusted binaries.

---

### Containers

OCI / Docker images may be inspected for:

- packages;
- binaries;
- certificates;
- configuration files;
- cryptographic dependencies.

Container inspection does not automatically prove how the final runtime deployment behaves.

---

### X.509 Certificates

Certificate parsing may extract:

- certificate fingerprint;
- public-key algorithm;
- key size;
- signature algorithm;
- subject;
- issuer;
- validity period;
- public-key fingerprint.

---

### Network / TLS

Authorized TLS observations may reveal:

- TLS version;
- presented certificate;
- negotiated key-establishment mechanism;
- record-protection algorithm;
- endpoint information.

A single observed handshake describes that observation only.

It does not prove every client negotiates the same configuration.

---

### KMS / HSM Metadata

Future integrations may use read-only metadata from supported key-management or hardware-security systems.

Raw secret/private-key material is not required for the cryptographic inventory.

---

## Evidence Model

CryptoAtlas-E intentionally avoids turning all findings into one generic confidence percentage.

Instead, observations support explicit claim types.

Examples include:

```text
PRESENT
PROVIDES
REFERENCES
STATIC_CALL
CONFIGURES
DEPLOYS
NEGOTIATES
OBSERVED_OPERATION
PROTECTS
```

---

## Example

```text
OpenSSL Package
      ↓
PROVIDES
RSA / X25519 capability
```

This is different from:

```text
Application Source
      ↓
STATIC_CALL
RSA Signature Operation
```

which is different from:

```text
Production TLS Endpoint
      ↓
DEPLOYS
RSA Certificate
```

and also different from:

```text
Observed TLS Session
      ↓
NEGOTIATES
X25519 Key Establishment
```

CryptoAtlas-E does not silently convert one of these claims into another.

---

## Evidence Provenance

Each important observation can preserve information such as:

- collector;
- scan scope;
- observation time;
- source location;
- parser/rule version;
- evidence type;
- claim type;
- limitations;
- resolution state;
- freshness.

This allows an analyst to inspect why a conclusion exists.

---

## Cryptographic Identity Resolution

The same cryptographic asset may be observed through multiple discovery surfaces.

CryptoAtlas-E therefore performs conservative identity reconciliation.

For example, matching certificate fingerprints from:

```text
Source Repository
Container Image
Live TLS Endpoint
```

can support the conclusion that the same certificate appears in several contexts.

However:

```text
Same Algorithm
≠
Same Cryptographic Asset
```

```text
Same Public Key
≠
Same Certificate
```

```text
Same Package Version
≠
Same Deployment
```

Ambiguous relationships remain unresolved instead of being automatically merged.

---

## Knowledge Graph

The cryptographic inventory can be represented as a typed dependency graph.

Possible entities include:

- Business Service
- Application
- Deployment
- Artifact
- Component
- Cryptographic Use
- Algorithm
- Protocol
- Certificate
- Key Metadata
- Protected Data
- Owner

Possible relationships include:

```text
DEPENDS_ON
DEPLOYED_AS
INSTANCE_OF
PROVIDES
STATIC_CALL
CONFIGURES
DEPLOYS
NEGOTIATES
USES_ALGORITHM
HAS_PUBLIC_KEY
SIGNED_BY
PROTECTS
OWNED_BY
```

---

## Correct TLS Role Separation

CryptoAtlas-E treats different cryptographic roles independently.

Example:

```text
Payment API
      ↓
TLS Gateway
      ↓
TLS 1.3
      │
      ├── Key Establishment
      │      X25519
      │
      ├── Authentication
      │      RSA Certificate
      │
      └── Record Protection
             AES-GCM
```

An RSA certificate does not automatically mean RSA is being used for TLS key establishment.

Similarly, replacing the key-establishment mechanism does not automatically migrate certificate authentication.

---

## Cryptography Bill of Materials

CryptoAtlas-E supports a **CycloneDX 1.7-aligned Cryptography Bill of Materials (CBOM)**.

The CBOM can represent information about:

- cryptographic algorithms;
- certificates;
- key references;
- protocols;
- software components;
- services;
- relationships.

CBOM is a standards-based representation and is not a proprietary CryptoAtlas-E invention.

The richer evidence ledger remains separate from the exported CBOM.

---

## Post-Quantum Cryptography

CryptoAtlas-E helps identify cryptographic uses that may require future migration.

Examples of public-key families affected by a sufficiently capable future quantum computer include:

- RSA;
- finite-field Diffie-Hellman;
- elliptic-curve Diffie-Hellman;
- ECDSA.

Strong symmetric cryptography remains a separate consideration.

Algorithms such as AES are not broken by Shor's algorithm.

---

## Mosca X/Y/Z Migration Model

CryptoAtlas-E uses Mosca's migration-planning concept.

```text
X = Required confidentiality lifetime

Y = Time required to migrate

Z = Analyst-selected quantum-threat scenario horizon
```

A useful planning expression is:

```text
U = X + Y - Z
```

If:

```text
X + Y > Z
```

migration pressure becomes more urgent under that scenario.

`Z` is treated as a **scenario assumption**.

It is not presented as a known date or predicted “Q-Day”.

---

## Role-Aware Migration Planning

CryptoAtlas-E does not blindly replace one algorithm name with another.

The recommendation depends on the role being performed.

### Key Establishment

Possible post-quantum candidate:

```text
ML-KEM
```

or an appropriate standardized hybrid mechanism where supported.

---

### Digital Signatures

Possible candidates include:

```text
ML-DSA
SLH-DSA
```

depending on requirements and ecosystem support.

---

### Symmetric Encryption

Strong symmetric algorithms may remain appropriate according to policy and threat model.

---

## Planner Outcomes

The migration planner may return:

```text
READY FOR LAB
CONDITIONAL
BLOCKED
NO CHANGE INDICATED
ABSTAIN
```

For example, if CryptoAtlas-E observes RSA but cannot determine whether it is being used for signing or another role, the planner may return:

```text
ABSTAIN

Determine cryptographic role before selecting a migration candidate.
```

---

## Urgency vs Migration Readiness

Migration urgency and implementation readiness are treated separately.

For example:

```text
Urgency:
HIGH
```

while simultaneously:

```text
Migration Readiness:
BLOCKED BY LEGACY CLIENT
```

A difficult migration is not automatically a low-priority migration.

---

## Conditional Migration Impact

CryptoAtlas-E can evaluate a proposed migration without modifying the real environment.

Example:

```text
Current Key Establishment
X25519

        ↓

Proposed Key Establishment
X25519 + ML-KEM
```

The system may then identify:

```text
Modern Client A
Compatible

Modern Client B
Compatible

Legacy Client
Blocked / Fallback Required

Unknown Client
Not Tested
```

It also preserves residual issues such as:

- classical certificate authentication;
- PKI dependencies;
- unsupported clients;
- required compatibility tests;
- rollback requirements.

This is a **conditional change-impact analysis**.

It is not automatic production migration and does not prove that a deployed change will succeed.

A real rescan and validation step is required afterward.

---

## Cryptographic Drift

CryptoAtlas-E distinguishes multiple forms of change.

### Artifact Drift

Software, configuration or deployment changed.

### Policy Drift

The underlying cryptography remained the same but security policy or standards changed.

### Context Drift

Business information changed, such as:

- data-retention requirements;
- ownership;
- business criticality.

### Coverage Drift

A collector, endpoint or permission was unavailable during the latest scan.

A failed scan does not mean the cryptography disappeared.

Instead:

```text
NOT OBSERVED
COVERAGE INCOMPLETE
```

may be reported.

---

## Current Prototype

The current repository contains an **offline interactive prototype** using a simulated enterprise estate.

The prototype demonstrates:

- cryptographic discovery workflow;
- evidence recording;
- observation/assertion separation;
- cryptographic identity reconciliation;
- inventory creation;
- knowledge graph;
- TLS role separation;
- quantum-vulnerability analysis;
- Mosca scenarios;
- role-aware migration planning;
- conditional migration-impact analysis;
- coverage handling;
- cryptographic drift;
- CBOM preview/export;
- automatic demonstration mode.

---

## Important Prototype Note

The current browser environment is a **simulation prototype**.

Its:

- enterprise assets;
- observation counts;
- vulnerabilities;
- migration priorities;
- compatibility states;
- migration outcomes

are illustrative unless explicitly identified as measured results.

The prototype demonstrates the architecture and decision workflow.

Real scanner performance must be evaluated separately.

---

## Prototype Scenarios

### Enterprise PQC Readiness

Demonstrates the primary workflow:

```text
Discover
   ↓
Collect Evidence
   ↓
Reconcile Identity
   ↓
Build Inventory
   ↓
Assess
   ↓
Prioritize
   ↓
Plan Migration
   ↓
Evaluate Impact
   ↓
Export CBOM
```

---

### Evidence & Identity Challenge

Demonstrates cases where the system should refuse to overclaim.

Examples include:

```text
Installed Crypto Library
≠
Active Algorithm Usage
```

```text
Certificate Present
≠
Certificate Deployed
```

```text
Same Public Key
≠
Same Certificate
```

```text
Failed Endpoint Probe
≠
Safe Endpoint
```

---

### Crypto Drift Detection

Demonstrates comparison between inventory snapshots while distinguishing:

- artifact drift;
- policy drift;
- context drift;
- coverage drift.

---

## Technology Stack

### Current Prototype

- HTML
- CSS
- JavaScript
- SVG
- Offline browser execution

### Proposed Implementation

- Python
- FastAPI
- Tree-sitter
- static-analysis rules
- package-inventory tooling
- LIEF
- OCI / Docker inspection
- Python `cryptography`
- TLS inspection tooling
- PostgreSQL
- NetworkX
- React
- Cytoscape.js
- CycloneDX tooling
- Docker

---

## Security Principles

Because a discovery system processes potentially hostile or sensitive material, CryptoAtlas-E is designed around several safety requirements:

- scanned binaries are not executed;
- parsing should occur in isolated workers;
- file and archive size limits should be enforced;
- archive/decompression attacks must be considered;
- path traversal must be prevented;
- secret material should be redacted;
- raw private keys should never be stored;
- scanning must remain within authorized scope;
- access to inventory information should be controlled;
- evidence and administrative actions should be auditable.

---

## Evaluation Plan

Real implementation evaluation is intended to measure the entire pipeline.

### Discovery

- Precision
- Recall
- F1 Score
- false-positive behaviour

### Parameter Extraction

- algorithm;
- role;
- key size;
- mode;
- protocol;
- certificate attributes.

### Identity Resolution

- correct merges;
- false merges;
- missed merges.

### Evidence Semantics

- capability incorrectly promoted to usage;
- deployment-claim correctness;
- observation-scope correctness.

### Knowledge Graph

- edge correctness;
- dependency-path correctness;
- impact-path correctness.

### CBOM

- schema/reference validation;
- semantic integrity;
- relationship correctness.

### Migration Planner

- correct role mapping;
- policy consistency;
- incompatible recommendation rate;
- abstention behaviour.

### Drift

- new;
- changed;
- removed;
- policy-driven;
- context-driven;
- coverage-driven changes.

### Security

- malformed-input handling;
- secret leakage;
- authorized-scope enforcement.

No unmeasured scanner-performance values should be presented as achieved results.

---

## Key Design Contributions

CryptoAtlas-E does not claim to have invented cryptographic discovery, dependency graphs or CBOM.

The project's focus is the integration of:

- multi-surface cryptographic observations;
- explicit evidence provenance;
- capability-versus-usage distinction;
- conservative identity resolution;
- coverage-aware unknown states;
- typed cryptographic dependency relationships;
- role-aware post-quantum planning;
- Mosca-based migration timing;
- coverage-aware drift;
- conditional migration-impact analysis;
- rescan-based validation.

---

## Running the Demo

Clone the repository:

```bash
git clone <YOUR_GITHUB_REPOSITORY_URL>
```

Open the downloaded project folder.

Then open:

```text
index.html
```

using a modern desktop browser such as Google Chrome or Microsoft Edge.

The current prototype requires:

```text
No package installation
No backend
No external API
No internet connection
```

For a complete guided walkthrough, select:

```text
Auto Demo
```

---

## Current Status

| Component | Status |
|---|---|
| Interactive Simulation Prototype | ✅ Implemented |
| Evidence-Centered Workflow | ✅ Implemented |
| Observation / Assertion Model | ✅ Implemented |
| Cryptographic Knowledge Graph | ✅ Implemented |
| Mosca Scenario Workflow | ✅ Implemented |
| Migration Planning Workflow | ✅ Implemented |
| Conditional Impact Analysis | ✅ Implemented |
| Drift Demonstration | ✅ Implemented |
| CBOM Prototype Export | ✅ Implemented |
| Real Source Scanner | 🔄 Planned / In Development |
| Dependency Scanner | 🔄 Planned / In Development |
| Binary Scanner | 🔄 Planned |
| OCI / Container Scanner | 🔄 Planned |
| X.509 Discovery | 🔄 Planned |
| Active TLS Observation | 🔄 Planned |
| KMS / HSM Metadata Adapters | 🔄 Later Stage |
| Measured Evaluation Corpus | 🔄 Planned |

---

## Team

### Rangers101

Smart India Hackathon 2026

---

## Disclaimer

CryptoAtlas-E is currently a research and prototype system developed for **Smart India Hackathon 2026**.

The built-in enterprise environment is simulated.

Displayed asset counts, cryptographic findings, priorities, compatibility states and migration outcomes are illustrative unless explicitly identified as measured results.

The system is intended exclusively for authorized cryptographic inventory, defensive cybersecurity research and migration planning.