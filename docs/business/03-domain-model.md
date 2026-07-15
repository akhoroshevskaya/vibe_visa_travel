> **Architecture Note**
>
> Документ является частью методологии V!BE Business OS.
>
> Его цель — описать принципы проектирования цифровых компаний, а не только реализацию конкретного проекта V!BE Visa & Travel.

# Domain Model

---

# Knowledge Domain

## Purpose

Knowledge Domain — это корпоративная память компании.

Здесь хранится вся информация, необходимая для принятия решений, консультирования клиентов и оказания услуг компании.

Этот домен не зависит от конкретных клиентов или текущих дел. Он содержит знания, которые многократно используются сотрудниками, AI-ассистентами, сайтом и автоматизациями.

Каждый успешно завершенный кейс должен делать компанию умнее.

---

## Business Question

На какие вопросы отвечает этот домен?

- Какие визы существуют?
- Какие документы необходимы?
- Какие требования предъявляет конкретная страна?
- Какие консульства принимают документы?
- Какие сроки оформления?
- Какие услуги оказывает компания?
- Какие правила действуют сегодня?

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

- Service Catalog
- FAQ
- Knowledge Articles
- Service Playbooks

### Templates

- Document Templates
- Letter Templates

---

## Does NOT contain

- Clients
- Cases
- Tasks
- Payments
- Communications
- Marketing Campaigns
- Analytics

---

## Source of Truth

Все нормативные знания компании хранятся только в Knowledge Domain.

Любая другая часть системы использует эти данные через связи или API, но не создает собственные копии.

---

## Used by

- CRM
- Website
- AI Assistants
- Telegram Bot
- Менеджеры
- Автоматизации
- Email Templates

# CRM Domain

## Purpose

CRM Domain управляет взаимоотношениями компании с клиентами на протяжении всего жизненного цикла сотрудничества.

Он хранит информацию о клиентах, их обращениях, текущих делах, истории взаимодействия и ответственных сотрудниках.

CRM Domain отвечает не за выполнение услуг, а за понимание того, **кому**, **что** и **на каком этапе** оказывает компания.

---

## Business Question

На какие вопросы отвечает этот домен?

- Кто наш клиент?
- Какие услуги ему необходимы?
- Какие дела открыты?
- Кто отвечает за сопровождение?
- На каком этапе находится каждое дело?
- Какова история взаимодействия с клиентом?
- Какие обращения уже были?

---

## Contains

- 🟡 Leads *(Under Discussion)*
- 🟢 Clients
- 🟢 Cases
- 🟢 Managers
- 🟢 Communications
- 🟢 Contact Persons
- 🟢 Client Notes

---

## Open Decisions

### 🟡 CRM-001 — Нужна ли сущность Lead?

**Status:** Under Discussion

**Question**

Создается ли Case сразу после первого обращения клиента,
или сначала существует отдельная сущность Lead?

**Options**

**Option A**

Lead → Case → Client

**Option B**

Case создается сразу после первого обращения.

**Decision**

Будет принято после проектирования Customer Journey.
---

## Does NOT contain

- Visa Requirements
- Countries
- Embassies
- Checklists
- Templates
- Payments
- Analytics

---

## Source of Truth

CRM Domain является единственным источником информации о клиентах, их делах и истории взаимодействия.

---

## Used by

- Website
- Telegram Bot
- Managers
- Operations
- AI Assistants
- Analytics

---

# Open Decisions

| ID | Decision | Status |
|----|----------|--------|
| CRM-001 | Нужна ли сущность Lead | 🟡 Under Discussion |
| CRM-002 | Может ли один Case иметь нескольких клиентов | 🟡 Under Discussion |