## Prospects — Frontend Flow

### Objective
- Provide end-to-end UX for prospect lifecycle: capture → follow-up → convert to member.
- Align with existing API and model fields; respect RBAC/ABAC.

### Key Relationships
- Prospect belongs to a coach (`coach_id` → `users.uuid`).
- Can convert to a member (`converted_to_user_uuid`, `converted_at`).
- Related data shown in tabs: Health Data, Measurements, Contacts, Addresses, Photos.

### Entry Points
- Navigation: Dashboard → Prospects List (`/dashboard/prospects`).
- Dashboard cards deep-link to filtered lists.

### Screens
1) Prospects List
- Route: `/dashboard/prospects`
- Components: table with server-side pagination, column chooser, saved filters
- Columns: name, primary_phone, lead_stage, next_follow_up_date, conversion_probability, coach, created_at, actions
- Filters: search (name/phone), lead_stage, next_follow_up_date (overdue/today/this week/range), coach scope (Me/Directs/Team), active flag
- Quick chips: New, Contacted, Interested, Qualified, Hold, Lost, Archived
- Actions per row: View, Edit, Convert to Member, Delete (soft)
- Bulk actions: Assign coach, Change lead stage, Export CSV (current filter)

2) Prospect Detail
- Route: `/dashboard/prospects/:uuid`
- Header: name, lead stage badge, conversion probability, coach, follow-up dates, active toggle
- Actions: Edit, Convert to Member, Add Follow-up (updates dates), Add Measurement, Add Photo
- Tabs:
  - Overview: core profile fields, source, notes, follow-up history
  - Health: embed Health Data panel (create/update via health endpoints)
  - Measurements: list + add, open compare
  - Contacts: manage contact methods
  - Addresses: manage addresses
  - Photos: gallery (by photo type)

3) Create / Edit Prospect
- Route: `/dashboard/prospects/new`, `/dashboard/prospects/:uuid/edit`
- Form groups: Identity, Demographics, Source, Lead, Follow-up, Health intent
- Validation: required name, enum checks, sensible ranges; normalize phone/email
- Defaulting: `is_active = true`; auto-assign `coach_id` to current user if coach role

4) Needs Follow-up
- Route: `/dashboard/prospects?filter=needs-follow-up`
- Preset filter using API shortcut; allows quick complete & reschedule

5) By Lead Stage
- Route: `/dashboard/prospects?stage=New|Contacted|...`
- Badge clicks from list/dashboard apply this filter

### API Mapping
- Auth required for all below.
- List/CRUD:
  - GET `/api/prospects`
  - POST `/api/prospects`
  - GET `/api/prospects/{prospect}`
  - PUT `/api/prospects/{prospect}`
  - DELETE `/api/prospects/{prospect}`
- Shortcuts:
  - GET `/api/prospects/needs-follow-up`
  - GET `/api/prospects/by-lead-stage/{stage}`
  - POST `/api/prospects/{prospect}/convert` → returns created `user_uuid` and optional `profile_detail`

Payload notes (create/update)
```json
{
  "uuid": "client-generated uuid",
  "name": "string",
  "primary_phone": "string|null",
  "birth_date": "YYYY-MM-DD|null",
  "birth_year": 1988,
  "age": 36,
  "age_by": "birth_date|birth_year|age",
  "height": 170,
  "gender": "male|female|other|null",
  "marital_status": "single|married|divorced|widowed|null",
  "occupation": "student|employed|unemployed|retired|housewife|other|null",
  "mother_tongue": "string|null",
  "source": "TTP|Assessment|Measurement_Camp|Referral|...|Other|null",
  "source_details": "string|null",
  "lead_stage": "New|Contacted|Interested|Qualified|Converted|Lost|Hold|Archived",
  "referred_by": "user uuid|null",
  "notes": "string|null",
  "last_follow_up_date": "YYYY-MM-DD|null",
  "next_follow_up_date": "YYYY-MM-DD|null",
  "conversion_probability": 0-100,
  "coach_id": "user uuid|null",
  "is_active": true
}
```

Convert to Member (example)
```json
POST /api/prospects/{uuid}/convert
Body: { "assign_coach_to_user": true }
Response 200: {
  "user_uuid": "...",
  "converted_at": "...",
  "profile_detail_uuid": "..." // when created
}
```

### Permissions
- Create/Update/Delete gated by RBAC: `create.*`, `update.*`, `delete.*`.
- Read scope via ABAC (tags/groups) when implemented; default to role scope (self/team).

### UI Integration Points
- From Prospect Detail:
  - Add Measurement → opens Measurement form prefilled with `measurementable_type = Prospect` and id
  - Add Health Data → opens Health Data form bound to prospect
  - Add Contact/Address/Photo → opens respective dialogs

### Edge Cases
- Duplicate detection on phone/email (show merge option if backend signals conflict).
- Conversion idempotency: disable Convert when already converted; show link to created member.
- Follow-up dates consistency: `next_follow_up_date >= today`.

### Performance and UX
- Debounced search; infinite scroll or server pagination.
- Persist user filters in route/query and localStorage for recall.
- Optimistic UI for stage and follow-up updates with rollback on error.



