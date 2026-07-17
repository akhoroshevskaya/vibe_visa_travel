# Business Entity Integrity Review

**Document ID:** GOV-003

**Status:** Final

**Stage:** Enterprise Architecture

---

# Purpose

Business Entity Integrity Review определяет обязательные
правила целостности Business Entity.

Документ описывает архитектурные инварианты,
которые должны соблюдаться независимо
от реализации системы.

Integrity Rules используются:

- Repository;
- API;
- Validator;
- Automation;
- AI;
- Migration.

---

# Integrity Principles

## Business Entity Identity

Каждая Business Entity имеет:

- собственный идентификатор;
- собственного владельца;
- собственный Repository.

Совместное владение запрещено.

---

## Single Source of Truth

Business Entity существует
только в одном Repository.

Дублирование запрещено.

---

## Referential Integrity

Все ссылки должны указывать
на существующие Business Entity.

Недействительные ссылки запрещены.

---

## Ownership Integrity

Каждая Business Entity
имеет единственного Owner.

Owner определяется
Property Definition.

---

## Lifecycle Integrity

Жизненный цикл дочерней сущности
не может противоречить
жизненному циклу родительской.

---

# Entity Integrity Rules

## REP-001 Clients

### Required

- Client ID
- Status
- Created At

### Rules

- Client может существовать без Request.
- Client является корневой сущностью.
- Client нельзя удалить при наличии связанных Request.
- Архивация предпочтительнее удаления.

---

## REP-002 Requests

### Required

- Request ID
- Client ID
- Status

### Rules

- Request всегда принадлежит Client.
- Request обязан ссылаться на существующий Client.
- Один Client может иметь много Request.
- Удаление Client не должно нарушать целостность Request.

---

## REP-003 Cases

### Required

- Case ID
- Request ID
- Status

### Rules

- Case создается только из Request.
- Case без Request существовать не может.
- Один Request может содержать несколько Case.
- Все Case принадлежат одному Request.

---

## REP-004 Services

### Rules

- Service является справочной сущностью.
- Service не зависит от других Repository.
- Service может использоваться многими Workflow.

---

## REP-005 Documents

### Rules

- Document всегда принадлежит Case.
- Document не может существовать самостоятельно.
- Document Requirement не равен Document Instance.

---

## REP-006 Workflows

### Rules

- Workflow принадлежит одному Service.
- Workflow является шаблоном процесса.
- Workflow не содержит данных клиента.

---

## REP-007 Tasks

### Rules

- Task принадлежит одному Case.
- Task может иметь исполнителя.
- Закрытая Task не должна автоматически удаляться.

---

## REP-008 Payments

### Rules

- Payment относится к одному Case.
- Payment не зависит от Document.
- Payment хранит факт финансовой операции.

---

# Cross Repository Integrity

Дополнительно проверяются:

- отсутствие циклических ссылок;
- отсутствие осиротевших записей;
- отсутствие ссылок на архивные сущности (если запрещено);
- корректность Owner;
- корректность Status;
- обязательные Property;
- обязательные Relationship.

---

# Forbidden States

Недопустимы:

- Case без Request;
- Request без Client;
- Task без Case;
- Payment без Case;
- Workflow без Service;
- Document без Case.

---

# Validation Rules

Architecture Validator проверяет:

- Required Property;
- Required Relationship;
- Ownership;
- Lifecycle;
- Referential Integrity;
- Cardinality;
- Forbidden States.

---

# Architecture Impact

Используется:

- Repository Specifications;
- Relationship Model;
- Repository Dependency Matrix;
- Property Coverage Matrix;
- Architecture Validator;
- Metadata Schema.

---

# Status

✅ Approved

Business Entity Integrity Review является
основным документом проверки
архитектурной целостности Business Entity.