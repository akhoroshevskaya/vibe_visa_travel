# 2. Data Architecture

**Статус:** Approved

---

# Назначение

Data Architecture определяет правила организации операционных данных Business OS.

Документ описывает архитектурную модель данных, используемую всеми Operational Repository, независимо от их назначения и технологической реализации.

---

# Цели Data Architecture

Data Architecture обеспечивает:

- единообразие структуры данных;
- повторное использование Property;
- независимость данных от платформы;
- единый словарь данных;
- масштабируемость Repository;
- архитектурную целостность Business OS.

---

# Data Principles

Data Architecture строится на следующих принципах:

- Property определяются один раз.
- Repository используют существующие Property Definition.
- Property принадлежат общему каталогу.
- Repository не создают собственные локальные Property.
- Каждая Property имеет уникальный идентификатор.
- Каждая Property имеет единственное архитектурное определение.

---

# Data Model

Business OS использует трехуровневую модель данных.

```text
Property Category
        │
        ▼
Property Definition
        │
        ▼
Property Usage
```

---

# Property Category

Property Category объединяет Property по назначению.

Категория используется исключительно для организации каталога.

Категория не влияет на поведение Property.

Примеры:

- Common
- Relations
- System
- Computed
- Integration

---

# Property Definition

Property Definition описывает архитектурное определение свойства.

Property Definition является глобальным объектом Business OS.

Каждая Definition создается один раз и может использоваться многими Repository.

---

# Property Usage

Property Usage определяет использование Property Definition внутри конкретного Repository.

Usage не изменяет Definition.

Usage определяет только правила использования свойства в данном Repository.

---

# Property Life Cycle

Каждая Property проходит следующий жизненный цикл.

```text
Draft
    │
    ▼
Review
    │
    ▼
Approved
    │
    ▼
Deprecated
    │
    ▼
Archived
```

---

# Business Data Dictionary

Business Data Dictionary является единым словарем данных Business OS.

Dictionary содержит:

- Property Definition;
- архитектурные описания;
- правила использования;
- допустимые значения;
- связи с Business Entity.

---

# Property Catalog

Property Catalog представляет перечень всех утвержденных Property Definition.

Каталог ведется отдельно и используется всеми Repository.

---

# Repository Integration

Repository используют Property посредством Property Usage.

Repository запрещается изменять Property Definition.

Изменение Property Definition автоматически распространяется на все Repository, использующие данное свойство.

---

# Technology Independence

Data Architecture не зависит от возможностей конкретной платформы.

Notion Properties, SQL Columns, Airtable Fields или другие механизмы рассматриваются исключительно как технологическая реализация Property Definition.