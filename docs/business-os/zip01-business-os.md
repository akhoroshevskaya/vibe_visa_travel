# 01 Business OS

Версия: Draft v0.1

Статус: In Progress

Этап: Business OS Design Sprint

---

# 1. Назначение

Business OS — это операционный слой компании, который превращает утвержденную Business Architecture в ежедневно используемую цифровую рабочую среду.

Business OS не является CRM.

Business OS объединяет людей, процессы, данные, автоматизации и AI в единую систему управления компанией.

Business Architecture описывает, **как устроен бизнес**.

Business OS описывает, **как этот бизнес работает каждый день**.

Business OS является реализацией утвержденной архитектуры и не изменяет ее.

---

# 2. Принципы проектирования

Business OS полностью наследует принципы, утвержденные в Business Architecture.

## Business First

Рабочее пространство строится вокруг бизнес-процессов, а не возможностей конкретной платформы.

---

## One Source of Truth

Каждая бизнес-сущность существует только в одном месте.

Все остальные объекты используют ссылки на первоисточник.

---

## Domain-Oriented Design

Business OS организована вокруг утвержденных Business Domain.

Каждый домен отвечает только за свои данные и процессы.

---

## Human First

Ответственность за принятие решений всегда остается за человеком.

Автоматизации помогают выполнять работу, но не заменяют владельца процесса.

---

## AI as Copilot

Искусственный интеллект помогает анализировать информацию, готовить материалы, выполнять рутинные операции и предлагать решения.

Окончательные бизнес-решения принимает человек.

---

## Modular Architecture

Каждый Business Domain развивается независимо, оставаясь частью единой Business OS.

---

## Low-Code First

Конфигурация имеет приоритет над разработкой.

Программирование используется только тогда, когда возможности платформы исчерпаны.

---

## Evolutionary Architecture

Business OS развивается постепенно.

Любые изменения должны оставаться совместимыми с утвержденной Business Architecture.

## Technology Independence

Business OS проектируется независимо от конкретной платформы.

Все операционные данные организуются через Operational Repository.

Конкретная технология хранения данных рассматривается как реализация Repository и может быть заменена без изменения архитектуры Business OS.
---

# 3. Архитектура Business OS

Business OS является операционным уровнем между архитектурой бизнеса и ежедневной работой компании.

```text
Business Architecture
        │
        ▼
Business OS
        │
 ┌──────┼────────┐
 │      │        │
 ▼      ▼        ▼
Люди   AI   Автоматизации
 │      │        │
 └──────┼────────┘
        ▼
Бизнес-данные
```

Business OS не заменяет Business Architecture.

Она реализует ее в виде цифровой рабочей среды.

Структура Business OS строится вокруг утвержденных Business Domain:

- CRM
- Catalog
- Operations
- Finance
- Knowledge

Каждый домен содержит собственные базы данных, представления, шаблоны, автоматизации, AI-компоненты и рабочие панели.

Взаимодействие между доменами осуществляется через отношения между бизнес-сущностями, определенными в Business Information Model.

Никакие операционные объекты не могут существовать вне утвержденного Business Domain.

Business OS остается независимой от конкретной технологии.

На текущем этапе Notion рассматривается как первая платформа реализации Business OS, а не как ее архитектурная основа.

# 4. Operational Repositories
Каждый Operational Repository описывается с использованием единого Repository Specification.

Repository Specification является обязательным архитектурным контрактом и определяет назначение, ответственность, связи и правила использования Repository независимо от технологии реализации.
## 4.1 Архитектура репозиториев

Business OS организует операционные данные через систему Operational Repository.

Operational Repository — это логическое операционное хранилище, реализующее одну Business Entity.

Repository является архитектурной абстракцией и не зависит от конкретной технологии хранения данных.

Технология хранения рассматривается как реализация Repository и может быть заменена без изменения архитектуры Business OS.

На текущем этапе все Operational Repository реализуются с использованием баз данных Notion.

Архитектурная модель состоит из трех уровней:

```text
Business Entity
        │
        ▼
Operational Repository
        │
        ▼
Technology Implementation
```

Каждая Business Entity имеет один и только один Operational Repository.

Repository является единственным источником операционных данных для соответствующей Business Entity.

---

## 4.2 Принципы проектирования Repository

### One Repository — One Entity

