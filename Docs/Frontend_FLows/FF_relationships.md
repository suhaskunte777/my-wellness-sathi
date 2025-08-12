## Frontend Data Relationships — Overview

This document maps UI surfaces to server models and key relationships to guide navigation and state.

### Core Entities and Links
- User (`users.uuid`)
  - 1..1 ProfileDetail (`profile_details.user_uuid`)
  - 1..n DailyTracker (`daily_trackers.user_uuid`)
  - n..n Groups (via tags/assignment; planned endpoints)
  - 1..n Contacts (polymorphic; planned)
  - 1..n Addresses (polymorphic; planned)
  - 1..n Photos (polymorphic; planned)
- Prospect (`prospects.uuid`)
  - → User on convert (`converted_to_user_uuid`, `converted_at`)
  - 0..1 HealthData (polymorphic)
  - 0..n Measurements (polymorphic)
  - 0..n Photos (polymorphic)
- HealthData (`health_data.*`)
  - Polymorphic to User or Prospect
- Measurement (`measurements.*`)
  - Polymorphic to User or Prospect
- DailyTracker / DailyTrackerIntake
  - User → 0..n Trackers; Tracker → 0..n Intakes
- Group / MarketingLevel
  - ProfileDetail.marketing_level drives group auto-assignment (server-side)

### Navigation Map
- Dashboard
  - Prospects KPIs → `/dashboard/prospects` with filters
  - Measurements → `/dashboard/measurements`
  - Daily Trackers → `/dashboard/daily-trackers`
  - Quick Add actions dispatch to respective create pages
- Prospect Detail
  - Tabs: Overview, Health, Measurements, Contacts, Addresses, Photos
  - Convert → opens created member profile
- Profile Detail (Member)
  - Sections: Contacts, Addresses, Marketing Level, Assignments, Photos
  - Links to Health and Measurements

### Authorization Notes
- RBAC: `create.*`, `update.*`, `delete.*` for write operations
- ABAC (tags/groups) planned for read scope

### Offline-first Notes
- Client generates `uuid` on create for user-generated records
- Sync strategies: optimistic insert, reconcile on failure
