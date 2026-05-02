# Signed Impact Attestation v0.1

**Signed Impact Attestation v0.1** is a minimal, portable, and auditable envelope for attaching cryptographic accountability to an **Impact Score Profile**.  
It does **not** define ownership or payment. It defines **who attested an impact evaluation, when, and over which exact payload**.  
This repository provides the YAML specification, JSON Schema, sample JSON, and validation workflow for the attestation layer of AI-civilization trace infrastructure.

---

## What this repository is

This repository defines a specification for **signing and attesting contribution evaluations**.

In the broader trace architecture:

- **CTR-ID** answers **what happened**
- **Impact Score Profile** answers **how much it contributed**
- **Signed Impact Attestation** answers **who stands behind that evaluation, when, and with what integrity proof**

That makes this repository a core building block for:

- Communication Royalty OS
- Royalty OS
- Gratitude OS
- Trust OS
- Existence Proof OS integrations
- auditable downstream allocation pipelines

---

## Why this matters

An impact score by itself is useful, but incomplete.

Without attestation, an impact profile is only a claim.
With attestation, it becomes:

- **accountable**
- **integrity-bound**
- **reviewable**
- **auditable**
- **ready for downstream trust or allocation layers**

This specification separates:

- **scoring logic** from
- **signature / attestation logic**

That separation is important.

It keeps the **Impact Score Profile** minimal and reusable, while allowing higher layers to add:

- issuer responsibility
- cryptographic signatures
- review approval
- dispute tracking
- existence-proof linkage
- allocation readiness

---

## Design goals

Signed Impact Attestation v0.1 is designed to be:

- **minimal**  
  Small enough to adopt early.

- **portable**  
  Usable across repositories, systems, and governance layers.

- **machine-readable**  
  Suitable for automated validation and verification pipelines.

- **human-auditable**  
  Clear enough for reviewers and institutions to inspect.

- **layered**  
  It does not replace scoring systems, provenance systems, or payment systems.

- **future-compatible**  
  It can connect to Trust OS, Gratitude OS, Royalty OS, and Existence Proof OS later.

---

## Non-goals

This specification is **not**:

- a legal ownership system
- a payment protocol
- a royalty settlement engine
- a universal provenance graph
- an identity verification framework
- a mandatory scoring formula

It only defines a **signed attestation envelope** around an impact evaluation.

---

## Core idea

A **Signed Impact Attestation** is a cryptographically bound statement that says:

> a specified issuer asserted, reviewed, approved, rejected, or disputed  
> a specific impact evaluation  
> for a specific communication-derived trace subject.

A valid signature in this specification means:

- the signer accepts responsibility for the attested statement
- the statement is bound to a specific payload hash
- the attestation can later be verified, reviewed, disputed, or consumed downstream

It does **not** mean universal truth.
It means **accountable assertion**.

---

## Repository structure

