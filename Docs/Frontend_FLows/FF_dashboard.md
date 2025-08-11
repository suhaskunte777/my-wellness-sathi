# Dashboard

This document specifies the Dashboard for the client (Quasar) app, aligned with the current server API and data model. It is designed primarily for Coaches and above, with scoped views and drill-down actions.

## UI Component (Coach and above)
- Role perspective switcher: view "as" Member, Coach, Mentor (default to highest level the user holds)
- View switcher: use switcher UX
<!-- - Global filters: date range (Today, Yesterday, This Week, This Month, Custom), scope (Me, My Directs, My Team), timezone, quick refresh -->
- All stats are clickable to navigate to filtered lists
- Respect RBAC/ABAC: only show stats the user can access

---

## Row 1: Daily Score and Today at a glance

- Yesterday Score (Circle) – computed from Daily Tracker, Activities, Prospects, Measurements
- Today Score (Circle) – computed from Daily Tracker, Activities, Prospects, Measurements
- Today summary chips:
  - Water intake progress vs target (Show for members view)
  - Sleep last night vs target (Show for members view)
  - Exercise minutes and done/not done (Show for members view)
  - Streak days (count of consecutive days with Daily Tracker entry) (Show for members view)
  - Validations today (coach validations done/remaining) (Show for members view)
  <!-- - Today's Activities (Show for members view)
  - Today's New Prospects Count (Show for members view)
  - Today's New Measurements Count (Show for members view)
   -->

Definitions (purely derived, no new API required):
- Score per day (0–100). Tunable weights; default:
  - Shake 2 times : 40
  - Water (target 3000ml): min(water/3000,1) × 20
  - Sleep (target 7.5h): min(sleep_hours/7.5,1) × 20
  - Exercise: (exercise_done ? 5 : 0) + min(exercise_duration/30,1) × 5
  - Mood: (mood_rating/10) × 10
  - Mindset: (mindset_rating/10) × 10
  - Energy: (energy_level/10) × 10
- Streak: number of consecutive days with `daily_trackers` entries ending today (per user; aggregate card shows: members with 7+ day streak)

Quick actions:
- Add Today’s Tracker (self)
- Review & Validate Trackers (team) (Coach view only)
- Nudge members without today’s entry (bulk action) (Coach view only)

Notes:
- "Major/Minor Time" depends on Activities.
- Major Time: Activities, Scanning, Health Check Camp, Road Show, Afresh Party, MIW, Business Opportunity, Passive, Social Media Ads, Bulk Messaging, Bulk Emailing, Poster - Wall / Auto / Banner
- Minor Time: Training Taken, Club hours, excersize
- Other Time: Sleep, Daily life activities

---

## Row 2: Activities (Planned)

Status: Planned. Depends on Activities model and endpoints.

Intended UI once available:
- Links: Start New Activity, Activity List (paginated, filters preserved)
- KPIs (Today / This Week / This Month):
  - Time spent in Activities (Active vs Passive)
  - People invited
  - Prospects created from Activities
  - Planned Activities with status (Scheduled, In Progress, Completed, Cancelled)
- Leaderboard (Top 5 by Activity time or Invites)

Quick actions:
- Start timer / Log activity
- Share activity to other coaches (with approval flow if outside hierarchy)
- Bulk invite via vCard/CSV import

---

## Row 3: Prospects

Links:
- Add New Prospect
- Prospect List (paginated, filters persisted)

KPIs (scoped by filter: Me / My Directs / My Team):
- Prospects Converted This Month: count where `converted_at` in current month
- New Prospects Today / Yesterday: `created_at` = today/yesterday
- Follow-up Planned Today: `next_follow_up_date` = today
- Follow-up Carry Forward: `next_follow_up_date` < today
- Follow-up Completed Today: `last_follow_up_date` = today
- Follow-up Due Tomorrow: `next_follow_up_date` = tomorrow
- By Lead Stage (badge group): New, Contacted, Interested, Qualified, Hold, Lost, Archived
- Average Conversion Probability (mean of `conversion_probability` where not null)

Drill-down filters applied on click:
- Date range (created_at/converted_at/next_follow_up_date)
- Lead stage
- Coach assignment (`coach_id` = me or my team)
- Active flag

Quick actions:
- Add prospect, Convert to Member (when eligible)
- Start today’s follow-ups (opens list filtered to today/today+overdues)
- Export filtered list (CSV)

Data sources:
- GET /api/prospects
- GET /api/prospects/by-lead-stage/{stage}
- GET /api/prospects/needs-follow-up (optional shortcut for overdue/today)

---

## Row 4: Measurements

Links:
- Add New Measurement
- Recent Measurements (paginated list)

KPIs:
- New Measurements Today / Yesterday: `created_at` counts
- This Month: Prospects Measured: `measurementable_type = Prospect`
- This Month: Members Measured: `measurementable_type = User`
- Average BMI (last 30 days) vs previous 30 days (delta arrow)
- Top Progress (last 30 days): top 5 by absolute weight change
- First-Time Measurements This Month: distinct entities whose first ever measurement is within current month

Drill-down filters:
- Entity type (Prospect/User)
- Date range
- Coach scope (by `created_by` or by entity’s `coach_id` when available)

Quick actions:
- Add measurement for selected profile
- Compare two measurements (opens compare view)
- Export recent list

Data sources:
- GET /api/measurements

---

## Row 5: Daily Wellness (Trackers)

Links:
- Add Today’s Entry (self)
- Daily Trackers (by date)

KPIs:
- Tracker Coverage Today: members with an entry today / total active members in scope
- Avg Water Intake Today (ml) and % hitting target (≥3000ml)
- Avg Sleep Last Night (hours)
- Exercise: total minutes today and members who exercised
- Validated Today: trackers validated (count) vs pending
- Streak Champions: members with ≥7-day streak

Drill-down filters:
- Date / range
- Coach scope (Me / Directs / Team)
- Validated status, exercise done

Quick actions:
- Validate all pending for today (bulk)
- Remind members without entry (bulk)

Data sources:
- GET /api/daily-trackers
- GET /api/daily-trackers/by-date
- POST /api/daily-trackers/{dailyTracker}/validate

---

## UX/Technical Notes

- Empty states: show helpful CTAs (Add Prospect, Add Measurement, Add Today’s Tracker)
- Loading: skeletons for cards; optimistic updates for quick actions
- Timezone: compute “today/yesterday” using user’s timezone
- Permission-aware: hide counts the user isn’t authorized to view
- Accessibility: high-contrast and keyboard navigable; dark mode by design
- Performance: aggregate on server when possible; otherwise paginate & lazy-load drill-downs

## Mapping to current API and models

- Prospects: uses fields `lead_stage`, `next_follow_up_date`, `last_follow_up_date`, `converted_at`, `conversion_probability`, `coach_id`, `created_by`, timestamps
- Measurements: uses polymorphic `measurementable_type/_id`, fields `weight`, `bmi`, timestamps, `created_by`
- Daily Trackers: `water_intake`, `sleep_hours`, `exercise_done`, `exercise_duration`, `mood_rating`, `mindset_rating`, `energy_level`, `validated_by`, `validated_at`, `date`, `created_by`

No schema changes required to render the above. Activities row is marked Planned and will be enabled once Activities module is available.
