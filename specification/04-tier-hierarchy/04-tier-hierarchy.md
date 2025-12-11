# UAPF Specification — Chapter 4: Multi-Tier Enterprise Algorithm Hierarchy (Tier 0–Tier 4)

_Version 1.0 — Draft for Inclusion in UAPF Core Spec_

## 4.0 Overview

The Unified Algorithmic Process Format (UAPF) supports hierarchical algorithm packaging to represent the structure of real-world enterprises.
This hierarchy allows processes, decisions, tasks, and resource bindings to be organized in composable, reusable, versioned UAPF modules.

UAPF defines five tiers, from atomic tasks (Tier 4) to full enterprise representations (Tier 0).

This hierarchy is essential for:

- enterprise-wide algorithm governance
- process reuse
- AI agent capability mapping
- digital twins of organizations
- versioning and dependency management
- portability across systems and environments

## 4.1 Tier Definitions

### ⭐ Tier 0 — Enterprise UAPF Bundle ("Digital Twin Package")

**Purpose:**
Represents the entire enterprise algorithmic structure in a single portable package.
A Tier 0 package is a bundle containing all Tier 1–4 UAPFs.

**Characteristics:**

- Highest-level container
- Portable “digital twin” of an organization
- Contains full dependency graph
- Includes enterprise-wide identity, compliance, resource bindings, and integrations
- Suitable for migration, simulation, cloning, training and testing AI agents

**Recommended Structure:**

```
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

**Execution:**
A Tier 0 UAPF can be loaded into an enterprise orchestration engine or an AI agent cluster, reconstructing the entire behavioral model of the organization.

### ⭐ Tier 1 — Enterprise Domain Algorithms

**Purpose:**
Define the top-level enterprise capabilities that every organization possesses.

**Examples:**

- Finance
- HR
- Customer Lifecycle
- IT Operations
- Supply Chain
- Product Lifecycle
- Risk & Compliance

Tier 1 UAPFs provide high-level process structure, not detailed flows.

**Characteristics:**

- Foundational, stable over years
- Serve as domain “entry points”
- Contain references to multiple Tier 2 modules
- Define enterprise-wide policies and invariants

**Recommended Structure:**

```
finance.uapf/
  manifest.json
  process/bpmn.xml        (high-level domain BPMN)
  dependencies:
    tier2/invoice-processing.uapf
    tier2/record-to-report.uapf
  metadata/
    compliance.json
