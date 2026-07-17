# 99 Business OS Backlog

Статус: Active

---

# Назначение

Business OS Backlog содержит архитектурные идеи, предложения и улучшения, относящиеся к проектированию Business OS.

Записи в Backlog не являются частью утвержденной архитектуры и не влияют на текущую реализацию до момента принятия отдельного архитектурного решения.

Backlog используется для сохранения идей без нарушения принципа Architecture Freeze.

---

# Статусы

- Proposed
- Deferred
- Approved
- Rejected
- Implemented

---

# BUS-OS-001

## Knowledge Repository Decomposition

**Статус:** Deferred

### Описание

Оценить необходимость разделения Knowledge Repository на несколько специализированных Operational Repository.

Возможные варианты:

- Knowledge Articles Repository
- SOP Repository
- Templates Repository
- AI Prompts Repository

### Причина

Определить оптимальный уровень детализации Knowledge Domain без изменения утвержденной Business Architecture.

### Планируемый этап

Repository Properties

---

# BUS-OS-002

## Repository Naming Convention

**Статус:** Proposed

### Описание

Разработать единый стандарт именования Operational Repository.

Необходимо определить правила:

- единственное или множественное число;
- использование суффикса Repository;
- отображаемое имя;
- техническое имя;
- имя базы Notion.

### Планируемый этап

Repository Properties

---

# BUS-OS-003

## Repository Icons & Visual Language

**Статус:** Proposed

### Описание

Разработать единый визуальный стандарт Business OS.

Необходимо определить:

- иконки Repository;
- цветовую систему Domain;
- обозначение типов Repository;
- правила визуальной навигации.

### Планируемый этап

Navigation & Dashboards

---

# BUS-OS-004

## Design Standards Document

**Статус:** Proposed

### Описание

При накоплении достаточного количества архитектурных стандартов создать отдельный документ:

`02-business-os-design-standards.md`

Документ будет содержать единые правила проектирования Business OS.

### Планируемый этап

После завершения Repository Specification