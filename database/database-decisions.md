# Forever Hotel Database Decision Log

This document records database design issues identified during the
SENG 34213 development phase and the implementation decisions agreed
by the development team.

---

## DB-001 - Walk-in Guest Model

### Problem
The current schema requires account credentials for every guest,
while the Front Desk System supports walk-in bookings.

### SRS/SDS Traceability
- SRS: FD-04 - Walk-in Booking
- SDS: Guest and Booking Data Model

### Proposed Decision
Allow walk-in guests to exist without a Hotel Website account.

### Final Decision
Pending team review.

### Schema Impact
Pending.

### Status
Under Review.

---

## DB-002 - Service Request to Task Relationship

### Problem
The current schema contains Service Request and Task entities without
an explicit relationship.

### Proposed Decision
Add a nullable one-to-one service_request_id reference to WKMS tasks.

### Final Decision
Pending team review.

### Status
Under Review.