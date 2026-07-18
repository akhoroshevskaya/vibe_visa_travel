---
document_id: GOV-000
title: Document Hierarchy & Source of Truth
version: 1.0
status: Approved
owner: Enterprise Architecture
layer: Governance
type: Architecture Governance
last_updated: YYYY-MM-DD
---

# GOV-000 — Document Hierarchy & Source of Truth

## Purpose

This document defines the hierarchy of architectural documentation within the **V!BE Business OS** project.

It establishes:

- architecture documentation levels;
- document priority rules;
- Source of Truth for every architectural aspect;
- conflict resolution rules;
- document lifecycle;
- mandatory rules for AI agents performing architecture analysis.

This document is the primary governance document for architecture documentation.

---

# 1. Architecture Documentation Hierarchy

Architecture documentation is organized into hierarchical levels.

Each level has a specific responsibility and authority.

Higher levels define business intent.

Lower levels define implementation.

Lower-level documents **must not contradict** higher-level documents.

---

# Level 0 — Reference

## Purpose

Navigation only.

These documents help humans and AI navigate the repository.

They do **not** define architecture.

## Documents

- README.md
- docs/README.md
- architecture/architecture-index.md

---

# Level 1 — Vision

## Purpose

Defines why the system exists.

Answers questions such as:

- Why are we building this system?
- What business problem does it solve?
- What is the long-term vision?

## Documents

- Business Vision
- PRD
- North Star

---

# Level 2 — Business Architecture

## Purpose

Defines how the business operates.

Describes:

- business domains;
- business capabilities;
- business principles;
- domain boundaries.

## Documents

- Business Architecture
- Business Domain Model
- Catalog Domain
- Business Principles

---

# Level 3 — Normative Architecture

## Purpose

Defines the official business architecture.

This level is considered the **normative architectural model**.

Implementation must follow this level.

## Documents

- Business Information Model
- Architecture Decision Records (ADR)
- Business Entity Relationships
- Business State Machines
- Architecture Freeze

---

# Level 4 — Governance

## Purpose

Defines how architecture is validated and governed.

Contains architectural rules, matrices, integrity checks and review policies.

## Documents

- Repository Dependency Matrix
- Property Coverage Matrix
- Business Entity Integrity Review
- Architecture Gap Analysis
- Repository Relationship Types
- Architecture Backlog
- Business OS Backlog

---

# Level 5 — Solution Architecture

## Purpose

Defines how the Business OS is designed.

Contains reusable architectural patterns.

## Documents

### Foundations

- Purpose
- Design Principles
- Workspace Architecture
- Architecture Building Blocks

### Standards

- Repository Meta Model
- Data Architecture
- Relationship Model

---

# Level 6 — Repository Specifications

## Purpose

Defines how every Business Entity is implemented inside Business OS.

Repository Specifications are implementation models.

They must conform to all higher architecture levels.

## Documents

- REP-001 Requests
- REP-002 Clients
- REP-003 Cases
- REP-004 Services
- REP-005 Documents
- REP-006 Workflows
- REP-007 Tasks
- REP-008 Payments

---

# Level 7 — Runtime

## Purpose

Defines runtime validation rules.

Contains metadata requirements and architecture validation logic.

## Documents

- Architecture Manifest
- Metadata Schema
- Validation Rules

---

# Level 8 — Implementation

## Purpose

Physical implementation of the architecture.

Examples:

- Notion Workspace
- Backend
- API
- Automation
- AI Agents
- Integrations

---

# 2. Source of Truth Matrix

| Architecture Aspect | Source of Truth |
|---------------------|-----------------|
| Product Vision | Business Vision |
| Business Principles | Business Principles |
| Business Domains | Business Architecture |
| Domain Boundaries | Business Domain Model |
| Product / Service Catalog | Catalog Domain |
| Business Entities | Business Information Model |
| Entity Relationships | Business Entity Relationships |
| Entity State Machines | Business State Machines |
| Architecture Decisions | ADR |
| Repository Design | Repository Specifications |
| Architecture Validation | Runtime |
| Repository Navigation | README / Architecture Index |

