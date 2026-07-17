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

### Назначение

Определяет расширение существующего архитектурного объекта без изменения его базовой модели.

### Использование

```text
Repository

extends

Repository Meta Model
```

---

# Допустимые отношения

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

Все архитектурные документы Business OS используют исключительно типы отношений, определенные в данном документе.

Добавление нового типа отношения допускается только после проведения Architecture Review.

Relationship Model является обязательным для:

- Repository Specifications;
- Architecture Decision Records (ADR);
- Data Architecture;
- Architecture Diagrams;
- AI Analysis;
- Architecture Review.