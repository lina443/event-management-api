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