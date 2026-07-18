> **Architecture Note**
>
> Документ является частью методологии **V!BE Business OS**.
>
> Его цель — определить жизненные циклы ключевых бизнес-сущностей компании.
>
> Business State Machine описывает допустимые состояния сущности, правила переходов между ними и бизнес-события, инициирующие изменение состояния.
>
> Документ определяет единственный Source of Truth для жизненных циклов бизнес-сущностей и является основой для:
>
> - Business OS (Notion);
> - Backend;
> - API;
> - Automation;
> - AI Agents.

# V!BE Business OS

# Business State Machines v1.1

**Version:** 1.1

**Status:** Approved

**Owner:** Aleksandra

**Project:** V!BE Visa & Travel

---

# 1. Purpose

Business State Machines определяет жизненные циклы основных бизнес-сущностей компании.

Документ описывает:

- допустимые состояния сущностей;
- разрешенные переходы между состояниями;
- бизнес-события, инициирующие переходы;
- бизнес-правила управления состояниями;
- точки запуска автоматизаций.

Business State Machines описывает поведение бизнеса независимо от используемых технологий и является нормативным документом для реализации статусов в Business OS, Backend и Automation.

---

# 2. Общие принципы

## State Represents Business

Состояние отражает исключительно бизнес-смысл сущности.

Техническая реализация, пользовательский интерфейс и внутренние процессы системы не влияют на модель состояний.

---

## Only Valid Transitions

Переход между состояниями допускается только по заранее определенным правилам.

Любые переходы, отсутствующие в настоящем документе, считаются недопустимыми.

---

## Events Trigger State Changes

Каждый переход между состояниями инициируется бизнес-событием.

Например:

- получено новое обращение;
- проведена квалификация;
- клиент принял предложение;
- поступила оплата;
- менеджер завершил этап работы;
- получен паспорт.

---

## State History Is Immutable

История переходов между состояниями никогда не удаляется и не изменяется.

История используется для:

- аудита;
- аналитики;
- автоматизаций;
- работы AI Agents.

---

## Automation Reacts To State

Автоматизации реагируют на изменение состояния.

Автоматизации не определяют состояние сущности и не являются частью бизнес-модели.

---

## One Entity — One State Machine

Каждая бизнес-сущность имеет собственную независимую State Machine.

Жизненный цикл одной сущности не должен дублировать жизненный цикл другой.

---

# 3. Business State Machines

В первой версии Business OS определены следующие машины состояний.

| Business Entity | Owner Domain |
|-----------------|--------------|
| Request | CRM |
| Case | CRM |
| Workflow | Operations |
| Workflow Stage | Operations |
| Financial Transaction | Finance |

---

# 4. Entity: Request

## Purpose

Request отражает жизненный цикл обращения клиента — от первого контакта до возникновения коммерческих отношений и создания одного или нескольких Case либо закрытия обращения без оказания услуги.

Request управляет исключительно этапом взаимодействия с потенциальным клиентом.

После создания первого Case операционная деятельность полностью переносится в соответствующие Workflow.

---

## Lifecycle

```text
New
 │
 ▼
Qualification
 │
 ▼
Waiting Decision
 │
 ▼
Ready To Start
 │
 ├──────────────► Closed – Not Interested
 │
 ├── (7 дней) ─► Reminder Event
 │
 ├── (30 дней) ► Closed – No Response
 │
 ▼
Offer Accepted
 │
 ▼
First Commercial Payment
 │
 ▼
Case Created
```

---

## Terminal States

- Case Created
- Closed – Not Interested
- Closed – No Response

---

## Allowed Transitions

| From | To |
|------|-----|
| New | Qualification |
| Qualification | Waiting Decision |
| Waiting Decision | Ready To Start |
| Ready To Start | Offer Accepted |
| Ready To Start | Closed – Not Interested |
| Ready To Start | Closed – No Response |
| Offer Accepted | First Commercial Payment |
| First Commercial Payment | Case Created |

---

## Trigger Events

