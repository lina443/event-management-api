# 🎡 Event Management API — Ferris Wheel Venue

> **Portfolio project** · Business Analyst · Docs-as-Code practice

An OpenAPI specification for an event management system at a venue featuring VIP gondola cabins on a Ferris wheel. This project demonstrates the **Docs-as-Code** approach: documentation lives in the repository, is version-controlled, and can be deployed as interactive API docs.

## 📌 Background

A real operational case: coordinating **8 parallel events in a single day** — weddings, birthday celebrations, and tourist delegations — at a Ferris wheel venue with VIP gondola cabins.

Each event required end-to-end synchronization of:

- Client requirements (catering, decoration, equipment)
- Photographers and videographers
- Guest reception and escort staff
- Wheel launch timing with the engineering team

This specification describes an API that would automate that coordination and eliminate manual scheduling errors.

## 📂 Repository Structure

```
.
├── docs/
│   └── event-management-api.yaml   # OpenAPI 3.0 specification
└── README.md
```
## 🔍 What the Specification Covers

|Endpoint Group|Description                                                                    |
|--------------|-------------------------------------------------------------------------------|
|**Events**    |Create bookings, manage lifecycle (DRAFT → CONFIRMED → IN_PROGRESS → COMPLETED)|
|**Cabins**    |Check cabin availability, assign cabins to specific time slots                 |
|**Services**  |Add services: catering, decoration, photography, video, equipment              |
|**Staff**     |Assign roles (guest manager, decorator, photographer, videographer, engineer)  |
|**Schedule**  |Minute-by-minute daily timeline + automatic conflict detection                 |

### Event Lifecycle

```
DRAFT → CONFIRMED → IN_PROGRESS → COMPLETED
              ↓
          CANCELLED
```


## 🚀 How to View Interactive Docs

1. Go to [editor.swagger.io](https://editor.swagger.io)
1. Click **File → Import file**
1. Select `docs/event-management-api.yaml`
1. The interactive documentation will render on the right — all endpoints, schemas, and request/response examples included


## 🛠 Skills Demonstrated

- **OpenAPI 3.0** — REST API design: endpoints, schemas, request/response examples, error handling
- **Docs-as-Code** — documentation as code: Git versioning, structured repository layout
- **Requirements & Modeling** — decomposing a real operational process into entities and operations
- **Data Governance** — status lifecycle model, input validation, conflict detection
- **Cross-functional Coordination** — translated multi-stakeholder workflows into a structured API contract


## 👤 Author

**Karolina Gergert** — Business Analyst  
Background: client relationship management, requirements documentation (BPMN, User Story Map, OpenAPI)  
📍 Buenos Aires, Argentina


*This project was created as a Docs-as-Code practice to demonstrate business analysis and technical documentation skills.*