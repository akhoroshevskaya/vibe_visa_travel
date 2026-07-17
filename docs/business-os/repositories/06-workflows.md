# REP-006 — Workflows Repository

**Статус:** Approved

**Repository ID:** REP-006

**Business Entity:** BE-006 Workflow

---

# 1. Purpose

Workflows Repository является единым каталогом бизнес-процессов Business OS.

Repository хранит шаблоны выполнения услуг, определяющие последовательность этапов, правила переходов и требования к обработке.

Workflow описывает процесс, но не содержит данных конкретного клиента или обращения.

---

# 2. Repository Definition

Repository реализует Business Entity **Workflow**.

Каждая запись представляет один шаблон бизнес-процесса.

Workflow может использоваться многими Service и тысячами Case.

Repository является единственным источником информации о бизнес-процессах.

---

# 3. Repository Responsibilities

Repository отвечает за:

- описание этапов процесса;
- определение последовательности этапов;
- определение правил переходов;
- хранение контрольных точек;
- связь с Service;
- предоставление процесса для исполнения Case.

Repository не отвечает за:

- выполнение процесса;
- хранение статусов Case;
- хранение задач;
- хранение документов;
- регистрацию клиентов.

---

# 4. Supported Business Entity

| Entity | ID |
|----------|-----|
| Workflow | BE-006 |

---

# 5. Repository Properties

## Common

- PD-001 Created At
- PD-002 Updated At
- PD-003 Status

## Workflow Information

- Workflow Name
- Workflow Code
- Description
- Version
- Category

## Structure

- Steps
- Transitions
- Entry Conditions
- Exit Conditions

## Relations

- Services
- Templates
- Automations

## Operational

- Is Default
- Is Active

---

# 6. Property Usage Matrix

Repository использует только утвержденные Property Definition.

Все свойства описываются централизованно в Data Architecture.

---

# 7. Repository Relations

| Repository | Relationship |
|-------------|--------------|
| Services | referenced by |
| Cases | used by |
| Tasks | generates |
| Automations | references |

---

# 8. Lifecycle Integration

```text
Draft
    │
    ▼
Testing
    │
    ▼
Active
    │
    ▼
Deprecated
    │
    ▼
Archived
```

Новые версии Workflow создаются как отдельные записи.

Активные Case продолжают использовать версию Workflow, с которой были созданы.

---

# 9. Workflow Structure

Каждый Workflow состоит из последовательности Step.

Каждый Step содержит:

- название;
- описание;
- обязательность выполнения;
- условия начала;
- условия завершения;
- возможные переходы;
- рекомендуемый SLA.

Workflow может содержать линейные и условные переходы.

---

# 10. Views

Минимальный набор представлений:

- All Workflows
- Active Workflows
- Draft Workflows
- Workflows by Service
- Workflow Versions

---

# 11. Templates

Repository использует шаблоны:

- Standard Visa Workflow
- Express Visa Workflow
- Insurance Workflow
- Consultation Workflow
- Tour Workflow

---

# 12. Automations

Repository участвует в следующих автоматизациях:

- выбор Workflow при создании Case;
- создание первоначального набора Task;
- автоматический переход между этапами;
- проверка обязательных условий;
- контроль SLA по этапам.

---

# 13. Dashboards

Repository используется следующими Dashboard:

- Workflow Usage
- Workflow Performance
- Average Duration by Workflow
- Workflow Versions

---

# 14. AI Integration

AI может:

- анализировать эффективность Workflow;
- выявлять узкие места процесса;
- рекомендовать оптимизацию этапов;
- предлагать объединение шагов;
- прогнозировать длительность процесса.

AI не изменяет Workflow автоматически.

---

# 15. Permissions

| Role | Access |
|------|--------|
| Administrator | Full |
| Process Owner | Read / Write |
| Manager | Read |
| AI | Read |
| Automation | Read |

---

# 16. Architectural Constraints

Workflows Repository:

- реализует только Business Entity Workflow;
- хранит описание процесса, а не его исполнение;
- поддерживает версионирование;
- не изменяет существующие Case;
- является единственным источником бизнес-процессов;
- соответствует Repository Meta Model;
- соответствует Data Architecture;
- соответствует Relationship Model.

---

# 17. Business Rules

Repository реализует следующие правила:

- Каждый Workflow имеет уникальную версию.
- Workflow может использоваться несколькими Service.
- Один Service может иметь несколько Workflow.
- Новый Case создается только на основе активной версии Workflow.
- После создания Case версия Workflow не изменяется.
- Изменение Workflow не влияет на уже выполняемые Case.

---

# 18. Future Evolution

Возможные направления развития:

- графическое моделирование BPMN;
- поддержка параллельных ветвей;
- условные переходы;
- пользовательские Workflow;
- импорт и экспорт BPMN;
- библиотека типовых процессов;
- симуляция выполнения Workflow;
- анализ эффективности процессов с помощью AI.