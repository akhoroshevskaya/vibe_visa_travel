> **Architecture Note**
>
> Документ является частью методологии V!BE Business OS.
>
> Его цель — описать взаимосвязи между бизнес-сущностями компании независимо от способа реализации.
>
> Business Entity Relationships определяет:
>
> - владельцев сущностей;
> - типы связей;
> - правила владения;
> - навигацию между сущностями;
> - границы агрегатов.
>
> Документ является мостом между Business Information Model и инженерной реализацией (Notion, PostgreSQL, FastAPI, API и Frontend).

# V!BE Business OS

# Business Entity Relationships v1.0

**Version:** 1.0

**Status:** Approved

**Owner:** Aleksandra

**Project:** V!BE Visa & Travel

---

# 1. Purpose

Business Entity Relationships определяет, каким образом бизнес-сущности связаны между собой.

Документ не описывает структуру хранения данных или физическую модель базы данных.

Его задача — зафиксировать бизнес-логику связей между сущностями до начала проектирования Business OS и инженерной реализации.

---

# 2. Relationship Principles

## Business First

Связи определяются бизнес-процессами, а не особенностями хранения данных.

---

## One Source of Truth

Каждая сущность имеет единственного владельца (Owner Domain).

Только владелец может изменять данные сущности.

Остальные домены используют связи, но не создают собственных копий данных.

---

## Relationships Instead of Duplication

Связи между сущностями предпочтительнее копирования данных.

Повторное хранение одной и той же информации запрещено.

---

## Stable Identity

Каждая сущность обладает собственной стабильной идентичностью.

Изменение связей между сущностями не изменяет идентичность самой сущности.

---

## Explicit Ownership

Для каждой связи должен быть определен владелец.

Владелец отвечает за создание, изменение и жизненный цикл связи.

---

## Lifecycle Independence

Если это возможно, сущности должны иметь независимый жизненный цикл.

Удаление одной сущности не должно автоматически приводить к удалению других сущностей.

Используется принцип Soft Delete и архивирования данных.

---

# 3. Ownership Matrix

| Entity | Owner Domain |
|---------|--------------|
| Request | CRM |
| Client | CRM |
| Case | CRM |
| Manager | CRM |
| Communication | CRM |
| Product | Catalog |
| Service | Catalog |
| Workflow Template | Catalog |
| Workflow | Operations |
| Workflow Stage | Operations |
| Case Document | Operations |
| Appointment | Operations |
| External Partner | Operations |
| Financial Transaction | Finance |
| Country | Knowledge |
| City | Knowledge |
| Visa Program | Knowledge |
| Visa Requirement | Knowledge |
| Embassy | Knowledge |
| Consulate | Knowledge |
| Visa Center | Knowledge |
| Knowledge Article | Knowledge |
| FAQ | Knowledge |
| Document Template | Knowledge |

---

# 4. Relationship Types

В модели используются следующие типы связей.

## One-to-One (1:1)

Каждому экземпляру сущности соответствует только один экземпляр другой сущности.

Пример:

```text
Case
    │
    ▼
Workflow
```

---

## One-to-Many (1:N)

Одна сущность может быть связана с несколькими экземплярами другой сущности.

Пример:

```text
Client

│

├──► Case

├──► Case

└──► Case
```

---

## Many-to-Many (N:M)

Две сущности могут быть связаны друг с другом многократно.

Такие связи реализуются через промежуточную бизнес-сущность.

Пример:

```text
Product

↓

Case Service

↓

Service
```

⚠️ На текущем этапе сущность **Case Service** еще не утверждена.

Она рассматривается как возможное решение и будет окончательно определена после анализа полной модели связей.

---

# 5. Navigation Rules

Навигация между сущностями должна быть предсказуемой.

Каждая связь должна отвечать на два вопроса:

- Кто является владельцем связи?
- В каком направлении допускается навигация?

Пример:

```text
Request

↓

Case
```

Навигация возможна:

- Request → Cases
- Case → Request

Навигация должна быть симметричной только тогда, когда это оправдано бизнесом.

---

# 6. Следующий этап

В следующих разделах документа будут подробно описаны все связи между сущностями с указанием:

- кратности;
- владельца;
- бизнес-правил;
- направления навигации;
- политики удаления;
- дополнительных архитектурных ограничений.

# 7. Relationship Specifications

---

# Request ⇄ Client

## Cardinality

```text
Request N : M Client
```

Один Request может включать нескольких Client.

Например:

- семья;
- группа туристов;
- корпоративная поездка.

Один Client может участвовать в нескольких Request.

---

## Owner

CRM

---

## Business Rule

Request объединяет участников одного обращения.

При этом один из Client обязательно назначается Primary Contact.

Primary Contact является ролью внутри Request, а не отдельной сущностью.

---

## Navigation

Request

→ Clients

Client

→ Requests

---

## Delete Policy

Удаление запрещено.

Используется Soft Delete.

---

# Request ⇄ Case

## Cardinality

```text
Request 1 : N Case
```

---

## Owner

CRM

---

