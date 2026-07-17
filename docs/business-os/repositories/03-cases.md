# REP-003 — Cases Repository

**Статус:** Approved

**Repository ID:** REP-003

**Business Entity:** BE-003 Case

---

# 1. Purpose

Cases Repository является центральным операционным репозиторием исполнения услуг Business OS.

Repository хранит сведения о каждом экземпляре оказываемой услуги, включая полный жизненный цикл обработки, связанные документы, статусы, сроки и результаты.

Case представляет собой конкретную работу компании по выполнению одной услуги для одного клиента.

---

# 2. Repository Definition

Repository реализует Business Entity **Case**.

Каждая запись представляет одно выполнение услуги.

Case всегда относится к одному Client, одной Service и одному Request.

Один Request может содержать несколько Case.

Примеры:

- Семья из четырёх человек → один Request → четыре Case.
- Один клиент оформляет визу и страховку → один Request → два Case.
- Повторное обращение через полгода → новый Request → новые Case.

Repository является единственным источником операционных данных по выполнению услуги.

---

# 3. Repository Responsibilities

Repository отвечает за:

- выполнение услуги;
- хранение этапов обработки;
- контроль текущего статуса;
- хранение рабочих комментариев;
- контроль сроков;
- связь с документами;
- связь с платежами;
- хранение результата оказания услуги.

Repository не отвечает за:

- регистрацию обращения;
- хранение данных клиента;
- описание услуги;
- финансовый учет;
- управление пользователями.

---

# 4. Supported Business Entity

| Entity | ID |
|----------|-----|
| Case | BE-003 |

---

# 5. Repository Properties

## Common

- PD-001 Created At
- PD-002 Updated At
- PD-003 Status
- PD-004 Owner

## Relations

- Request
- Client
- Service
- Workflow
- Documents
- Payments
- Tasks

## Processing

- Current Stage
- Stage Started At
- Stage Deadline
- Case Result
- Completion Date

## Operational

- Manager Comment
- Internal Notes
- Next Action
- Priority

## Computed

- Processing Time
- Days Remaining
- SLA Status

---

# 6. Property Usage Matrix

Все Property определяются через утвержденный каталог Property Definition.

Repository использует только глобальные Property Definition.

Для каждого Property определяется Lifecycle Usage в соответствии с Data Architecture.

---

# 7. Repository Relations

| Repository | Relationship |
|-------------|--------------|
| Requests | references |
| Clients | references |
| Services | references |
| Documents | references |
| Payments | references |
| Tasks | references |
| Workflows | references |

---

# 8. Lifecycle Integration

Case проходит полный жизненный цикл исполнения услуги.

```text
Created
      │
      ▼
Preparation
      │
      ▼
In Progress
      │
      ▼
Waiting
      │
      ▼
Submitted
      │
      ▼
Decision
      │
      ▼
Completed
```

Переход между этапами фиксируется системой.

История изменений сохраняется для аудита и аналитики.

---

# 9. Stage Management

Каждый Case состоит из последовательности этапов.

Для каждого этапа фиксируются:

- дата начала;
- дата завершения;
- ответственный менеджер;
- комментарий;
- результат этапа;
- связанные документы.

История этапов является неизменяемой.

Изменяется только текущее состояние Case.

---

# 10. Views

Минимальный набор представлений:

- All Cases
- My Cases
- Active Cases
- Waiting Cases
- Completed Cases
- Urgent Cases
- Cases by Country
- Cases by Service
- Cases by Manager
- Cases Near Deadline

---

# 11. Templates

Repository использует следующие шаблоны:

- Schengen Visa
- National Visa
- UK Visa
- USA Visa
- Insurance
- Invitation
- Bank Card
- APEC Card

---

# 12. Automations

Repository участвует в следующих автоматизациях:

- создание Case из Request;
- создание Task при переходе этапа;
- контроль дедлайнов;
- уведомление менеджера;
- уведомление клиента;
- автоматическое изменение статуса;
- создание событий календаря;
- расчет SLA.

---

# 13. Dashboards

Repository используется следующими Dashboard:

- Active Cases
- Processing Time
- SLA Dashboard
- Manager Load
- Country Statistics
- Service Statistics
- Overdue Cases

---

# 14. AI Integration

AI может:

- анализировать полноту кейса;
- выявлять отсутствующие документы;
- предлагать следующий этап;
- прогнозировать сроки;
- оценивать вероятность задержки;
- формировать краткое резюме Case;
- готовить рекомендации менеджеру.

AI не изменяет статус Case самостоятельно.

---

# 15. Permissions

| Role | Access |
|------|--------|
| Administrator | Full |
| Manager | Read / Write |
| Sales | Read |
| AI | Read |
| Automation | Limited Write |

---

# 16. Architectural Constraints

Cases Repository:

- реализует только Business Entity Case;
- является единственным источником данных об исполнении услуги;
- не содержит персональных данных клиента, кроме ссылок;
- не хранит описание услуги;
- не дублирует документы;
- не содержит бизнес-логики автоматизаций;
- соответствует Repository Meta Model;
- соответствует Data Architecture;
- соответствует Relationship Model.

---

# 17. Business Rules

Repository реализует следующие правила:

- Каждый Case принадлежит одному Request.
- Каждый Case принадлежит одному Client.
- Каждый Case относится к одной Service.
- Один Request может содержать несколько Case.
- Case может быть завершен только после завершения всех обязательных этапов.
- История этапов не редактируется.
- Ответственный менеджер может изменяться, но история изменений сохраняется.

---

# 18. Future Evolution

Возможные направления развития:

- поддержка параллельных Workflow;
- SLA по каждому этапу;
- шаблоны этапов по странам;
- автоматическая оценка риска;
- интеграция с внешними визовыми системами;
- поддержка нескольких исполнителей;
- событийная модель (Case Events);
- версия Workflow для каждого Case.