Каждый Repository реализует хранение только одной Business Entity.

Repository не объединяет данные нескольких сущностей.

---

### One Source of Truth

Каждая Business Entity имеет только один Repository.

Дублирование операционных данных не допускается.

---

### Domain Ownership

Каждый Repository принадлежит одному Business Domain.

Ответственность за структуру, качество и жизненный цикл Repository несет владелец соответствующего домена.

---

### Relation over Duplication

Связи между Repository реализуются через отношения между Business Entity.

Копирование данных между Repository запрещено.

---

### Platform Independence

Repository описывает бизнес-модель хранения данных.

Способ реализации Repository зависит от выбранной технологической платформы и не влияет на архитектуру Business OS.

---

### Evolution without Breaking Architecture

Repository может развиваться.

Добавление новых свойств допускается при условии сохранения совместимости с утвержденной Business Architecture.

Изменение Business Entity производится только через Architecture Decision Record (ADR).

---

## 4.3 Классификация Repository

Все Repository Business OS делятся на несколько категорий.

### Master Repository

Хранят основные бизнес-сущности компании.

Примеры:

- Clients
- Services
- Countries

---

### Operational Repository

Используются для ежедневной работы компании.

Примеры:

- Requests
- Cases
- Workflows

---

### Transaction Repository

Хранят неизменяемые события и операции.

Пример:

- Financial Transactions

---

### Knowledge Repository

Используются для хранения корпоративных знаний.

Примеры:

- Knowledge Base
- SOP
- Templates

Каждый Operational Repository описывается с использованием Repository Specification.

Стандарт Repository Specification определяется в документе
"02 Business OS Design Standards".

## 4.4 Core Operational Repositories

Operational Repository группируются по утвержденным Business Domain.

Каждый Repository реализует одну Business Entity и является единственным операционным источником данных для этой сущности.

### Catalog Domain

Repository:

- Services Repository
- Countries Repository

Назначение домена:

Каталог определяет, какие услуги предоставляет компания и в каком контексте они могут быть оказаны.

Catalog является источником справочных данных для остальных Business Domain.

---

### CRM Domain

Repository:

- Requests Repository
- Clients Repository
- Cases Repository

Назначение домена:

CRM отвечает за регистрацию обращений клиентов, ведение клиентской базы и управление жизненным циклом Case.

CRM является точкой входа в Business OS.

---

### Operations Domain

Repository:

- Workflows Repository
- Workflow Stages Repository

Назначение домена:

Operations обеспечивает выполнение услуг и управление операционной деятельностью компании.

---

### Finance Domain

Repository:

- Financial Transactions Repository

Назначение домена:

Finance отвечает за регистрацию всех финансовых операций компании.

Financial Transaction является Append-only сущностью.

---

### Knowledge Domain

Repository:

- Knowledge Repository

Назначение домена:

Knowledge обеспечивает накопление корпоративного опыта, инструкций, шаблонов и знаний.

Knowledge Domain развивается вместе с компанией и используется людьми, автоматизациями и AI.

## 4.5 Requests Repository Specification

### Назначение

Requests Repository является операционным репозиторием для хранения и управления Business Entity **Request**.

Repository обеспечивает регистрацию всех входящих обращений клиентов независимо от источника поступления.

Request является точкой входа в Business OS.

---

### Business Entity

Request

---

### Business Domain

CRM

---

### Ответственность

Requests Repository отвечает за:

- регистрацию нового Request;
- хранение исходной информации обращения;
- фиксацию канала поступления;
- отслеживание жизненного цикла Request;
- передачу Request в дальнейшую обработку;
- инициирование создания одного или нескольких Case.

---

### Repository Owner

CRM Domain Owner

---

### Связанные Repository

Входящие связи:

- Services Repository
- Countries Repository

Исходящие связи:

- Clients Repository
- Cases Repository

---

### Бизнес-правила

Каждый Request создается один раз.

Каждый Request имеет уникальный идентификатор.

Request не изменяет информацию о Client.

Request может существовать без Client до момента идентификации клиента.

Один Request может привести к созданию нескольких Case.

После создания Case Request продолжает существовать как самостоятельная Business Entity.

---

### Основные операции

Repository поддерживает следующие операции:

- регистрация Request;
- просмотр;
- обновление разрешенных данных;
- поиск;
- фильтрация;
- архивирование.

