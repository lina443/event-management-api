# Requirements Traceability Matrix

**Purpose:** Maintain a trace from business need to user story, API operation, acceptance criteria, and verification approach.  
**Status:** Proposed requirements for an illustrative portfolio case; stakeholder validation is outstanding.

The matrix covers 13 user stories and the 13 operations defined in [`event-management-api.yaml`](event-management-api.yaml). Business context, objectives, assumptions, and open questions are documented in [`business-requirements.md`](business-requirements.md).

## Traceability model

`Business objective / requirement → User story → Acceptance criteria → API operation → Verification scenario`

The mock server and Postman collection can verify request/response contract behavior. They do not establish that production business logic, persistence, or concurrency control has been implemented.

## Events

| ID | Business need | User story | API operation | Acceptance criteria / expected behavior | Verification scenario |
|---|---|---|---|---|---|
| US-01 | BO-01, BR-01 | As a venue manager, I want to see events for a date so that I can plan the day. | `GET /events` | Given a valid date with events, when the date is requested, then the response includes each event's ID, title, time, guest count, and status. A missing required date is rejected with the documented 400 response. | Valid date; missing `date`; inspect `items` and `total`. |
| US-02 | BO-01, BR-01 | As a venue manager, I want to create a booking so that the client's request is recorded. | `POST /events` | Given valid required booking data, when the event is created, then the contract returns 201, an event ID, and `DRAFT` status. Invalid timing or guest count is rejected with the documented validation response. | Valid create; invalid time interval; guest count below minimum; verify status and error code. |
| US-03 | BO-01, BR-01 | As a coordinator, I want to open an event so that I can review its details and arrangements. | `GET /events/{eventId}` | Given an existing ID, the response includes event details and the documented arrangement fields. An unknown ID returns 404 `EVENT_NOT_FOUND`. | Existing event; unknown event ID; inspect required response fields and error code. |
| US-04 | BO-01, BR-01 | As a venue manager, I want to change editable event details so that the booking reflects the latest request. | `PATCH /events/{eventId}` | Given an existing event and a non-empty valid body, only supplied editable fields are changed and `updatedAt` is returned. An empty body fails schema validation; invalid timing is rejected. | Partial update; empty object; invalid time interval; verify response/error schema. |
| US-05 | BO-03, BR-02 | As a venue manager, I want to confirm or cancel an event so that the team knows its current status. | `PATCH /events/{eventId}/status` | A permitted transition is accepted. An invalid transition, such as `COMPLETED` to `CONFIRMED`, returns 409 `INVALID_STATUS_TRANSITION`. | Valid transition; invalid transition; inspect status and error code. |

## Cabins

| ID | Business need | User story | API operation | Acceptance criteria / expected behavior | Verification scenario |
|---|---|---|---|---|---|
| US-06 | BO-02, BR-03 | As a coordinator, I want to find available cabins for a time slot so that I can offer a feasible option. | `GET /cabins` | Given a date, start time, and duration, the response lists cabins that satisfy the proposed availability rules. Missing required parameters are rejected. | Valid search; omit each required parameter; verify response shape. |
| US-07 | BO-02, BR-04 | As a coordinator, I want to assign a cabin to an event so that the guests have a reserved space. | `PUT /events/{eventId}/cabin` | A valid available cabin can be assigned. An overlapping booking returns 409 `CABIN_ALREADY_BOOKED` with documented conflict details. | Valid assignment; overlapping assignment; unknown event; inspect error code and details. |

## Services

| ID | Business need | User story | API operation | Acceptance criteria / expected behavior | Verification scenario |
|---|---|---|---|---|---|
| US-08 | BR-05 | As a coordinator, I want to see services booked for an event so that I can check whether arrangements are complete. | `GET /events/{eventId}/services` | For an existing event, the response lists its services with the documented fields; an event without services returns an empty list. | Existing event with services; empty list; unknown event. |
| US-09 | BR-05 | As a coordinator, I want to add a service so that the event package reflects the client's needs. | `POST /events/{eventId}/services` | A valid service request creates a service and returns its identifier. An unsupported service type is rejected with the documented validation response. | Valid service; unsupported type; unknown event. |

## Staff

| ID | Business need | User story | API operation | Acceptance criteria / expected behavior | Verification scenario |
|---|---|---|---|---|---|
| US-10 | BR-06 | As an operations lead, I want to see staff assigned to an event so that I can check operational coverage. | `GET /events/{eventId}/staff` | For an existing event, the response lists assignments with the documented staff, role, and briefing fields. | Event with assignments; empty list; unknown event. |
| US-11 | BO-02, BR-06 | As an operations lead, I want to assign a staff member so that each operational task has an owner. | `POST /events/{eventId}/staff` | A valid assignment is recorded in the proposed behavior. An overlapping assignment returns 409 `STAFF_ALREADY_ASSIGNED` with documented conflict details. | Valid assignment; overlapping assignment; unknown event; invalid input. |

## Schedule

| ID | Business need | User story | API operation | Acceptance criteria / expected behavior | Verification scenario |
|---|---|---|---|---|---|
| US-12 | BO-04, BR-07 | As an operations lead, I want to see the daily timeline so that I can plan activities. | `GET /schedule/{date}` | For a valid date, the response lists scheduled activities with start time, end time, and status. A malformed date is rejected with the documented 400 response. | Valid date; malformed date; inspect schedule structure. |
| US-13 | BO-02, BO-04, BR-07 | As an operations lead, I want to detect scheduling conflicts before event day so that they can be resolved early. | `GET /schedule/{date}/conflicts` | When a defined conflict exists, `hasConflicts` is true and the corresponding item identifies the conflict type and relevant entities/time window. When no conflict exists, `hasConflicts` is false and `items` is empty. | Seeded conflict; no-conflict scenario; verify conflict structure. |

## Coverage summary

| Functional area | Operations | User stories | Business requirements |
|---|---:|---|---|
| Events | 5 | US-01–US-05 | BR-01, BR-02 |
| Cabins | 2 | US-06–US-07 | BR-03, BR-04 |
| Services | 2 | US-08–US-09 | BR-05 |
| Staff | 2 | US-10–US-11 | BR-06 |
| Schedule | 2 | US-12–US-13 | BR-07 |
| Cross-cutting validation and error handling | Referenced by multiple stories | US-01–US-13 | BR-08 |
| **Total** | **13** | **13 user stories** | **8 high-level business requirements** |

Coverage here means that each user story is mapped to an operation and a verification direction. It does not mean that every acceptance criterion has passed against a real backend.

## Open questions and change control

The following decisions may change requirements, diagrams, and the API contract:

1. Can a cabin be assigned during event creation, or only as a separate operation?
2. Which event-status transitions are permitted, including cancellation from `DRAFT` or `IN_PROGRESS`?
3. Which fields must be complete before confirmation?
4. What interval boundaries and setup/cleanup buffers define an overlap?
5. Are `DRAFT` and `CANCELLED` events excluded from availability and conflict checks?
6. What staff eligibility, role, break, and travel-time rules apply?
7. How should concurrent resource-allocation requests be handled?
8. Who approves business rules and acceptance criteria?

These questions should be resolved with the business owner before requirements are treated as implementation-ready. See [`business-requirements.md`](business-requirements.md).