| Business Event | New State |
|----------------|-----------|
| Получено новое обращение | New |
| Проведена квалификация | Qualification |
| Отправлено коммерческое предложение | Waiting Decision |
| Клиент подтвердил готовность начать сотрудничество | Ready To Start |
| Через 7 дней без ответа | Reminder Event |
| Через 30 дней без ответа | Closed – No Response |
| Клиент отказался от сотрудничества | Closed – Not Interested |
| Клиент принял коммерческое предложение | Offer Accepted |
| Получена первая коммерческая оплата | First Commercial Payment |
| Создан первый Case | Case Created |

---

## Business Rules

### BR-001

Request может одновременно находиться только в одном состоянии.

---

### BR-002

Каждый переход между состояниями сохраняется в истории Request.

---

### BR-003

Reminder Event является бизнес-событием и не представляет отдельного состояния Request.

---

### BR-004

Если в течение 30 календарных дней после отправки коммерческого предложения не получен ответ клиента, Request автоматически переходит в состояние **Closed – No Response**.

---

### BR-005

После создания первого Case жизненный цикл Request считается завершенным.

Дальнейшее сопровождение клиента осуществляется через соответствующие Case и Workflow.

---

### BR-006

Один Request может породить один или несколько Case.

Создание первого Case завершает жизненный цикл Request независимо от количества создаваемых Case.

---

## Automation Opportunities

| State | Automation |
|---------|------------|
| New | Создать Request и назначить ответственного Manager |
| Qualification | Создать задачу на проведение квалификации |
| Waiting Decision | Подготовить и отправить коммерческое предложение |
| Ready To Start | Запустить таймер ожидания решения клиента |
| Offer Accepted | Подготовить оферту и необходимые документы |
| First Commercial Payment | Создать один или несколько Case |
| Closed – No Response | Закрыть Request автоматически |
| Closed – Not Interested | Закрыть Request с сохранением причины отказа |

# 5. Entity: Case

## Purpose

Case представляет коммерческое обязательство компании перед одним Client по одному Product.

Case является центральной бизнес-сущностью операционной деятельности и объединяет все связанные с оказанием услуги объекты:

- Workflow;
- документы;
- финансовые операции;
- коммуникации;
- ответственного Manager.

Case не управляет процессом выполнения услуги.

Выполнение услуги полностью осуществляется через соответствующий Workflow.

---

## Lifecycle

```text
Created
    │
    ▼
In Progress
    │
    ▼
Completed
    │
    ▼
Archived
```

---

## Terminal States

- Archived

---

## Allowed Transitions

| From | To |
|------|-----|
| Created | In Progress |
| In Progress | Completed |
| Completed | Archived |

---

## Trigger Events

| Business Event | New State |
|----------------|-----------|
| Автоматически создан первый Workflow | In Progress |
| Workflow успешно завершен | Completed |
| Истек срок хранения Case | Archived |

---

## Business Rules

### BR-001

Case создается только после получения первой коммерческой оплаты.

---

### BR-002

Каждый Case принадлежит одному Request.

---

### BR-003

Каждый Case относится только к одному Client.

---

### BR-004

Каждый Case имеет один Product.

---

### BR-005

Каждый Case имеет ровно один Workflow.

---

### BR-006

Case не содержит операционных этапов выполнения услуги.

---

### BR-007

Текущее состояние выполнения услуги определяется исключительно Workflow.

---

### BR-008

Case может перейти в состояние Completed только после завершения Workflow.

---

### BR-009

Case может быть архивирован только после завершения всех связанных бизнес-процессов.

---

## Automation Opportunities

| State | Automation |
|---------|------------|
| Created | Автоматически создать Workflow |
| In Progress | Контролировать выполнение Workflow |
| Completed | Запросить отзыв и предложить повторные услуги |
| Archived | Переместить Case в архив |

---

# 6. Entity: Workflow

## Purpose

Workflow представляет выполнение Workflow Template для конкретного Case.

Workflow управляет всем процессом оказания услуги.

Именно Workflow отражает фактическое состояние выполнения обязательств компании перед клиентом.

Каждый Workflow создается автоматически при создании нового Case.

---

## Lifecycle