Удаление Request не допускается.

---

### Текущая реализация

Notion Database

---

### Будущие расширения

В рамках текущей версии архитектуры не реализуются:

- Request Participant;
- Request Timeline;
- Product Service.

Данные возможности находятся в Architecture Backlog.

## 4.6 Repository Interaction Model

### Назначение

Business OS рассматривает Operational Repository как участников единого операционного процесса компании.

Repository не являются независимыми хранилищами данных.

Каждый Repository занимает строго определенное место в жизненном цикле создания ценности для клиента.

Repository взаимодействуют посредством утвержденных Business Entity и их жизненного цикла.

---

### Принципы взаимодействия

#### Sequential Flow

Repository взаимодействуют в соответствии с бизнес-процессом компании.

Создание следующего Repository возможно только после наступления соответствующего бизнес-события.

---

#### Business Lifecycle Driven

Переход между Repository определяется изменением состояния Business Entity, а не техническими событиями платформы.

---

#### Controlled Creation

Repository не создают друг друга напрямую.

Создание новой Business Entity инициируется утвержденным бизнес-процессом.

---

#### No Direct Repository Dependency

Repository не зависят друг от друга на уровне реализации.

Связь существует только через утвержденную Business Architecture.

---

#### Traceability

Business OS обеспечивает возможность проследить полный путь обработки обращения клиента от первого Request до завершения оказания услуги.

---

### Основной операционный поток

```text
External World
      │
      ▼
Request
      │
      ▼
Client
      │
      ▼
Case
      │
      ▼
Workflow
      │
      ▼
Financial Transaction
      │
      ▼
Knowledge
```

---

### Repository Interaction

#### Request → Client

После идентификации клиента данные Request используются для создания или связывания Business Entity Client.

Request остается самостоятельной Business Entity.

---

#### Client → Case

При подтверждении необходимости оказания услуги создается один или несколько Case.

Case всегда принадлежит одному Client.

---

#### Case → Workflow

Каждый Case инициирует выполнение соответствующего Workflow.

Workflow отвечает за выполнение услуги.

---

#### Workflow → Financial Transaction

Финансовые операции регистрируются в процессе выполнения Workflow в соответствии с утвержденными бизнес-правилами.

Financial Transaction является Append-only Business Entity.

---

#### Workflow → Knowledge

После завершения Workflow Business OS может сохранить накопленные знания, шаблоны, инструкции или рекомендации в Knowledge Domain.

Knowledge Repository не влияет на выполнение Workflow и используется для накопления корпоративного опыта.

---

### Сквозной жизненный цикл

Business OS должна обеспечивать возможность проследить полную цепочку обработки обращения.

```text
Request
    │
    ▼
Client
    │
    ▼
Case
    │
    ▼
Workflow
    │
    ▼
Financial Transaction
```

Каждый этап связан с предыдущим через утвержденные Business Entity.

---

### Архитектурные ограничения

Business OS запрещает:

- создание Repository вне утвержденных Business Domain;
- создание Repository без соответствующей Business Entity;
- обход утвержденного жизненного цикла Business Entity;
- дублирование ответственности между Repository;
- нарушение принципа One Source of Truth.

Любые изменения последовательности взаимодействия Repository требуют изменения Business Architecture и оформляются через ADR.

## 4.7 Clients Repository Specification

### Назначение

Clients Repository является операционным репозиторием для хранения и управления Business Entity **Client**.

Repository представляет клиента как долгосрочную сущность Business OS независимо от количества Request и Case.

---

### Business Entity

Client

---

### Business Domain

CRM

---

### Ответственность

Clients Repository отвечает за:

- хранение карточки клиента;
- идентификацию клиента;
- предотвращение дублирования клиентов;
- связь клиента со всеми Request;
- связь клиента со всеми Case;
- предоставление единого представления клиента компании.

---

### Repository Owner

CRM Domain Owner

---

### Связанные Repository

Входящие:

- Requests Repository

Исходящие:

- Cases Repository

---

### Бизнес-правила

Client создается один раз.

Один Client может иметь множество Request.

Один Client может иметь множество Case.

Client существует независимо от конкретного Request.

Удаление Client не допускается.

---

### Основные операции

- создание;
- идентификация;
- объединение дубликатов;
- обновление данных;
- поиск;
- архивирование.

