> **Architecture Note**
>
> Документ является частью методологии V!BE Business OS.
>
> Его цель — описать архитектуру цифровой компании на уровне бизнеса, независимо от используемых технологий.
>
> Данный документ определяет основные бизнес-домены, их ответственность и границы. Детальная структура данных, API и техническая реализация описываются в документах следующих этапов.

# V!BE Business OS

# Business Architecture v1.0

**Version:** 1.0

**Status:** Approved

**Owner:** Aleksandra

**Project:** V!BE Visa & Travel

---

# 1. Главная идея

V!BE Business OS — это не CRM и не сайт.

Это цифровая операционная система компании, которая управляет полным жизненным циклом взаимодействия с клиентом — от первого обращения до повторных услуг.

Сайт, Telegram, AI, социальные сети и любые другие каналы являются интерфейсами взаимодействия с Business OS, но не содержат собственную бизнес-логику.

---

# 2. Миссия Business OS

Помогать клиентам достигать своих целей — путешествий, обучения, работы или международной мобильности — за счет прозрачных процессов, системного подхода и автоматизации.

Business OS берет на себя организационную сложность, позволяя сотрудникам сосредоточиться на качестве сервиса, а клиентам — на результате.

---

# 3. Видение (Vision)

Построить масштабируемую цифровую платформу, объединяющую:

- сопровождение клиентов;
- коммерческий каталог;
- выполнение услуг;
- финансы;
- знания компании;
- автоматизацию;
- аналитику;
- AI-помощников.

Business OS становится единым цифровым ядром компании, вокруг которого строятся все остальные сервисы.

---

# 4. Главный архитектурный принцип

## Business OS — единственный Source of Truth

Любая бизнес-информация существует только в одном месте.

Все остальные системы получают данные через API, автоматизации или интеграции и не создают собственных независимых копий бизнес-данных.

---

# 5. Source of Truth

Внутри Business OS находятся:

- Requests
- Clients
- Cases
- Product Catalog
- Service Catalog
- Workflow Templates
- Financial Transactions
- Knowledge Base
- Analytics

Website, Telegram, CRM-интерфейсы, AI и внешние сервисы работают с этими данными, но не являются их владельцами.

---

# 6. Границы системы

## Внутри Business OS

- CRM
- Catalog
- Operations
- Finance
- Knowledge Base
- Marketing
- Analytics
- AI Layer
- Automation Layer

---

## Внешние системы

- Website
- Telegram
- WhatsApp
- Email
- Instagram
- Threads
- VK
- Google
- Google Maps
- Партнерские сервисы
- Государственные порталы
- Платежные системы

# 7. Домены Business OS

## Knowledge Domain

### Purpose

Knowledge Domain хранит экспертные знания компании.

Он является единственным источником информации о странах, визовых программах, требованиях, правилах оформления документов и внутренних инструкциях.

Изменения в законодательстве или требованиях консульств отражаются именно здесь.

---

### Contains

- Countries
- Cities
- Visa Programs
- Embassies
- Visa Requirements
- Document Templates
- Knowledge Articles
- FAQ

---

## CRM Domain

### Purpose

CRM Domain управляет отношениями с клиентами.

Он отвечает за регистрацию обращений, ведение клиентов, создание Case и сопровождение взаимодействия с момента первого контакта до завершения обслуживания.

---

### Contains

- Requests
- Clients
- Cases
- Managers
- Communications

---

## Catalog Domain

### Purpose

Catalog Domain управляет коммерческой моделью компании.

Он определяет, какие продукты и услуги предоставляет компания, какие Workflow используются для их выполнения и какие коммерческие предложения доступны клиентам.

---

### Contains

- Products
- Services
- Workflow Templates

---

## Operations Domain

### Purpose

Operations Domain управляет выполнением услуг.

Он отвечает за прохождение Case по Workflow, управление документами, взаимодействие с партнерами и выполнение всех операционных действий, необходимых для оказания услуги.

---

### Business Questions

На какие вопросы отвечает этот домен?

- На каком этапе находится Case?
- Какие этапы уже завершены?
- Какие документы уже получены?
- Какие документы еще ожидаются?
- Какие встречи или подачи назначены?
- Какие действия выполняют партнеры?
- Как выглядит история прохождения Workflow?

---

### Contains

- Workflow Execution
- Workflow Stages
- Workflow History
- Case Documents
- Files
- Appointments
- External Partners

---

### Does NOT contain

- Clients
- Requests
- Products
- Services
- Financial Transactions
- Knowledge Base
- Marketing

---

## Finance Domain

### Purpose

Finance Domain управляет всеми финансовыми операциями компании.

Он хранит как доходы, так и расходы, обеспечивая полную финансовую картину каждого Case.

---

### Contains

- Financial Transactions

---

### Business Rules

- Один Case может содержать любое количество Financial Transactions.
- Каждая Financial Transaction относится к одному Case.
- Финансовые операции являются неизменяемыми (Append-only).
- Исправление ошибок выполняется новой Financial Transaction, а не изменением существующей записи.

---

## Marketing Domain

### Purpose

Marketing Domain отвечает за привлечение клиентов и анализ эффективности маркетинговых каналов.

---

### Contains

- Website
- SEO
- Campaigns
- UTM
- Content
- Social Media
- Reviews

