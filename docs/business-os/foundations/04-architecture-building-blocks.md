# 4. Architecture Building Blocks

**Статус:** Approved

---

# Назначение

Architecture Building Blocks определяет основные архитектурные объекты Business OS.

Документ формирует единый архитектурный словарь проекта и устанавливает роли каждого объекта в общей архитектуре.

Все последующие документы используют определения, приведенные в данном разделе.

---

# Архитектурная модель

```text
Business Architecture
        │
        ▼
Business Entity
        │
        ▼
Architecture Building Blocks
        │
        ├── Repository
        ├── View
        ├── Template
        ├── Automation
        ├── Dashboard
        ├── AI Component
        ├── Navigation
        └── Permission
```

---

# Business Entity

Business Entity представляет объект предметной области компании.

Business Entity существует независимо от технологии реализации.

Примеры:

- Request
- Client
- Case
- Workflow

---

# Repository

Repository является операционной реализацией одной Business Entity.

Repository является единственным источником операционных данных для данной сущности.

---

# View

View представляет данные Repository в форме, удобной для выполнения конкретной рабочей задачи.

View не владеет данными.

---

# Template

Template определяет стандартный способ создания новых экземпляров Business Entity.

Template обеспечивает единообразие структуры данных.

---

# Automation

Automation реализует повторяемые бизнес-правила.

Automation выполняет операции над Repository в соответствии с утвержденной архитектурой.

---

# Dashboard

Dashboard агрегирует информацию из нескольких Repository для поддержки принятия решений.

Dashboard не изменяет данные.

---

# AI Component

AI Component предоставляет интеллектуальные функции Business OS.

AI анализирует данные, формирует рекомендации и помогает пользователям выполнять работу.

---

# Navigation

Navigation определяет способы перехода между компонентами Business OS.

Navigation организует пользовательский опыт, но не влияет на бизнес-данные.

---

# Permission

Permission определяет правила доступа пользователей к архитектурным объектам Business OS.

Permission применяется независимо от технологической реализации.

---

# Взаимодействие объектов

```text
Business Entity
        │
        ▼
Repository
        │
 ┌──────┼─────────────────────────┐
 ▼      ▼          ▼              ▼
View Template Automation Dashboard
 │       │          │              │
 └───────┼──────────┴──────────────┘
         ▼
     AI Component
```

Repository является центральным архитектурным объектом Business OS.

Все остальные компоненты взаимодействуют с данными через Repository.

---

# Архитектурные правила

- Каждый Building Block имеет одну ответственность.
- Каждый Building Block развивается независимо.
- Building Blocks взаимодействуют только через утвержденные архитектурные интерфейсы.
- Business Entity не зависит от Repository.
- Repository не зависит от конкретной платформы.
- View, Dashboard, Automation и AI не являются источниками данных.