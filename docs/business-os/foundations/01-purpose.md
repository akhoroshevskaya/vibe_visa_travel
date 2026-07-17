# 1. Purpose

**Статус:** Approved

---

# Назначение

Business OS — это операционный слой компании, который превращает утвержденную Business Architecture в ежедневно используемую цифровую рабочую среду.

Business OS объединяет:

- людей;
- бизнес-процессы;
- операционные данные;
- автоматизации;
- AI-компоненты;
- рабочие интерфейсы.

Business OS определяет **как компания работает каждый день**, оставаясь независимой от конкретной технологической платформы.

---

# Место Business OS в архитектуре

Business Architecture описывает:

> **Как устроен бизнес.**

Business OS описывает:

> **Как этот бизнес функционирует ежедневно.**

Business OS реализует утвержденную Business Architecture и не изменяет ее.

```text
Business Vision
        │
        ▼
Business Architecture
        │
        ▼
Business OS
        │
        ▼
Technology Implementation
```

---

# Scope

## Входит в Business OS

- Operational Repository
- Business Data
- Views
- Templates
- Automations
- Dashboards
- AI Components
- Navigation
- Permissions

---

## Не входит в Business OS

Business OS не определяет:

- бизнес-стратегию;
- организационную структуру компании;
- бизнес-домены;
- бизнес-сущности;
- жизненные циклы Business Entity.

Эти решения принимаются на уровне Business Architecture.

---

# Design Goal

Основная цель Business OS — предоставить единую операционную среду, в которой все участники компании работают с одним источником данных, едиными процессами и согласованными правилами.

Business OS должна обеспечивать:

- прозрачность процессов;
- единообразие данных;
- повторное использование компонентов;
- масштабируемость;
- независимость от технологической платформы;
- эволюционное развитие архитектуры.

---

# Architecture Principles

При проектировании Business OS используются принципы, определенные в документе:

> **02-design-principles.md**

Purpose определяет назначение Business OS.

Все архитектурные решения принимаются на основе Design Principles.