```text
Created
    │
    ▼
Active
    │
    ├────────────► Paused
    │                   │
    │                   ▼
    │                Active
    │
    ▼
Completed

или

Cancelled
```

---

## Terminal States

- Completed
- Cancelled

---

## Allowed Transitions

| From | To |
|------|-----|
| Created | Active |
| Active | Paused |
| Paused | Active |
| Active | Completed |
| Active | Cancelled |

---

## Trigger Events

| Business Event | New State |
|----------------|-----------|
| Создан новый Case | Created |
| Начато выполнение Workflow | Active |
| Выполнение временно приостановлено | Paused |
| Выполнение возобновлено | Active |
| Все обязательные этапы завершены | Completed |
| Выполнение прекращено | Cancelled |

---

## Business Rules

### BR-001

Каждый Workflow принадлежит одному Case.

---

### BR-002

Каждый Workflow создается на основании одного Workflow Template.

---

### BR-003

Workflow всегда имеет один текущий активный Workflow Stage.

---

### BR-004

Workflow хранит историю прохождения всех Workflow Stage.

---

### BR-005

Workflow не может перейти в состояние Completed, пока существует хотя бы один незавершенный обязательный Workflow Stage.

---

### BR-006

Workflow может быть переведен в состояние Paused автоматически или по решению Manager.

---

### BR-007

Workflow является единственным владельцем операционного состояния выполнения услуги.

---

## Automation Opportunities

| State | Automation |
|---------|------------|
| Created | Создать первый Workflow Stage |
| Active | Выполнять автоматизации текущего Workflow Stage |
| Paused | Отправить уведомления заинтересованным участникам |
| Completed | Завершить связанный Case |
| Cancelled | Закрыть связанные задачи и уведомить Manager |

---

# 7. Entity: Workflow Stage

## Purpose

Workflow Stage представляет отдельный этап выполнения Workflow.

Workflow Stage является минимальной управляемой единицей операционной деятельности.

Большинство автоматизаций Business OS реагируют именно на изменение состояния Workflow Stage.

---

## Lifecycle

```text
Not Started
      │
      ▼
Active
      │
      ├────────────► Paused
      │                   │
      │                   ▼
      │                Active
      │
      ├────────────► Skipped
      │
      ▼
Completed

или

Cancelled
```

---

## Terminal States

- Completed
- Skipped
- Cancelled

---

## Allowed Transitions

| From | To |
|------|-----|
| Not Started | Active |
| Active | Paused |
| Paused | Active |
| Active | Completed |
| Active | Skipped |
| Active | Cancelled |

---

## Trigger Events

| Business Event | New State |
|----------------|-----------|
| Менеджер начал выполнение этапа | Active |
| Требуется ожидание внешнего события | Paused |
| Выполнение этапа продолжено | Active |
| Этап успешно завершен | Completed |
| Необязательный этап пропущен | Skipped |
| Выполнение Workflow прекращено | Cancelled |

---

## Business Rules

### BR-001

В рамках одного Workflow одновременно может находиться в состоянии Active только один обязательный Workflow Stage.

---

### BR-002

Каждый Workflow Stage принадлежит одному Workflow.

---

### BR-003

Каждый переход между Workflow Stage сохраняется в истории Workflow.

---

### BR-004

Workflow Stage может содержать комментарии Manager.

---

### BR-005

Комментарии Workflow Stage становятся частью истории соответствующего Case.

---

### BR-006

Workflow не может перейти в состояние Completed, пока существует незавершенный обязательный Workflow Stage.

---

### BR-007

Состояние Skipped допускается только для необязательных Workflow Stage.

---

### BR-008

Состояние Cancelled используется только при прекращении выполнения соответствующего Workflow.

---

## Automation Opportunities

| State | Automation |
|---------|------------|
| Active | Создать задачи, уведомить ответственных участников |
| Paused | Ожидать внешнее событие или отправить напоминание |
| Completed | Активировать следующий Workflow Stage |
| Skipped | Зафиксировать причину пропуска |
| Cancelled | Остановить дальнейшее выполнение Workflow |

# 8. Entity: Financial Transaction

## Purpose

