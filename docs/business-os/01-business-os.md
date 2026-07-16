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