---

### Текущая реализация

Notion Database

## 4.8 Cases Repository Specification

### Назначение

Cases Repository является операционным репозиторием для хранения и управления Business Entity **Case**.

Case представляет конкретное обязательство компании по оказанию услуги клиенту.

---

### Business Entity

Case

---

### Business Domain

CRM

---

### Ответственность

Cases Repository отвечает за:

- регистрацию нового Case;
- связь Case с Client;
- связь Case с Request;
- определение оказываемой услуги;
- передачу Case в Operations Domain;
- контроль жизненного цикла Case.

---

### Repository Owner

CRM Domain Owner

---

### Связанные Repository

Входящие:

- Requests Repository
- Clients Repository
- Services Repository

Исходящие:

- Workflows Repository

---

### Бизнес-правила

Каждый Case принадлежит одному Client.

Каждый Case создается на основании Request.

Один Request может создать несколько Case.

Case может существовать только при наличии Client.

Case не хранит операционные действия выполнения услуги.

За выполнение отвечает Workflow.

---

### Основные операции

- создание;
- изменение статуса;
- назначение Workflow;
- поиск;
- архивирование.

---

### Текущая реализация

Notion Database

## 4.9 Workflows Repository Specification

### Назначение

Workflows Repository является операционным репозиторием для хранения и управления Business Entity **Workflow**.

Repository обеспечивает выполнение услуги в соответствии с утвержденным бизнес-процессом.

---

### Business Entity

Workflow

---

### Business Domain

Operations

---

### Ответственность

Workflows Repository отвечает за:

- выполнение услуги;
- управление этапами Workflow;
- контроль выполнения;
- фиксацию результата;
- передачу информации в Finance и Knowledge Domain.

---

### Repository Owner

Operations Domain Owner

---

### Связанные Repository

Входящие:

- Cases Repository

Исходящие:

- Workflow Stages Repository
- Financial Transactions Repository
- Knowledge Repository

---

### Бизнес-правила

Каждый Workflow принадлежит одному Case.

Workflow описывает процесс оказания услуги.

Workflow состоит из одного или нескольких Workflow Stage.

Workflow завершается после выполнения услуги.

---

### Основные операции

- запуск;
- изменение этапов;
- завершение;
- приостановка;
- поиск.

---

### Текущая реализация

Notion Database

## 4.10 Workflow Stages Repository Specification

### Назначение

Workflow Stages Repository является операционным репозиторием для хранения и управления Business Entity **Workflow Stage**.

Repository определяет последовательность этапов выполнения Workflow.

---

### Business Entity

Workflow Stage

---

### Business Domain

Operations

---

### Ответственность

Workflow Stages Repository отвечает за:

- описание этапов Workflow;
- определение последовательности выполнения;
- контроль прохождения этапов;
- фиксацию текущего состояния выполнения услуги.

---

### Repository Owner

Operations Domain Owner

---

### Связанные Repository

Входящие:

- Workflows Repository

Исходящие:

- отсутствуют

---

### Бизнес-правила

Каждый Workflow состоит из одного или нескольких Workflow Stage.

Каждый Workflow Stage принадлежит одному Workflow.

Изменение последовательности этапов допускается только в рамках утвержденного Workflow.

---

### Основные операции

- создание;
- изменение;
- завершение;
- поиск.

---

### Текущая реализация

Notion Database

## 4.11 Financial Transactions Repository Specification

### Назначение

Financial Transactions Repository является операционным репозиторием для хранения и управления Business Entity **Financial Transaction**.

Repository обеспечивает регистрацию всех финансовых операций компании.

---

### Business Entity

Financial Transaction

---

### Business Domain

Finance

---

### Ответственность

Financial Transactions Repository отвечает за:

- регистрацию финансовых операций;
- хранение истории операций;
- обеспечение финансовой прозрачности;
- поддержку финансовой отчетности.

---

### Repository Owner

Finance Domain Owner

---

### Связанные Repository

Входящие:

- Workflows Repository
- Cases Repository

Исходящие:

- отсутствуют

---

### Бизнес-правила

Financial Transaction является Append-only Business Entity.

Удаление Financial Transaction запрещено.

Корректировки выполняются созданием новой Financial Transaction.

Каждая операция должна иметь бизнес-основание.

