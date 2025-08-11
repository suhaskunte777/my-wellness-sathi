# Suggest frontend flow and UI design
_Exported on 11/08/2025 at 17:04:27 GMT+5:30 from Cursor (1.4.2)_

---

**User**

You know very well application developed so far by analysing @Wellness and Lead Management.md and @server/ @api.php .
Also you can check @Frontend_FLows/ so far.

Base on it can you suggestion flow for other models and their relationship, usecases in @server/ can you suggest frontend flow and UI. 

Think properly

---

**Cursor**

I’m going to scan the backend to confirm existing models, controllers, and patterns (UUIDs, auditing, ABAC/tags, policies). This lets me propose new flows that fit the current architecture and naming.

Quick scan of key backend directories and docs to ground suggestions in the actual code. Then I’ll propose backend model flows, relationships, and the corresponding frontend flows/UI.

I reviewed the implemented models, controllers, enums and routes, plus the Frontend Flows docs. The domain skeleton is solid: UUID primaries, Auditing, SoftDeletes, polymorphic relations for user/prospect data, REST controllers with scoped filters, and plans for ABAC via tags and groups. Below are concise, concrete proposals that fit the current architecture and naming.

### Backend: new models, relationships, use cases
All “user-facing” models below should: use UUID primary, SoftDeletes, OwenIt Auditing; have `created_by`, `updated_by`, `deleted_by`; route key = `uuid`; casts with enums; policies; and request validators.

- Activities
  - Purpose: Track planned/actual time, invites, participants, prospect distribution, attachments.
  - Tables:
    - `activities`:
      - schedule/actual start_end, location fields, `type` (enum), `activity_category` (enum), `distribution_method` (enum), counts for invites/accepted, notes, status (Scheduled/In Progress/Completed/Cancelled).
      - FKs: `created_by`, `approved_by` (for cross-hierarchy approval), optional `club_uuid`.
    - `activity_participants` (pivot): `activity_uuid`, `user_uuid`, role enum (Organizer/Coach/Member/Guest), `invited_at`, `accepted_at`, `status` (Invited/Accepted/Declined), `is_external`.
    - `activity_prospects` (pivot): `activity_uuid`, `prospect_uuid`, `assigned_to_uuid` (coach), `source_detail`, status.
  - Relationships: Activity hasMany participants, hasMany prospects; belongsTo club; hasMany photos.
  - Endpoints:
    - REST `activities` + custom: `/start`, `/complete`, `/cancel`, `/share`, `/participants/{id}/accept`, `/prospects/{id}/assign`.
  - Enums: `ActivityType`, `ActivityCategory`, `ActivityStatus`, `ProspectDistributionMethod`.

- Prospect assignment history
  - Purpose: Keep immutable history of who got which prospect when; current coach stays on `prospects.coach_id`.
  - Table: `prospect_assignments`:
    - `prospect_uuid`, `assigned_to_uuid`, `assigned_by_uuid`, `assigned_at`, `reason`, `is_active`.
  - Flow: Create new assignment row and flip `is_active`; update `prospects.coach_id`.
  - Endpoints: `POST /api/prospects/{uuid}/assign`, `GET /api/prospects/{uuid}/assignments`.

- Group membership (ABAC foundation)
  - Purpose: Assign users/prospects/clubs to groups and drive ABAC with tags.
  - Table: `group_assignments`:
    - `group_uuid`, `assignable_type`, `assignable_id`, `is_auto_assigned`, `assigned_by`.
  - Service: Auto-assign groups when `profile_details.marketing_level` or club changes; run on save via observer + nightly job.
  - Endpoints: REST `group-assignments` + `POST /api/groups/{group}/assign`, `DELETE /api/groups/{group}/assign/{assignable}`.

- Family/Household
  - Purpose: “User belongs to family” and can view/switch profiles.
  - Tables:
    - `families` (uuid, name, created_by)
    - `family_members` (pivot): `family_uuid`, `user_uuid`, role enum (Owner/Adult/Child), permissions (view/update).
  - Endpoints: REST `families` and `family-members` + `POST /switch/{userUuid}` to set active context.

- Clubs and attendance
  - Tables:
    - `clubs` (uuid, name, description, timezone, created_by)
    - Use existing polymorphic `addresses`, `contacts`, `photos` with `addressable/contactable/photoable = Club`.
    - `club_sessions` (schedule/actual, status, created_by)
    - `club_attendance` (session_uuid, user_uuid/prospect_uuid, status, notes).
  - Endpoints: REST `clubs`, `club-sessions`, `club-attendance`.

