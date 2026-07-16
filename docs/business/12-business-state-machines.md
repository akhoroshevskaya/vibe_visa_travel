> **Architecture Note**
>
> Документ является частью методологии V!BE Business OS.
>
> Его цель — определить жизненные циклы ключевых бизнес-сущностей компании.
>
> State Machine описывает допустимые состояния сущности, правила переходов между ними и события, которые инициируют изменение состояния.
>
> Документ является основой для:
>
> - Business OS (Notion);
> - Backend;
> - API;
> - Automation;
> - AI Agents.

# V!BE Business OS

# Business State Machines v1.0

**Version:** 1.0

**Status:** Approved

**Owner:** Aleksandra

**Project:** V!BE Visa & Travel

---

# 1. Purpose

Business State Machines определяет жизненные циклы основных бизнес-сущностей компании.

Документ описывает:

- возможные состояния;
- допустимые переходы;
- бизнес-события;
- ограничения;
- точки автоматизации.

State Machine описывает поведение бизнеса независимо от используемых технологий.

---

# 2. Общие принципы

## State Is Business

Состояние отражает бизнес-смысл, а не техническую реализацию.

---

## Only Valid Transitions

Сущность может переходить только по разрешенным переходам.

Недопустимые переходы запрещены.

---

## Events Change State

Изменение состояния всегда вызывается бизнес-событием.

Например:

- клиент подтвердил сотрудничество;
- поступила оплата;
- менеджер завершил этап;
- получен паспорт.

---

## State History Never Disappears

История переходов между состояниями сохраняется.

Это необходимо для:

- аналитики;
- аудита;
- автоматизации;
- AI.

---

## Automation Reacts To State

Автоматизации реагируют на изменение состояния.

Само состояние не зависит от автоматизации.

---

# 3. State Machines

В первой версии Business OS определяются следующие машины состояний.

| Entity | Domain |
|----------|---------|
| Request | CRM |
| Case | CRM |
| Workflow | Operations |
| Workflow Stage | Operations |
| Financial Transaction | Finance |

---

# 4. Entity: Request

## Purpose

Request отражает жизненный цикл обращения клиента — от первого контакта до создания одного или нескольких Case либо закрытия обращения без оказания услуги.

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
 ├── 7 дней ───► Reminder Sent
 │
 ├── 30 дней ─► Closed – No Response
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

| Event | Result |
|---------|--------|
| Получено новое обращение | New |
| Проведена квалификация | Qualification |
| Клиенту отправлено предложение | Waiting Decision |
| Клиент готов начать сотрудничество | Ready To Start |
| Через 7 дней без ответа | Reminder (событие) |
| Через 30 дней без ответа | Closed – No Response |
| Клиент отказался | Closed – Not Interested |
| Оферта принята | Offer Accepted |
| Получена первая коммерческая оплата | First Commercial Payment |
| Создан первый Case | Case Created |

---

## Business Rules

### BR-001

Request может находиться только в одном состоянии одновременно.

### BR-002

Переход между состояниями сохраняется в истории.

### BR-003

Reminder не является состоянием.

Reminder является бизнес-событием.

### BR-004

Через 30 дней без ответа Request автоматически закрывается.

### BR-005

После создания первого Case жизненный цикл Request считается завершенным.

---

## Automation Opportunities

При изменении состояния могут автоматически выполняться действия.

| State | Automation |
|---------|------------|
| New | Создать Request |
| Qualification | Создать задачу менеджеру |
| Waiting Decision | Отправить коммерческое предложение |
| Ready To Start | Запустить таймер ожидания |
| Offer Accepted | Подготовить оферту |
| First Commercial Payment | Создать первый Case |
| Closed – No Response | Закрыть Request |
| Closed – Not Interested | Закрыть Request |

---

# 5. Entity: Case

## Purpose

Case отражает жизненный цикл оказания конкретной услуги одному Client.

Case создается после возникновения коммерческих отношений и завершается после полного выполнения обязательств компании.

---

## Lifecycle

```text
Created
    │
    ▼
Active
    │
    ├──────────────► Waiting Client
    │                     │
    │                     ▼
    │                 Active
    │
    ├──────────────► Waiting Partner
    │                     │
    │                     ▼
    │                 Active
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

# Entity: Case

## Purpose

Case отражает жизненный цикл коммерческого обязательства компании перед клиентом.

Case не управляет операционной работой.

Выполнение услуги полностью управляется Workflow.

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

| Event | Result |
|---------|--------|
| Создан первый Workflow | In Progress |
| Workflow завершен | Completed |
| Истек срок хранения | Archived |

---

## Business Rules

### BR-001

Case создается после первой коммерческой оплаты.

### BR-002

Каждый Case имеет один Workflow.

### BR-003

Case не содержит операционных этапов.

### BR-004

Все этапы выполнения находятся внутри Workflow.

### BR-005

Case может быть архивирован только после завершения Workflow.

---

## Automation Opportunities

| State | Automation |
|---------|------------|
| Created | Создать Workflow |
| In Progress | Контролировать выполнение Workflow |
| Completed | Запросить отзыв, предложить повторные услуги |
| Archived | Перевести в архив |

---

# 6. Entity: Workflow

## Purpose

Workflow управляет выполнением услуги.

Именно Workflow отражает реальное состояние работы по Case.

---

## Lifecycle

```text
Created
    │
    ▼