---

### Основные операции

- регистрация;
- поиск;
- фильтрация;
- формирование отчетности.

---

### Текущая реализация

Notion Database

## 4.12 Knowledge Repository Specification

### Назначение

Knowledge Repository является операционным репозиторием для накопления и управления корпоративными знаниями.

Repository обеспечивает сохранение опыта компании и его повторное использование людьми, автоматизациями и AI.

---

### Business Domain

Knowledge

---

### Ответственность

Knowledge Repository отвечает за:

- хранение инструкций;
- хранение SOP;
- хранение шаблонов;
- накопление лучших практик;
- поддержку AI знаниями.

---

### Repository Owner

Knowledge Domain Owner

---

### Связанные Repository

Входящие:

- Workflows Repository
- Cases Repository

Исходящие:

- AI Services
- Users

---

### Бизнес-правила

Knowledge не участвует непосредственно в выполнении бизнес-процессов.

Knowledge используется для повышения качества работы компании.

Каждая запись должна иметь владельца и источник происхождения.

---

### Основные операции

- создание;
- актуализация;
- поиск;
- архивирование.

---

### Текущая реализация

Notion Database

# 5. Repository Properties

## 5.1 Назначение

Repository Properties определяют структуру Operational Repository.

Properties описывают характеристики Business Entity и обеспечивают хранение, обработку и анализ операционных данных.

Business OS использует единую модель Properties для всех Repository.

Каждое Property относится к одной из утвержденных категорий.

---

## 5.2 Принципы проектирования

### Business First

Каждое Property существует только при наличии бизнес-необходимости.

Property не добавляется из-за ограничений или возможностей конкретной платформы.

---

### One Meaning

Одно Property хранит только одно бизнес-понятие.

Property не объединяет несколько независимых значений.

---

### Single Source of Truth

Каждое значение хранится только в одном Repository.

Если информация уже существует в другом Repository, используется Relation, а не дублирование.

---

### Platform Independence

Property описывает бизнес-смысл.

Тип данных конкретной платформы является способом реализации.

---

### Evolution

Добавление новых Property допускается без изменения Repository Specification.

Изменение смысла существующего Property требует Architecture Review.

---

## 5.3 Классификация Properties

Все Properties Business OS относятся к одной из следующих категорий.

### Identity Properties

Идентифицируют Business Entity.

Примеры:

- ID
- Number
- External ID

---

### Lifecycle Properties

Отражают жизненный цикл Business Entity.

Примеры:

- Status
- Created At
- Closed At
- Archived At

---

### Business Properties

Хранят бизнес-информацию.

Примеры:

- Client Name
- Visa Type
- Country
- Service

---

### Relation Properties

Определяют связи между Repository.

Примеры:

- Client
- Request
- Case
- Workflow

---

### Financial Properties

Используются для финансовой информации.

Примеры:

- Amount
- Currency
- Payment Method

---

### AI Properties

Используются AI-компонентами.

Примеры:

- AI Summary
- AI Classification
- AI Confidence

---

### Audit Properties

Используются для контроля изменений.

Примеры:

- Created By
- Updated By
- Last Activity

---

### Computed Properties

Вычисляются автоматически.

Примеры:

- Duration
- Total Amount
- Active Cases

## 5.4 Property Model

### Назначение

Property Model определяет архитектурную модель бизнес-свойств, используемых в Business OS.

Модель описывает не конкретные свойства Repository, а единые принципы организации данных.

Property Model является основой Business Data Dictionary.

Все Operational Repository используют общую модель Properties.

---

### Архитектурная модель

Каждое Property проходит три уровня определения.

```text
Property Category
        │
        ▼
Property Definition
        │
        ▼
Repository Usage
```

---

### Property Category

Property Category определяет роль свойства в Business OS.

Категория отвечает на вопрос:

> Для чего существует данное Property?

Каждое Property принадлежит только одной категории.

---

### Property Definition

Property Definition является каноническим описанием Property.

Каждое Property определяется только один раз.

Property Definition содержит:

- имя;
- бизнес-смысл;
- тип данных;
- правила использования;
- ограничения;
- возможность повторного использования.

Property Definition является частью Business Data Dictionary.

---

### Repository Usage

Repository Usage определяет использование Property внутри конкретного Operational Repository.

