> **Architecture Note**
>
> Документ является частью методологии V!BE Business OS.
>
> Его цель — определить бизнес-домены компании, их ответственность, границы и взаимодействие.
>
> Каждый домен является самостоятельной областью бизнеса со своим языком, правилами и Source of Truth.
>
> Документ не описывает структуру данных или реализацию. Эти вопросы рассматриваются в Business Information Model и на этапе Engineering Sprint.

# V!BE Business OS

# Business Domain Model v1.0

**Version:** 1.0

**Status:** Approved

**Owner:** Aleksandra

**Project:** V!BE Visa & Travel

---

# 1. Purpose

Business Domain Model определяет основные домены V!BE Business OS.

Каждый домен представляет самостоятельную область ответственности бизнеса.

Домены взаимодействуют между собой через бизнес-сущности, события и связи, не дублируя ответственность друг друга.

Главная цель модели — обеспечить единый язык предметной области (Ubiquitous Language) и четкое разделение ответственности между частями Business OS.

---

# 2. Business Domains

| Domain | Responsibility |
|----------|----------------|
| Knowledge | Корпоративные знания компании |
| CRM | Клиенты, Requests и Cases |
| Catalog | Коммерческий каталог компании |
| Operations | Выполнение услуг |
| Finance | Финансовые операции |
| Automation | Автоматизация процессов |
| Analytics | Аналитика бизнеса |

---

# 3. Knowledge Domain

## Purpose

Knowledge Domain является корпоративной памятью компании.

Он хранит экспертные знания, необходимые для консультирования клиентов, оказания услуг и принятия решений.

Knowledge Domain не зависит от конкретных клиентов или отдельных Case.

Все знания должны многократно использоваться сотрудниками, AI, сайтом и автоматизациями.

Каждый успешно завершенный Case должен расширять Knowledge Domain.

---

## Business Questions

На какие вопросы отвечает домен?

- Какие существуют визовые программы?
- Какие требования предъявляет конкретная страна?
- Какие документы необходимы?
- Какие правила подачи действуют сейчас?
- Какие консульства принимают документы?
- Какие сроки оформления?
- Какие внутренние инструкции существуют?

---

## Contains

### Geography

- Countries
- Cities

### Visa Knowledge

- Visa Programs
- Visa Categories
- Visa Requirements
- Required Documents
- Visa Processing Rules

### Organizations

- Embassies
- Consulates
- Visa Centers

### Company Knowledge

- Knowledge Articles
- FAQ
- Service Playbooks

### Templates

- Document Templates
- Letter Templates

---

## Does NOT contain

- Requests
- Clients
- Cases
- Financial Transactions
- Communications
- Marketing Campaigns
- Analytics

---

## Source of Truth

Knowledge Domain является единственным источником корпоративных знаний компании.

Никакой другой домен не хранит собственные копии нормативной информации.

---

## Used by

- CRM
- Catalog
- Website
- AI Assistants
- Automation
- Telegram Bot
- Managers

# 4. CRM Domain

## Purpose

CRM Domain управляет взаимоотношениями компании с клиентами на протяжении всего жизненного цикла сотрудничества.

Он отвечает за регистрацию обращений, сопровождение клиентов, создание Case и хранение истории взаимодействия.

CRM Domain определяет **кто является клиентом**, **какие обращения поступили**, **какие Case существуют** и **кто отвечает за сопровождение**.

CRM Domain не отвечает за выполнение услуг — это ответственность Operations Domain.

---

## Business Questions

На какие вопросы отвечает домен?

- Кто является нашим клиентом?
- Какие обращения поступали?
- Какие Case открыты?
- Кто является ответственным Manager?
- Какие услуги оказываются клиенту?
- Какова история взаимодействия?
- Какие Case уже были успешно завершены?

---

## Contains

- Requests
- Clients
- Cases
- Managers
- Communications
- Client Notes

---

## Does NOT contain

- Workflow
- Workflow Templates
- Documents
- Financial Transactions
- Knowledge Articles
- Products
- Services

---

## Source of Truth

CRM Domain является единственным источником информации о клиентах, обращениях, Case и истории взаимоотношений.

---

## Used by

- Operations
- Finance
- Website
- Telegram Bot
- AI Assistants
- Analytics

---

# 5. Catalog Domain

## Purpose

Catalog Domain управляет коммерческой моделью компании.

Он определяет:

- какие продукты предоставляет компания;
- какие услуги входят в продукты;
- какой Workflow используется;
- какие коммерческие предложения доступны клиентам.