Active
    │
    ├──────────────► Paused
    │                     │
    │                     ▼
    │                  Active
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

| Event | Result |
|---------|--------|
| Создан Case | Created |
| Начато выполнение | Active |
| Требуется ожидание | Paused |
| Работа возобновлена | Active |
| Выполнены все Stage | Completed |
| Case отменен | Cancelled |

---

## Business Rules

### BR-001

Каждый Workflow принадлежит одному Case.

### BR-002

Workflow всегда имеет один текущий Stage.

### BR-003

Stage определяет фактическое состояние выполнения услуги.

### BR-004

Workflow не может быть Completed, пока существует незавершенный обязательный Stage.

### BR-005

Workflow может быть Paused по инициативе менеджера или автоматически.

---

## Automation Opportunities

| State | Automation |
|---------|------------|
| Created | Создать Stage |
| Active | Выполнять автоматизации Stage |
| Paused | Отправить уведомление |
| Completed | Завершить Case |
| Cancelled | Закрыть связанные задачи |

# 7. Entity: Workflow Stage

## Purpose

Workflow Stage представляет отдельный этап выполнения Workflow.

Именно Workflow Stage отражает текущую операционную работу по Case.

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
      │                  │
      │                  ▼
      │               Active
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

| Event | Result |
|---------|--------|
| Менеджер начал этап | Active |
| Требуется ожидание | Paused |
| Работа продолжена | Active |
| Этап успешно завершен | Completed |
| Этап пропущен | Skipped |
| Выполнение прекращено | Cancelled |

---

## Business Rules

### BR-001

В каждый момент времени только один обязательный Workflow Stage может находиться в состоянии Active.

### BR-002

Workflow Stage может содержать комментарий Manager.

### BR-003

Комментарий Workflow Stage является частью истории Case.

### BR-004

Workflow не может перейти в состояние Completed, пока существуют незавершенные обязательные Workflow Stage.

### BR-005

Skipped допускается только для необязательных Workflow Stage.

### BR-006

Cancelled используется только при прекращении выполнения Workflow.

---

## Automation Opportunities

| State | Automation |
|---------|------------|
| Active | Создать задачи, уведомить Manager |
| Paused | Напомнить клиенту, ожидать внешнее событие |
| Completed | Активировать следующий Workflow Stage |
| Skipped | Зафиксировать причину |
| Cancelled | Остановить дальнейшее выполнение Workflow |

---

# 8. Entity: Financial Transaction

## Purpose

Financial Transaction отражает жизненный цикл отдельной финансовой операции.

Каждая операция проходит собственный путь — от создания до успешной оплаты, отмены или возврата.

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

| Event | Result |
|---------|--------|
| Создана финансовая операция | Created |
| Выставлен счет / ожидается платеж | Pending |
| Получена оплата | Paid |
| Платеж отменен | Cancelled |
| Выполнен возврат | Refunded |

---

## Business Rules

### BR-001

Каждая Financial Transaction проходит собственный жизненный цикл.

### BR-002

После перехода в состояние Paid финансовая операция считается завершенной.

### BR-003

Refunded не изменяет исходную операцию.

Возврат оформляется отдельной Financial Transaction, связанной с исходной операцией.

### BR-004

После перехода в Paid, Cancelled или Refunded изменение состояния запрещено.

---

## Automation Opportunities

| State | Automation |
|---------|------------|
| Created | Подготовить реквизиты или ссылку на оплату |
| Pending | Контролировать поступление оплаты |
| Paid | Обновить финансовую отчетность, активировать связанные бизнес-процессы |
| Cancelled | Зафиксировать отмену операции |
| Refunded | Создать корректирующую финансовую запись |

---

# 9. Общие правила

- Каждая сущность имеет собственную State Machine.
- Изменение состояния всегда инициируется бизнес-событием.
- История переходов сохраняется.
- Автоматизации реагируют на изменение состояния, но не определяют его.
- Недопустимые переходы запрещены.

---

# 10. Engineering Notes

Business State Machines являются основой для:

- статусов в Notion;
- enum в Backend;
- API Contracts;
- Automation Rules;
- Telegram Bot;
- AI Agents.

---

# 11. Open Decisions

На текущем этапе открытых вопросов по State Machines нет.

Все жизненные циклы ключевых сущностей считаются утвержденными.

---

# 12. Статус документа

**Status:** 🟢 Approved

Business State Machines определяет официальные жизненные циклы основных бизнес-сущностей V!BE Business OS.

Все инженерные решения должны соответствовать данной модели.