Repository не определяет новые Property.

Repository использует только Property, определенные в Business Data Dictionary.

Repository может:

- использовать Property;
- сделать Property обязательным;
- сделать Property необязательным;
- не использовать Property.

---

### Архитектурные принципы

#### Define Once

Каждое Property определяется только один раз.

---

#### Reuse Everywhere

Одно и то же Property может использоваться во многих Repository.

---

#### Consistent Meaning

Property сохраняет одинаковый бизнес-смысл независимо от Repository.

---

#### Independent Evolution

Repository могут изменять набор используемых Properties без изменения Property Definition.

---

#### Business Before Platform

Property определяется бизнесом.

Тип данных платформы является способом реализации.

## 5.5 Property Catalog

### Назначение

Property Catalog является единым словарем бизнес-свойств Business OS.

Каждое Property Definition определяется только один раз и может использоваться в любом Operational Repository.

Property Catalog обеспечивает единообразие данных, терминологии и правил хранения информации во всей Business OS.

---

### Структура Property Definition

Каждое Property Definition описывается следующими характеристиками:

| Атрибут | Описание |
|----------|----------|
| Property Name | Каноническое имя свойства |
| Category | Категория Property |
| Business Meaning | Бизнес-смысл свойства |
| Data Type | Логический тип данных |
| Required by Default | Обязательность по умолчанию |
| Multiple Values | Допускается ли несколько значений |
| Computed | Вычисляется автоматически |
| Reusable | Может использоваться повторно |
| Description | Дополнительные пояснения |

---

### Каталог категорий

Business OS использует следующие категории Property:

| Category | Назначение |
|----------|------------|
| Identity | Идентификация Business Entity |
| Lifecycle | Жизненный цикл сущности |
| Business | Основные бизнес-данные |
| Relation | Связи между Repository |
| Financial | Финансовые данные |
| AI | Данные, создаваемые или используемые AI |
| Audit | Контроль изменений |
| Computed | Вычисляемые свойства |
| System | Служебные свойства платформы |

---

### Правила Property Catalog

#### Canonical Definition

Каждое Property существует только в одном экземпляре как Property Definition.

---

#### Shared Vocabulary

Все Repository используют единый словарь Property Definition.

---

#### Consistent Naming

Название Property одинаково во всей Business OS.

Не допускается создание одинаковых по смыслу Property под разными именами.

---

#### Business Meaning First

Property определяется бизнес-смыслом.

Тип реализации определяется на уровне технологической платформы.

---

#### Backward Compatibility

Изменение существующего Property Definition требует Architecture Review.

Добавление нового Property допускается без изменения существующих Repository.

---

### Жизненный цикл Property

```text
Property Proposal
        │
        ▼
Architecture Review
        │
        ▼
Property Definition
        │
        ▼
Property Catalog
        │
        ▼
Repository Usage
        │
        ▼
Platform Implementation
```

---

### Ответственность

Property Catalog является частью архитектуры Business OS.

Изменение существующих Property Definition допускается только после Architecture Review.

Repository не могут самостоятельно изменять Property Definition.

## 5.6 Property Definition Standard

### Назначение

Property Definition Standard определяет единый архитектурный контракт для всех Property Definition, используемых в Business OS.

Каждое Property Definition описывается только один раз и становится частью Business Data Dictionary.

Operational Repository используют Property Definition посредством Property Instance.

Property Definition не зависит от конкретной технологической платформы.

---

### Архитектурная модель

```text
Property Category
        │
        ▼
Property Definition
        │
        ▼
Property Instance
        │
        ▼
Repository
```

---

### Обязательная структура Property Definition

Каждое Property Definition содержит следующие разделы.

| Раздел | Назначение |
|---------|------------|
| Property ID | Уникальный идентификатор Property |
| Property Name | Каноническое имя |
| Category | Категория Property |
| Business Meaning | Бизнес-смысл |
| Business Purpose | Для чего используется Property |
| Data Type | Логический тип данных |
| Required by Default | Обязательность |
| Multiple Values | Возможность хранения нескольких значений |
| Computed | Вычисляется автоматически |
| Reusable | Допускается повторное использование |
| Repository Usage | Где используется |
| Validation Rules | Правила проверки |
| Business Rules | Бизнес-ограничения |
| Current Implementation | Реализация на текущей платформе |
| Notes | Дополнительные комментарии |

