# Unified Algorithmic Process Format (UAPF)

![Status: Draft](https://img.shields.io/badge/status-draft-orange)
![Version 0.9](https://img.shields.io/badge/version-0.9-blue)
![License: MIT](https://img.shields.io/badge/license-MIT-green)
![Spec Type: Open Standard](https://img.shields.io/badge/spec-open%20standard-6f42c1)

> Open standard for machine-executable enterprise processes.

## Overview
UAPF (Unified Algorithmic Process Format) is an open, machine-readable standard for expressing enterprise workflows, decision logic, roles, exceptions, and multi-agent execution semantics. It enables organizations to convert human workflows and undocumented operational knowledge into explicit, portable, AI-executable algorithms—the universal algorithmization layer for modern AI-driven enterprises.

## Motivation
Enterprises increasingly deploy AI agents to execute business processes, but:

- Most processes are described only in text.
- Many operational rules live only in employees’ heads.
- Process knowledge disappears when employees change roles.
- Every AI agent platform uses different internal structures.

UAPF solves this by providing:

- A universal import/export format.
- Alignment with established international standards (OMG BPMN/DMN/CMMN).
- AI-native capabilities (MCP, A2A).
- Version-controllable, machine-executable models.
- Interoperability across agents, orchestrators, and institutions.

## Design Principles
- **No new modelling language:** Reuses existing ISO/OMG standards.
- **Agent-native:** Models include agent roles, capabilities, MCP tools, and A2A messaging schemas.
- **Interoperable:** Portable across modelling tools, automation engines, and AI runtimes.
- **Composable:** BPMN + DMN + CMMN + MCP + A2A → one algorithmic package.
- **Governable:** Supports metadata, provenance, auditability, and compliance.

## UAPF Enterprise Hierarchy (Tier 0–4)
UAPF packages are organized into five tiers that scale from atomic, reusable tasks (Tier 4) to complete enterprise digital twins (Tier 0). The hierarchy enforces downward-only dependencies, enabling predictable composition, governance, and reuse across domains, cross-functional processes, departmental variations, and enterprise-wide bundles.

## Specification Structure
A UAPF package is a zip-based container:

```
<process>.uapf/
  ├── manifest.json
  ├── process/
  │     ├── bpmn.xml
  │     ├── cmmn.xml
  ├── decisions/
  │     ├── *.dmn
  │     └── glossary.json
  ├── agents/
  │     ├── roles.json
  │     ├── capabilities.json
  │     └── bindings.json
  ├── integration/
  │     ├── mcp-tools.json
  │     ├── a2a-schemas.json
  │     └── api-endpoints.json
  └── metadata/
        ├── version.json
        ├── provenance.json
        └── compliance.json
```

Each file has a formal JSON Schema defined under `/schemas`.

## Core Technologies
UAPF aligns with globally adopted standards:

| Component             | Standard                     |
| --------------------- | ---------------------------- |
| Workflow              | BPMN 2.0 / BPMN 3.0 draft    |
| Decision Logic        | DMN 1.3+                     |
| Case Management       | CMMN 1.1                     |
| Agent Tooling         | MCP (Model Context Protocol) |
| Multi-Agent Messaging | A2A Specification            |
| Serialization         | JSON Schema / XML            |

### Validator Schemas
Schemas located in `/schemas` validate UAPF packages and ensure cross-platform compatibility, including:

- `uapf-manifest.schema.json`
- `uapf-roles.schema.json`
- `uapf-capabilities.schema.json`
- `uapf-bindings.schema.json`
- `uapf-mcp-tools.schema.json`
- `uapf-a2a-schemas.schema.json`

## Example Use Cases
### Government
- Document intake → routing → approvals → archival.
- Inter-institution coordination.
- Automatic classification via DMN.
- Multi-agent distribution using A2A.

### Software Development (RUP/Agile)
- Iteration workflows.
- Design → analysis → implementation flows.
- Agent-based code generation, testing, reporting.

### Enterprise Operations
- Procurement.
- Compliance workflows.
- Customer service logic.
- Risk evaluation.

## Execution Lifecycle
An AI orchestrator (e.g., LangGraph, CrewAI, AutoGen) loads UAPF as:

1. Graph (from BPMN/CMMN).
2. Ruleset (from DMN).
3. Agent mapping (roles → tasks).
4. Tool interface via MCP.
5. Multi-agent coordination via A2A.

This enables full organizational operations to be executed by agents.

## Who should use this repo?

This repository is intended for:

- **Tool vendors** adding UAPF import/export to modelling tools.
- **AI platform and agent orchestrator authors** who want to execute UAPF packages.
- **Enterprise / government architects** defining canonical, cross-tool process models.

If you simply want to load a `.uapf` file in your application, you may prefer using:

- [`uapf-python`](https://github.com/UAPFormat/uapf-python)
- [`uapf-typescript`](https://github.com/UAPFormat/uapf-typescript)

which both embed and track the schemas from this repository.

## Validation workflow

A typical validation flow for a `.uapf` package is:

1. Unzip the package.
2. Validate `manifest.json` against `schemas/uapf-manifest.schema.json`.
3. Validate `agents/*.json` against:
   - `schemas/uapf-roles.schema.json`
   - `schemas/uapf-capabilities.schema.json`
   - `schemas/uapf-bindings.schema.json`
4. Validate integration metadata:
   - `integration/mcp-tools.json` → `schemas/uapf-mcp-tools.schema.json`
   - `integration/a2a-schemas.json` → `schemas/uapf-a2a-schemas.schema.json`

Language SDKs and conformance fixtures:

- Parsers & validators: [`uapf-python`](https://github.com/UAPFormat/uapf-python), [`uapf-typescript`](https://github.com/UAPFormat/uapf-typescript)
- Conformance fixtures: [`uapf-conformance`](https://github.com/UAPFormat/uapf-conformance)


## Roadmap
### v1.0 Goals
- Finalize schema versioning.
- Add canonical examples.
- Publish reference parser (Python/Node.js).
- Publish UAPF Playground (web viewer).
- Integrate Veritrust decentralized identity.

### v1.1 Goals
- Formal BPMN 3.0 agentic extensions.
- Simulation engine.
- Compliance and safety modules.
- EU AI Act alignment.

## License
MIT License — open standard, free to use, free to implement.

## Contributions
Pull requests, issues, and proposals are welcome. Standardization direction is coordinated under the Algomation / UAPF working group.