## Business Rule

Case создается только на основании существующего Request.

Один Request может породить несколько Case.

Например:

```text
Request

↓

Семья

↓

Case — Иван

Case — Мария

Case — Анна
```

---

## Navigation

Request

→ Cases

Case

→ Request

---

## Delete Policy

Удаление запрещено.

История должна сохраняться.

---

# Client ⇄ Case

## Cardinality

```text
Client 1 : N Case
```

---

## Owner

CRM

---

## Business Rule

Каждый Case принадлежит одному Client.

Один Client может иметь любое количество Case.

Например:

- Италия 2026
- Великобритания 2027
- США 2028

---

## Navigation

Client

→ Cases

Case

→ Client

---

## Delete Policy

Удаление запрещено.

---

# Request ⇄ Manager

## Cardinality

```text
Manager 1 : N Request
```

---

## Owner

CRM

---

## Business Rule

Каждый Request имеет одного ответственного Manager.

История смены Manager сохраняется.

---

## Navigation

Manager

→ Requests

Request

→ Manager

---

## Delete Policy

История смены Manager никогда не удаляется.

---

# Case ⇄ Manager

## Cardinality

```text
Manager 1 : N Case
```

---

## Owner

CRM

---

## Business Rule

Каждый Case имеет одного ответственного Manager.

Manager может сопровождать любое количество Case.

История смены Manager является частью истории Case.

---

## Navigation

Manager

→ Cases

Case

→ Manager

---

## Delete Policy

Используется Soft Delete.

---

# Communication ⇄ Client

## Cardinality

```text
Client 1 : N Communication
```

---

## Owner

CRM

---

## Business Rule

Каждая Communication всегда связана хотя бы с одним Client.

Communication не существует сама по себе.

---

## Navigation

Client

→ Communications

Communication

→ Client

---

# Communication ⇄ Request

## Cardinality

```text
Request 1 : N Communication
```

---

## Business Rule

На этапе Qualification большинство Communication относятся к Request.

После создания Case новые Communication могут относиться уже непосредственно к Case.

---

# Communication ⇄ Case

## Cardinality

```text
Case 1 : N Communication
```

---

## Business Rule

Communication является частью истории сопровождения Case.

Канал связи не влияет на бизнес-модель.

Telegram, Email, WhatsApp и телефон используют одну и ту же сущность Communication.

---

## CRM Aggregate

На текущем этапе в CRM Domain формируется следующий агрегат.

```text
                Request
                   │
         ┌─────────┴─────────┐
         │                   │
         ▼                   ▼
     Clients             Communications
         │
         ▼
       Cases
         │
         ▼
      Manager
```

CRM Aggregate отвечает за:

- регистрацию обращения;
- сопровождение клиента;
- создание Case;
- историю взаимодействия;
- назначение ответственных сотрудников.

# Product ⇄ Service

## Cardinality

```text
Product N : M Service
```

---

## Owner

Catalog

---

## Business Rule

Product состоит из одной или нескольких Service.

Одна Service может использоваться в составе нескольких Product.

Например:

```text
Шенгенская виза
        │
        ├──► Страховка
        ├──► Запись
        ├──► Перевод
        └──► Курьер

Виза США
        │
        ├──► Перевод
        ├──► Страховка
        └──► Запись
```

---

## Navigation

Product

→ Services

Service

→ Products

---

## Delete Policy

Удаление запрещено.

Используется архивирование.

---

# Product ⇄ Workflow Template

## Cardinality

```text
Product N : 1 Workflow Template
```

---

## Owner

Catalog

---

## Business Rule

Каждый Product использует один Workflow Template.

Один Workflow Template может использоваться несколькими Product.

---

## Navigation

Product

→ Workflow Template

Workflow Template

→ Products

---

## Delete Policy

Workflow Template не удаляется после использования.

Используется версионирование.

---

# Case ⇄ Workflow

## Cardinality

```text
Case 1 : 1 Workflow
```

---

## Owner

Operations

---

## Business Rule

При создании Case автоматически создается собственный Workflow.

Каждый Workflow принадлежит только одному Case.

---

## Navigation

Case

→ Workflow

Workflow

→ Case

---

## Delete Policy

Удаление запрещено.

---

# Workflow ⇄ Workflow Stage

## Cardinality

```text
Workflow 1 : N Workflow Stage
```

---

## Owner

Operations

---

## Business Rule

Workflow состоит из последовательности Stage.

В каждый момент времени активен только один Stage.

История прохождения Stage сохраняется.

---

## Navigation

Workflow

→ Workflow Stages

Workflow Stage

→ Workflow

---

## Delete Policy

История прохождения Stage никогда не удаляется.

---

# Case ⇄ Case Document

## Cardinality

```text
Case 1 : N Case Document
```

---

## Owner

Operations

---

## Business Rule

Все документы принадлежат Case.

Документы не принадлежат Client.

Например:

```text
Case

↓

Паспорт

↓

Страховка

↓

Анкета

↓

Бронь

↓

Виза
```