---

### Правила Property Definition

#### Canonical Definition

Каждое Property Definition определяется только один раз.

---

#### Stable Identifier

Property ID остается неизменным на протяжении всего жизненного цикла Business OS.

---

#### Consistent Meaning

Business Meaning не зависит от Repository, в котором используется Property.

---

#### Platform Independent

Property Definition описывает бизнес.

Тип данных платформы является способом реализации.

---

#### Reuse First

Перед созданием нового Property необходимо проверить существование аналогичного Property Definition в Property Catalog.

Создание дублирующих Property запрещено.

---

#### Architecture Review

Изменение существующего Property Definition требует Architecture Review.

Добавление нового Property допускается без изменения существующих Repository.

### PD-001 — Created At

---

### Property ID

PD-001

---

### Property Name

Created At

---

### Category

Lifecycle

---

### Business Meaning

Дата и время создания Business Entity в Business OS.

---

### Business Purpose

Позволяет определить момент появления Business Entity и используется для отслеживания жизненного цикла, аналитики, аудита и автоматизаций.

---

### Data Type

DateTime

---

### Required by Default

Да

---

### Multiple Values

Нет

---

### Computed

Да

Заполняется системой автоматически.

---

### Reusable

Да

---

### Repository Usage

Используется всеми Operational Repository.

---

### Validation Rules

- не может быть пустым;
- устанавливается только один раз;
- изменение значения запрещено.

---

### Business Rules

Created At является частью жизненного цикла любой Business Entity.

Используется при:

- расчете времени обработки;
- построении аналитики;
- работе автоматизаций;
- аудите изменений;
- построении отчетности.

---

### Current Implementation

Notion Property

Type: Created Time

---

### Notes

Является обязательным Property для всех Operational Repository.

## 5.7 Architecture Object Identification

### Назначение

Business OS использует единую систему идентификации архитектурных объектов.

Каждый архитектурный объект получает уникальный идентификатор, который остается неизменным на протяжении всего жизненного цикла проекта.

Идентификаторы используются для:

- ссылок между документами;
- проведения Architecture Review;
- Architecture Decision Records;
- отслеживания изменений;
- автоматической генерации документации;
- автоматической генерации реализации.

---

### Правила идентификации

Каждый идентификатор состоит из:

```
<Prefix>-<Number>
```

Например:

```
BE-001
REP-003
PD-015
AUT-008
```

Номер никогда не изменяется.

При удалении объекта его идентификатор повторно не используется.

---

### Типы архитектурных объектов

| Prefix | Object |
|---------|--------|
| BE | Business Entity |
| REP | Operational Repository |
| PD | Property Definition |
| VIEW | View Definition |
| TMP | Template Definition |
| AUT | Automation Definition |
| DASH | Dashboard Definition |
| AI | AI Component |
| NAV | Navigation Element |
| PERM | Permission Definition |

---

### Примеры

#### Business Entity

- BE-001 Request
- BE-002 Client
- BE-003 Case

---

#### Repository

- REP-001 Requests Repository
- REP-002 Clients Repository
- REP-003 Cases Repository

---

#### Property Definition

- PD-001 Created At
- PD-002 Updated At
- PD-003 Status
- PD-004 Owner

---

#### View

- VIEW-001 Active Requests
- VIEW-002 Open Cases

---

#### Template

- TMP-001 New Request
- TMP-002 New Case

---

#### Automation

- AUT-001 Create Case
- AUT-002 Notify Manager

---

#### Dashboard

- DASH-001 CRM Dashboard
- DASH-002 Operations Dashboard

---

### Архитектурные правила

Каждый архитектурный объект имеет только один идентификатор.

Идентификатор не зависит от названия объекта.

Переименование объекта не изменяет его идентификатор.

Удаленный идентификатор повторно не используется.

Все ссылки между архитектурными документами рекомендуется выполнять через идентификаторы объектов.

## 5.8 Business Data Dictionary

### Назначение

Business Data Dictionary является единым источником истины для всех Property Definition Business OS.

Dictionary определяет канонические определения бизнес-свойств, используемых Operational Repository.

Dictionary обеспечивает единообразие терминологии, структуры данных и повторное использование Property Definition.

---

### Архитектурная модель