- Programs, batches, enrollments
  - Tables:
    - `programs` (uuid, name, type enum, description)
    - `program_batches` (uuid, program_uuid, code, schedule start/end, capacity, status)
    - `program_enrollments` (uuid, batch_uuid, user_uuid/prospect_uuid, enrolled_at, status)
    - `program_sessions` and `program_attendance` (if needed)
  - Endpoints: REST `programs`, `program-batches`, `program-enrollments`, `program-sessions`.

- Inventory (phase-able)
  - Tables:
    - `products` (id or uuid; since not user-generated, bigint id acceptable per rule; but consistent uuid OK)
    - `inventory_locations` (club_uuid or user_uuid)
    - `stock_movements` (product_id, location, qty, type: In/Out/Adjust, ref_type/id)
  - Endpoints: `products`, `inventory-locations`, `stock-movements`.

- Finance (phase-able)
  - Tables:
    - `invoices` (uuid, bill_to_type/id, total, status)
    - `payments` (uuid, invoice_uuid, amount, method, status)
    - `transactions` (uuid, type, ref, amount)
  - Endpoints: `invoices`, `payments`.

- Assessments/Surveys
  - Tables:
    - `assessment_templates`, `assessment_questions`, `assessment_responses` (responseable: User/Prospect), `assessment_answers`.
  - Endpoints: `assessments` (templates), `assessment-responses`.

- Notifications and preferences
  - Use Laravel Notifications; add tables:
    - `push_tokens` (user_uuid, device_id, token, platform)
    - `notification_preferences` (user_uuid, channel enums, flags)
  - Endpoints: `push-tokens`, `notification-preferences`, `notifications` (list/dismiss).

- Photos: lifecycle and compression
  - Add `expires_at` (nullable), `processed_variants` (json).
  - Background job: compress to webp, generate variants; prune expired.
  - Endpoints: `photos` REST + `POST /api/photos/{uuid}/make-primary`.

- Address validation
  - Service: Parse `google_map_link` into lat/long; optional third‑party for pincode/city normalization; run in FormRequest.

- Contacts verification
  - Normalizers/validators for phone/email; webhook or scheduled job to verify flags.

- User settings and consent
  - Tables:
    - `user_settings` (theme, timezone, language, measurement_units, marketing opt-ins)
    - `consents` (user_uuid, type enum, granted_at, withdrawn_at)
  - Endpoints: `user-settings`, `consents`.

### Backend: policies, queries, events, jobs
- Policies: Create for all new models mirroring existing CRUD roles, with ABAC for view by tags/groups.
- Filters: Consistent query params: `?date_from&date_to&status&coach_id&scope=me|directs|team&search&tags[]&with[]=relation`.
- Events/Jobs:
  - On `ProfileDetail` marketing_level change → AutoAssignGroups job.
  - On Activity share outside hierarchy → ActivityApprovalRequested notification.
  - PhotoUploaded → PhotoProcess job; PhotoExpiredPrune (daily).
- Caching: Long-cache `marketing_levels`; memoize group criteria rules.
- Observers: Fill `created_by/updated_by`; enforce UUID.

### ABAC/RBAC strategy
- RBAC: Use Spatie roles for create/update/delete; destroy for super admin only (as defined).
- ABAC: Attach Spatie tags to:
  - `groups` (e.g., marketing bands, regions, clubs)
  - data records (`prospects`, `profile_details`, `activities`, `program_batches`) via `taggables`.
- Policy read checks:
  - A user can view a record if record’s tags intersect with user’s group tags, or user has role with `*.all`.
  - Coach scope: allow where `coach_id = auth()->id()` or within direct/team scope.

### Frontend flows and UI (Quasar)
Use same UX patterns as existing: list → filters → detail → edit dialogs; Pinia stores per domain; keep role-aware UI and tag-aware visibility.

- Activities
  - Pages:
    - ActivitiesList: filters (date range, type, status, scope), bulk export.
    - ActivityForm: wizard (Schedule → Location → Participants → Prospects → Review).
    - ActivityDetail: timeline, participants, prospects, attachments, status actions.
  - Quick actions: Start/Complete/Cancel, Share, Bulk invite (vCard/CSV).
  - Components: `ActivityCard`, `ParticipantPicker`, `ProspectPicker`, `LocationField`.

