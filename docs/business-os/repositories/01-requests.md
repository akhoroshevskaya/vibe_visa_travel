# REP-001 — Requests Repository

**Статус:** Approved

**Repository ID:** REP-001

**Business Entity:** BE-001 Request

---

# 1. Purpose

Requests Repository является центральным операционным репозиторием Business OS.

Repository хранит все обращения клиентов независимо от источника поступления, типа услуги и текущего этапа обработки.

Каждый запрос клиента существует в системе только один раз и проходит полный жизненный цикл от первого обращения до завершения работы.

Requests Repository является единственной точкой регистрации клиентского намерения воспользоваться услугами компании.

---

# 2. Repository Definition

Repository реализует Business Entity **Request**.

Каждая запись Repository представляет одно обращение клиента.

Repository не описывает клиента, услугу или процесс оформления визы.

Repository управляет только самим запросом и его состоянием.

Repository является единственным источником данных о Request.

---

# 3. Repository Responsibilities

Repository отвечает за:

- регистрацию нового обращения;
- хранение операционных данных Request;
- отслеживание статуса обработки;
- назначение ответственного менеджера;
- хранение связей с другими Repository;
- фиксацию источника обращения;
- предоставление данных другим компонентам Business OS.

Repository не отвечает за:

- хранение данных клиента;
- хранение документов;
- финансовый учет;
- календарное планирование;
- выполнение автоматизаций;
- отображение информации пользователям.

---

# 4. Supported Business Entity

Repository реализует:

| Entity | ID |
|----------|-----|
| Request | BE-001 |

---

# 5. Repository Properties

Requests Repository использует только утвержденные Property Definition.

## Common

| Property | ID |
|-----------|----|
| Created At | PD-001 |
| Updated At | PD-002 |
| Status | PD-003 |
| Owner | PD-004 |

## Request

| Property | ID |
|-----------|----|
| Request Source | PD-005 |
| Request Channel | PD-006 |
| Client Message | PD-007 |
| Priority | PD-008 |
| AI Summary | PD-009 |

## Relations

| Property | ID |
|-----------|----|
| Client | PD-010 |
| Service | PD-011 |
| Case | PD-012 |

## Computed

| Property | ID |
|-----------|----|
| Processing Time | PD-013 |

---

# 6. Property Usage Matrix

| Property | Required | Editable | Lifecycle |
|-----------|----------|----------|-----------|
| Created At | Yes | No | Create |
| Updated At | Yes | No | System |
| Status | Yes | Yes | Entire Lifecycle |
| Owner | Yes | Yes | Entire Lifecycle |
| Request Source | Yes | No | Create |
| Request Channel | Yes | No | Create |
| Client Message | No | Yes | Initial Review |
| Priority | No | Yes | Entire Lifecycle |
| AI Summary | No | System | Review |
| Client | Yes | Yes | Entire Lifecycle |
| Service | Yes | Yes | Entire Lifecycle |
| Case | No | System | Processing |
| Processing Time | No | System | Computed |

---

# 7. Repository Relations

Requests Repository взаимодействует со следующими Repository.

| Repository | Relationship |
|-------------|--------------|
| Clients | references |
| Services | references |
| Cases | references |
| Workflows | references |
| Documents | references |
| Payments | references |
| Tasks | references |

---

# 8. Lifecycle Integration

Request проходит полный жизненный цикл Business Entity.

```text
New
    │
    ▼
Review
    │
    ▼
In Progress
    │
    ▼
Waiting
    │
    ▼
Completed
```

Repository хранит только текущее состояние.

История переходов хранится отдельно и используется для аналитики и аудита.

---

# 9. Views

Минимальный набор представлений Repository.

## All Requests

Все обращения.

---

## New Requests

Новые обращения.

---

## My Requests

Обращения текущего менеджера.

---

## Waiting

Ожидающие действия.

---

## Completed

Завершенные обращения.

---

## High Priority

Высокий приоритет.

---

## By Service

Группировка по услугам.

---

## By Country

Группировка по стране назначения.

---

# 10. Templates

Repository использует следующие шаблоны.

- Standard Request
- Family Request
- Corporate Request
- Consultation
- Visa Support
- Full Travel Package

---

# 11. Automations

Requests Repository участвует в следующих автоматизациях.

## AUT-001

Создание Request из формы сайта.

---

## AUT-002

Создание Request из Telegram.

---

## AUT-003

Назначение Owner.

---

## AUT-004

Создание Client при отсутствии.

---

## AUT-005

Создание связанных Case.

---

## AUT-006

Изменение статуса.

---

## AUT-007

Расчет Processing Time.

---

## AUT-008

Уведомления менеджеров.

---

# 12. Dashboards

Repository используется следующими Dashboard.

- Requests Overview
- Manager Workload
- SLA Control
- Conversion Funnel
- Sources Analytics
- Services Analytics

---

# 13. AI Integration

Repository поддерживает AI-функции.

AI может:

- формировать краткое резюме обращения;
- определять предполагаемый тип услуги;
- извлекать ключевые данные из сообщения клиента;
- предлагать приоритет;
- выявлять отсутствующую информацию;
- подготавливать рекомендации менеджеру.

AI не изменяет Repository самостоятельно.

Все изменения подтверждаются человеком.

---

# 14. Permissions

Минимальная модель доступа.

| Role | Access |
|------|--------|
| Administrator | Full |
| Manager | Read / Write |
| Sales | Read / Create |
| AI | Read |
| Automation | Limited Write |

---

# 15. Architectural Constraints

Requests Repository должен соответствовать следующим ограничениям.

- Реализует только одну Business Entity.
- Не содержит данных других Repository.
- Не содержит пользовательских интерфейсов.
- Не содержит бизнес-логики автоматизаций.
- Использует только утвержденные Property Definition.
- Является единственным источником данных Request.
- Соответствует Repository Meta Model.
- Соответствует Data Architecture.
- Соответствует Relationship Model.

---

# 16. Future Evolution

В дальнейшем Repository может быть расширен без нарушения архитектуры.

Потенциальные направления развития:

- поддержка нескольких владельцев с сохранением одного ответственного;
- SLA и контроль сроков обработки;
- AI-оценка сложности обращения;
- автоматическое определение риска;
- автоматическое распределение запросов;
- поддержка внешних API;
- событийная модель (Domain Events).

Все расширения должны сохранять совместимость с утвержденной Repository Meta Model.