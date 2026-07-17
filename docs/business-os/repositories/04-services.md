# REP-004 — Services Repository

**Статус:** Approved

**Repository ID:** REP-004

**Business Entity:** BE-004 Service

---

# 1. Purpose

Services Repository является единым каталогом услуг Business OS.

Repository определяет все услуги, предоставляемые компанией, и хранит их архитектурные характеристики.

Каждая услуга описывается один раз и может использоваться неограниченным количеством Case.

---

# 2. Repository Definition

Repository реализует Business Entity **Service**.

Каждая запись представляет одну услугу компании.

Repository является единым справочником услуг и используется всеми операционными процессами Business OS.

Repository не содержит сведения о выполнении услуги.

Для этого используется Cases Repository.

---

# 3. Repository Responsibilities

Repository отвечает за:

- хранение каталога услуг;
- определение типа услуги;
- хранение базового описания;
- определение стоимости;
- связь с Workflow;
- связь с шаблонами;
- связь с обязательными документами;
- предоставление данных другим Repository.

Repository не отвечает за:

- выполнение услуги;
- регистрацию клиентов;
- обработку обращений;
- хранение документов;
- управление задачами.

---

# 4. Supported Business Entity

| Entity | ID |
|----------|-----|
| Service | BE-004 |

---

# 5. Repository Properties

## Common

- PD-001 Created At
- PD-002 Updated At
- PD-003 Status

## Service Information

- Service Name
- Service Code
- Category
- Description
- Country
- Estimated Duration
- Default Price
- Currency

## Relations

- Workflow
- Required Documents
- Templates
- Cases

## Operational

- Is Active
- Requires Appointment
- Requires Biometrics
- Requires Insurance

---

# 6. Property Usage Matrix

Repository использует только утвержденные Property Definition.

Все свойства определяются централизованно через Data Architecture.

---

# 7. Repository Relations

| Repository | Relationship |
|-------------|--------------|
| Cases | referenced by |
| Workflows | references |
| Documents | references |
| Templates | references |

---

# 8. Lifecycle Integration

```text
Draft
    │
    ▼
Active
    │
    ▼
Suspended
    │
    ▼
Archived
```

Архивирование услуги не влияет на уже существующие Case.

---

# 9. Service Composition

Каждая услуга может определять:

- стандартный Workflow;
- список обязательных документов;
- рекомендуемые шаблоны;
- типовые сроки выполнения;
- базовую стоимость;
- обязательные проверки.

Эти параметры используются при создании нового Case.

---

# 10. Views

Минимальный набор представлений:

- All Services
- Active Services
- Visa Services
- Tourism Services
- Insurance Services
- Financial Services
- Services by Country

---

# 11. Templates

Repository использует:

- Visa Service
- Insurance Service
- Tour Service
- Financial Service
- Consultation

---

# 12. Automations

Repository участвует в следующих автоматизациях:

- выбор Workflow при создании Case;
- создание списка обязательных документов;
- заполнение стандартных параметров;
- расчет предварительных сроков;
- определение необходимости биометрии;
- определение необходимости записи.

---

# 13. Dashboards

Repository используется следующими Dashboard:

- Services Catalog
- Popular Services
- Revenue by Service
- Active Services
- Country Portfolio

---

# 14. AI Integration

AI может:

- рекомендовать наиболее подходящую услугу;
- определять категорию обращения;
- анализировать популярность услуг;
- выявлять устаревшие услуги;
- рекомендовать объединение похожих услуг.

AI не изменяет параметры услуги автоматически.

---

# 15. Permissions

| Role | Access |
|------|--------|
| Administrator | Full |
| Product Manager | Read / Write |
| Manager | Read |
| AI | Read |
| Automation | Read |

---

# 16. Architectural Constraints

Services Repository:

- реализует только Business Entity Service;
- является единственным источником информации об услугах;
- не хранит сведения о клиентах;
- не хранит сведения о выполнении услуг;
- не хранит документы;
- соответствует Repository Meta Model;
- соответствует Data Architecture;
- соответствует Relationship Model.

---

# 17. Business Rules

Repository реализует следующие правила:

- Каждая услуга имеет уникальный идентификатор.
- Каждая услуга может использоваться многими Case.
- Каждая услуга может иметь один или несколько Workflow.
- Архивирование услуги не влияет на существующие Case.
- Стоимость услуги может изменяться без изменения истории завершенных Case.

---

# 18. Future Evolution

Возможные направления развития:

- версии услуг;
- пакеты услуг;
- региональные варианты услуг;
- динамическое ценообразование;
- интеграция с прайс-листами;
- управление акциями;
- многовалютная поддержка;
- публикация каталога через API.