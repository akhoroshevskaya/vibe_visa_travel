> **Architecture Note**
>
> Документ является частью методологии V!BE Business OS.
>
> Его цель — описать информационную модель бизнеса независимо от способа реализации.
>
> Business Information Model определяет бизнес-сущности, их ответственность, взаимосвязи и правила существования.
>
> Документ является основой для:
>
> - Business OS;
> - Notion;
> - PostgreSQL;
> - FastAPI;
> - API;
> - AI;
> - Automation.
>
> Поля, типы данных и технические ограничения описываются позже в Data Dictionary на этапе Engineering Sprint.

# V!BE Business OS

# Business Information Model v1.0

**Version:** 1.0

**Status:** Approved

**Owner:** Aleksandra

**Project:** V!BE Visa & Travel

---

# 1. Purpose

Business Information Model описывает основные бизнес-сущности компании, их ответственность и взаимосвязи.

Документ определяет единый язык предметной области (Ubiquitous Language), который используется во всех доменах Business OS.

Каждая сущность существует только один раз и имеет единственного владельца (Source of Truth).

---

# 2. Принципы модели

## Business First

Информационная модель строится вокруг бизнеса, а не вокруг базы данных.

---

## One Source of Truth

Каждая бизнес-сущность имеет единственного владельца.

---

## One Entity — One Responsibility

Каждая сущность отвечает только за одну область бизнеса.

---

## Relationships Instead of Duplication

Сущности связываются между собой отношениями.

Копирование информации запрещено.

---

## Case Is the Core Entity

Главной сущностью операционной деятельности является Case.

Все остальные сущности существуют для создания, сопровождения или завершения Case.

---

# 3. Business Entities Overview

## CRM Domain

- Request
- Client
- Case
- Manager
- Communication

---

## Catalog Domain

- Product
- Service
- Workflow Template

---

## Operations Domain

- Workflow
- Workflow Stage
- Case Document
- Appointment
- External Partner

---

## Finance Domain

- Financial Transaction

---

## Knowledge Domain

- Country
- City
- Visa Program
- Visa Requirement
- Embassy
- Consulate
- Visa Center
- Knowledge Article
- FAQ
- Document Template

---

# 4. Relationships Overview

```text
Request
      │
      ▼
Client
      │
      ▼
Case
      │
      ├────────► Product
      │
      ├────────► Workflow
      │                 │
      │                 ▼
      │          Workflow Stages
      │
      ├────────► Case Documents
      │
      ├────────► Financial Transactions
      │
      └────────► Communications
```

---

# 5. CRM Domain

CRM Domain является владельцем следующих сущностей:

- Request
- Client
- Case
- Manager
- Communication

CRM отвечает за взаимоотношения компании с клиентами.

Он не отвечает за выполнение услуг, финансовые операции или знания компании.

---

# Entity: Request

## Purpose

Request представляет первое обращение потенциального клиента.

Request фиксирует намерение воспользоваться услугами компании.

Request еще не означает начало оказания услуги.

Он становится основой для дальнейшего принятия решения о сотрудничестве.

После выполнения условий начала коммерческих отношений один Request может породить один или несколько Case.

---

## Owner Domain

CRM

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
    ├── 7 дней → Reminder
    │
    ├── 30 дней → Closed – No Response
    │
    ▼
Offer Accepted
    │
    ▼
First Commercial Payment
    │
    ▼
Case(s) Created

или

