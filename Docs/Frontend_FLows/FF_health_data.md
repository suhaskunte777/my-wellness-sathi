## Health Data — Frontend Flow

### Objective
- Capture and maintain health context for a user or prospect to guide coaching.

### Relationships
- Polymorphic: `health_dataable_type` = `User|Prospect`, `health_dataable_id` = uuid
- Audit: created_by/updated_by/deleted_by

### Entry Points
- From Prospect Detail → Health tab
- From Member Profile Detail → Health tab
- Optional global list for coaches: `/dashboard/health` (planned)

### Screens
1) Health Panel (embedded)
- Shown inside Prospect/Member detail
- Fields: challenges, goals, medical_history, current_medications, allergies, dietary_restrictions, fitness_level
- Actions: Create if none; Edit/Delete if exists

2) Health List (planned)
- Route: `/dashboard/health?type=User|Prospect&id=...`
- Filters: entity type, created_by, date range

### API Mapping
- GET `/api/health-data`
- POST `/api/health-data`
- GET `/api/health-data/{uuid}`
- PUT `/api/health-data/{uuid}`
- DELETE `/api/health-data/{uuid}`

Create/Update payload
```json
{
  "uuid": "client-generated uuid",
  "health_dataable_type": "User|Prospect",
  "health_dataable_id": "uuid",
  "challenges": "string|null",
  "goals": "string|null",
  "medical_history": "string|null",
  "current_medications": "string|null",
  "allergies": "string|null",
  "dietary_restrictions": "string|null",
  "fitness_level": "beginner|intermediate|advanced|null"
}
```

### Permissions & Validation
- Create/Update per RBAC `create.*` / `update.*` and coach scope.
- Validate text lengths; keep PII minimal per privacy policy.

### UX Notes
- Single-record per entity is acceptable; or allow multiple revisions (show latest by default).
- Show last updated timestamp and author.