---

# 3. Priority Rules

When two documents contain conflicting information, the document with the higher architectural priority prevails.

Priority order:

```
Vision
    ↓
Business Architecture
    ↓
Normative Architecture
    ↓
Governance
    ↓
Solution Architecture
    ↓
Repository Specification
    ↓
Runtime
    ↓
Implementation
```

---

## Rule 1

Business Architecture has priority over Repository Specifications.

---

## Rule 2

Business Information Model has priority over Repository Specifications.

---

## Rule 3

Business Entity Relationships have priority over Repository implementation.

---

## Rule 4

Business State Machines define the official lifecycle.

Repository lifecycle must conform to them.

---

## Rule 5

ADR has priority over every architectural document except Architecture Freeze.

---

## Rule 6

Architecture Freeze defines the approved baseline for the current Engineering Sprint.

After a Freeze is approved, architecture changes require a new ADR and a new Freeze version.

---

# 4. Conflict Resolution Rules

Whenever an architecture conflict is detected, the following process must be used.

## Step 1

Determine the document level.

---

## Step 2

Determine the Source of Truth for the architectural aspect.

---

## Step 3

Compare only documents that belong to the same architectural concern.

---

## Step 4

Apply Priority Rules.

---

## Step 5

If a contradiction still exists, create an Architecture Review issue.

No implementation decision may be made until the conflict is resolved.

---

# 5. AI Review Rules

All AI agents must follow these rules.

## Rule 1

Always determine the architectural level before comparing documents.

---

## Rule 2

Never compare unrelated architectural levels directly.

For example:

❌ Runtime ↔ Business Model

without following Priority Rules.

---

## Rule 3

Never assume that every document has equal authority.

---

## Rule 4

Architecture Review must identify:

- document level;
- Source of Truth;
- applicable Priority Rule;
- confidence level.

---

## Rule 5

If document authority cannot be determined,

report:

> Unknown Authority

instead of reporting an architecture contradiction.

---

# 6. Document Lifecycle

Every architecture document must have a lifecycle.

```
Draft
    ↓
Review
    ↓
Approved
    ↓
Frozen
    ↓
Deprecated
    ↓
Archived
```

## Draft

Work in progress.

Not authoritative.

---

## Review

Awaiting approval.

May change.

---

## Approved

Official project documentation.

---

## Frozen

Locked baseline.

Used for Engineering implementation.

---

## Deprecated

Superseded by a newer document.

Should not be used for new work.

---

## Archived

Historical documentation.

Maintained only for traceability.

---

# 7. AI Working Agreement

Before performing any architecture analysis, every AI agent must:

1. Read `GOV-000 Document Hierarchy & Source of Truth`.
2. Determine document levels.
3. Determine the Source of Truth.
4. Apply Priority Rules.
5. Only then perform architecture analysis.

Failure to follow this process may produce false-positive architecture conflicts.

---

# 8. Governance Principles

The following principles govern the architecture documentation.

## Principle 1

Business drives architecture.

---

## Principle 2

Architecture drives implementation.

---

## Principle 3

Documentation is part of the architecture.

---

## Principle 4

Every architectural decision must be traceable.

---

## Principle 5

Every implementation decision must reference an approved architectural document.

---

## Principle 6

No implementation may redefine the Business Information Model.

---

## Principle 7

Repository Specifications implement architecture.

They do not redefine it.

---

# 9. Compliance

All future architecture reviews, AI agents, engineering documentation, Notion implementation, backend implementation and automation workflows shall comply with this document.

This document is the primary governance entry point for architecture documentation.

Business Information Model является единственным нормативным источником определения бизнес-атрибутов сущностей. Repository Specifications могут содержать только подмножество этих атрибутов и дополнительные технические поля, необходимые для хранения и реализации. Они не должны вводить новые бизнес-атрибуты без соответствующего изменения Business Information Model.