```

### ⭐ Tier 2 — Cross-Functional Domain Processes

**Purpose:**
Represent operational workflows that span multiple departments but belong to a domain.

**Examples:**

- Invoice-to-Pay
- Order-to-Cash
- Hire-to-Retire
- Incident-to-Resolution
- Customer Onboarding

**Characteristics:**

- Contain real executable logic
- Reference Tier 3 sub-processes
- Include decisions (DMN), case logic (CMMN), and tasks

**Recommended Structure:**

```
invoice-processing.uapf/
  manifest.json
  process/bpmn.xml
  decisions/*.dmn
  dependencies:
    tier3/vat-check-latvia.uapf
    tier3/vat-check-germany.uapf
  agents/
    roles.json
    capabilities.json
```

### ⭐ Tier 3 — Department / Regional Sub-Processes

**Purpose:**
Capture variations, regional rules, or detailed sub-processes.

**Examples:**

- HR Onboarding — Latvia
- HR Onboarding — Germany
- Finance VAT Workflow — Latvia
- Sales Contract Review — Legal Department

**Characteristics:**

- Represent organization-specific or region-specific logic
- Reference Tier 4 atomic tasks
- Often contain more detailed BPMN than Tier 1 or 2

**Recommended Structure:**

```
vat-check-latvia.uapf/
  manifest.json
  process/bpmn.xml
  decisions/vat-rules-lv.dmn
  dependencies:
    tier4/validate-document-format.uapf
    tier4/extract-invoice-fields.uapf
  metadata/
    compliance.json
```

### ⭐ Tier 4 — Atomic Tasks (“Executable Algorithm Units”)

**Purpose:**
Define the smallest reusable algorithms in the enterprise.

**Examples:**

- Extract fields from ID document
- Validate invoice format
- Apply risk scoring formula
- Generate email template
- Perform simple compliance check

These are the building blocks used by all higher tiers.

**Characteristics:**

- Very small UAPFs
- AI agents execute these directly via MCP tools
- Purely functional and context-independent

**Recommended Structure:**

```
validate-document-format.uapf/
  manifest.json
  process/bpmn.xml (minimal)
  rules/dmn.json (optional)
  integrations/mcp-tools.json
```

## 4.2 Dependency Rules

Tier dependencies are strictly hierarchical:

- Tier 0 → contains Tier 1–4
- Tier 1 → depends on Tier 2
- Tier 2 → depends on Tier 3
- Tier 3 → depends on Tier 4
- Tier 4 → no dependencies

Cross-tier upward references are forbidden.
This ensures clean, composable enterprise models.

Each manifest.json MUST declare:

```
"dependencies": [
  "tier3/vat-check-latvia@^1.0.0",
  "tier4/validate-document-format@^2.1.0"
]
```

## 4.3 Resource Binding Across Tiers

Resource bindings (roles, identities, capabilities, integrations) must follow:

- Declared at the lowest applicable tier
- Inherited upward as needed
- Overridden only at Tier 3

**Example:**

- Tier 4 defines capability “validate-format”
- Tier 3 inherits and binds local agents (e.g. FinanceBot_LV)
- Tier 2 uses the task without redefining resources
- Tier 1 remains implementation-independent

This ensures predictable composition.

## 4.4 Execution Model Across Tiers

When a Tier 0 package is loaded:

- The dependency graph is resolved
- Tier 4 tasks are registered as atomic capabilities
- Tier 3 workflows bind tasks to roles or agents
- Tier 2 processes orchestrate Tier 3 modules
- Tier 1 defines domain entry points

AI agents or orchestration engines execute the hierarchy

This enables full enterprise reconstruction from a single package.

## 4.5 Tier 0 Packaging Format

A Tier 0 bundle may be:

- .uapf-bundle (recommended new format)
- or .uapf with special bundle manifest

**Bundle manifest:**

```
{
  "type": "bundle",
  "tier": 0,
  "domains": ["finance", "hr", "it"],
  "specVersion": "1.0",
  "dependencies": [
    "tier1/finance.uapf",
    "tier1/hr.uapf",
    ...
  ]
}
```

## 📁 Where This Goes in the GitHub Organization

(Under https://github.com/UAPFormat)

To properly integrate this specification:

1. Add a new folder in the UAPF-Specification repository

   - Repo: https://github.com/UAPFormat/UAPF-Specification
   - New folder: `specification/04-tier-hierarchy/`
   - Create file: `04-tier-hierarchy.md`
   - Paste the full chapter there.

2. Update main specification index

   - File: `specification/index.md`
   - Add: `- [Chapter 4: Multi-Tier Enterprise Algorithm Hierarchy](04-tier-hierarchy/04-tier-hierarchy.md)`

3. Update the UAPF README to reference tiers

   - Repo: `UAPFormat/UAPF-Specification`
   - Add a section:

```
## UAPF Enterprise Hierarchy (Tier 0–4)
UAPF supports hierarchical structuring of algorithms...
(Insert summary paragraph)
```

4. Add example files to UAPF-Examples repo

   - Repo: https://github.com/UAPFormat/UAPF-Examples
   - Create folders:

```
tiers/
  tier0-enterprise/
  tier1-domain/
  tier2-process/
  tier3-subprocess/
  tier4-tasks/
```

   - Populate with:
     - empty example .uapf templates
     - example manifests
     - sample BPMN/DMN

5. OPTIONAL (Highly recommended):

   - Create new repo: `UAPFormat/UAPF-Tier0-Library`
   - A collection of:
     - Tier 1 templates
     - Tier 2 templates
     - Tier 3 variations
     - Tier 4 reusable atomic tasks

   This would be the official “UAPF Core Library.”
