# Business Process and Solution Diagrams

These diagrams support the business analysis case documented in [`business-requirements.md`](business-requirements.md) and [`traceability.md`](traceability.md). The AS-IS diagram is an **illustrative hypothesis**, not a verified model of a real venue. The TO-BE process is a proposal requiring stakeholder validation.

## 1. AS-IS — illustrative current-state hypothesis

The assumed current process may involve manual coordination and reconciliation between event details, cabin availability, services, staff, and schedule information. Validate this model through stakeholder interviews and observation before treating it as an actual AS-IS finding.

```mermaid
flowchart TD
    A([Client request received]) --> B[Record event details]
    B --> C[Check cabin availability]
    C --> D[Arrange services]
    D --> E[Assign staff]
    E --> F[Assemble event-day schedule]
    F --> G{Missing information or conflict found?}
    G -- Yes --> H[Contact people and reconcile information manually]
    H --> B
    G -- No --> I[Prepare event]
```

**Potential pain points to validate:** repeated data entry, inconsistent event information, late conflict discovery, and unclear criteria for confirming a booking. These are hypotheses, not observed facts.

## 2. TO-BE — proposed target business process

```mermaid
flowchart TD
    A([Client request]) --> B[Register event as DRAFT]
    B --> C[Validate required booking data]
    C --> D{Data valid?}
    D -- No --> E[Return validation feedback and correct data]
    E --> C
    D -- Yes --> F[Search for suitable cabin]
    F --> G[Attempt cabin assignment]
    G --> H{Cabin available under agreed rules?}
    H -- No --> I[Review conflict and select another option]
    I --> F
    H -- Yes --> J[Arrange event services]
    J --> K[Assign staff]
    K --> L[Review daily schedule and run conflict check]
    L --> M{Unresolved conflicts?}
    M -- Yes --> N[Resolve resource or timing conflicts]
    N --> K
    M -- No --> O{Confirmation criteria met?}
    O -- No --> P[Keep event in DRAFT and complete missing items]
    P --> J
    O -- Yes --> Q[Confirm event]
    Q --> R([Prepare and deliver event])
```

**Decision points requiring business approval:** required data for confirmation, conflict definitions and buffers, treatment of draft/cancelled events, and the rules for correcting assignments.

## 3. Cabin assignment — business decision and exception flow

This sequence describes the intended behavior for US-06 and US-07. It shows both the coordinator's business decision and the proposed API interaction.

```mermaid
sequenceDiagram
    actor C as Event coordinator
    participant API as Proposed Event Management API

    C->>API: Search cabins for date and time slot
    API-->>C: Candidate cabins
    C->>API: Assign selected cabin to event
    API->>API: Validate event and requested cabin
    API->>API: Check overlapping bookings under agreed policy
    alt Event or cabin not found
        API-->>C: Documented not-found response
    else Overlapping booking exists
        API-->>C: 409 CABIN_ALREADY_BOOKED + conflict details
        C->>C: Review conflict and choose another option
        C->>API: Assign alternative cabin
        API-->>C: Assignment response, subject to validation
    else No conflict
        API-->>C: Assignment response
    end
```

The mock contract can illustrate response shapes. Actual conflict enforcement, especially when two coordinators act simultaneously, must be verified in a real implementation.

## 4. Event lifecycle — proposed state model

```mermaid
stateDiagram-v2
    [*] --> DRAFT: Event created
    DRAFT --> CONFIRMED: Confirmation criteria met
    CONFIRMED --> IN_PROGRESS: Event starts
    IN_PROGRESS --> COMPLETED: Event ends
    CONFIRMED --> CANCELLED: Event cancelled
    COMPLETED --> [*]
    CANCELLED --> [*]
```

The status-transition matrix is a proposal, not a confirmed policy. The business owner must decide whether transitions such as `DRAFT → CANCELLED` or `IN_PROGRESS → CANCELLED` are allowed, who can initiate them, and whether a cancellation reason is required. Invalid transitions are proposed to return `409 INVALID_STATUS_TRANSITION` (US-05).

## 5. Requirements mapping

| Diagram | Business question represented | Traceability |
|---|---|---|
| AS-IS hypothesis | Where could manual reconciliation and late conflict discovery occur? | Business requirements §2–3 |
| TO-BE process | What sequence and decisions should the target process support? | BR-01–BR-08; US-01–US-13 |
| Cabin assignment | How should the process respond when a resource is unavailable? | US-06, US-07 |
| Event lifecycle | Which event-state transitions should be allowed? | BR-02, US-05 |

## 6. Validation needed before implementation

- Confirm or replace the AS-IS hypothesis with evidence from interviews, observation, and existing artifacts.
- Validate the TO-BE process with the venue manager, coordinator, and operations lead.
- Agree confirmation criteria, status transitions, conflict taxonomy, and buffer rules.
- Update requirements, diagrams, OpenAPI, and acceptance criteria together when a business decision changes.
