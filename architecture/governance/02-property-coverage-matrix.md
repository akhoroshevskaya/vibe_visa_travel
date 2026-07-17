# Property Coverage Matrix

**Document ID:** GOV-002

**Status:** Final

**Stage:** Enterprise Architecture

---

# Purpose

Property Coverage Matrix определяет использование Property Definition
во всех Repository V!BE Business OS.

Документ является частью корпоративной модели данных и обеспечивает единое
использование Property Definition во всей системе.

Property Definition являются единственным источником определения свойств.

Repository только используют Property Definition.

---

# Coverage Principles

## Single Source of Truth

Каждое свойство определяется только один раз
в Property Definition.

Repository запрещено создавать собственные свойства,
не зарегистрированные в Property Definition.

---

## Centralized Property Management

Все Repository используют общий корпоративный словарь данных.

Любое изменение Property Definition автоматически влияет
на все Repository, использующие данное свойство.

---

## Explicit Usage

Каждое использование Property должно быть зарегистрировано
в данной матрице.

Неиспользуемые Property не допускаются.

---

# Matrix A — Property Usage

| Property Definition | Clients | Requests | Cases | Documents | Tasks | Payments | Services | Workflows |
|---------------------|:-------:|:--------:|:-----:|:---------:|:-----:|:--------:|:--------:|:---------:|
| PD-001 Created At | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| PD-002 Updated At | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| PD-003 Status | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| PD-004 Owner | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | — | — |
| PD-005 Notes | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | — | — |
| PD-006 Source | — | ✓ | ✓ | — | — | — | — | — |
| PD-007 Priority | — | — | ✓ | — | ✓ | — | — | ✓ |
| PD-008 Due Date | — | — | ✓ | ✓ | ✓ | ✓ | — | — |
| PD-009 Assigned To | — | — | ✓ | ✓ | ✓ | — | — | — |
| PD-010 External ID | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| PD-011 Tags | ✓ | ✓ | ✓ | ✓ | ✓ | — | ✓ | ✓ |
| PD-012 Archived | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| PD-013 Processing Time | — | ✓ | ✓ | ✓ | ✓ | ✓ | — | ✓ |

---

# Matrix B — Property Policies

| Property | Required | Immutable | System Managed | Indexed | Searchable | Nullable |
|-----------|----------|-----------|----------------|----------|------------|-----------|
| Created At | Yes | Yes | Yes | Yes | No | No |
| Updated At | Yes | No | Yes | Yes | No | No |
| Status | Yes | No | Workflow | Yes | Yes | No |
| Owner | Depends | No | No | Yes | Yes | Yes |
| Notes | No | No | No | No | Yes | Yes |
| Source | Depends | Yes | Yes | Yes | Yes | Yes |
| Priority | No | No | Workflow | Yes | Yes | Yes |
| Due Date | No | No | No | Yes | Yes | Yes |
| Assigned To | Depends | No | No | Yes | Yes | Yes |
| External ID | Depends | Yes | Yes | Yes | Yes | Yes |
| Tags | No | No | No | No | Yes | Yes |
| Archived | Yes | No | Yes | Yes | No | No |
| Processing Time | Yes | Yes | Calculated | Yes | Yes | No |

---

# Repository Coverage Summary

## REP-001 Clients

- Created At
- Updated At
- Status
- Owner
- Notes
- External ID
- Tags
- Archived

---

## REP-002 Requests

- Created At
- Updated At
- Status
- Owner
- Notes
- Source
- External ID
- Tags
- Archived
- Processing Time

---

## REP-003 Cases

- Created At
- Updated At
- Status
- Owner
- Notes
- Source
- Priority
- Due Date
- Assigned To
- External ID
- Tags
- Archived
- Processing Time

---

## REP-004 Services

- Created At
- Updated At
- Status
- External ID
- Tags
- Archived

---

## REP-005 Documents

- Created At
- Updated At
- Status
- Owner
- Notes
- Due Date
- Assigned To
- External ID
- Tags
- Archived
- Processing Time

---

## REP-006 Workflows

- Created At
- Updated At
- Status
- Priority
- External ID
- Tags
- Archived
- Processing Time

---

## REP-007 Tasks

- Created At
- Updated At
- Status
- Owner
- Notes
- Priority
- Due Date
- Assigned To
- External ID
- Tags
- Archived
- Processing Time

---

## REP-008 Payments

- Created At
- Updated At
- Status
- Owner
- Notes
- Due Date
- External ID
- Archived
- Processing Time

---

# Validation Rules

Architecture Validator должен проверять:

- каждая Property Definition существует;
- каждая Property используется минимум одним Repository;
- Repository использует только зарегистрированные Property;
- отсутствуют дублирующие Property;
- обязательные Property присутствуют;
- политика использования Property соблюдается;
- System Managed Property недоступны для ручного изменения;
- отсутствуют неизвестные Property Definition.

---

# Architecture Impact

Документ используется:

- Property Definition
- Repository Specifications
- Repository Dependency Matrix
- Relationship Model
- Business Entity Integrity Review
- Architecture Validator
- Metadata Schema
- Architecture Manifest

---

# Status

✅ Approved

Property Coverage Matrix является нормативным документом,
определяющим использование Property Definition
во всех Repository V!BE Business OS.