```text
.
├── .github/
│   └── workflows/
│       └── validate-specs.yml
├── examples/
│   └── signed-impact-attestation.sample.json
├── schemas/
│   └── signed-impact-attestation-v0.1.schema.json
├── spec/
│   └── signed-impact-attestation-v0.1.yaml
└── README.md
Files
spec/signed-impact-attestation-v0.1.yaml

Human-readable canonical specification in YAML form.

This file explains:

purpose
scope
design principles
document model
processing rules
semantic rules
security and privacy considerations
interoperability expectations
schemas/signed-impact-attestation-v0.1.schema.json

JSON Schema for validating Signed Impact Attestation documents.

This schema ensures:

required fields exist
enums stay consistent
conditional rules are enforced
sample documents are structurally valid
examples/signed-impact-attestation.sample.json

A minimal working sample document that validates against the schema.

This file is the quickest way to understand the profile in practice.

.github/workflows/validate-specs.yml

GitHub Actions workflow for automated schema validation.

This lets the repository function as a machine-checked specification repo, not just a prose draft.

Relationship to other specifications
Impact Score Profile

Impact Score Profile evaluates how strongly a communication-derived trace event contributed to later meaning generation.

Signed Impact Attestation does not replace it.
Instead, it adds:

issuer declaration
integrity binding
signature metadata
review state
governance-ready verification hooks

In short:

Impact Score Profile = evaluation layer
Signed Impact Attestation = attestation layer
CTR-ID

CTR-ID identifies the communication-derived trace subject.

Signed Impact Attestation may reference CTR-ID to clarify:

which communication event is being discussed
which trace-derived subject is being attested

In short:

CTR-ID = what
Impact Score = how much
Signed Impact Attestation = who attested it, when, and over what exact payload
Existence Proof OS

Signed Impact Attestation is designed to connect naturally with existence-proof systems.

This allows future integration of:

issuer identity anchoring
signer credential linkage
accountable review trails
durable entity references
What the profile contains

A Signed Impact Attestation document typically includes:

subject references
which impact profile or trace subject is being attested
issuer declaration
who issues the attestation
attestation meaning
whether the score is asserted, reviewed, approved, rejected, disputed, etc.
integrity block
how the payload was canonicalized and hashed
signature policy
how many valid signatures are required, and under what threshold rule
signatures
one or more cryptographic signatures
optional evidence / governance / chain links
for richer audit and downstream usage
Example flow

A typical downstream flow looks like this:

CTR-ID
→ Trace Record
→ Impact Score Profile
→ Signed Impact Attestation
→ Audit / Review
→ Trust OS / Gratitude OS / Royalty OS / Communication Royalty OS

This repository covers the Signed Impact Attestation step.

Validation
Local validation

Install dependencies:

pip install jsonschema pyyaml

Validate the sample against the schema:

python - <<'PY'
import json
from jsonschema import validate

with open("schemas/signed-impact-attestation-v0.1.schema.json", "r", encoding="utf-8") as f:
    schema = json.load(f)

with open("examples/signed-impact-attestation.sample.json", "r", encoding="utf-8") as f:
    sample = json.load(f)

validate(instance=sample, schema=schema)
print("OK: Signed Impact Attestation v0.1 sample is valid.")
PY
GitHub Actions

This repository also supports automated validation via:

.github/workflows/validate-specs.yml

That workflow is intended to verify that:

the schema file exists
the schema is valid JSON Schema
the sample file exists
the sample validates successfully
Processing model

A verifier is expected to:

resolve the referenced impact subject if available
recompute the subject hash
confirm the subject hash matches
canonicalize the attestation payload
recompute the payload hash
verify the required signatures
confirm the signature policy threshold is satisfied
respect status, validity windows, disputes, and revocations

This means the profile is not just descriptive.
It is meant to be verifiable in practice.

Security considerations

Key risks include:

compromised private keys
weak canonicalization
unstable subject references
over-centralized trust in a single issuer

Recommended mitigations include:

strong key management
deterministic canonicalization
hash-bound subject snapshots
multi-signature review for allocation-critical paths
Privacy considerations

Implementers should avoid leaking unnecessary metadata.

Recommendations:

allow pseudonymous but stable issuer identifiers where policy permits
minimize exposed evidence
use only the least disclosure needed for downstream verification
Why the separation matters

This repository intentionally keeps attestation separate from score computation.

That separation gives several advantages:

scoring systems can evolve independently
multiple evaluators can sign the same score profile
disputes can be added without mutating the original score logic
trust and allocation systems can consume attested records without owning the scoring model itself

This is one of the main architectural principles of the specification.

Intended downstream use

Signed Impact Attestation can be consumed by higher layers such as:

Trust OS
to weigh confidence in contribution records
Gratitude OS
to route non-monetary acknowledgment based on approved impact traces
Royalty OS
to feed accountable allocation inputs into value-routing systems
Communication Royalty OS
to connect communication-derived contribution traces to downstream value circulation

In all such cases, the attestation layer helps answer:

who approved the contribution record
whether it was integrity-checked
whether it is ready for downstream use
Versioning note

This is v0.1.

That means the repository aims to establish:

the minimal envelope
the field model
the validation baseline
the interoperability direction

It does not attempt to solve every future requirement in the first version.

That restraint is intentional.

Future directions

Possible future expansions include:

stronger issuer identity / credential binding
richer multi-signature governance modes
dispute and revocation registries
audit protocol integration
explicit linkage to Existence Proof OS credentials
allocation-readiness profiles for Royalty OS pipelines
Who this is for

This repository may be useful for:

protocol designers
AI trace infrastructure builders
provenance and attestation researchers
trust / gratitude / royalty system architects
governance-layer implementers
anyone designing accountable contribution evaluation pipelines
Summary

Signed Impact Attestation v0.1 is a minimal specification for making impact evaluations:

signed
accountable
integrity-bound
auditable
ready for downstream trust and value-circulation systems

It does not define ownership.
It does not execute payment.
It defines the attestation layer that lets later systems rely on an impact evaluation responsibly.

License

Add the license that matches your intended governance model for this repository.

For example:

open specification style license
source-available documentation license
custom protocol publication license

If you already use a project-wide license pattern, keep this repository aligned with it.

Status

Draft specification, but validation-ready.
