# Repository Relationship Types

**Document ID:** GOV-005

**Document Type:** Normative

**Status:** Final

**Stage:** Enterprise Architecture

---

# Purpose

Repository Relationship Types определяет допустимые типы
связей между Business Entity и Repository
в рамках V!BE Business OS.

Документ является единым стандартом
для моделирования отношений,
используется Repository Specifications,
Architecture Validator,
Metadata Schema
и Runtime.

---

# Relationship Principles

## Single Source of Truth

Relationship никогда не переносит данные.

Relationship хранит только ссылку
на Business Entity.

---

## Explicit Relationship

Любая связь должна быть явно определена.

Неявные связи запрещены.

---

## Typed Relationship

Каждая связь должна иметь тип.

Неопределённые отношения запрещены.

---

## Ownership Preservation

Relationship никогда
не меняет владельца Business Entity.

Ownership определяется Repository.

---

# Relationship Types

## REL-001 Composition

Описание

Дочерняя сущность
не может существовать
без родительской.

Lifecycle полностью зависит
от родителя.

Примеры

Request → Case

Case → Task

Case → Document

Case → Payment

Validator

✓ Parent Required

✓ Cascade Archive

✓ Orphan Forbidden

---

## REL-002 Reference

Описание

Ссылка
на справочную Business Entity.

Жизненный цикл независим.

Примеры

Workflow → Service

Validator

✓ Reference Exists

✓ Reference Active

---

## REL-003 Association

Описание

Свободная связь.

Используется
для будущих расширений.

Не влияет
на жизненный цикл.

Примеры

Task → Employee

Case → External System

Validator

✓ ID Exists

---

## REL-004 Dependency

Описание

Используется
для архитектурных зависимостей.

Не является бизнес-связью.

Пример

Repository → Repository

Architecture → Metadata

Используется Validator.

---

# Cardinality Rules

Поддерживаются следующие типы:

One-to-One (1:1)

One-to-Many (1:N)

Many-to-One (N:1)

Many-to-Many (M:N)

Для MVP допускаются только:

1:1

1:N

N:1

M:N допускается
только через промежуточную Business Entity.

---

# Allowed Relationships

| From | To | Type | Cardinality |
|-------|----|------|-------------|
| Client | Request | Composition | 1:N |
| Request | Case | Composition | 1:N |
| Case | Document | Composition | 1:N |
| Case | Task | Composition | 1:N |
| Case | Payment | Composition | 1:N |
| Workflow | Service | Reference | N:1 |

---

# Forbidden Relationships

Запрещены:

Client → Payment

Client → Document

Client → Task

Request → Payment

Request → Document

Task → Payment

Document → Payment

Workflow → Client

Service → Client

Любые циклические отношения.

---

# Relationship Lifecycle

Relationship может находиться
в следующих состояниях:

Active

Archived

Removed

Удаление Relationship
не должно нарушать
Business Entity Integrity.

---

# Validation Rules

Architecture Validator проверяет:

- корректность типа Relationship;
- допустимость Cardinality;
- наличие целевой Business Entity;
- отсутствие циклов;
- соблюдение Ownership;
- соблюдение Lifecycle;
- отсутствие запрещённых связей.

---

# Architecture Impact

Документ используется:

- Repository Specifications
- Relationship Model
- Business Entity Integrity Review
- Architecture Manifest
- Metadata Schema
- Validator Design

---

# Status

✅ Approved

Repository Relationship Types является
единым корпоративным стандартом
описания отношений
между Repository.