# Requirements Traceability Matrix

This matrix links each user story to the API operation that implements it, the response codes it returns, and the acceptance criteria that confirm it. It covers all 13 operations in [`event-management-api.yaml`](event-management-api.yaml).

Roles: **Venue manager** books and manages events. **Coordinator** assigns cabins, services, and staff. **Operations lead** runs the day schedule.

## Events

| ID | User story | Endpoint | Success | Errors | Acceptance criteria |
|---|---|---|---|---|---|
| US-01 | As a venue manager, I want to see all events for a date, so that I can plan the day. | `GET /events` | 200 | 400 | Given events exist on 2026-11-14, when I request that date, then the response lists each event with ID, title, times, guest count, and status. A request without a date returns 400. |
| US-02 | As a venue manager, I want to create a booking, so that the client's request is on record. | `POST /events` | 201 | 400, 422 | Given a title, client name, date, start and end time, and guest count, when I submit the booking, then the system creates the event with status DRAFT. If the end time is not later than the start time, or the guest count is below 1, the system returns 422 and names the field. |
| US-03 | As a coordinator, I want to open an event, so that I can see its cabin, services, and staff. | `GET /events/{eventId}` | 200 | 404 | Given an existing event ID, when I request it, then the response includes client details, cabin, services, and staff. An unknown ID returns 404 EVENT_NOT_FOUND. |
| US-04 | As a venue manager, I want to change the time, guest count, or notes, so that the booking reflects the client's latest request. | `PATCH /events/{eventId}` | 200 | 400, 404, 422 | Given an existing event, when I send new values for some fields, then the system changes only those fields and updates `updatedAt`. A body with no fields fails request-schema validation and returns 422. An end time earlier than the start time returns 422. |
| US-05 | As a venue manager, I want to confirm or cancel an event, so that the team knows which bookings are final. | `PATCH /events/{eventId}/status` | 200 | 400, 404, 409, 422 | Given a DRAFT event, when I set the status to CONFIRMED, then the system accepts it. Given a COMPLETED event, when I set the status to CONFIRMED, then the system returns 409 INVALID_STATUS_TRANSITION. |

## Cabins

| ID | User story | Endpoint | Success | Errors | Acceptance criteria |
|---|---|---|---|---|---|
| US-06 | As a coordinator, I want to find free cabins for a time slot, so that I can offer the client a real option. | `GET /cabins` | 200 | 400 | Given a date, a start time, and a duration, when I search, then the response lists only cabins with no overlapping booking. A request without one of the three parameters returns 400. |
| US-07 | As a coordinator, I want to assign a cabin to an event, so that the client's guests have a reserved space. | `PUT /events/{eventId}/cabin` | 200 | 400, 404, 409, 422 | Given a free cabin, when I assign it, then the event shows that cabin. Given a cabin booked for an overlapping slot, when I assign it, then the system returns 409 CABIN_ALREADY_BOOKED and names the conflicting event in `details`. |

## Services

| ID | User story | Endpoint | Success | Errors | Acceptance criteria |
|---|---|---|---|---|---|
| US-08 | As a coordinator, I want to see the services booked for an event, so that I can check that nothing is missing. | `GET /events/{eventId}/services` | 200 | 404 | Given an event with services, when I request the list, then the response shows each service with type, description, and scheduled time. An event without services returns an empty list. |
| US-09 | As a coordinator, I want to add catering, decoration, photo, video, or equipment to an event, so that the client receives the full package. | `POST /events/{eventId}/services` | 201 | 400, 404, 422 | Given a valid service type and a description, when I add the service, then the system creates it and returns its ID. A type outside the five allowed values returns 422. |

## Staff

| ID | User story | Endpoint | Success | Errors | Acceptance criteria |
|---|---|---|---|---|---|
| US-10 | As an operations lead, I want to see who works on an event, so that I can confirm coverage. | `GET /events/{eventId}/staff` | 200 | 404 | Given an event with assignments, when I request the list, then the response shows each staff member with role and briefing time. |
| US-11 | As an operations lead, I want to assign a staff member with a role, so that each task has an owner. | `POST /events/{eventId}/staff` | 201 | 400, 404, 409, 422 | Given a free staff member, when I assign them, then the system creates the assignment. Given a staff member with an overlapping assignment, when I assign them, then the system returns 409 STAFF_ALREADY_ASSIGNED and names the conflicting event. |

## Schedule

| ID | User story | Endpoint | Success | Errors | Acceptance criteria |
|---|---|---|---|---|---|
| US-12 | As an operations lead, I want to see the day's timeline, so that I know what happens and when. | `GET /schedule/{date}` | 200 | 400 | Given a date with events, when I request the schedule, then the response lists each activity with start time, end time, and status. A malformed date returns 400. |
| US-13 | As an operations lead, I want to detect conflicts before the event day, so that I can fix them early. | `GET /schedule/{date}/conflicts` | 200 | 400 | Given two events that share a cabin in overlapping slots, when I run the check, then `hasConflicts` is true and the item shows type CABIN_OVERLAP, both event IDs, the cabin ID, and the overlap window. Given no conflicts, then `hasConflicts` is false and `items` is empty. |

## Coverage

| Area | Operations | Stories |
|---|---|---|
| Events | 5 | US-01 to US-05 |
| Cabins | 2 | US-06, US-07 |
| Services | 2 | US-08, US-09 |
| Staff | 2 | US-10, US-11 |
| Schedule | 2 | US-12, US-13 |
| **Total** | **13** | **13** |

## Open questions for stakeholders

1. Does the venue allow cabin assignment at booking creation, or only after confirmation?
2. Is the status transition matrix in the specification correct?
3. Does a cancellation require a reason?
4. What minimum gap between operational activities counts as a conflict?
5. Does the conflict check ignore DRAFT and CANCELLED events?