- Prospects (enhancements)
  - Add “Assign” action with history panel.
  - “Follow-ups” smart list: today, overdue, tomorrow.
  - Convert to user → opens ProfileDetail create prefilled.

- Group membership
  - GroupAssignmentManager in user/prospect detail.
  - Auto-assigned badges; manual override chips.

- Family
  - FamilyManager (create, invite members); ProfileSwitcher shows family members; switch context without logout.

- Clubs
  - ClubList + ClubDetail (address, contacts, sessions, photos).
  - Session calendar and attendance check-in; export attendance.

- Programs/Batches
  - ProgramList, BatchList (per program), EnrollmentManager.
  - Batch calendar and attendance; progress view; export enrollments.

- Measurements
  - Compare view (select two); trend charts (BMI, weight); filters by entity, date.

- Daily Tracker
  - Today’s quick entry; intakes editor with presets; streak visualization.
  - Coach validation queue with bulk validate.

- Surveys/Assessments
  - Template builder (admin); Respond flow for user/prospect; results summary.

- Inventory
  - Product catalog, stock by location, move/adjust; scan-and-add (QR).

- Finance
  - Invoice list, payment capture; simple reports.

- Notifications
  - Preferences page; device opt-in; in-app inbox.

- Settings
  - UserSettings: theme, timezone, language; Consent manager.

- Navigation and routing
  - Routes under `/activities`, `/clubs`, `/programs`, `/batches`, `/inventory`, `/finance`, `/surveys`, `/settings`.
  - Keep existing `prospects`, `measurements`, `daily-trackers` as-is; add new child routes for assignment/history/validation.

- Pinia stores
  - `useActivityStore`, `useGroupStore`, `useFamilyStore`, `useClubStore`, `useProgramStore`, `useInventoryStore`, `useFinanceStore`, `useSurveyStore`, `useNotificationStore`, `useUserSettingsStore`.
  - Reuse axios boot; include `scope` and tag filters in getters.

- UX notes
  - Offline-first for trackers, measurements, activities logging: generate UUID client-side; queue sync.
  - Consistent filter persistence via route query params.
  - Dark mode by default; keyboard navigable forms; skeleton loaders; optimistic UI for quick actions.

### Minimal API shape examples (naming aligned)
- Activities:
  - GET/POST/PUT/DELETE `/api/activities`
  - POST `/api/activities/{uuid}/start|complete|cancel|share`
  - POST `/api/activities/{uuid}/participants`, DELETE same
  - POST `/api/activities/{uuid}/prospects`, POST `/assign`
- Prospect assignment:
  - POST `/api/prospects/{uuid}/assign`
  - GET `/api/prospects/{uuid}/assignments`
- Groups:
  - POST `/api/groups/{uuid}/assign`, DELETE `/assign/{type}/{id}`
- Family:
  - GET/POST `/api/families`, POST `/api/families/{uuid}/members`, POST `/api/families/switch/{userUuid}`
- Clubs, Programs, Batches, Enrollments: standard `apiResource` + by-date/by-status filters.

### Implementation order (pragmatic)
1) Activities core + ProspectAssignment + GroupAssignment (unlocks Dashboard Row 2 and ABAC).
2) Clubs and Programs/Batches (attendance, enrollments).
3) Surveys, Inventory, Finance, Notifications, Settings/Consent.

Status update:
- Parsed existing models/controllers and FE flows to ensure proposals align: UUID keys, Auditing, SoftDeletes, enums, polymorphs, REST and filter patterns. Proposed backend models, endpoints, and FE screens that slot into current architecture without breaking patterns.

- Added high-level flows for Activities, Groups/ABAC, Assignments, Clubs, Programs, plus secondary domains; mapped to concrete endpoints and Quasar screens.

- Emphasized auto-assignment, approvals, and background jobs to keep UX snappy and data consistent.

Summary:
- Proposed concrete backend models and endpoints for Activities, assignment history, group membership, family, clubs, programs/batches/enrollments, surveys, inventory, finance, notifications, user settings, plus photo lifecycle and address/contact validation.
- Mapped these to Quasar UI flows: lists, forms, detail views, and dashboards; identified Pinia stores and reusable components.
- Preserved your conventions: UUIDs, Auditing, SoftDeletes, enums, policies, and REST naming; integrated ABAC with groups/tags and coach-scoped queries.
- Suggested a practical build order to deliver value incrementally (Activities → Groups/Assignments → Clubs/Programs).

