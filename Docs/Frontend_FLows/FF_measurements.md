## Measurements — Frontend Flow

### Objective
- Record and visualize body metrics for prospects and members; compare progress.

### Relationships
- Polymorphic: `measurementable_type` = `User|Prospect`, `measurementable_id` = uuid
- Created by user scope; lists filtered by entity and coach scope

### Entry Points
- Dashboard → Measurements
- Prospect/Member detail → Measurements tab

### Screens
1) Measurements List
- Route: `/dashboard/measurements`
- Columns: entity (name + type chip), date, weight, bmi, body_fat, visceral_fat, muscle_mass, notes
- Filters: type (User/Prospect), date range, created_by (Me/Team), search by name
- Actions: View, Edit, Delete; Add Measurement

2) Measurement Detail / Edit
- Route: `/dashboard/measurements/:uuid`
- Show all captured fields; compute BMI if not provided
- Compare: pick two records for same entity → comparison view

3) Add Measurement (modal or page)
- Prefill entity from context when launched from entity detail
- Fields include: condition, age, height, weight, body_fat, bmi, visceral_fat, bmr_resting, body_age, trunk_fat, muscle_mass, belly_inches_top/bottom, notes

### API Mapping
- GET `/api/measurements`
- POST `/api/measurements`
- GET `/api/measurements/{uuid}`
- PUT `/api/measurements/{uuid}`
- DELETE `/api/measurements/{uuid}`

Create/Update payload
```json
{
  "uuid": "client-generated uuid",
  "measurementable_type": "User|Prospect",
  "measurementable_id": "uuid",
  "condition": "Empty Stomach|After Breakfast|After Lunch|After Dinner|2 Hours After Meal|1 Hour after Breakfast|null",
  "age": 34,
  "height": 170.0,
  "weight": 70.4,
  "body_fat": 22.1,
  "bmi": 24.3,
  "visceral_fat": 8.0,
  "bmr_resting": 1500,
  "body_age": 32,
  "trunk_fat": 12.3,
  "muscle_mass": 55.5,
  "belly_inches_top": 34.0,
  "belly_inches_bottom": 33.0,
  "notes": "string|null"
}
```

### UX Notes
- Compute BMI client-side when height and weight provided; allow override.
- Graphs: time series of weight, BMI, body fat per entity; delta arrows.
- Comparison: highlight improvements in green, regressions in red.