Closed – Not Interested
```

---

## Relationships

Request

→ Primary Contact (Client) (1)

→ Clients (1..N)

→ Cases (1..N)

→ Manager (1)

→ Communications (0..N)

---

## Business Rules

### BR-001

Request создается при первом обращении потенциального клиента.

### BR-002

Request не является коммерческим обязательством компании.

### BR-003

Бесплатная консультация не создает Case.

### BR-004

После первой коммерческой оплаты Request становится основанием для создания одного или нескольких Case.

### BR-005

Один Request может породить несколько Case.

Например:

- семья из трех человек;
- корпоративная поездка;
- группа туристов.

### BR-006

Каждый Request имеет одного Primary Contact.

Primary Contact всегда является существующим Client.

### BR-007

Каждый Request имеет одного ответственного Manager.

### BR-008

История смены Manager сохраняется.

### BR-009

Через 7 дней без ответа автоматически отправляется Reminder.

### BR-010

Через 30 дней Request автоматически закрывается со статусом "Closed – No Response".

---

## Related ADR

- ADR-005 — Request Lifecycle
- ADR-008 — Every Request Has an Owner
- ADR-010 — Contact Person Is a Role

---

# Entity: Client

## Purpose

Client представляет человека или организацию, с которыми компания строит долгосрочные отношения.

Client существует независимо от отдельных Request и Case.

Один и тот же Client может многократно обращаться в компанию, участвовать в различных Case и выполнять разные роли в разных Request.

Client создается один раз и никогда не дублируется.

---

## Owner Domain

CRM

---

## Relationships

Client

→ Requests (1..N)

→ Cases (1..N)

→ Financial Transactions (0..N)

→ Primary Contact for Request (0..N)

→ Communications (0..N)

---

## Business Rules

### BR-001

Client создается один раз.

### BR-002

Повторное обращение создает новый Request, но не нового Client.

### BR-003

Один Client может иметь любое количество Request.

### BR-004

Один Client может иметь любое количество Case.

### BR-005

Client может быть Primary Contact в одном Request и обычным участником в другом.

### BR-006

История взаимодействия с Client никогда не удаляется.

---

## Related ADR

- ADR-005 — Request Lifecycle
- ADR-010 — Contact Person Is a Role

# Entity: Case

## Purpose

Case представляет конкретную услугу, оказываемую одному Client.

Case является центральной операционной сущностью V!BE Business OS.

Именно вокруг Case строится выполнение Workflow, обработка документов, финансовые операции, коммуникации и аналитика.

Case создается только после возникновения коммерческих отношений между компанией и клиентом.

Product не участвует непосредственно в выполнении заказа.

При создании Case Product используется как шаблон для автоматического создания соответствующего набора Case Service.

Первичный набор Case Service формируется автоматически на основании Product.
---

## Owner Domain

CRM

---

## Relationships

Case

→ Request (1)

→ Client (1)

→ Product (1)

→ Services (1..N)

→ Workflow (1)

→ Manager (1)

→ Case Documents (0..N)

→ Financial Transactions (0..N)

→ Communications (0..N)

---

## Business Rules

### BR-001

Case всегда создается на основании существующего Request.

### BR-002

Каждый Case относится только к одному Client.

### BR-003

Один Request может породить несколько Case.

### BR-004

Case создается после первой коммерческой оплаты.

### BR-005

Каждый Case имеет одного ответственного Manager.

### BR-006

Каждый Case проходит собственный Workflow.

### BR-007

Каждый Case всегда находится только на одном текущем Workflow Stage.

### BR-008

Изменение Workflow Stage сохраняется в истории Workflow.

### BR-009

Каждый Workflow Stage может содержать комментарий Manager.

### BR-010

Комментарий Workflow Stage становится частью истории Case.

### BR-011

Каждый Case может содержать любое количество Financial Transaction.

### BR-012

Каждый Case может содержать любое количество Communication.

### BR-013

Каждый Case может содержать любое количество Case Document.

---

## Related ADR

- ADR-005 — Request Lifecycle
- ADR-008 — Every Request Has an Owner
- ADR-011 — Workflow Templates
- ADR-012 — Case Is Managed by Workflow Stage
- ADR-013 — Communications Belong to CRM Domain

---

# Entity: Manager

## Purpose

Manager представляет сотрудника компании, отвечающего за сопровождение Request и Case.

Manager отвечает за организацию процесса, коммуникацию с клиентом и контроль прохождения Workflow.

Организационная структура компании будет описана отдельно в Organization Domain.

---

## Owner Domain

CRM

---

## Relationships

Manager

→ Requests (0..N)

→ Cases (0..N)

→ Communications (0..N)

---

## Business Rules

### BR-001

Каждый Request имеет одного ответственного Manager.

### BR-002

Каждый Case имеет одного ответственного Manager.

### BR-003

История смены Manager сохраняется.

### BR-004

Manager может сопровождать любое количество Request и Case.

---

## Related ADR

- ADR-008 — Every Request Has an Owner

---

# Entity: Communication

## Purpose

Communication представляет любое взаимодействие компании с клиентом.

Communication является частью истории взаимоотношений и всегда существует в контексте Request или Case.

Канал связи не влияет на бизнес-смысл Communication.

---

## Owner Domain

CRM

---

## Relationships

Communication

→ Client (1)

→ Request (0..1)

→ Case (0..1)

→ Manager (1)

---

## Business Rules

### BR-001

Communication всегда связана хотя бы с одним Client.

### BR-002

Communication относится либо к Request, либо к Case.

### BR-003

Communication хранит полную историю взаимодействия.

### BR-004

Канал коммуникации не влияет на бизнес-модель.

---

## Related ADR

- ADR-013 — Communications Belong to CRM Domain

# Entity: Product

## Purpose

Product представляет коммерческий продукт компании.

Именно Product продается клиенту и становится основанием для создания Case.

Product определяет коммерческое предложение, набор доступных Service и Workflow Template.

---

## Owner Domain

Catalog

---

## Relationships

Product

→ Services (1..N)

→ Workflow Template (1)

→ Cases (0..N)

---

## Business Rules

### BR-001

Каждый Product использует один Workflow Template.

### BR-002

Product состоит из одной или нескольких Service.

### BR-003

Один Product может использоваться в любом количестве Case.

---

## Related ADR

- ADR-011 — Workflow Template

---

# Entity: Service

## Purpose

Service представляет отдельную бизнес-услугу компании.

Service является переиспользуемой сущностью и может входить в состав различных Product.

---

## Owner Domain

Catalog

---

## Relationships

Service

→ Products (1..N)

---

## Business Rules

### BR-001

Service может использоваться в нескольких Product.

### BR-002

Service может продаваться отдельно.

### BR-003

Service является каталоговой сущностью.

Связь с конкретным Case осуществляется
через сущность Case Service.

Связь Service с Case будет определена на этапе проектирования ERD.

---

# Entity: Workflow Template

## Purpose

Workflow Template описывает эталонный бизнес-процесс выполнения Product.

На основании Workflow Template создается Workflow конкретного Case.

---

## Owner Domain

Catalog

---

## Relationships

Workflow Template

→ Products (1..N)

→ Workflow Stages (1..N)

→ Workflows (0..N)

---

## Business Rules

### BR-001

Каждый Product использует один Workflow Template.

### BR-002

Изменение Workflow Template не изменяет Workflow уже существующих Case.

### BR-003

Workflow Template является шаблоном и не содержит информации о конкретных клиентах.

---

## Related ADR

- ADR-011 — Workflow Template

---

# Entity: Workflow

## Purpose

Workflow представляет выполнение Workflow Template для конкретного Case.

Он хранит текущее состояние выполнения услуги.

---

## Owner Domain

Operations

---

## Relationships

Workflow

→ Case (1)

→ Workflow Template (1)

→ Workflow Stages (1..N)

---

## Business Rules

### BR-001

Каждый Case имеет один Workflow.

### BR-002

Workflow создается автоматически при создании Case.

### BR-003

Workflow хранит текущий Stage и историю прохождения этапов.

---

## Related ADR

- ADR-011 — Workflow Template
- ADR-012 — Workflow Stage

---

# Entity: Workflow Stage

## Purpose

Workflow Stage представляет отдельный этап выполнения Workflow.

В каждый момент времени активен только один Workflow Stage.

Каждый переход между этапами сохраняется в истории Workflow.

---

## Owner Domain

Operations

---

## Relationships

Workflow Stage

→ Workflow (1)

---

## Business Rules

### BR-001

Workflow всегда имеет один текущий активный Stage.

### BR-002

Переход между Stage сохраняется в истории.

### BR-003

Каждый Stage может содержать комментарий Manager.

### BR-004

Комментарий становится частью истории Case.

---

## Related ADR

- ADR-012 — Workflow Stage

---
# Entity: Case Service

### Назначение

Case Service представляет выполнение конкретной бизнес-услуги в рамках определенного Case.

Сущность связывает каталог услуг компании с операционной деятельностью по выполнению заказа клиента.

Case Service создается автоматически при создании Case на основании выбранного Product.

---

### Бизнес-правила

- Каждый Case Service принадлежит ровно одному Case.
- Каждый Case Service связан ровно с одной Service.
- Один Case может содержать одну или несколько Case Service.
- Первичный набор Case Service формируется автоматически на основании Product.
- В процессе исполнения Case состав Case Service может изменяться в соответствии с бизнес-правилами.
- Case Service является операционной сущностью и не изменяет описание Service в каталоге.

---

### Связи

| Связь | Кардинальность |
|--------|----------------|
| Case | N → 1 |
| Service | N → 1 |

---

### Бизнес-атрибуты

| Атрибут | Описание |
|----------|----------|
| id | Уникальный идентификатор |
| case_id | Ссылка на Case |
| service_id | Ссылка на Service |
| status | Текущий статус выполнения |
| owner | Ответственный исполнитель |
| planned_start_at | Плановая дата начала |
| planned_end_at | Плановая дата завершения |
| completed_at | Фактическая дата завершения |
| notes | Операционные комментарии |

---

### Примечания

Case Service является операционной сущностью.

Service остается каталоговой сущностью и не изменяется в процессе выполнения заказа.

Связь между Case и Service осуществляется исключительно через сущность Case Service.

Case является Aggregate Root и управляет жизненным циклом всех принадлежащих ему Case Service.

# Entity: Case Document

## Purpose

Case Document представляет документ, относящийся к конкретному Case.

Документы принадлежат Case, а не Client.

---

## Owner Domain

Operations

---

## Relationships

Case Document

→ Case (1)

---

## Business Rules

### BR-001

Документ относится только к одному Case.

### BR-002

Один Case может содержать любое количество документов.

### BR-003

Документ может иметь несколько версий.

---

# Entity: Financial Transaction

## Purpose

Financial Transaction представляет любую финансовую операцию, связанную с Case.

В системе хранятся как доходы, так и расходы.

---

## Owner Domain

Finance

---

## Relationships

Financial Transaction

→ Case (1)

---

## Business Rules

### BR-001

Любая финансовая операция относится к одному Case.

### BR-002

Financial Transaction является неизменяемой записью (Append-only).

### BR-003

Исправление ошибки выполняется новой Financial Transaction.

### BR-004

Возврат денежных средств оформляется отдельной Financial Transaction, связанной с исходной операцией.
---

## Related ADR

- ADR-009 — Financial Transactions

---

# 6. Open Decisions
---

# 7. Следующий этап

После утверждения Business Information Model выполняется проектирование:

- Business Entity Relationship Diagram (ERD);
- кратностей связей;
- навигации между сущностями;
- агрегатов;
- границ транзакций.

---

# 8. Статус документа

**Status:** 🟢 Approved

Business Information Model является главным документом информационной архитектуры V!BE Business OS.

Все инженерные решения (Notion, PostgreSQL, FastAPI, API, Frontend и AI) должны строиться на основе данной модели.

### ADR Result

В архитектуре V!BE Business OS вводится отдельная бизнес-сущность
Case Service.

Case представляет коммерческое обязательство компании перед клиентом.

Case Service представляет выполнение конкретной Service
в рамках конкретного Case.

Product определяет набор Service,
которые автоматически создаются при создании Case.

Case является Aggregate Root.