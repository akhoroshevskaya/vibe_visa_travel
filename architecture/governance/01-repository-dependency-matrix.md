# Repository Dependency Matrix

**Document ID:** GOV-001

**Status:** Final

**Stage:** Enterprise Architecture

---

# Purpose

Repository Dependency Matrix определяет допустимые зависимости между Repository
в рамках V!BE Business OS.

Документ используется для:

- проектирования Business OS;
- определения порядка реализации Repository;
- построения API;
- построения миграций данных;
- проверки архитектурной целостности;
- работы Architecture Validator.

Repository никогда не зависят друг от друга циклически.

Все зависимости являются направленными.

---

# Dependency Principles

## Single Source of Truth

Каждая Business Entity имеет единственный Repository-владелец.

Repository никогда не хранит данные другой Business Entity.

---

## Dependency Direction

Допустимы только зависимости сверху вниз.

Reference Layer

↓

Core Layer

↓

Execution Layer

↓

Presentation Layer

Обратные зависимости запрещены.

---

## Repository Ownership

Repository может ссылаться только на идентификатор другой Business Entity.

Никакое дублирование данных не допускается.

Например:

Request

✔ Client ID

✖ Client Name

✖ Client Phone

✖ Client Email

---

# Repository Dependency Matrix

| Repository | Depends On | Dependency Type | Required | Notes |
|-------------|------------|----------------|----------|-------|
| Services | — | — | — | Reference Repository |
| Workflows | Services | Reference | Yes | Workflow принадлежит Service |
| Clients | — | — | — | Root Business Entity |
| Requests | Clients | Composition | Yes | Request создается клиентом |
| Cases | Requests | Composition | Yes | Case создается из Request |
| Documents | Cases | Composition | Yes | Документы относятся к Case |
| Tasks | Cases | Composition | Yes | Работа выполняется по Case |
| Payments | Cases | Composition | Yes | Финансовые операции относятся к Case |

---

# Dependency Graph

Reference

Services

↓

Workflows

Core

Clients

↓

Requests

↓

Cases

Execution

Cases

├── Documents

├── Tasks

└── Payments

---

# Allowed Dependency Types

## Reference

Используется для справочников.

Repository может ссылаться на Reference Repository.

Reference Repository не зависит ни от кого.

Пример:

Workflow

↓

Service

---

## Composition

Жизненный цикл дочерней сущности определяется родительской.

Например:

Case

↓

Request

Если Request архивируется,

Case также становится архивным.

---

## Association

Допустимая слабая связь.

Repository хранит только внешний идентификатор.

Association не влияет на жизненный цикл сущности.

На текущем этапе MVP не используется.

---

# Forbidden Dependencies

Следующие зависимости запрещены.

Clients → Cases

Clients → Documents

Clients → Tasks

Clients → Payments

Requests → Documents

Requests → Payments

Payments → Documents

Documents → Payments

Tasks → Documents

Любая циклическая зависимость запрещена.

---

# Repository Creation Order

При реализации системы Repository должны создаваться
в следующем порядке.

1. Services

2. Workflows

3. Clients

4. Requests

5. Cases

6. Documents

7. Tasks

8. Payments

Данный порядок гарантирует отсутствие ссылок
на еще не существующие Repository.

---

# API Dependency Order

При проектировании API используются те же правила.

Сначала:

GET Clients

↓

GET Requests

↓

GET Cases

↓

GET Documents

↓

GET Tasks

↓

GET Payments

---

# Migration Order

При переносе данных Repository импортируются
в том же порядке.

Reference

↓

Core

↓

Execution

---

# Validation Rules

Architecture Validator должен проверять:

- отсутствие циклических зависимостей;
- отсутствие ссылок вверх по слоям;
- отсутствие ссылок между Execution Repository;
- наличие всех обязательных зависимостей;
- отсутствие ссылок на несуществующие Repository;
- корректность типов зависимостей.

---

# Architecture Impact

Используется документами:

- Repository Meta Model
- Repository Specifications
- Relationship Model
- Property Coverage Matrix
- Architecture Validator

---

# Status

✅ Approved

Repository Dependency Matrix считается базовым документом Governance.