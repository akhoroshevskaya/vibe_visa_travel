# 3. Relationship Model

**Статус:** Approved

---

# Назначение

Relationship Model определяет стандартные типы архитектурных связей между объектами Business OS.

Документ устанавливает единый язык описания взаимодействия архитектурных компонентов и обеспечивает единообразие архитектурной документации.

Relationship Model применяется ко всем архитектурным объектам Business OS независимо от технологической реализации.

---

# Цели

Relationship Model обеспечивает:

- единообразное описание архитектурных связей;
- понятную навигацию по архитектуре;
- единый язык для ADR;
- возможность автоматического анализа архитектуры;
- независимость архитектурных связей от платформы.

---

# Основные правила

Все архитектурные объекты взаимодействуют только посредством утвержденных типов отношений.

Каждое отношение имеет:

- направление;
- семантическое значение;
- ограничения использования.

Отношения являются частью архитектуры и должны использоваться последовательно во всей документации.

---

# Relationship Types

## implements

### Назначение

Определяет реализацию архитектурной модели.

### Использование

```text
Repository
        │
implements
        ▼
Business Entity
```

### Пример

```text
REP-001 Requests

implements

BE-001 Request
```

---

## uses

### Назначение

Определяет использование другого архитектурного объекта без владения им.

### Использование

```text
Repository
        │
uses
        ▼
Property Definition
```

### Пример

```text
REP-001 Requests

uses

PD-005 Request Source
```

---

## references

### Назначение

Определяет связь между двумя независимыми Repository.

Связь осуществляется посредством идентификаторов Business Entity.

### Использование

```text
Repository

references

Repository
```

### Пример

```text
REP-001 Requests

references

REP-002 Clients
```

---

## creates

### Назначение

Определяет создание нового экземпляра объекта.

### Использование

```text
Automation

creates

Repository Record
```

### Пример

```text
AUT-001

creates

Request
```

---

## updates

### Назначение

Определяет изменение существующего объекта.

### Использование

```text
Automation

updates

Repository Record
```

---

## reads

### Назначение

Определяет использование данных только для чтения.

### Использование

```text
Dashboard

reads

Repository
```

или

```text
AI

reads

Repository
```

---

## displays

### Назначение

Определяет отображение данных пользователю.

### Использование

```text
View

displays

Repository
```

---

## secures

### Назначение

Определяет применение модели безопасности.

### Использование

```text
Permission

secures

Repository
```

---

## triggers

### Назначение

Определяет запуск одного архитектурного объекта другим.

### Использование

```text
Automation

triggers

Automation
```

или

```text
Repository Event

triggers

Automation
```

---

## extends
# Business Relationship Types

Business Relationship Types описывают отношения между бизнес-сущностями предметной области.

В отличие от Architecture Relationships, они используются для моделирования бизнес-структуры, жизненного цикла объектов и зависимостей между Business Entity.

---

## belongs to

### Назначение

Определяет отношение принадлежности одной Business Entity другой.

### Использование

```text
Case

belongs to

Client
```

---

## contains

### Назначение

Определяет композицию, при которой одна Business Entity содержит другую.

### Использование

```text
Case

contains

Case Service
```

---

## created from

### Назначение

Определяет происхождение Business Entity на основе другой Business Entity.

### Использование

```text
Case

created from

Product
```

или

```text
Case Service

created from

Product
```

---

## derived from

### Назначение

Определяет наследование или вычисление одной Business Entity на основе другой.

### Использование

```text
Commercial Offer

derived from

Product
```

---

## assigned to

### Назначение

Определяет назначение ответственности за Business Entity.

### Использование

```text
Case

assigned to

Manager
```

---

## executed by

### Назначение

Определяет исполнителя Business Entity или бизнес-процесса.

### Использование

```text
Task

executed by

Manager
```

или

```text
Workflow Stage

executed by

Partner
```

---

## part of

### Назначение

Определяет отношение, при котором Business Entity является частью другой Business Entity.

### Использование

```text
Workflow Stage

part of

Workflow
```

### Назначение

Определяет расширение существующего архитектурного объекта без изменения его базовой модели.

### Использование

```text
Repository

extends

Repository Meta Model
```

---

# Supported Relationship Types

## Architecture Relationships

| Source | Relationship | Target |
|---------|--------------|--------|
| Repository | implements | Business Entity |
| Repository | uses | Property Definition |
| Repository | references | Repository |
| View | displays | Repository |
| Dashboard | reads | Repository |
| AI Component | reads | Repository |
| Automation | creates | Repository Record |
| Automation | updates | Repository Record |
| Automation | triggers | Automation |
| Permission | secures | Repository |
| Repository | extends | Repository Meta Model |

---

## Business Relationships

| Source | Relationship | Target |
|---------|--------------|--------|
| Business Entity | belongs to | Business Entity |
| Business Entity | contains | Business Entity |
| Business Entity | created from | Business Entity |
| Business Entity | derived from | Business Entity |
| Business Entity | assigned to | Business Entity |
| Business Entity | executed by | Business Entity |
| Business Entity | part of | Business Entity |
---

# Relationship Diagram

```text
Business Entity
        ▲
        │ implements
        │
Repository
   │   │   │
   │   │   └──────────── references ────────► Repository
   │
   ├──────────── uses ─────────────────────► Property Definition
   │
   ├──────────── displayed by ─────────────► View
   │
   ├──────────── read by ──────────────────► Dashboard
   │
   ├──────────── read by ──────────────────► AI Component
   │
   └──────────── secured by ───────────────► Permission

Repository Events
        │
        ▼
Automation
        │
        ├──────── creates ─────────► Repository Record
        ├──────── updates ─────────► Repository Record
        └──────── triggers ────────► Automation
```

---

# Architectural Constraints

Relationship Model устанавливает следующие ограничения:

- Repository не взаимодействуют напрямую с пользовательскими интерфейсами.
- Repository не содержат автоматизации.
- Dashboard не изменяют данные.
- View не являются источником данных.
- AI Component не владеет данными.
- Permission не изменяет бизнес-логику.
- Automation не изменяет архитектуру Repository.

---

# Relationship Usage

Все архитектурные документы Business OS используют только типы отношений, определенные в настоящем документе.

Relationship Model состоит из двух уровней:

- Architecture Relationships — используются для описания взаимодействия архитектурных компонентов (Repository, Automation, AI, Dashboard, View, Permission и других).
- Business Relationships — используются для описания связей между Business Entity предметной области.

Использование отношений вне данного стандарта допускается только после проведения Architecture Review.

Relationship Model является обязательным для:

- Repository Specifications;
- Business Domain Model;
- Business Information Model;
- Architecture Decision Records (ADR);
- Data Architecture;
- Architecture Diagrams;
- AI Analysis;
- Architecture Review.