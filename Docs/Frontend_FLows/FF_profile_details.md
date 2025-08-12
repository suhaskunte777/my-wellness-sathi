## Profile Details — Frontend Flow

### Objective
- Manage member profile metadata and business hierarchy info; bridge from converted prospects.

### Key Relationships
- `profile_details.user_uuid` -> `users.uuid`
- Assignment references: `coach_id`, `referred_by`, `supervisor_id`, `world_team_id`, `get_id`, `mil_id`, `president_team_id` -> `users.uuid`
- Conversion link: `converted_from_prospect_uuid`, `converted_at`

### Entry Points
- From Dashboard: Profile card → `/dashboard/profile`
- From Prospect Convert → open created member profile detail
- From Lists/Global search: open profile detail for a given `userUuid`

### Screens
1) Profile Detail View
- Route: `/dashboard/profile` (self) and `/dashboard/users/:userUuid/profile` (coach scope)
- Sections: Identity, Contacts, Addresses, Marketing Level, Assignments, Notes, Follow-ups
- Actions: Edit, Add Contact, Add Address, View Measurements, View Health

2) Edit Profile Detail
- Route: `/dashboard/users/:userUuid/profile/edit`
- Fields: names, `primary_email`, `primary_phone`, age_by group (birth_date|birth_year|age), demographics, source, marketing level (dropdown from fixed table), diamonds/stones/royalty_level/discount_level/years_active/mark_hughes_award, assignments, `is_active`
- Validation: enums, numeric ranges, email/phone normalization

3) Convert From Prospect (link-back)
- Route: `/dashboard/prospects/:prospectUuid/convert`
- After success, navigate to `/dashboard/users/:userUuid/profile`

### API Mapping
- GET `/api/profile-details`
- POST `/api/profile-details`
- GET `/api/profile-details/{uuid}`
- PUT `/api/profile-details/{uuid}`
- DELETE `/api/profile-details/{uuid}`
- GET `/api/profile-details/by-user/{userUuid}`
- GET `/api/profile-details/needs-follow-up`
- POST `/api/profile-details/convert-from-prospect/{prospectUuid}/{userUuid}`

Payload (create/update)
```json
{
  "uuid": "client-generated uuid",
  "user_uuid": "...",
  "first_name": "string|null",
  "middle_name": "string|null",
  "last_name": "string|null",
  "primary_email": "string|null",
  "primary_phone": "string|null",
  "birth_date": "YYYY-MM-DD|null",
  "birth_year": 1990,
  "age": 34,
  "age_by": "birth_date|birth_year|age",
  "gender": "male|female|other|null",
  "marital_status": "single|married|divorced|widowed|null",
  "source": "Assessment|Referral|...|Other|null",
  "source_details": "string|null",
  "occupation": "string|null",
  "mother_tongue": "string|null",
  "notes": "string|null",
  "last_follow_up_date": "YYYY-MM-DD|null",
  "next_follow_up_date": "YYYY-MM-DD|null",
  "marketing_level": "Guest|Preferred_Customer|Associate|Senior_Consultant|Success_Builder|Supervisor|Active_Supervisor|World_Team|Active_World_Team|GET|GET 2500|MIL|MIL 7500|PRESIDENT TEAM|Executive Presidents Team|Senior Executive Presidents Team|International Executive Presidents Team|Chief Executive Presidents Team|Chairmans Club|Founder Circle",
  "diamonds": 0,
  "stones": 0,
  "royalty_level": 0,
  "discount_level": 0,
  "years_active": 0,
  "mark_hughes_award": 0,
  "is_active": true,
  "coach_id": "user uuid|null",
  "referred_by": "user uuid|null",
  "supervisor_id": "user uuid|null",
  "world_team_id": "user uuid|null",
  "get_id": "user uuid|null",
  "mil_id": "user uuid|null",
  "president_team_id": "user uuid|null"
}
```

### Permissions & Scope
- Update by self for own profile; by coach/mentor for assigned members; RBAC `update.*`.
- Read via ABAC (future). Default: self/coach scope.

### UX Notes
- Show marketing level chips with colors.
- On level change, backend may auto-assign groups; reflect after refresh.
- Contacts/Addresses/Photos are managed via supporting flows.
