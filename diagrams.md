# Diagrams

Two diagrams describe the core behavior of the [Event Management API](event-management-api.yaml). GitHub renders them in the browser.

## 1. Cabin assignment with a conflict

A coordinator searches for free cabins and assigns one to an event. The API rejects the first choice because another event already holds the cabin, so the coordinator picks a different cabin. This flow covers [US-06 and US-07](traceability.md).

```mermaid
sequenceDiagram
    actor C as Coordinator
    participant API as Event Management API

    C->>API: GET /cabins (date, startTime, durationMinutes)
    API-->>C: 200 list of available cabins

    C->>API: PUT /events/evt-1042/cabin (cabinId: cabin-vip-02)
    API->>API: Find the event and its time slot
    API->>API: Check cabin-vip-02 for overlapping bookings

    alt Event does not exist
        API-->>C: 404 EVENT_NOT_FOUND
    else Cabin is booked for an overlapping slot
        API-->>C: 409 CABIN_ALREADY_BOOKED (overlaps with evt-1043)
        C->>API: PUT /events/evt-1042/cabin (cabinId: cabin-vip-01)
        API->>API: Check cabin-vip-01 for overlapping bookings
        API-->>C: 200 event with cabin-vip-01
    else Cabin is free
        API-->>C: 200 event with cabin-vip-02
    end
```

## 2. Event lifecycle

An event starts as a draft and moves forward through confirmation, the event day, and completion. A confirmed event can move to cancelled. Any other transition returns `409 INVALID_STATUS_TRANSITION`. The status endpoint is `PATCH /events/{eventId}/status` ([US-05](traceability.md)).

```mermaid
stateDiagram-v2
    [*] --> DRAFT: POST /events
    DRAFT --> CONFIRMED: Venue manager confirms
    CONFIRMED --> IN_PROGRESS: Event starts
    CONFIRMED --> CANCELLED: Venue manager cancels
    IN_PROGRESS --> COMPLETED: Event ends
    COMPLETED --> [*]
    CANCELLED --> [*]
```

The transition matrix is a proposal. The business owner needs to confirm it, for example whether a DRAFT event can move directly to CANCELLED.
