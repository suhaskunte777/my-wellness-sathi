## Supporting Models — Frontend Flow

Covers embedded/linked UI for Contacts, Addresses, Photos, Groups, Marketing Levels, and DailyTracker Intakes. Some endpoints are not yet exposed; flows are planned and integrated into existing detail pages.

### Contacts
- Model: `contacts` (polymorphic to User/Club; we’ll use User first)
- Usecases: add/mark primary, verify, tag, delete
- Embedded in: Profile Detail → Contacts tab/section
- Planned Endpoints:
  - GET `/api/contacts?contactable_type=User&contactable_id={uuid}`
  - POST `/api/contacts`
  - PUT `/api/contacts/{uuid}`
  - DELETE `/api/contacts/{uuid}`
- Validation: country code, email format, phone format

### Addresses
- Model: `addresses` (polymorphic to User/Club; use User first)
- Usecases: add/mark primary, geocode from google_map_link (planned), delete
- Embedded in: Profile Detail → Addresses tab/section
- Planned Endpoints:
  - GET `/api/addresses?addressable_type=User&addressable_id={uuid}`
  - POST `/api/addresses`
  - PUT `/api/addresses/{uuid}`
  - DELETE `/api/addresses/{uuid}`

### Photos
- Model: `photos` (polymorphic to User/Club/Prospect)
- Usecases: upload with compression (client), set primary, tag by type, expire cleanup
- Embedded in: Prospect Detail and Profile Detail → Photos tab
- Planned Endpoints:
  - GET `/api/photos?photoable_type=Prospect&photoable_id={uuid}`
  - POST `/api/photos` (multipart)
  - PUT `/api/photos/{uuid}`
  - DELETE `/api/photos/{uuid}`

### Groups
- Model: `groups`
- Usecases: view auto-assigned groups per user; manual attach/detach (role gated)
- Embedded in: Profile Detail → Groups section; Filters in lists by group
- Planned Endpoints:
  - GET `/api/users/{userUuid}/groups`
  - POST `/api/users/{userUuid}/groups/{groupUuid}`
  - DELETE `/api/users/{userUuid}/groups/{groupUuid}`

### Marketing Levels
- Model: `marketing_levels` (fixed table; cache)
- Usecases: populate dropdowns; show badges; filter users
- Endpoints (read-only):
  - GET `/api/marketing-levels` (planned)

### Daily Tracker Intakes
- Model: `daily_tracker_intakes`
- Usecases: add supplement/meal entries to a tracker
- Embedded in: Daily Tracker Detail
- Planned Endpoints:
  - GET `/api/daily-trackers/{uuid}/intakes`
  - POST `/api/daily-trackers/{uuid}/intakes`
  - PUT `/api/daily-tracker-intakes/{uuid}`
  - DELETE `/api/daily-tracker-intakes/{uuid}`

### UX Notes
- All embedded sections use dialogs/drawers.
- Optimistic add/update with server confirmation.
- Respect RBAC/ABAC for write operations.