Catalog Domain не содержит данных клиентов или выполнения услуг.

---

## Business Questions

На какие вопросы отвечает домен?

- Какие продукты предлагает компания?
- Какие услуги входят в продукт?
- Какой Workflow используется?
- Какие услуги являются дополнительными?
- Какие продукты доступны сейчас?

---

## Contains

### Products

- Schengen Visa
- UK Visa
- USA Visa
- Canada Visa
- Australia Visa
- Tours
- Travel Concierge
- Foreign Bank Card
- APEC Card

### Services

- Visa Support
- Appointment Booking
- Insurance
- Hotel Booking
- Flight Booking
- Translation
- Notary
- Courier
- Passport Delivery
- Travel Planning

### Workflow

- Workflow Templates

---

## Does NOT contain

- Clients
- Requests
- Cases
- Payments
- Communications

---

## Source of Truth

Catalog Domain является единственным источником информации о коммерческих предложениях компании.

Все остальные домены используют каталог через связи.

---

## Used by

- CRM
- Operations
- Website
- AI Assistants
- Automation

# 6. Operations Domain

## Purpose

Operations Domain отвечает за выполнение услуг компании.

Он управляет прохождением каждого Case по Workflow, обработкой документов, взаимодействием с партнерами и выполнением всех операционных действий.

---

## Business Questions

На какие вопросы отвечает домен?

- На каком этапе находится Case?
- Какие этапы уже завершены?
- Какие документы уже получены?
- Какие документы ожидаются?
- Какие встречи назначены?
- Какие действия выполняют партнеры?
- Как выглядит история выполнения Workflow?

---

## Contains

- Workflow Execution
- Workflow Stages
- Workflow History
- Case Documents
- Files
- Appointments
- External Partners

---

## Does NOT contain

- Clients
- Requests
- Products
- Services
- Financial Transactions
- Knowledge Articles

---

## Source of Truth

Operations Domain является единственным источником информации о выполнении услуг и прохождении Workflow.

---

## Used by

- CRM
- Finance
- AI Assistants
- Automation
- Analytics

---

# 7. Finance Domain

## Purpose

Finance Domain управляет всеми финансовыми операциями компании.

Он хранит полную финансовую историю каждого Case и обеспечивает построение управленческой отчетности.

---

## Business Questions

На какие вопросы отвечает домен?

- Какие услуги оплачены?
- Какие расходы понесены?
- Какова прибыль по Case?
- Какие платежи ожидаются?
- Какие операции требуют возврата?

---

## Contains

- Financial Transactions

---

## Business Rules

- Один Case может содержать любое количество Financial Transactions.
- Каждая Financial Transaction относится только к одному Case.
- Финансовые операции являются неизменяемыми (Append-only).
- Исправления выполняются новой Financial Transaction.

---

## Does NOT contain

- Clients
- Products
- Documents
- Knowledge

---

## Source of Truth

Finance Domain является единственным источником финансовых данных компании.

---

## Used by

- CRM
- Analytics
- AI
- Automation

---

# 8. Automation Domain

## Purpose

Automation Domain обеспечивает автоматическое выполнение бизнес-процессов.

Он не хранит бизнес-данные, а реагирует на события Business OS.

---

## Contains

- Notion
- Make
- n8n
- FastAPI
- Telegram Bot
- Email
- AI Integrations

---

## Used by

Все бизнес-домены.

---

# 9. Analytics Domain

## Purpose

Analytics Domain предоставляет руководству объективную картину работы компании.

---

## Contains

- KPI
- Revenue
- Conversion
- Funnel
- Dashboards
- Business Metrics

---

## Source of Truth

Analytics строится на данных остальных доменов и не создает собственных бизнес-сущностей.

---

# 10. Взаимодействие доменов

```text
                Knowledge
                     │
                     ▼
Catalog ───────► CRM ───────► Operations
                    │               │
                    │               ▼
                    ├────────► Finance
                    │
                    ├────────► Automation
                    │
                    └────────► Analytics
```

---

# 11. Архитектурные принципы

- Каждый домен имеет собственную ответственность.
- Каждый домен является Source of Truth только для своих данных.
- Домены взаимодействуют через связи и события.
- Дублирование ответственности между доменами запрещено.
- Любой новый функционал сначала относится к существующему домену и только при невозможности — создается новый.

---

# 12. Статус документа

**Status:** 🟢 Approved

Business Domain Model определяет границы ответственности всех доменов V!BE Business OS.

Все последующие архитектурные и инженерные решения должны соответствовать данной модели.