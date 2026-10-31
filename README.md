# Event Management API — Business Analysis Portfolio Project

**Role:** Junior Business Analyst (portfolio case)  
**Domain:** Event operations / hospitality  
**Primary focus:** business process analysis, requirements engineering, business rules, acceptance criteria, traceability, and contract-level verification.

> **Project status and evidence boundary.** This is an illustrative analysis and solution-design prototype, not a system deployed at a real venue. The AS-IS process and pain points are hypotheses for the case, not findings from stakeholder interviews. The repository contains an OpenAPI contract and a mock-server testing setup, not a production backend. No operational improvements have been measured.

## 1. Business context and problem statement

A venue coordinating VIP events needs to bring together several related activities: registering a booking, allocating a cabin, arranging additional services, assigning staff, and reviewing the event-day schedule. These activities share resources and depend on time, status, and availability rules.

If event details and assignments are maintained in separate spreadsheets, chats, or individual workflows, coordinators may have to reconcile information manually. This creates a risk of incomplete booking data, duplicate resource allocation, late conflict detection, and inconsistent decisions about when an event is ready to confirm.

**Problem statement:** How might we make VIP event coordination more consistent and predictable by defining a shared event lifecycle, explicit resource-allocation rules, and a repeatable way to identify scheduling conflicts?

The business problem is not “the venue needs an API.” The API is one proposed means of implementing a clearer process. The analysis therefore starts with the business scenario and its rules, then translates them into requirements and a verifiable interface contract.

## 2. AS-IS and TO-BE at a glance

### AS-IS — illustrative hypothesis

1. A request is received and the event details are recorded.
2. The coordinator checks cabin availability against the information available to them.
3. Services are arranged and staff are assigned, potentially in separate records or conversations.
4. The schedule is assembled from event and resource information.
5. Missing information and overlaps are resolved when someone notices them.

**Potential weaknesses to validate:** fragmented information, inconsistent validation, unclear status-transition rules, and late discovery of cabin or staff conflicts. These are hypotheses, not confirmed facts about a real business.

### TO-BE — proposed target process

1. Register an event as `DRAFT` with required booking details.
2. Retrieve and update event information through a consistent record.
3. Search for cabins and assign a resource subject to availability rules.
4. Add required services and assign staff.
5. Review the daily schedule and run a conflict check.
6. Confirm an event only through a permitted status transition.
7. Return documented validation, not-found, and conflict errors when an operation cannot be completed.

The proposed target process is shown in more detail in [`diagrams.md`](diagrams.md). The assumptions and questions requiring stakeholder confirmation are recorded in [`business-requirements.md`](business-requirements.md).

## 3. Business objectives and success measures

The intended outcomes are improved consistency of booking data, earlier visibility of resource conflicts, and clearer control of the event lifecycle. These outcomes have not been measured because no production system or real operational baseline is available.

| Objective | Proposed measure after implementation | Target-setting approach |
|---|---|---|
| Improve booking-data quality | Share of bookings passing required-field and business validation | Establish baseline, then agree target with business owner |
| Reduce resource conflicts | Number of conflicting cabin/staff assignments per period | Measure baseline and compare after rollout |
| Enforce lifecycle rules | Number of invalid transitions accepted by the system | Target: zero accepted invalid transitions |
| Improve conflict visibility | Share of known test conflicts detected by the conflict-check process | Agree coverage and test data with operations |
| Improve requirement verifiability | Share of in-scope requirements linked to acceptance criteria and verification scenarios | Target: 100% traceability for agreed scope |

These are proposed measures, not achieved results. Baselines, target values, data sources, and the measurement period would be agreed before implementation.

## 4. Scope and boundaries

### In scope

The API contract describes 13 operations across five functional areas:

| Area | Business capability | Operations |
|---|---|---|
| Events | List, create, retrieve, update, and change event status | `GET /events`, `POST /events`, `GET /events/{eventId}`, `PATCH /events/{eventId}`, `PATCH /events/{eventId}/status` |
| Cabins | Search for available cabins and assign a cabin | `GET /cabins`, `PUT /events/{eventId}/cabin` |
| Services | Retrieve and add services for an event | `GET /events/{eventId}/services`, `POST /events/{eventId}/services` |
| Staff | Retrieve and add staff assignments | `GET /events/{eventId}/staff`, `POST /events/{eventId}/staff` |
| Schedule | Retrieve a daily schedule and identify conflicts | `GET /schedule/{date}`, `GET /schedule/{date}/conflicts` |

### Out of scope

- Production backend, database persistence, and transactional concurrency control.
- Authentication, authorization, and role-based access control.
- A user-facing application or operational dashboard.
- Integrations with external catering, payment, or workforce systems.
- Notifications, deployment, monitoring, and production support.
- Evidence of actual cost savings or process improvement.

## 5. Stakeholders and requirements approach

The case assumes three primary business roles: a **venue manager** responsible for bookings and event status, a **coordinator** responsible for cabin and service arrangements, and an **operations lead** responsible for staff assignments and the daily schedule. Development and QA are delivery stakeholders who need unambiguous, verifiable specifications.

The requirements approach separates the business need from the solution design:

1. State the problem and expected business outcome.
2. Model the assumed current and proposed target processes.
3. Identify stakeholders, scope, constraints, assumptions, and open questions.
4. Define functional requirements and business rules.
5. Express expected behavior as acceptance criteria, including negative scenarios.
6. Map user stories to API operations and response behavior.
7. Verify the contract with a mock server and Postman checks, while distinguishing contract validation from testing a real implementation.

Detailed objectives, stakeholder needs, requirements, assumptions, and open questions are documented in [`business-requirements.md`](business-requirements.md). The story-to-operation trace is in [`traceability.md`](traceability.md).

## 6. Key business rules in the proposed design

- A newly created event starts in `DRAFT`.
- Event status changes are restricted to the proposed lifecycle: `DRAFT → CONFIRMED → IN_PROGRESS → COMPLETED`, with `CONFIRMED → CANCELLED` as a cancellation path.
- An update request must contain at least one editable field.
- Event timing must be valid; the end time must be later than the start time.
- Cabin and staff assignments must be checked for relevant overlapping time slots.
- A conflict or invalid status transition should return a documented, machine-readable error.
- The precise rules for draft/cancelled events, operational buffers, and concurrent bookings remain open questions until confirmed by the business owner.

These rules are proposed requirements. The mock server can illustrate contract responses, but it does not prove that a real backend enforces every business rule.

## 7. Acceptance criteria — examples

### AC-01 — Create an event

**Given** the user submits valid required booking data, **when** the user creates an event, **then** the API contract specifies `201 Created`, an event identifier, and the initial status `DRAFT`.

**Negative case:** invalid data must produce the documented validation response.

### AC-02 — Reject an invalid status transition

**Given** an event is `COMPLETED`, **when** a user attempts to change it to `CONFIRMED`, **then** the proposed behavior is `409 Conflict` with `INVALID_STATUS_TRANSITION`.

### AC-03 — Prevent an overlapping cabin assignment

**Given** a cabin is booked during an overlapping time slot, **when** a coordinator attempts to assign that cabin to another event, **then** the proposed behavior is `409 Conflict` with `CABIN_ALREADY_BOOKED`.

### AC-04 — Prevent an overlapping staff assignment

**Given** a staff member has an overlapping assignment, **when** the operations lead attempts another conflicting assignment, **then** the proposed behavior is `409 Conflict` with `STAFF_ALREADY_ASSIGNED`.

### AC-05 — Reject invalid event timing

**Given** a request has start and end times, **when** the end time is not later than the start time, **then** the request is rejected with the documented validation response.

The complete set of 13 user stories and their acceptance criteria is maintained in [`traceability.md`](traceability.md). Criteria are written as proposed expected behavior; stakeholder validation is still required.

## 8. Solution design and deliverables

| Artifact | Purpose |
|---|---|
| [`business-requirements.md`](business-requirements.md) | Business context, objectives, stakeholders, scope, requirements, assumptions, risks, and open questions |
| [`diagrams.md`](diagrams.md) | AS-IS hypothesis, TO-BE process, cabin-conflict flow, and event lifecycle |
| [`traceability.md`](traceability.md) | User stories, acceptance criteria, endpoint mapping, error scenarios, and coverage overview |
| [`event-management-api.yaml`](event-management-api.yaml) | OpenAPI contract: operations, request/response schemas, and documented errors |
| [`event-management-api.postman_collection.json`](event-management-api.postman_collection.json) | Postman requests and contract-level assertions |
| [`mock-server.md`](mock-server.md) | Mock-server and testing instructions |

### Why an API?

The API is a solution-design artifact that makes operations, data expectations, and error behavior explicit. It gives business analysis, development, and QA a shared reference. It is not itself the business outcome, and it does not replace stakeholder validation or process change.

## 9. Contract-level validation

The repository includes an OpenAPI specification, a Prism mock-server setup, and a Postman collection. These artifacts can be used to check that requests and documented example responses are consistent with the proposed contract.

Start the mock server from the directory containing `event-management-api.yaml`:

```bash
npx @stoplight/prism-cli mock event-management-api.yaml
```

The default local address is `http://127.0.0.1:4010`.

Run the collection with Newman from the directory containing the collection file:

```bash
npx newman run event-management-api.postman_collection.json --env-var "baseUrl=http://127.0.0.1:4010"
```

The collection is designed to execute 18 requests and 36 test checks. A successful mock run is evidence of contract/example-level behavior only. It does not prove database persistence, actual conflict detection, concurrency safety, or end-to-end business behavior in a production backend. Refresh `newman-results.png` only after rerunning the current collection.

## 10. Current limitations and next analytical steps

The current model is based on an illustrative scenario. Before implementation, a business analyst would validate the AS-IS process with stakeholders, confirm the status-transition matrix and conflict policy, resolve open questions, and agree on measurable targets. The technical team would then define implementation details such as transactional conflict protection and persistence. QA would verify the rules against an implemented backend as well as the API contract.

## Documentation

- [OpenAPI specification](event-management-api.yaml)
- [Business requirements and assumptions](business-requirements.md)
- [AS-IS / TO-BE and workflow diagrams](diagrams.md)
- [Requirements traceability matrix](traceability.md)
- [Mock server and testing guide](mock-server.md)