Financial Transaction представляет отдельную финансовую операцию, связанную с коммерческой деятельностью компании.

Каждая Financial Transaction проходит собственный жизненный цикл независимо от других финансовых операций.

Финансовые операции являются неизменяемыми бизнес-записями и используются для учета, аналитики и автоматизации.

---

## Lifecycle

```text
Created
    │
    ▼
Pending
    │
    ├────────────► Cancelled
    │
    ▼
Paid
    │
    └────────────► Refunded
```

---

## Terminal States

- Paid
- Cancelled
- Refunded

---

## Allowed Transitions

| From | To |
|------|-----|
| Created | Pending |
| Pending | Paid |
| Pending | Cancelled |
| Paid | Refunded |

---

## Trigger Events

| Business Event | New State |
|----------------|-----------|
| Создана финансовая операция | Created |
| Счет выставлен / ожидается оплата | Pending |
| Получена оплата | Paid |
| Платеж отменен | Cancelled |
| Выполнен возврат денежных средств | Refunded |

---

## Business Rules

### BR-001

Каждая Financial Transaction проходит собственный независимый жизненный цикл.

---

### BR-002

После перехода в состояние **Paid** финансовая операция считается завершенной.

---

### BR-003

Возврат денежных средств оформляется новой Financial Transaction, связанной с исходной операцией.

Исходная Financial Transaction остается неизменной.

---

### BR-004

После перехода в одно из терминальных состояний изменение состояния запрещено.

---

### BR-005

Financial Transaction является неизменяемой бизнес-сущностью (Append-only).

Исправление ошибок выполняется созданием новой связанной операции.

---

## Automation Opportunities

| State | Automation |
|---------|------------|
| Created | Подготовить реквизиты или ссылку на оплату |
| Pending | Контролировать поступление оплаты |
| Paid | Обновить финансовую отчетность и активировать связанные бизнес-процессы |
| Cancelled | Зафиксировать отмену операции |
| Refunded | Создать корректирующую финансовую запись |

---

# 9. Общие правила

## One Entity — One State Machine

Каждая бизнес-сущность имеет единственную утвержденную State Machine.

Все инженерные решения должны использовать только данную модель.

---

## State Changes Only Through Business Events

Изменение состояния допускается только при наступлении соответствующего бизнес-события.

Прямое изменение состояния без бизнес-события запрещено.

---

## History Is Immutable

История переходов между состояниями никогда не изменяется и не удаляется.

История используется для:

- аудита;
- аналитики;
- автоматизаций;
- AI Agents.

---

## State Machine Is Technology Independent

State Machine описывает бизнес-логику.

Она не зависит от:

- Backend;
- Frontend;
- Notion;
- PostgreSQL;
- FastAPI;
- Telegram Bot;
- используемых средств автоматизации.

---

## Automation Reacts To State

Автоматизации реагируют на изменение состояния, но не определяют бизнес-логику.

Изменение используемой технологии автоматизации не должно изменять модель состояний.

---

## No Duplicate State Definitions

Каждая State Machine определяется только в одном месте.

Настоящий документ является единственным Source of Truth для жизненных циклов бизнес-сущностей.

Другие документы могут ссылаться на State Machine, но не должны переопределять ее.

---

# 10. Engineering Notes

Business State Machines являются нормативной основой для проектирования:

- статусов в Business OS;
- enum в Backend;
- API Contracts;
- Automation Rules;
- PostgreSQL;
- FastAPI;
- Telegram Bot;
- AI Agents.

При реализации допускается создание технических статусов, если они не изменяют бизнес-смысл настоящей модели.

---

# 11. Open Decisions

На текущем этапе открытых вопросов по моделям состояний нет.

Изменение любой State Machine допускается только после принятия соответствующего Architecture Decision Record (ADR).

---

# 12. Статус документа

**Status:** 🟢 Approved

Business State Machines является нормативным документом архитектуры V!BE Business OS.

Документ определяет официальные жизненные циклы бизнес-сущностей компании и является единственным Source of Truth для реализации состояний в Business OS, Backend, API, автоматизациях и AI Agents.

Все инженерные решения должны соответствовать настоящему документу.