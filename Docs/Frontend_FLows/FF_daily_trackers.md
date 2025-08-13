## Daily Trackers — Frontend Flow

### Objective
- Help members log daily wellness, and enable coaches to validate and nudge.

### Relationships
- `daily_trackers.user_uuid` -> `users.uuid`
- Intakes: `daily_tracker_intakes` linked by `daily_tracker_uuid` (planned UI)
- Audit: created_by/updated_by/validated_by

### Entry Points
- Dashboard → Daily Wellness cards
- Menu → `/dashboard/daily-trackers`
- Member Profile → Today’s tracker quick add

### Screens
1) Daily Trackers List
- Route: `/dashboard/daily-trackers`
- Columns: date, member, water_intake, sleep_hours, exercise_done, exercise_duration, mood, mindset, energy, is_validated, streak_count, actions
- Filters: date/range, validated status, exercise done, coach scope
- Actions: View, Edit, Validate, Delete; Add Today’s Entry

2) Tracker Detail / Edit
- Route: `/dashboard/daily-trackers/:uuid`
- Fields: water_intake, sleep_hours, exercise flags, mood/mindset/energy (1–10), notes, validation section
- Validate action (coach-only): POST validate → sets `validated_by`, `validated_at`, `is_validated`

3) Add Today / Backfill
- Route: `/dashboard/daily-trackers/new?date=YYYY-MM-DD`
- Default date to today; allow backfill with date picker

4) Intakes (Planned)
- Inline list under a tracker: supplement_type, taken_at, quantity, notes
- Add/Edit/Delete intake entries

### API Mapping
- GET `/api/daily-trackers`
- POST `/api/daily-trackers`
- GET `/api/daily-trackers/{uuid}`
- PUT `/api/daily-trackers/{uuid}`
- DELETE `/api/daily-trackers/{uuid}`
- GET `/api/daily-trackers/by-date`
- POST `/api/daily-trackers/{dailyTracker}/validate`

Create/Update payload (tracker)
```json
{
  "uuid": "client-generated uuid",
  "user_uuid": "...",
  "date": "YYYY-MM-DD",
  "water_intake": 0,
  "sleep_hours": 0,
  "exercise_done": false,
  "exercise_duration": 0,
  "mood_rating": 0,
  "mindset_rating": 0,
  "energy_level": 0,
  "streak_count": 0,
  "notes": "string|null"
}
```

### UX Notes
- Coverage bar for today: entries / total active members.
- Quick validate all pending (bulk) for today (coach scope).
- Streak logic derived on backend or computed; show badge on member profile.