---

## Automation Domain

### Purpose

Automation Domain объединяет все механизмы автоматизации Business OS.

---

### Contains

- Notion
- Make
- n8n
- FastAPI
- Telegram Bot
- Email
- AI Integrations

---

## Analytics Domain

### Purpose

Analytics Domain предоставляет руководству объективную картину работы компании.

---

### Contains

- KPI
- Revenue
- Conversion
- Funnel
- Dashboards
- Business Metrics

---

# 8. Центральная сущность

## Case

Главным объектом Business OS является **Case**.

Case представляет конкретную услугу, оказываемую одному Client.

Все остальные бизнес-сущности существуют либо для создания Case, либо для его сопровождения.

### Основные принципы

- Один Client может иметь множество Case.
- Один Request может породить несколько Case.
- Каждый Case относится только к одному Client.
- Каждый Case имеет собственного Manager.
- Каждый Case проходит собственный Workflow.
- Каждый Case содержит собственную финансовую историю.

# 9. Жизненный цикл клиента

Business OS сопровождает клиента на протяжении всего жизненного цикла.

```text
Request
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
Case Created
    │
    ▼
Workflow Execution
    │
    ▼
Case Completed
    │
    ▼
Repeat Request
```

Request и Case являются разными бизнес-сущностями.

Request отражает интерес потенциального клиента.

Case создается только после возникновения коммерческих отношений:

- принятия публичной оферты;
- первой коммерческой оплаты.

---

# 10. Три уровня Business OS

## Knowledge

Что знает компания.

- страны;
- визовые программы;
- требования;
- инструкции;
- шаблоны документов;
- база знаний.

---

## Operations

Что делает компания.

- сопровождение Case;
- выполнение Workflow;
- управление документами;
- взаимодействие с партнерами;
- коммуникации;
- встречи и подачи.

---

## Intelligence

Что помогает компании принимать решения.

- AI;
- аналитика;
- автоматизация;
- прогнозирование;
- рекомендации.

---

# 11. Архитектурные принципы

## 1. Business First

Сначала проектируется бизнес.

Потом данные.

Потом процессы.

И только потом технологии.

---

## 2. Один объект — один Source of Truth

Каждая бизнес-сущность имеет единственного владельца данных.

---

## 3. Case — центральная сущность системы

Все процессы компании существуют для создания, сопровождения и успешного завершения Case.

---

## 4. Workflow управляет выполнением услуги

Каждый Case проходит Workflow.

Workflow определяется Workflow Template, связанным с Product.

---

## 5. Любая автоматизация работает вокруг Business OS

Автоматизации не содержат собственную бизнес-логику.

Они используют данные Business OS.

---

## 6. Любой внешний сервис заменяем

Замена сайта, Telegram, Notion или любого другого инструмента не должна требовать изменения архитектуры компании.

---

# 12. AI Layer

AI является частью Business OS.

Он помогает сотрудникам выполнять работу быстрее и качественнее, но не заменяет архитектуру бизнеса.

Примеры использования:

- проверка документов;
- анализ кейсов;
- помощь менеджерам;
- генерация ответов клиентам;
- подготовка SEO-контента;
- создание инструкций;
- аналитика и рекомендации.

---

# 13. Automation Layer

Любое изменение бизнес-состояния может запускать автоматизацию.

Пример:

```text
Workflow Stage изменился

        │

        ▼

Отправлено уведомление клиенту

        │

        ▼

Создано внутреннее уведомление

        │

        ▼

Обновлена аналитика

        │

        ▼

AI подготовил рекомендации
```

Automation Layer не хранит бизнес-данные.

Он реагирует на события Business OS.

---

# 14. Технологический стек

## Backend

- FastAPI

## Frontend

- Next.js
- React

## Business OS

- Notion

## Database (будущий этап)

- PostgreSQL

## Automation

- Make
- n8n

## AI

- OpenAI
- Claude
- MCP

---

# 15. Roadmap

## Этап 1

Business Discovery

## Этап 2

Business Architecture

## Этап 3

Business Information Model

## Этап 4

Business OS (Notion)

## Этап 5

API

## Этап 6

Frontend

## Этап 7

Automation

## Этап 8

AI Layer

---

# 16. Методология разработки

V!BE Business OS проектируется сверху вниз.

```text
Business Discovery

        ↓

Business Architecture

        ↓

Domain Driven Design

        ↓

Business Information Model

        ↓

Business OS

        ↓

API

        ↓

Backend

        ↓

Frontend

        ↓

Automation

        ↓

AI
```

Архитектурные решения принимаются до начала реализации.

Все принятые решения фиксируются в Architecture Decision Records (ADR).

---

# 17. Следующие этапы проектирования

После завершения Business Architecture будут разработаны:

- Business Information Model (детализация сущностей);
- ER-модель;
- State Machine для Case;
- Event Model;
- API Contracts;
- AI Agents;
- Dashboard Architecture;
- Security Model;
- Permission Model;
- Automation Rules;
- Integration Architecture.

---

# 18. Статус документа

**Status:** 🟢 Approved

Документ определяет архитектурный фундамент V!BE Business OS.

Все последующие инженерные решения должны соответствовать принципам, описанным в данном документе.