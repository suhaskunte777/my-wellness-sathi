# 1. User Authentication — Detailed Specification

## 1.1 Login Page

- UI Elements:
    - Logo: "My Wellness Sathi" Logo
    - Main title with message:
        - My Wellness Sathi
        - Welcome back! Please login to your account.
    - Dropdown or UI component to select/switch between multiple accounts (if previously logged in).
    - Input field: username (text)
    - Input field: password (password type)
    - Checkbox: Remember Me
    - Button: Login
    - Link: Forgot Password?
    - Link: Forgot Username?
    - Legal content:
        - By continuing you agree to our [Terms of Use](/pages/legal/terms-of-use) and [Privacy Notice](/pages/legal/privacy-notice)
        - ©2025 All Rights Reserved. My Wellness Sathi

## 1.2 API Contract

- Login Endpoint:

```json
POST /api/auth/login
Content-Type: application/json
Body:
  {
    "username": "string",
    "password": "string",
    "remember_me": boolean
  }
Success Response (200):
  {
    "token": "JWT or Session Token",
    "expires_in": 3600, // seconds
    "user": {
      "id": "uuid",
      "username": "string",
      "name": "string",
      "roles": ["admin", "editor"],
      "remember_me": boolean
    }
  }
Failure Response (401):
  {
    "error": "invalid_credentials" | "too_many_attempts",
    "message": "Invalid username or password",
    "lockout_until": "2025-08-10T14:30:00Z" // only if locked
  }
```

- Authentication Type:
  - Preferred: Laravel Sanctum token stored in encrypted IndexedDB (never in plain localStorage).
  - Also we have to keep multiple users profiles in IndexedDB, for quick switch without password.(once logged in)

## 1.3 Login Flow

- User opens login page.
- User enters username & password.
- Optionally, user can select "Remember Me" checkbox.
- On Login click:
    - Validate that both fields are filled.
    - If empty → show inline validation error.
- Send API request to /api/auth/login.
- Handle API response:
  - 200 OK:
    - Save token and user profile in encrypted IndexedDB (never in plain localStorage).
    - Also keep in Pinia store (only non-sensitive fields).
    - Navigate to /dashboard.

  - 401 Unauthorized:
    - If error = too_many_attempts → show lockout message with unlock time.
    - Else → show "Invalid username or password".

  - Network Error: Show "Unable to connect to server" message.


## 1.4 Login Attempt Restrictions

- Backend rule:
    - 3 failed attempts → 1-hour lockout.
    - 5 failed attempts → 24-hour lockout.
- Scope: Per username.
- Additional Security: If more than 3 failed attempts → require CAPTCHA. (optional)
- If user is locked out, can reset password/get username from forgot password/forgot username page.

## 1.5 Multiple Accounts on Same Device

- Store each account's profile + token in encrypted IndexedDB under separate key:
- Check how we can keep active account in use and not mess with other accounts.

```json
accounts = {
  "<username>": { token, userProfile, expires_at }
}
```

- UI: Account switcher in right drawer.

- Switching account:
  - If token is still valid → instantly switch context in Pinia store.
  - If token expired → prompt for password.

## 1.6 Password Reset Flow

- Trigger: User clicks "Forgot Password" link.
- Show page:
    - Input: Email
    - Input: Username
    - Input: Birth Date
    - Button: Send Reset Link.
    - Link: Back to Login

- API:

```json
POST /api/auth/forgot-password  
Body: { "email": "string", "username": "string", "birth_date": "YYYY-MM-DD" }
Success Response (200):
  {
    "message": "Reset link sent to email"
  }
Failure Response (400):
  {
    "error": "invalid_request" | "user_not_found",
    "message": "Invalid email or username or birth date"
  }
```

- Backend sends reset email with token link.
- User clicks link → redirected to reset form:
- Page URL: /auth/reset-password?token=string
- Page UI:
    - Title: Reset Password
    - Message: Enter your new password below.
    - Input: New password
    - Input: Confirm password
    - Button: Reset Password
    - Link: Back to Login

- API:
```json
POST /api/auth/reset-password
Body: { 
    "token": "string", 
    "password": "newpassword",
    "confirm_password": "newpassword"
}
Success Response (200):
  {
    "message": "Password reset successfully"
  }
Failure Response (400):
  {
    "error": "invalid_request" | "invalid_token",
    "message": "Invalid token or password"
  }
```

## 1.7 Username Reset Flow

- Trigger: User clicks "Forgot Username" link.
- Show page:
    - Input: Email
    - Input: Birth Date
    - Input: Herbalife ID
    - Button: Send Reset Link.
- Backend sends username to email.

- API:

```json
POST /api/auth/forgot-username
Body: { "email": "string", "birth_date": "YYYY-MM-DD", "herbalife_id": "string" }
Success Response (200):
  {
    "message": "Username sent to email"
  }
Failure Response (400):
  {
    "error": "invalid_request" | "user_not_found",
    "message": "Invalid email or birth date or herbalife id"
  }
```

- API sends email with registered username.

## 1.8 Logout Flow

- Clear token and user profile from storage
- Request backend /api/auth/logout
- Clear Pinia state and redirect to /login

## 1.9 New User Flow

- If new user and not having account, show "New user? Request Account here" link.
- Clicking opens new user request page.
- Show page:
    - Input: Name
    - Input: Email
    - Input: Phone
    - Input: Herbalife ID
    - Button: Request Account
    - Link: Back to Login
- API:
    - POST /api/auth/new-user-request
    - Body: { "name": "string", "email": "string", "phone": "string", "herbalife_id": "string" }
    - Success Response (200):
        - { "message": "Account request sent successfully" }
    - Failure Response (400):
        - { "error": "invalid_request" | "user_not_found", "message": "Invalid name or email or phone or herbalife id" }

## 1.10 Session Expiration

- If token expires:
  - Attempt refresh via /api/auth/refresh
  - If refresh fails → logout & redirect to /login

## 1.11 Privacy & Terms

- Show links at login page footer.
- Clicking opens external page.
- On both pages, show back button to go back to login page / home page.
- Always show compliance: Checkbox “By continuing you agree to our [Terms of Use](/pages/legal/terms-of-use) and [Privacy Notice](/pages/legal/privacy-notice)” on login page.

## 1.12 Accessibility

- All forms are keyboard navigable.
- Labels associated with inputs.
- Error messages are screen-reader compatible.

## 1.13 Security

- All sensitive data is encrypted in IndexedDB.
