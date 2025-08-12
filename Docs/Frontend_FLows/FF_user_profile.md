## User Profile — Frontend Flow

### Objective
- Allow authenticated user to view/update account profile and password.

### Entry Points
- User Drawer → Profile
- Route: `/dashboard/profile`

### Screens
1) My Profile
- Show core user fields from `/api/user/profile` (via `UserController@show`)
- Actions:
  - Edit Profile → PUT `/api/user/profile`
  - Change Password → PUT `/api/user/password`
- Fields: name, email, mobile, birth_date, herbalife_id, business_owner, username (read-only), is_admin (read-only)
- Also show marketing/profile detail summary if available via `/api/profile-details/by-user/{userUuid}`

2) Edit Profile Dialog/Page
- Update only editable fields; validate email/phone; confirm before saving

3) Change Password Dialog
- Fields: current_password, new_password, confirm_password
- Enforce password policy (min length, complexity)

### API Mapping
- GET `/api/user/profile`
- PUT `/api/user/profile`
- PUT `/api/user/password`

### UX Notes
- After profile update, refresh local profile and profile-details summary.
- If multiple accounts on device, changes apply to active account only.
