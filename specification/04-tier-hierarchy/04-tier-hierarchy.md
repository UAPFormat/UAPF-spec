# UAPF Specification — Chapter 4: Multi-Tier Enterprise Algorithm Hierarchy (Tier 0–Tier 4)

_Version 0.9 — Draft_

## 4.0 Overview

The Unified Algorithmic Process Format (UAPF) supports **hierarchical algorithm packaging** to represent
the structure of real-world enterprises. This hierarchy allows processes, decisions, tasks, and resource
bindings to be organized into **composable, reusable, versioned UAPF modules**.

UAPF defines **five tiers**, from atomic task packages (Tier 4) to full enterprise representations (Tier 0):

- **Tier 0** — Enterprise bundle (digital twin of the organization)
- **Tier 1** — Enterprise domains (Finance, HR, IT, etc.)
- **Tier 2** — Cross-functional domain processes (Invoice-to-Pay, Hire-to-Retire, etc.)
- **Tier 3** — Departmental / regional subprocesses and variants
- **Tier 4** — Atomic task algorithms

This hierarchy is essential for:

- enterprise-wide algorithm governance
- process reuse and modularity
- AI agent capability mapping
- digital twin representations of organizations
- versioning and dependency management
- portability across environments

> **Note:** Tiers describe the **packaging and scope hierarchy** of UAPF artifacts.
> They are orthogonal to the **runtime architecture layers** described in `algorithmation_deep_dive.md`.

---

## 4.1 Tier Definitions

### 4.1.1 Tier 0 — Enterprise UAPF Bundle (“Digital Twin Package”)

**Purpose**

Tier 0 represents the **entire enterprise algorithmic structure** in a single portable package.
A Tier 0 bundle contains all Tier 1–4 UAPF artifacts required to reconstruct the organization’s
behavior in another environment.

**Characteristics**

- Highest-level container
- Portable “digital twin” of an organization
- Contains full dependency graph
- Includes enterprise-wide identity, compliance, resource bindings, and integrations
- Suitable for migration, simulation, cloning, training and testing AI agents

**Recommended Structure (informative)**

```text
enterprise.uapf/
  manifest.json
  tier1/
    *.uapf
  tier2/
    <domain>/*.uapf
  tier3/
    <department>/*.uapf
  tier4/
    tasks/*.uapf
  metadata/
    identity.json
    compliance.json
    provenance.json
```

### 4.1.2 Tier 1 — Enterprise Domain Algorithms

**Purpose**

Tier 1 defines the top-level enterprise domains that every organization possesses, such as:

Finance

Human Resources

Customer / Citizen Lifecycle

IT Operations

Supply Chain

Product / Service Lifecycle

Risk & Compliance

A Tier 1 UAPF describes the high-level process structure of a domain and references one or more
Tier 2 UAPFs for concrete implementations.

**Characteristics**

- Foundational and stable over time
- Serve as domain “entry points”
- Contain references to multiple Tier 2 modules
- Capture domain-wide policies and invariants

### 4.1.3 Tier 2 — Cross-Functional Domain Processes

**Purpose**

Tier 2 represents operational workflows that span multiple departments within a domain.

Examples include:

Invoice-to-Pay

Order-to-Cash

Hire-to-Retire

Incident-to-Resolution

Customer Onboarding

**Characteristics**

- Contain executable workflow logic
- May include BPMN, DMN, and CMMN models
- Reference Tier 3 subprocesses as reusable components
- Are reused by multiple departments

### 4.1.4 Tier 3 — Department / Regional Subprocesses

**Purpose**

Tier 3 UAPFs capture organization-specific or region-specific variants of domain processes.

Examples:

Finance VAT workflow — Latvia

Finance VAT workflow — Germany

HR onboarding — IT department

Sales contract review — Legal department

**Characteristics**

- Implement detailed subprocesses and local variations
- Reference Tier 4 atomic tasks
- Often contain more detailed BPMN than Tier 1 or 2
- Encode local compliance, regulatory or policy differences

### 4.1.5 Tier 4 — Atomic Task Algorithms

**Purpose**

Tier 4 defines small, reusable tasks that form the building blocks of higher tiers.

Examples:

Extract fields from an ID document

Validate invoice format

Apply a risk scoring formula

Generate an email template

Perform a simple compliance check

**Characteristics**

- Smallest units of executable behavior
- Directly executable by AI agents, RPA bots, or workflow engines
- Context-independent and reusable across multiple processes
- Often mapped directly to MCP tools or other agent capabilities

## 4.2 Dependency Rules

Dependencies between tiers are strictly hierarchical.

Tier 0 may contain and reference Tier 1, 2, 3, and 4 UAPFs.

Tier 1 may reference Tier 2 UAPFs.

Tier 2 may reference Tier 3 UAPFs.

Tier 3 may reference Tier 4 UAPFs.

Tier 4 MUST NOT depend on higher tiers.

In manifest.json, dependencies are declared explicitly. For example:

{
  "id": "invoice-processing",
  "tier": 2,
  "version": "1.3.0",
  "dependencies": [
    "tier3/vat-check-latvia@^1.0.0",
    "tier4/validate-document-format@^2.1.0"
  ]
}


Normative rule:
A UAPF MUST NOT declare a dependency on a UAPF at a higher tier number (e.g. Tier 3 depending on Tier 2).

## 4.3 Resource Binding Across Tiers

Resource definitions (roles, agents, systems, capabilities) are defined and resolved across tiers as follows:

Tier 4 tasks declare the capabilities required to execute them (e.g. “validate-document-format”).

Tier 3 subprocesses bind those capabilities to concrete roles or agents for a given organization or region.

Tier 2 processes orchestrate Tier 3 subprocesses and typically do not redefine resources.

Tier 1 domain definitions remain mostly implementation-independent, referring to domain concepts rather than concrete agents.

Tier 0 may contain global resource maps and identity bindings used to reconstruct the environment in a new deployment.

The detailed resource and identity model is defined in the Resource Model Specification (see resource_model.md or equivalent).

## 4.4 Execution Model Across Tiers

When a Tier 0 bundle is loaded into a UAPF-compliant runtime:

The runtime reads the Tier 0 manifest and resolves the full dependency graph of Tier 1–4 UAPFs.

Tier 4 task packages are registered as atomic capabilities.

Tier 3 subprocesses bind tasks to roles, agents, and systems using the resource model.

Tier 2 domain processes orchestrate Tier 3 subprocesses to implement end-to-end flows.

Tier 1 domain entries expose high-level operations (e.g. “Process Invoice”, “Onboard Employee”).

AI agents, workflow engines, or other orchestrators execute processes according to this hierarchy.

This model allows a complete enterprise behavior to be represented, transported, and reconstructed
from a single Tier 0 bundle.

## 4.5 Relationship to Other Specification Chapters

The package structure of a single UAPF (folders like process/, decisions/, agents/, integration/, metadata/)
is specified in the package structure chapter.

The resource and identity model (roles, capabilities, bindings) is specified in the resource model chapter.

The algorithmation architecture layers (audit, identity, decision logic, risk, human-in-the-loop) are described in
the architecture deep dive (algorithmation_deep_dive.md).

The tier hierarchy in this chapter describes how UAPF packages are composed across the enterprise.
It does not constrain the internal modeling notation beyond what is defined in those other chapters.