```text
Business Entity
        │
        ▼
Repository Specification
        │
        ▼
Property Definition
        │
        ▼
Business Data Dictionary
        │
        ▼
Repository Property Instance
        │
        ▼
Technology Implementation
```

---

### Ответственность Dictionary

Business Data Dictionary отвечает за:

- хранение Property Definition;
- контроль уникальности Property;
- поддержку единой терминологии;
- повторное использование Property;
- контроль изменений Property;
- поддержку генерации реализации.

---

### Архитектурные принципы

#### Single Definition

Каждое Property Definition существует только один раз.

---

#### Shared Vocabulary

Все Repository используют единый словарь.

---

#### Independent Repository

Repository используют Property, но не владеют ими.

---

#### Traceability

Каждое Property Instance может быть связано с Property Definition.

---

#### Evolution

Dictionary развивается независимо от Operational Repository.

Добавление новых Property не требует изменения существующих Repository.

---

### Жизненный цикл Property

Property Proposal

↓

Architecture Review

↓

Property Definition

↓

Business Data Dictionary

↓

Repository Usage

↓

Platform Implementation

### Принцип развития словаря

Business Data Dictionary развивается итеративно.

Новые Property Definition создаются только при возникновении бизнес-потребности в рамках проектирования Operational Repository.

Business OS избегает предварительного проектирования неиспользуемых Property.

Это обеспечивает эволюционное развитие модели данных и соответствует принципу Business Before Technology.

# 6. Repository Property Usage

## Назначение

Repository Property Usage определяет использование Property Definition внутри Operational Repository.

Repository не определяет собственные свойства.

Каждый Repository использует Property Definition, определенные в Business Data Dictionary.

Repository Property Usage определяет:

- какие Property используются;
- какие являются обязательными;
- какие являются вычисляемыми;
- какие являются служебными;
- какие используются AI;
- какие свойства специфичны для данного Repository.

Repository Property Usage не изменяет Property Definition.

---

## 6.1 Requests Repository Property Usage

### Repository

REP-001 Requests Repository

---

### Используемые Property

| Property ID | Property Name | Category | Required | Notes |
|-------------|---------------|----------|----------|-------|
| PD-001 | Created At | Lifecycle | ✅ | Автоматически |
| PD-002 | Updated At | Audit | ✅ | Автоматически |
| PD-003 | Status | Lifecycle | ✅ | Текущее состояние Request |
| PD-004 | Owner | Audit | ❌ | Ответственный менеджер |

---

### Repository-specific Property

Следующие Property впервые определяются для Requests Repository.

| Property ID | Property Name | Category |
|-------------|---------------|----------|
| PD-005 | Request Source | Business |
| PD-006 | Request Channel | Business |
| PD-007 | Client Message | Business |
| PD-008 | Priority | Business |
| PD-009 | AI Summary | AI |

---

### Relation Property

| Property ID | Property Name | Required |
|-------------|---------------|----------|
| PD-010 | Client | ❌ |
| PD-011 | Service | ✅ |
| PD-012 | Case | ❌ |

---

### Computed Property

| Property ID | Property Name |
|-------------|---------------|
| PD-013 | Processing Time |

---

### Property Statistics

| Category | Count |
|----------|------:|
| Identity | 0 |
| Lifecycle | 2 |
| Business | 4 |
| Relation | 3 |
| AI | 1 |
| Audit | 1 |
| Computed | 1 |

---

### Architecture Notes

Requests Repository является первым Repository, использующим Property Definition.

Все новые Property, впервые появляющиеся в данном Repository, автоматически добавляются в Business Data Dictionary и могут быть повторно использованы другими Repository.

---

## 6.2 Clients Repository Property Usage

Структура раздела аналогична Requests Repository.

Используются существующие Property Definition и создаются только новые Property, специфичные для Business Entity Client.

---

## 6.3 Cases Repository Property Usage

Структура раздела аналогична Requests Repository.

---

## 6.4 Workflows Repository Property Usage

Структура раздела аналогична Requests Repository.

---

## 6.5 Workflow Stages Repository Property Usage

Структура раздела аналогична Requests Repository.

---

## 6.6 Financial Transactions Repository Property Usage

Структура раздела аналогична Requests Repository.

---

## 6.7 Knowledge Repository Property Usage

Структура раздела аналогична Requests Repository.