При повторном обращении создается новый набор документов нового Case.

---

## Navigation

Case

→ Documents

Case Document

→ Case

---

## Delete Policy

Удаление запрещено.

Документы архивируются.

---

# Case ⇄ Financial Transaction

## Cardinality

```text
Case 1 : N Financial Transaction
```

---

## Owner

Finance

---

## Business Rule

Каждая финансовая операция относится только к одному Case.

В рамках одного Case могут существовать любые виды операций:

Доходы

- визовое сопровождение;
- страховка;
- запись;
- зарубежная карта;
- тревел-консьерж.

Расходы

- консульский сбор;
- услуги курьера;
- переводы;
- нотариус;
- услуги партнера;
- доставка паспорта;
- авиабилеты;
- проживание;
- комиссии.

Возврат денежных средств не изменяет существующую Financial Transaction.

Создается новая Financial Transaction, связанная с исходной операцией.


---

## Navigation

Case

→ Financial Transactions

Financial Transaction

→ Case

---

## Delete Policy

Удаление запрещено.

Используется Append-only.

---

# Operations Aggregate

На текущем этапе Operations Domain имеет следующую структуру.

```text
              Workflow
                  │
        ┌─────────┴─────────┐
        │                   │
        ▼                   ▼
Workflow Stages      Case Documents
        │
        ▼
  Appointments
        │
        ▼
External Partners
```

Operations Aggregate отвечает за:

- выполнение Workflow;
- управление этапами;
- документы;
- встречи;
- взаимодействие с партнерами.

---

# Finance Aggregate

```text
Case

│

▼

Financial Transactions
```

Finance Aggregate отвечает за:

- доходы;
- расходы;
- прибыль;
- финансовую историю Case.

# 8. Business Relationship Diagram

```text
                            Knowledge Domain
──────────────────────────────────────────────────────────────────────

Country
    │
    ▼
Visa Program
    │
    ▼
Visa Requirement
    │
    ▼
Document Template

──────────────────────────────────────────────────────────────────────

                           Catalog Domain

                Product
                   │
        ┌──────────┴──────────┐
        ▼                     ▼
    Services         Workflow Template
                              │
                              ▼
                       Workflow Stages

──────────────────────────────────────────────────────────────────────

                            CRM Domain

             Request
                │
      ┌─────────┴─────────┐
      ▼                   ▼
 Clients           Communications
      │
      ▼
    Cases
      │
      ▼
   Manager

──────────────────────────────────────────────────────────────────────

                        Operations Domain

              Case
               │
      ┌────────┴────────┐
      ▼                 ▼
 Workflow        Case Documents
      │
      ▼
Workflow Stages
      │
      ▼
Appointments
      │
      ▼
External Partners

──────────────────────────────────────────────────────────────────────

                         Finance Domain

Case
 │
 ▼
Financial Transactions
```

---

# 9. Business Groupings

На текущем этапе проектирования выделяются следующие агрегаты.

## CRM Aggregate

Root:

- Request

Contains:

- Clients
- Communications
- Cases

---

## Operations Aggregate

Root:

- Workflow

Contains:

- Workflow Stages
- Case Documents
- Appointments
- External Partners

---

## Finance Aggregate

Root:

- Financial Transaction

---

## Catalog Aggregate

Root:

- Product

Contains:

- Services
- Workflow Template

---

## Knowledge Aggregate

Root:

- Country

Contains:

- Visa Programs
- Requirements
- Embassies
- Consulates
- Visa Centers
- Knowledge Articles
- Templates

---

# 10. Relationship Candidates

Во время проектирования были обнаружены потенциальные промежуточные сущности.

На текущем этапе они **не входят в архитектуру**.

Их необходимость будет подтверждена или отклонена после начала Engineering Sprint.

## Candidate-001

Request Participant

Назначение:

Хранение роли Client внутри Request.

Возможные роли:

- Primary Contact
- Applicant
- Child
- Sponsor
- Family Member

---

## Candidate-002

Product Service

Назначение:

Хранение параметров Service внутри Product.

Например:

- обязательная услуга;
- стоимость по умолчанию;
- порядок отображения;
- возможность отключения.

---

## Candidate-003

Case Service

Назначение:

Хранение выполнения конкретной Service внутри Case.

Например:

- заказана;
- оплачена;
- выполнена;
- отменена;
- исполнитель;
- стоимость.

---

# 11. Engineering Notes

Данный документ является основой для:

- проектирования Notion Business OS;
- PostgreSQL;
- ER-модели базы данных;
- Pydantic Models;
- API Contracts;
- Backend Domain Models;
- Permission Model;
- Automation Rules;
- AI Agents.

---

# 12. Open Decisions

На текущем этапе остаются открытыми только вопросы, связанные с промежуточными сущностями.

Все остальные связи между бизнес-сущностями считаются утвержденными.

---

# 13. Статус документа

**Status:** 🟢 Approved

Business Entity Relationships определяет официальную модель связей между всеми бизнес-сущностями V!BE Business OS.

Все инженерные решения должны соответствовать данной модели.