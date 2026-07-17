# REP-002 — Clients Repository

**Статус:** Approved

**Repository ID:** REP-002

**Business Entity:** BE-002 Client

---

# 1. Purpose

Clients Repository является единым справочником клиентов Business OS.

Repository хранит постоянные сведения о физических и юридических лицах, взаимодействующих с компанией.

Repository обеспечивает повторное использование информации о клиентах во всех обращениях, кейсах и услугах.

---

# 2. Repository Definition

Repository реализует Business Entity **Client**.

Каждая запись представляет одного клиента.

Клиент может иметь неограниченное количество Request, Case, Payment и Document.

Repository является единственным источником информации о клиенте.

---

# 3. Repository Responsibilities

Repository отвечает за:

- хранение персональных данных клиента;
- хранение контактной информации;
- предотвращение дублирования клиентов;
- поддержку связей с другими Repository;
- предоставление информации другим компонентам Business OS.

Repository не отвечает за:

- обращения;
- оформление услуг;
- документы по конкретному кейсу;
- финансовые операции;
- рабочие процессы.

---

# 4. Supported Business Entity

| Entity | ID |
|----------|-----|
| Client | BE-002 |

---

# 5. Repository Properties

## Common

- PD-001 Created At
- PD-002 Updated At
- PD-004 Owner

## Client Identity

- Full Name
- Date of Birth
- Nationality
- Gender

## Contacts

- Phone
- Email
- Telegram
- WhatsApp

## Relations

- Requests
- Cases
- Documents
- Payments

## Computed

- Active Requests
- Active Cases
- Last Contact
- Client Since

---

# 6. Property Usage Matrix

Все Property определяются через утвержденные Property Definition.

Repository использует только глобальный каталог Property.

---

# 7. Repository Relations

| Repository | Relationship |
|-------------|--------------|
| Requests | referenced by |
| Cases | referenced by |
| Documents | referenced by |
| Payments | referenced by |

---

# 8. Lifecycle Integration

```text
Created
      │
      ▼
Active
      │
      ▼
Inactive
      │
      ▼
Archived
```

Repository хранит сведения о клиенте независимо от наличия активных обращений.

---

# 9. Views

Минимальный набор представлений:

- All Clients
- Active Clients
- New Clients
- VIP Clients
- Clients by Country
- Clients without Active Requests

---

# 10. Templates

- Individual
- Family
- Corporate Client

---

# 11. Automations

- Создание клиента при первом обращении.
- Поиск дублей.
- Обновление контактной информации.
- Объединение дублирующихся записей.
- Обновление даты последнего взаимодействия.

---

# 12. Dashboards

Repository используется следующими Dashboard:

- Client Base
- New Clients
- Returning Clients
- Client Geography
- Client Retention

---

# 13. AI Integration

AI может:

- выявлять возможные дубли;
- нормализовать имена;
- проверять полноту данных;
- рекомендовать объединение записей;
- определять отсутствующие контактные данные.

AI не изменяет Repository без подтверждения пользователя.

---

# 14. Permissions

| Role | Access |
|------|--------|
| Administrator | Full |
| Manager | Read / Write |
| Sales | Read / Write |
| AI | Read |
| Automation | Limited Write |

---

# 15. Architectural Constraints

Clients Repository:

- реализует только Business Entity Client;
- не хранит информацию о Request;
- не хранит информацию о Case;
- не хранит документы;
- не хранит платежи;
- является единственным источником данных о клиенте.

---

# 16. Future Evolution

Возможные направления развития:

- клиентские сегменты;
- история коммуникаций;
- предпочтения клиента;
- оценка лояльности;
- интеграция с внешними CRM;
- единый Client Profile;
- поддержка нескольких гражданств;
- семейные связи между клиентами.