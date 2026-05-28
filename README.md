🎡 Event Management API — Площадка «Колесо обозрения»

Портфолио-проект · Бизнес-аналитик · Практика Docs-as-Code

Живая документация →

📌 Проблема
VIP-площадка на колесе обозрения принимала до 8 одновременных мероприятий в день — свадьбы, дни рождения, туристические делегации. Вся координация велась через WhatsApp-чаты и рукописные заметки.
Последствия были предсказуемы:

2–3 конфликта при бронировании кабинок в день в пиковый сезон
Сотрудников записывали на несколько мероприятий одновременно — система это не отслеживала
Единого источника правды не было — каждый координатор вёл свою версию расписания
Из-за одной ошибки в расписании VIP-церемония бракосочетания началась на 40 минут позже

Эта API-спецификация — формализация операционной реальности: то, что действительно было нужно бизнесу, переведённое в структурированный, версионируемый контракт.

🎯 О чём этот проект
Спецификация OpenAPI 3.0, охватывающая полный жизненный цикл мероприятия — от первичного бронирования до завершения — со встроенным автоматическим обнаружением конфликтов.
Жизненный цикл мероприятия:
DRAFT → CONFIRMED → IN_PROGRESS → COMPLETED
              ↓
          CANCELLED
🔍 Группы эндпоинтов
ГруппаОперацийОписаниеEvents5Создание бронирований, управление полным жизненным цикломCabins2Проверка доступности, назначение с защитой от конфликтов (409)Services2Добавление кейтеринга, декора, фото, оборудованияStaff2Назначение ролей с указанием времени брифингаSchedule2Поминутное расписание + автоматическое обнаружение конфликтов

📂 Структура репозитория
.
├── docs/
│   └── event-management-api.yaml   # Спецификация OpenAPI 3.0
├── index.html                      # Живая документация на GitHub Pages
└── README.md
🚀 Просмотр интерактивной документации

Откройте editor.swagger.io
Нажмите File → Import file
Выберите docs/event-management-api.yaml

Или перейдите напрямую на страницу живой документации.

🛠 Продемонстрированные навыки

OpenAPI 3.0 — проектирование REST API: эндпоинты, схемы, примеры запросов/ответов, обработка ошибок
Docs-as-Code — документация как код: версионирование в Git, структура репозитория, деплой на GitHub Pages
Моделирование требований — декомпозиция реального операционного процесса в сущности и операции
Управление данными — модель жизненного цикла статусов, валидация входных данных, логика обнаружения конфликтов
Кросс-функциональная координация — многосторонние процессы (клиенты, кейтеринг, фотографы, инженеры), формализованные как API-контракт
Бизнес-анализ — выявление первопричин операционных сбоев и перевод их в системные требования


👤 Автор
Karolina Gergert — Бизнес-аналитик
Опыт: управление клиентскими отношениями, документирование требований (BPMN, User Story Map, OpenAPI)
📍 Буэнос-Айрес, Аргентина
linkedin.com/in/karolina-gergert-6176b2401
# 🎡 Event Management API — Ferris Wheel Venue

> **Portfolio project** · Business Analyst · Docs-as-Code practice

[**Live Documentation →**](https://lina443.github.io/event-management-api)


## 📌 The Problem

A Ferris wheel venue hosted up to **8 simultaneous VIP events per day** — weddings, birthday celebrations, and tourist delegations. Coordination ran entirely through WhatsApp chats and handwritten notes.

The results were predictable:

- 2–3 cabin booking conflicts per day during peak season
- Staff double-booked across simultaneous events with no system to catch it
- No single source of truth — each coordinator maintained their own version of the schedule
- One scheduling error caused a VIP wedding ceremony to start 40 minutes late

This API specification is a formalization of that operational reality: what the business actually needed, translated into a structured, version-controlled contract.


## 🎯 What This Project Does

An OpenAPI 3.0 specification covering the full event lifecycle — from initial client booking to post-event completion — with automatic conflict detection built in.

**Event Lifecycle:**

```
DRAFT → CONFIRMED → IN_PROGRESS → COMPLETED
              ↓
          CANCELLED
```

## 🔍 Endpoint Groups

|Group       |Operations|Description                                             |
|------------|----------|--------------------------------------------------------|
|**Events**  |5         |Create bookings, manage full lifecycle                  |
|**Cabins**  |2         |Availability check, assignment with conflict guard (409)|
|**Services**|2         |Add catering, decoration, photography, equipment        |
|**Staff**   |2         |Assign roles with briefing time                         |
|**Schedule**|2         |Minute-by-minute timeline + automatic conflict detection|


## 📂 Repository Structure

```
.
├── docs/
│   └── event-management-api.yaml   # OpenAPI 3.0 specification
├── index.html                      # GitHub Pages live documentation
└── README.md
```

## 🚀 View Interactive Docs
1. Open [editor.swagger.io](https://editor.swagger.io)
2. Click **File → Import file**
3. Select `docs/event-management-api.yaml`

Or visit the [live documentation page](https://lina443.github.io/event-management-api) directly.

## 🛠 Skills Demonstrated
- **OpenAPI 3.0** — REST API design: endpoints, schemas, request/response examples, error handling
- **Docs-as-Code** — documentation as code: Git versioning, structured repository layout, GitHub Pages deployment
- **Requirements Modeling** — decomposing a real operational process into entities and operations
- **Data Governance** — status lifecycle model, input validation, conflict detection logic
- **Cross-functional Coordination** — multi-stakeholder workflows (clients, catering, photographers, engineers) formalized as an API contract
- **Business Analysis** — identified root causes of operational failures and translated them into system requirements

## 👤 Author
**Karolina Gergert** — Business Analyst
Background: client relationship management, requirements documentation (BPMN, User Story Map, OpenAPI)
[linkedin.com/in/karolina-gergert-6176b2401](https://linkedin.com/in/karolina-gergert-6176b2401)