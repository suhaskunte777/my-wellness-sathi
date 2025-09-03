# Wellness and Lead Management Application 

## Table of Contents
- [Wellness and Lead Management Application](#wellness-and-lead-management-application)
  - [Table of Contents](#table-of-contents)
  - [System Overview](#system-overview)
    - [Application Purpose](#application-purpose)
    - [Core Functionalities](#core-functionalities)
  - [Technology Stack](#technology-stack)
    - [Backend Framework](#backend-framework)
    - [Backend Development Tools](#backend-development-tools)
    - [Frontend Framework \& Packages](#frontend-framework--packages)
  - [User Authentication](#user-authentication)
  - [User Roles \& Permissions](#user-roles--permissions)
    - [Package for User and Access Management](#package-for-user-and-access-management)
    - [Primary Roles](#primary-roles)
    - [Access Control \& Permission Structure (RBAC + ABAC)](#access-control--permission-structure-rbac--abac)
  - [User and Related Migrations](#user-and-related-migrations)
    - [Core User Migration](#core-user-migration)
    - [Required Spatie Permission Migrations](#required-spatie-permission-migrations)
    - [Required Spatie Tags Migrations](#required-spatie-tags-migrations)
    - [Laravel Sanctum Migration](#laravel-sanctum-migration)
    - [Password Reset Migration](#password-reset-migration)
    - [Laravel Auditing Migration](#laravel-auditing-migration)
  - [Permission Definitions](#permission-definitions)
    - [Core Permissions](#core-permissions)
    - [Role-Permission Mappings](#role-permission-mappings)
  - [Security Considerations](#security-considerations)
    - [Password Policy](#password-policy)
    - [Token Management](#token-management)
    - [Rate Limiting](#rate-limiting)
    - [Audit Trail](#audit-trail)
  - [API Authentication Endpoints](#api-authentication-endpoints)
  - [Prospect and Related Migrations](#prospect-and-related-migrations)
  - [User and Prospect Shared Migritions](#user-and-prospect-shared-migritions)
    - [Profile Details](#profile-details)
    - [User Groups](#user-groups)
    - [Marketing Level](#marketing-level)
    - [Contacts](#contacts)
    - [Addresses](#addresses)
    - [Photos](#photos)
    - [Health](#health)
  - [Activities](#activities)
  - [Stock Management System](#stock-management-system)
    - [Core Concepts](#core-concepts)
    - [Database Schema](#database-schema)
    - [Key Features](#key-features)
    - [Business Logic](#business-logic)
    - [API Endpoints](#api-endpoints)
  - [Club Management System](#club-management-system)
      - [Shake Counter Implementation:](#shake-counter-implementation)
  - [Calendar Management System](#calendar-management-system)
    - [Core Concepts](#core-concepts-1)
    - [Database Schema](#database-schema-1)
      - [Calendar Events](#calendar-events)
      - [Calendar Participants](#calendar-participants)
      - [External Calendar Integrations](#external-calendar-integrations)
      - [Time Tracking System](#time-tracking-system)
      - [Time Tracking Preferences](#time-tracking-preferences)
    - [Recurrence Patterns](#recurrence-patterns)
      - [Supported Recurrence Types:](#supported-recurrence-types)
      - [Recurrence Rule Examples:](#recurrence-rule-examples)
    - [Key Features](#key-features-1)
      - [1. Calendar Integration](#1-calendar-integration)
      - [2. Smart Scheduling](#2-smart-scheduling)
      - [3. Time Tracking](#3-time-tracking)
      - [4. Event Types and Integration](#4-event-types-and-integration)
      - [5. Notifications and Reminders](#5-notifications-and-reminders)
      - [6. Reporting and Analytics](#6-reporting-and-analytics)
    - [API Endpoints](#api-endpoints-1)
      - [Calendar Events](#calendar-events-1)
      - [External Calendar Integration](#external-calendar-integration)
      - [Time Tracking](#time-tracking)
      - [Calendar Views and Reports](#calendar-views-and-reports)
    - [Business Logic](#business-logic-1)
      - [1. Event Creation and Management](#1-event-creation-and-management)
      - [2. Recurrence Handling](#2-recurrence-handling)
      - [3. External Calendar Sync](#3-external-calendar-sync)
      - [4. Time Tracking Integration](#4-time-tracking-integration)
      - [5. Notification System](#5-notification-system)
    - [Frontend Integration](#frontend-integration)
      - [Calendar Views](#calendar-views)
      - [Time Tracking Interface](#time-tracking-interface)
      - [Integration Features](#integration-features)
    - [Security and Permissions](#security-and-permissions)
      - [Access Control](#access-control)
      - [Data Privacy](#data-privacy)
    - [Implementation Phases](#implementation-phases)
      - [Phase 1: Core Calendar (Weeks 1-4)](#phase-1-core-calendar-weeks-1-4)
      - [Phase 2: Advanced Features (Weeks 5-8)](#phase-2-advanced-features-weeks-5-8)
      - [Phase 3: Integration \& Analytics (Weeks 9-12)](#phase-3-integration--analytics-weeks-9-12)
  - [Implementation Status](#implementation-status)
    - [✅ Completed Features](#-completed-features)
    - [🚧 In Progress / Planned Features](#-in-progress--planned-features)

## System Overview

### Application Purpose
A comprehensive wellness industry platform serving four primary user types:
- **Coaches**: Wellness professionals who help others achieve health goals
- **Members**: Individuals progressing toward health goals
- **Mentors**: Primarily Coaches but also successful business leaders providing guidance
- **Guest**: Individuals who are prospect members of the organization, they are not registered yet so can't access any content. Those added by admin or coach will be called as Guest. Can be converted to member by admin or coach, once they take our product/program/service.

### Core Functionalities
- User Authentication and Authorization
- Prospect management and tracking 
- Member measurement and tracking
- Club Management and attendance
- Stock and inventory management
- Financial tracking and reporting
- Assessment, survey, TTP and Camp tools 
- Product/Service/Program/Batch management
- Marketing level management
- Communication and notification systems

## Technology Stack

- Backend is API only for application
- Also backend will have some UI for Horizon and Pulse
- Frontend will be Separate repository based on Quasar
- we will use uuid wherever data is getting generated by user, to facilitate offline creation of records.
- Models which only can create in online mode or not created by user can still continue using bigint id.
- SoftDelete all the tables, if excepting any table mention specifically why softDelete is not required their.
- Application should support dark mode (using Quasar)
- Use Quasar css classes for styling

### Backend Framework
- **Laravel 12** with PHP 8.4
- **MySQL 8.0+** for primary database
- **Redis** for caching and session management
- **Laravel Sanctum** for API authentication
- **Laravel Auditing** for audit trails
- **Spatie Laravel Permission** for RBAC
- **Spatie Laravel Tags** for tags ABAC
- **Laravel Backup** for data backup
- **Laravel Telescope** for debugging
- **Laravel Horizon** for queue management
- **Laravel Pulse** for application monitoring

### Backend Development Tools
- **Laravel Pail** for file storage
- **Laravel Pint** for code formatting
- **Larastan** for static analysis
- **PHPUnit** for testing
- **Laravel IDE Helper** for development assistance

### Frontend Framework & Packages
- **Vue.js 4** 
- **Node.js 22**
- **PNPM** for package management
- **Quasar framework 2.18**
- **Vue router**
- **Pinia** Store Management
- **Axios** 
- **vcards-js**
- **dayjs**
- **html2pdf.js**
- **uuid** - for uuid generation

## User Authentication

- Use Token based authentication provided by *laravel/sanctum*
- User may have same email and mobile for multiple accounts
- We will use username (Unique across users) and password for authentication
- User belongs to family for viewing / switching other profiles
- Frontend should have quick profile switching support for authenticated user as Google provides for Gmail, Maps etc.
- For forget password, use username along with birth date and email.
- For forget username, use birth date, email, herbalife ID
- User only be created by invitation only

## User Roles & Permissions

### Package for User and Access Management
- Use *laravel/sanctum* for token based authentication (not cookie based)
- Use *spatie/laravel-permission* package for Role and Permission Management
- Use *spatie/laravel-tags* package for Tag Management

### Primary Roles
1. **Super Admin** - Full system access (Only one seeded by seeders)
2. **Admin** - Organization-level management (User is admin for his Organization. User will get manage organization feature after Active World Team. User org is members joined by him and his downline members)
3. **Mentor** - Business leadership and guidance (User is above GET marketing level is called as mentor)
4. **Coach** - Wellness coaching and member management (User in helping others to achieve their health goals is called as coach until they achieve GET marketing level)
5. **Member** - Personal wellness journey tracking (User is tracking their own wellness journey)
6. **Guest** - Limited access for assessments (User is not a member of the organization)

### Access Control & Permission Structure (RBAC + ABAC)

- Role Based Access Control
  - Used for Create, Update and Delete permission

- Attribute Based Access Control
  - Used for view can view given Model (data)
  - Tags will be used as attribute to control this behavior
  - User Groups will be assigned tags

## User and Related Migrations

### Core User Migration
```php
Schema::create('users', function (Blueprint $t) {
    $t->uuid('uuid')->primary();
    $t->string('name');
    $t->date('birth_date');
    $t->string('herbalife_id')->nullable();
    $t->boolean('business_owner')->default(0);
    $t->string('username')->unique();
    $t->string('email');
    $t->timestamp('email_verified_at')->nullable();
    $t->string('mobile');
    $t->timestamp('mobile_verified_at')->nullable();
    $t->string('password');
    $t->boolean('is_admin')->default(false);
    $t->foreignUuid('invited_by')->nullable()->constrained('users', 'uuid')->onDelete('set null');
    $t->foreignUuid('created_by')->nullable()->constrained('users', 'uuid')->onDelete('cascade');
    $t->rememberToken();
    $t->timestamps();
    $t->softDeletes();
});
```

### Required Spatie Permission Migrations
 - We will use default tables from spatie/laravel-permission

### Required Spatie Tags Migrations
- Use spatie/laravel-tags default tags tables

### Laravel Sanctum Migration
```php
Schema::create('personal_access_tokens', function (Blueprint $t) {
    $t->id();
    $t->morphs('tokenable');
    $t->string('name');
    $t->string('type');
    $t->string('hash', 64)->unique();
    $t->text('abilities')->nullable();
    $t->timestamp('last_used_at')->nullable();
    $t->timestamp('expires_at')->nullable();
    $t->timestamps();
});
```

### Password Reset Migration
```php
Schema::create('password_reset_tokens', function (Blueprint $t) {
    $t->string('username')->primary();
    $t->string('token');
    $t->timestamp('created_at')->nullable();
});
```

### Laravel Auditing Migration
- We will use default migration from auditing package.

## Permission Definitions

### Core Permissions
```php
// Create permissions
'create.all', 'create.own'

// Update permissions  
'update.all', 'update.own'

// Delete permissions
'delete.all', 'delete.own'

// Destroy permission (only for super admin)
'destroy.all'
```

### Role-Permission Mappings
```php
// Super Admin - All permissions
'Super Admin' => ['create.all', 'update.all', 'delete.all', 'destroy.all']

// Mentor - can create all, update all, delete all, but cannot destroy
'Mentor' => ['create.all', 'update.all', 'delete.all']

// Coach - can create own, update own/all, delete own
'Coach' => ['create.own', 'update.own', 'update.all', 'delete.own']

// Member - can create own, update own, delete own
'Member' => ['create.own', 'update.own', 'delete.own']

// Guest - no write permissions, will use ABAC for read access
'Guest' => []
```

## Security Considerations

### Password Policy
- Minimum 8 characters
- Must contain uppercase, lowercase, number, and special character
- Password history: 3 previous passwords
- Password expiration: 90 days

### Token Management
- Access token expiration: 24 hours
- Refresh token expiration: 30 days
- Maximum active tokens per user: 3

### Rate Limiting
- Login attempts: 5 per 15 minutes
- API requests: 1000 per hour per user
- Password reset: 3 per hour per email

### Audit Trail
- All user actions logged
- Sensitive operations require additional verification
- Failed authentication attempts logged
- Token usage tracked

## API Authentication Endpoints

**Currently Implemented:**
```php
// Authentication Routes
POST /api/register  (invitation only)
POST /api/login
POST /api/logout
POST /api/forgot-password
POST /api/forgot-username
POST /api/reset-password
GET /api/verify-email/{id}/{hash}
POST /api/email/verification-notification
```

**API Resource Endpoints:**
```php
// Prospect Management
GET /api/prospects
POST /api/prospects
GET /api/prospects/{prospect}
PUT /api/prospects/{prospect}
DELETE /api/prospects/{prospect}
GET /api/prospects/needs-follow-up
GET /api/prospects/by-lead-stage/{stage}
POST /api/prospects/{prospect}/convert

// Profile Details Management
GET /api/profile-details
POST /api/profile-details
GET /api/profile-details/{profileDetail}
PUT /api/profile-details/{profileDetail}
DELETE /api/profile-details/{profileDetail}
GET /api/profile-details/by-user/{userUuid}
GET /api/profile-details/needs-follow-up
POST /api/profile-details/convert-from-prospect/{prospectUuid}/{userUuid}

// Health Data Management
GET /api/health-data
POST /api/health-data
GET /api/health-data/{healthData}
PUT /api/health-data/{healthData}
DELETE /api/health-data/{healthData}

// Measurement Management
GET /api/measurements
POST /api/measurements
GET /api/measurements/{measurement}
PUT /api/measurements/{measurement}
DELETE /api/measurements/{measurement}

// Daily Tracker Management
GET /api/daily-trackers
POST /api/daily-trackers
GET /api/daily-trackers/{dailyTracker}
PUT /api/daily-trackers/{dailyTracker}
DELETE /api/daily-trackers/{dailyTracker}
GET /api/daily-trackers/by-date
POST /api/daily-trackers/{dailyTracker}/validate

// Product Management
GET /api/products
POST /api/products
GET /api/products/{product}
PUT /api/products/{product}
DELETE /api/products/{product}

// Stock Location Management
GET /api/stock-locations
POST /api/stock-locations
GET /api/stock-locations/{stockLocation}
PUT /api/stock-locations/{stockLocation}
DELETE /api/stock-locations/{stockLocation}

// Company Invoice Management
GET /api/company-invoices
POST /api/company-invoices
GET /api/company-invoices/{invoice}
PUT /api/company-invoices/{invoice}
DELETE /api/company-invoices/{invoice}
POST /api/company-invoices/{invoice}/verify

// Invoice Items Management
GET /api/invoice-items
POST /api/invoice-items
GET /api/invoice-items/{item}
PUT /api/invoice-items/{item}
DELETE /api/invoice-items/{item}
GET /api/invoice-items/by-invoice/{invoiceUuid}

// Stock Batch Management
GET /api/stock-batches
POST /api/stock-batches
GET /api/stock-batches/{batch}
PUT /api/stock-batches/{batch}
DELETE /api/stock-batches/{batch}
GET /api/stock-batches/by-location/{locationUuid}
GET /api/stock-batches/by-product/{productUuid}
GET /api/stock-batches/expiring-soon

// Stock Movement Management
GET /api/stock-movements
POST /api/stock-movements
GET /api/stock-movements/{movement}
PUT /api/stock-movements/{movement}
DELETE /api/stock-movements/{movement}
GET /api/stock-movements/by-location/{locationUuid}
GET /api/stock-movements/by-type/{type}
POST /api/stock-movements/{movement}/approve

// Stock Reservation Management
GET /api/stock-reservations
POST /api/stock-reservations
GET /api/stock-reservations/{reservation}
PUT /api/stock-reservations/{reservation}
DELETE /api/stock-reservations/{reservation}
GET /api/stock-reservations/by-customer/{customerUuid}
POST /api/stock-reservations/{reservation}/fulfill

// Stock Valuation Management
GET /api/stock-valuations
POST /api/stock-valuations
GET /api/stock-valuations/{valuation}
PUT /api/stock-valuations/{valuation}
DELETE /api/stock-valuations/{valuation}
GET /api/stock-valuations/by-location/{locationUuid}
GET /api/stock-valuations/profit-loss-report

// Club Management
GET /api/clubs
POST /api/clubs
GET /api/clubs/{club}
PUT /api/clubs/{club}
DELETE /api/clubs/{club}
GET /api/clubs/by-owner/{ownerId}

// Club Memberships Management
GET /api/club-memberships
POST /api/club-memberships
GET /api/club-memberships/{clubMembership}
PUT /api/club-memberships/{clubMembership}
DELETE /api/club-memberships/{clubMembership}
GET /api/club-memberships/by-club/{clubId}
GET /api/club-memberships/by-user/{userId}

// Shake Combinations Management
GET /api/shake-combinations
POST /api/shake-combinations
GET /api/shake-combinations/{shakeCombination}
PUT /api/shake-combinations/{shakeCombination}
DELETE /api/shake-combinations/{shakeCombination}

// Club Shake Counter Management
GET /api/club-shake-counter
POST /api/club-shake-counter
GET /api/club-shake-counter/{clubShakeCounter}
PUT /api/club-shake-counter/{clubShakeCounter}
DELETE /api/club-shake-counter/{clubShakeCounter}
GET /api/club-shake-counter/by-club/{clubId}
GET /api/club-shake-counter/by-date/{date}
```

## Prospect and Related Migrations

```php
Schema::create('prospects', function (Blueprint $t) {
    $t->uuid('uuid')->primary();
    $t->string('name');
    $t->string('primary_phone')->nullable();
    $t->date('birth_date')->nullable();
    $t->unsignedSmallInteger('birth_year')->nullable();
    $t->unsignedTinyInteger('age')->nullable();
    $t->enum('age_by', ['birth_date', 'birth_year', 'age'])->nullable();
    $t->unsignedTinyInteger('height')->nullable();
    $t->enum('gender', ['male', 'female', 'other'])->nullable();
    $t->enum('marital_status', ['single', 'married', 'divorced', 'widowed'])->nullable();
    $t->enum('occupation', ['student', 'employed', 'unemployed', 'retired', 'housewife', 'other'])->nullable();
    $t->string('mother_tongue', 25)->nullable();
    $t->enum('source', ['TTP', 'Assessment', 'Measurement_Camp', 'Referral', 'Paper_Insert', 'Circular_of_Influence', 'Social_Media', 'Advertisement', 'Walk_In', 'Other'])->nullable();
    $t->string('source_details', 191)->nullable();
    $t->enum('lead_stage', ['New', 'Contacted', 'Interested', 'Qualified', 'Converted', 'Lost', 'Hold', 'Archived'])->nullable();
    $t->string('referred_by', 36)->nullable();
    $t->string('notes')->nullable();
    $t->date('last_follow_up_date')->nullable();
    $t->date('next_follow_up_date')->nullable();

    $t->string('health_challenge', 255)->nullable();
    $t->string('medication', 255)->nullable();
    $t->string('health_goal', 255)->nullable();
    
    $t->unsignedTinyInteger('conversion_probability')->nullable();
    $t->string('converted_to_user_uuid', 36)->nullable();
    $t->timestamp('converted_at')->nullable();
    $t->boolean('is_active')->default(true);

    $t->foreignUuid('created_by')->nullable()->constrained('users', 'uuid')->onDelete('cascade');
    $t->foreignUuid('updated_by')->nullable()->constrained('users', 'uuid')->onDelete('cascade');
    $t->foreignUuid('deleted_by')->nullable()->constrained('users', 'uuid')->onDelete('cascade');
    $t->foreignUuid('coach_id')->nullable()->constrained('users', 'uuid')->onDelete('cascade');

    $t->timestamps();
    $t->softDeletes();
    
    // Performance indexes
    $t->index(['lead_stage', 'is_active']);
    $t->index(['coach_id', 'is_active']);
    $t->index(['created_by', 'is_active']);
    $t->index(['next_follow_up_date']);
    $t->index(['converted_to_user_uuid']);
});
```


## User and Prospect Shared Migritions

### Profile Details
```php
Schema::create('profile_details', function (Blueprint $t) {
    $t->uuid('uuid')->primary();
    $t->foreignUuid('user_uuid')->constrained('users', 'uuid')->onDelete('cascade');
    $t->string('first_name', 191)->nullable();
    $t->string('middle_name', 191)->nullable();
    $t->string('last_name', 191)->nullable();
    $t->string('primary_email', 50)->nullable();
    $t->string('primary_phone', 18)->nullable();
    $t->date('birth_date')->nullable();
    $t->unsignedSmallInteger('birth_year')->nullable();
    $t->unsignedTinyInteger('age')->nullable();
    $t->enum('age_by', ['birth_date', 'birth_year', 'age'])->nullable();
    $t->enum('gender', ['male', 'female', 'other'])->nullable();
    $t->enum('marital_status', ['single', 'married', 'divorced', 'widowed'])->nullable();
    $t->enum('source', ['TTP', 'Assessment', 'Measurement_Camp', 'Referral', 'Paper_Insert', 'Circular_of_Influence', 'Social_Media', 'Advertisement', 'Walk_In', 'Other'])->nullable();
    $t->string('source_details', 191)->nullable();
    $t->string('occupation', 191)->nullable();
    $t->string('mother_tongue', 25)->nullable();
    $t->string('notes')->nullable();
    $t->date('last_follow_up_date')->nullable();
    $t->date('next_follow_up_date')->nullable();
    $t->string('converted_from_prospect_uuid', 36)->nullable();
    $t->timestamp('converted_at')->nullable();
    
    $t->enum('marketing_level', ['Guest', 'Preferred_Customer', 'Associate', 'Senior_Consultant', 'Success_Builder', 'Supervisor', 'Active_Supervisor', 'World_Team', 'Active_World_Team', 'GET', 'GET 2500','MIL', 'MIL 7500', 'PRESIDENT TEAM', 'Executive Presidents Team', 'Senior Executive Presidents Team', 'International Executive Presidents Team', 'Chief Executive Presidents Team', 'Chairmans Club', 'Founder Circle' ])->default('Guest');
    $t->unsignedTinyInteger('diamonds')->nullable();
    $t->unsignedTinyInteger('stones')->nullable();
    $t->unsignedTinyInteger('royalty_level')->nullable();
    $t->unsignedTinyInteger('discount_level')->default(0);
    $t->unsignedTinyInteger('years_active')->default(0);
    $t->unsignedTinyInteger('mark_hughes_award')->default(0);

    $t->boolean('is_active')->default(true);
    $t->foreignUuid('coach_id')->nullable()->constrained('users', 'uuid')->onDelete('cascade');
    $t->foreignUuid('referred_by')->nullable()->constrained('users', 'uuid')->onDelete('cascade');
    $t->foreignUuid('supervisor_id')->nullable()->constrained('users', 'uuid')->onDelete('cascade');
    $t->foreignUuid('world_team_id')->nullable()->constrained('users', 'uuid')->onDelete('cascade');
    $t->foreignUuid('get_id')->nullable()->constrained('users', 'uuid')->onDelete('cascade');
    $t->foreignUuid('mil_id')->nullable()->constrained('users', 'uuid')->onDelete('cascade');
    $t->foreignUuid('president_team_id')->nullable()->constrained('users', 'uuid')->onDelete('cascade');
    $t->foreignUuid('created_by')->nullable()->constrained('users', 'uuid')->onDelete('cascade');
    $t->foreignUuid('updated_by')->nullable()->constrained('users', 'uuid')->onDelete('cascade');
    $t->foreignUuid('deleted_by')->nullable()->constrained('users', 'uuid')->onDelete('cascade');
    
    $t->timestamps();
    $t->softDeletes();
    
    // Performance indexes
    $t->index(['user_uuid', 'is_active']);
    $t->index(['coach_id', 'is_active']);
    $t->index(['created_by', 'is_active']);
    $t->index(['next_follow_up_date']);
    $t->index(['converted_from_prospect_uuid']);
});
```

### User Groups

```php
Schema::create('groups', function (Blueprint $t) {
    $t->uuid('uuid')->primary();
    $t->string('groupable_type', 191)->nullable(); // User or Club or Prospect
    $t->string('groupable_id', 36)->nullable(); // User or Club or Prospect ID
    $t->string('name')->notNull();
    $t->string('description')->nullable();
    $t->enum('type', ['marketing_level', 'coach_hierarchy', 'club', 'non-supervisor', 'batch', 'demographic', 'program_batch', 'custom'])->notNull();
    $t->string('custom_type')->nullable();
    $t->string('group_code')->nullable()->unique();
    $t->json('criteria')->nullable();
    $t->boolean('auto_assign')->default(false);
    $t->boolean('is_active')->default(true);
    $t->foreignUuid('created_by')->nullable()->constrained('users', 'uuid')->onDelete('cascade');
    $t->foreignUuid('updated_by')->nullable()->constrained('users', 'uuid')->onDelete('cascade');
    $t->foreignUuid('deleted_by')->nullable()->constrained('users', 'uuid')->onDelete('cascade');
    $t->timestamps();
    $t->softDeletes();
    
    // Performance indexes
    $t->index(['groupable_type', 'groupable_id']);
    $t->index(['type', 'is_active']);
    $t->index(['created_by', 'is_active']);
});
```

- **User Auto assing Groups Criteria**
  - marketing_level
    - Based on marketing level, assing the group to the user.
  - club
    - Based on club, assing the group to the user.
  - All non-supervisor users
    - Users belogns to 'Associate', 'Senior_Consultant', 'Success_Builder' marketing level are called as non-supervisor.
  - All supervisor users
    - Users belogns to 'Supervisor' and above marketing level are called as supervisor.
  - All TAB team users all 
    - Users belogns to GET and above till MIL7500 marketing level are called as TAB team.
  - All President team users
    - Users belogns to 'President Team', 'Executive Presidents Team', 'Senior Executive Presidents Team', 'International Executive Presidents Team', 'Chief Executive Presidents Team', 'Chairmans Club', 'Founder Circle' marketing level are called as President team.


### Marketing Level 

- This is a fix value table, feel with seeding only. And can be keep long cached.
```php
Schema::create('marketing_levels', function (Blueprint $t) {
    $t->id();
    $t->integer('level')->unique();
    $t->string('name')->index();
    $t->timestamps();
});
```


### Contacts

- All contacts with country code, email format, phone format, etc. should be validated during input. if not available in laravel inbuilt, then use its ecosystem or third party package.

```php
Schema::create('contacts', function (Blueprint $t) {
    $t->uuid('uuid')->primary();
    $t->string('contactable_type', 191)->nullable(); // User or Club
    $t->string('contactable_id', 36)->nullable(); // User or Club ID
    $t->enum('type', ['Mobile', 'TelePhone', 'WhatsApp', 'Email', 'Facebook', 'Instagram', 'YouTube', 'Telegram', 'Other', 'WhatsApp Business', 'WhatsApp Personal', 'Custom'])->default('Mobile');
    $t->string('custom_type')->nullable();
    $t->string('value')->notNull();
    $t->boolean('is_primary')->default(false);
    $t->boolean('is_verified')->default(false);
    $t->string('tags')->nullable();
    $t->foreignUuid('created_by')->nullable()->constrained('users', 'uuid')->onDelete('cascade');
    $t->foreignUuid('updated_by')->nullable()->constrained('users', 'uuid')->onDelete('cascade');
    $t->foreignUuid('deleted_by')->nullable()->constrained('users', 'uuid')->onDelete('cascade');
    $t->timestamps();
    $t->softDeletes();
    
    // Performance indexes
    $t->index(['contactable_type', 'contactable_id']);
    $t->index(['type', 'is_primary']);
    $t->index(['is_verified']);
});
```

### Addresses

- As we dont have all city, taluka, district, state, country, pincode in our system, we need to use any third party service to get this verified or also strip, capitalization, etc. during input validation. check if laravel or its ecosystem or third party has any package for this.
- If google map link is provided, then can we get latitude and longitude from it without any cost? if yes, then we can use it.
- Also can we get city, taluka, district, state, country, pincode from google map link without any cost? if yes, then we can use it.

```php  
Schema::create('addresses', function (Blueprint $t) {
    $t->uuid('uuid')->primary();
    $t->string('addressable_type', 191)->nullable(); // User or Club
    $t->string('addressable_id', 36)->nullable(); // User or Club ID
    $t->enum('type', ['home', 'office', 'club', 'temporary', 'other', 'custom'])->default('home');
    $t->string('address_name')->nullable();
    $t->string('address_line_1')->notNull();
    $t->string('address_line_2')->nullable();
    $t->string('city')->notNull();
    $t->string('taluka')->nullable();
    $t->string('district')->notNull();
    $t->string('state')->notNull();
    $t->string('country')->notNull()->default('India');
    $t->string('pincode')->notNull();
    $t->string('google_map_link')->nullable();
    $t->decimal('latitude', 11, 8)->nullable();
    $t->decimal('longitude', 11, 8)->nullable();
    $t->boolean('is_primary')->default(false);
    $t->foreignUuid('created_by')->nullable()->constrained('users', 'uuid')->onDelete('cascade');
    $t->foreignUuid('updated_by')->nullable()->constrained('users', 'uuid')->onDelete('cascade');
    $t->foreignUuid('deleted_by')->nullable()->constrained('users', 'uuid')->onDelete('cascade');
    $t->timestamps();
    $t->softDeletes();
    
    // Performance indexes
    $t->index(['addressable_type', 'addressable_id']);
    $t->index(['type', 'is_primary']);
    $t->index(['city', 'state']);
    $t->index(['pincode']);
    $t->index(['latitude', 'longitude']);
});
```

### Photos

- All photos should be stored in storage/app/public/photos/user_photos/
- We should have a library or package to compress the photo before storing in database without losing quality.
- photos should have expiry date, after which it will be deleted from database and storage.
- frontend we can use some library to identify the photo type and suggest it accordingly.

```php 
Schema::create('photos', function (Blueprint $t) {
    $t->uuid('uuid')->primary();
    $t->string('photoable_type', 191)->nullable(); // User or Club or Prospect
    $t->string('photoable_id', 36)->nullable(); // User or Club or Prospect ID
    $t->enum('type', ['Profile', 'Front', 'Back', 'Left', 'Right', 'Full_body', 'Measurement', 'Shake', 'Afresh', 'Skin_Booster', 'H24_Hydrate', 'H24_Rebuild', 'Sleep_Enhancer', 'Niteworks', 'Beta_Heart', 'Tablet',  'Meal', 'Snacks', 'Activity_Proof', 'Custom'])->notNull();
    $t->string('custom_type')->nullable();
    $t->string('file_path', 500)->notNull();
    $t->string('file_name', 255)->notNull();
    $t->integer('file_size')->notNull();
    $t->string('mime_type', 100)->notNull();
    $t->string('caption')->nullable();
    $t->timestamp('taken_at')->nullable();
    $t->decimal('latitude', 10, 8)->nullable();
    $t->decimal('longitude', 11, 8)->nullable();
    $t->boolean('is_primary')->default(false);
    $t->string('tags')->nullable();
    $t->foreignUuid('created_by')->nullable()->constrained('users', 'uuid')->onDelete('cascade');
    $t->foreignUuid('updated_by')->nullable()->constrained('users', 'uuid')->onDelete('cascade');
    $t->foreignUuid('deleted_by')->nullable()->constrained('users', 'uuid')->onDelete('cascade');
    $t->timestamps();
    $t->softDeletes();
    
    // Performance indexes
    $t->index(['photoable_type', 'photoable_id']);
    $t->index(['type', 'is_primary']);
    $t->index(['file_path']);
    $t->index(['taken_at']);
    $t->index(['latitude', 'longitude']);
});
```

### Health

```php
Schema::create('health_data', function (Blueprint $t) {
    $t->uuid('uuid')->primary();
    $t->string('health_dataable_type', 191)->nullable(); // User or Prospect
    $t->string('health_dataable_id', 36)->nullable(); // User or Prospect ID
    $t->string('challenges')->nullable();
    $t->string('goals')->nullable();
    $t->string('medical_history')->nullable();
    $t->string('current_medications')->nullable();
    $t->string('allergies')->nullable();
    $t->string('dietary_restrictions')->nullable();
    $t->enum('fitness_level', ['beginner', 'intermediate', 'advanced'])->nullable();
    $t->foreignUuid('created_by')->nullable()->constrained('users', 'uuid')->onDelete('cascade');
    $t->foreignUuid('updated_by')->nullable()->constrained('users', 'uuid')->onDelete('cascade');
    $t->foreignUuid('deleted_by')->nullable()->constrained('users', 'uuid')->onDelete('cascade');
    $t->timestamps();
    $t->softDeletes();
});
```

```php
Schema::create('measurements', function (Blueprint $t) {
    $t->uuid('uuid')->primary();
    $t->string('measurementable_type', 191)->nullable(); // User or Prospect
    $t->string('measurementable_id', 36)->nullable(); // User or Prospect ID
    $t->enum('condition', ['Empty Stomach', 'After Breakfast', 'After Lunch', 'After Dinner', '2 Hours After Meal', '1 Hour after Breakfast'])->nullable();
    $t->unsignedTinyInteger('age')->nullable();
    $t->decimal('height', 5, 2)->nullable();
    $t->decimal('weight', 5, 2)->nullable();
    $t->decimal('body_fat', 5, 2)->nullable();
    $t->decimal('bmi', 5, 2)->nullable();
    $t->decimal('visceral_fat', 5, 2)->nullable();
    $t->unsignedSmallInteger('bmr_resting')->nullable();
    $t->unsignedTinyInteger('body_age')->nullable();
    $t->decimal('trunk_fat', 5, 2)->nullable();
    $t->decimal('muscle_mass', 5, 2)->nullable();
    $t->decimal('belly_inches_top', 5, 2)->nullable();
    $t->decimal('belly_inches_bottom', 5, 2)->nullable();
    $t->string('notes')->nullable();
    $t->foreignUuid('created_by')->nullable()->constrained('users', 'uuid')->onDelete('cascade');
    $t->foreignUuid('updated_by')->nullable()->constrained('users', 'uuid')->onDelete('cascade');
    $t->foreignUuid('deleted_by')->nullable()->constrained('users', 'uuid')->onDelete('cascade');
    $t->timestamps();
    $t->softDeletes();
    
    // Performance indexes
    $t->index(['measurementable_type', 'measurementable_id']);
    $t->index(['created_by']);
});
```


```php
Schema::create('daily_trackers', function (Blueprint $t) {
    $t->uuid('uuid')->primary();
    $t->foreignUuid('user_uuid')->constrained('users', 'uuid')->onDelete('cascade');
    $t->date('date')->index();
    // Fixed daily data fields
    $t->integer('water_intake')->default(0); // in ml
    $t->decimal('sleep_hours', 3, 1)->default(0); // in hours
    $t->boolean('exercise_done')->default(false);
    $t->integer('exercise_duration')->default(0); // in minutes
    $t->integer('mood_rating')->default(0); // 1-10 scale
    $t->integer('mindset_rating')->default(0); // 1-10 scale
    $t->integer('energy_level')->default(0); // 1-10 scale
    $t->integer('streak_count')->default(0);
    $t->boolean('is_validated')->default(false);
    // Validation and notes
    $t->string('notes')->nullable();
    $t->foreignUuid('validated_by')->nullable()->constrained('users', 'uuid')->onDelete('cascade')->index();
    $t->timestamp('validated_at')->nullable();
    // Audit fields
    $t->foreignUuid('created_by')->nullable()->constrained('users', 'uuid')->onDelete('cascade')->index();
    $t->foreignUuid('updated_by')->nullable()->constrained('users', 'uuid')->onDelete('cascade');
    $t->foreignUuid('deleted_by')->nullable()->constrained('users', 'uuid')->onDelete('cascade');
    $t->timestamps();
    $t->softDeletes();
    // Performance indexes
    $t->unique(['user_uuid', 'date']);
});
```

```php
Schema::create('daily_tracker_intakes', function (Blueprint $t) {
    $t->uuid('uuid')->primary();
    $t->foreignUuid('daily_tracker_uuid')->constrained('daily_trackers', 'uuid')->onDelete('cascade')->index();

            // Supplement intake details
    $t->enum('supplement_type', [
        'Shake',
        'Afresh',
        'Detox',
        'Skin Booster',
        'Niteworks',
        'Sleep Enhancer',
        'Meal',
        'Beta Heart',
        'Joint Support',
        'Probiotics',
        'Multivitamins',
        'Herbalifeline',
        'Other'
    ])->index();
    $t->string('supplement_name')->nullable();

    $t->timestamp('taken_at')->index();
    $t->decimal('quantity', 10, 2)->default(1); // For servings devided in multiple persons/parts
    $t->string('notes')->nullable(); // Optional notes for this specific intake

    // Audit fields
    $t->foreignUuid('created_by')->nullable()->constrained('users', 'uuid')->onDelete('cascade')->index();
    $t->foreignUuid('updated_by')->nullable()->constrained('users', 'uuid')->onDelete('cascade');
    $t->foreignUuid('deleted_by')->nullable()->constrained('users', 'uuid')->onDelete('cascade');
    $t->timestamps();
    $t->softDeletes();

    // Composite index for efficient queries
    $t->index(['daily_tracker_uuid', 'supplement_type']);
    $t->index(['daily_tracker_uuid', 'taken_at']);
});
```

## Activities

```php
Schema::create('activities', function (Blueprint $t) {
    $t->uuid('uuid')->primary();
    $t->string('name')->notNull();
    $t->string('description')->nullable();
    $t->dateTime('schedule_start_date_time')->nullable();
    $t->dateTime('schedule_end_date_time')->nullable();
    $t->string('location')->nullable();
    $t->string('location_remark')->nullable();
    $t->unsignedSmallInteger('invites')->nullable();
    $t->unsignedSmallInteger('accepted')->nullable();
    $t->enum('type', ['Group', 'Individual'])->default('Individual');
    $t->enum('activity_type', ['TTP', 'Flyer', 'Survey', 'Health Check Camp', 'Road Show', 'Afresh Party', 'MIW', 'Business Opportunity', 'Social Media Ads', 'Bulk Messaging', 'Bulk Emailing', 'Poster'])->default('TTP');
    $t->enum('activity_type_remark', ['major', 'minor', 'other'])->default('major');
    $t->string('activity_type_remark_other')->nullable();
    $t->enum('activity_status', ['Scheduled', 'Completed', 'Cancelled', 'Pending'])->default('Pending');
    $t->string('activity_status_remark')->nullable();
    $t->enum('prospect_distribution_method', ['only_COI_provider', 'equally_distributed', 'individual', 'other'])->default('only_COI_provider');
    $t->string('other_distribution_method')->nullable();
    $t->json('prospects')->nullable();
    $t->foreignUuid('created_by')->nullable()->constrained('users', 'uuid')->onDelete('cascade')->index();
    $t->foreignUuid('updated_by')->nullable()->constrained('users', 'uuid')->onDelete('cascade');
    $t->foreignUuid('deleted_by')->nullable()->constrained('users', 'uuid')->onDelete('cascade');
    $t->timestamps();
    $t->softDeletes();
});
```

```php
Schema::create('activity_participants', function (Blueprint $t) {
    $t->uuid('uuid')->primary();
    $t->foreignUuid('activity_uuid')->constrained('activities', 'uuid')->onDelete('cascade')->index();
    $t->foreignUuid('user_uuid')->constrained('users', 'uuid')->onDelete('cascade')->index();
    $t->enum('status', ['Invited', 'Accepted', 'Rejected', 'Cancelled'])->default('Invited');
    $t->string('status_remark')->nullable();
    $t->enum('role', ['Creator', 'Participant', 'Scanning', 'Counseling', 'Invitee', 'Other', 'Learner'])->default('Participant');
    $t->string('role_remark')->nullable();
    $t->string('notes')->nullable();
    $t->timestamps();
    $t->softDeletes();
});
```

```php
Schema::table('prospects', function (Blueprint $t) {
    $t->foreignUuid('activity_uuid')->nullable()->constrained('activities', 'uuid')->onDelete('cascade')->index();
});
```
- Prospects Array (with assignment)


```php
// add  consent,  theme, timezone, language to user profile
Schema::table('users', function (Blueprint $t) {
    $t->boolean('consent')->default(false);
    $t->string('theme')->default('system');
    $t->string('timezone')->default('Asia/Kolkata');
    $t->string('language')->default('en');
});
```


If Activity type is Group, and Participant coaches are not under creator, then he has to share link to other coaches to participate.  And creater has to approve it.
If coaches under creator, then he can send invites directly, coaches has to accept.

Missing Points to Add
1. Add user uuid to fixed table also to add their own content

## Stock Management System

### Core Concepts
The stock management system handles:
1. **Stock Receipt**: Stock received from company via invoices (99% of cases)
2. **Stock Consumption**: Personal consumption, sales to guests/customers, club usage, losses, etc.
3. **Batch Tracking**: Different price batches for same product (e.g., last year's stock vs current year)
4. **Profit Calculation**: Based on actual purchase price vs selling price using FIFO/LIFO methods

### Database Schema

```php
// Products table (unchanged - master product catalog)
Schema::create('products', function (Blueprint $t) {
    $t->uuid('uuid')->primary();
    $t->string('sku')->unique();
    $t->string('name')->index();
    $t->decimal('volume_points', 8, 2);
    $t->decimal('earn_base', 8, 2);
    $t->decimal('base_price', 10, 2);
    $t->decimal('tax_percentage', 5, 2);
    $t->decimal('mrp', 10, 2);
    $t->decimal('discount_15_price', 10, 2);
    $t->decimal('discount_25_price', 10, 2);
    $t->decimal('discount_35_price', 10, 2);
    $t->decimal('discount_42_price', 10, 2);
    $t->decimal('discount_50_price', 10, 2);
    $t->string('unit', 50);
    $t->decimal('vp_per_unit', 8, 2);
    $t->date('price_release_date');
    $t->boolean('is_active')->default(true);
    $t->foreignUuid('created_by')->nullable()->constrained('users', 'uuid')->onDelete('cascade');
    $t->foreignUuid('updated_by')->nullable()->constrained('users', 'uuid')->onDelete('cascade');
    $t->foreignUuid('deleted_by')->nullable()->constrained('users', 'uuid')->onDelete('cascade');
    $t->timestamps();
    $t->softDeletes();
});

// Product price history (unchanged)
Schema::create('product_histories', function (Blueprint $t) {
    $t->uuid('uuid')->primary();
    $t->foreignUuid('product_id')->constrained('products', 'uuid')->onDelete('cascade');
    $t->string('sku');
    $t->string('name');
    $t->decimal('volume_points', 8, 2);
    $t->decimal('earn_base', 8, 2);
    $t->decimal('base_price', 10, 2);
    $t->decimal('tax_percentage', 5, 2);
    $t->decimal('mrp', 10, 2);
    $t->decimal('discount_15_price', 10, 2);
    $t->decimal('discount_25_price', 10, 2);
    $t->decimal('discount_35_price', 10, 2);
    $t->decimal('discount_42_price', 10, 2);
    $t->decimal('discount_50_price', 10, 2);
    $t->string('unit', 50);
    $t->decimal('vp_per_unit', 8, 2);
    $t->date('price_release_date');
    $t->boolean('is_active')->default(true);
    $t->foreignUuid('created_by')->nullable()->constrained('users', 'uuid')->onDelete('cascade');
    $t->foreignUuid('updated_by')->nullable()->constrained('users', 'uuid')->onDelete('cascade');
    $t->foreignUuid('deleted_by')->nullable()->constrained('users', 'uuid')->onDelete('cascade');
    $t->timestamps();
    $t->softDeletes();
});

// Stock locations (unchanged)
Schema::create('stock_locations', function (Blueprint $t) {
    $t->uuid('uuid')->primary();
    $t->string('name');
    $t->enum('type', ['club', 'club_physical', 'club_virtual', 'coach', 'member', 'warehouse', 'Other']);
    $t->foreignUuid('owner_uuid')->constrained('users', 'uuid')->onDelete('cascade')->index();
    $t->text('address')->nullable();
    $t->boolean('is_active')->default(true);
    $t->foreignUuid('created_by')->nullable()->constrained('users', 'uuid')->onDelete('cascade');
    $t->foreignUuid('updated_by')->nullable()->constrained('users', 'uuid')->onDelete('cascade');
    $t->foreignUuid('deleted_by')->nullable()->constrained('users', 'uuid')->onDelete('cascade');
    $t->timestamps();
    $t->softDeletes();
});

// NEW: Company invoices (for stock receipt tracking)
Schema::create('company_invoices', function (Blueprint $t) {
    $t->uuid('uuid')->primary();
    $t->string('invoice_number')->unique();
    $t->date('invoice_date');
    $t->date('received_date');
    $t->decimal('total_amount', 12, 2);
    $t->decimal('total_tax', 10, 2)->default(0);
    $t->decimal('total_volume_points', 10, 2)->default(0);
    $t->enum('status', ['pending', 'received', 'verified', 'cancelled'])->default('pending');
    $t->text('notes')->nullable();
    $t->foreignUuid('received_by')->constrained('users', 'uuid')->onDelete('cascade');
    $t->foreignUuid('verified_by')->nullable()->constrained('users', 'uuid')->onDelete('cascade');
    $t->timestamp('verified_at')->nullable();
    $t->foreignUuid('created_by')->nullable()->constrained('users', 'uuid')->onDelete('cascade');
    $t->foreignUuid('updated_by')->nullable()->constrained('users', 'uuid')->onDelete('cascade');
    $t->foreignUuid('deleted_by')->nullable()->constrained('users', 'uuid')->onDelete('cascade');
    $t->timestamps();
    $t->softDeletes();
    
    // Performance indexes
    $t->index(['invoice_date']);
    $t->index(['received_date']);
    $t->index(['status']);
    $t->index(['received_by']);
});

// NEW: Invoice items (individual products in invoice)
Schema::create('invoice_items', function (Blueprint $t) {
    $t->uuid('uuid')->primary();
    $t->foreignUuid('invoice_uuid')->constrained('company_invoices', 'uuid')->onDelete('cascade');
    $t->foreignUuid('product_uuid')->constrained('products', 'uuid')->onDelete('cascade');
    $t->decimal('quantity', 10, 2);
    $t->decimal('unit_price', 10, 2);
    $t->decimal('total_price', 12, 2);
    $t->decimal('tax_amount', 10, 2)->default(0);
    $t->decimal('volume_points', 8, 2)->default(0);
    $t->string('batch_number')->nullable(); // Company batch number
    $t->date('expiry_date')->nullable();
    $t->text('notes')->nullable();
    $t->foreignUuid('created_by')->nullable()->constrained('users', 'uuid')->onDelete('cascade');
    $t->foreignUuid('updated_by')->nullable()->constrained('users', 'uuid')->onDelete('cascade');
    $t->foreignUuid('deleted_by')->nullable()->constrained('users', 'uuid')->onDelete('cascade');
    $t->timestamps();
    $t->softDeletes();
    
    // Performance indexes
    $t->index(['invoice_uuid']);
    $t->index(['product_uuid']);
    $t->index(['batch_number']);
    $t->index(['expiry_date']);
});

// REVISED: Stocks (main stock table for each SKU at each location)
Schema::create('stocks', function (Blueprint $t) {
    $t->uuid('uuid')->primary();
    $t->foreignUuid('product_uuid')->constrained('products', 'uuid')->onDelete('cascade');
    $t->foreignUuid('location_uuid')->constrained('stock_locations', 'uuid')->onDelete('cascade');
    $t->decimal('total_quantity', 10, 2)->default(0); // Total stock available
    $t->decimal('reserved_quantity', 10, 2)->default(0); // Reserved for **orders**
    $t->decimal('average_cost', 10, 2)->default(0); // Weighted average cost
    $t->decimal('total_value', 12, 2)->default(0); // Total stock value
    $t->date('last_updated')->nullable();
    $t->text('notes')->nullable();
    $t->foreignUuid('created_by')->nullable()->constrained('users', 'uuid')->onDelete('cascade');
    $t->foreignUuid('updated_by')->nullable()->constrained('users', 'uuid')->onDelete('cascade');
    $t->foreignUuid('deleted_by')->nullable()->constrained('users', 'uuid')->onDelete('cascade');
    $t->timestamps();
    $t->softDeletes();
    
    // Performance indexes
    $t->unique(['product_uuid', 'location_uuid']); // One stock record per product per location
    $t->index(['product_uuid']);
    $t->index(['location_uuid']);
    $t->index(['available_quantity']);
    $t->index(['last_updated']);
});

// NEW: Stock items (detailed tracking of individual stock receipts)
Schema::create('stock_items', function (Blueprint $t) {
    $t->uuid('uuid')->primary();
    $t->foreignUuid('stock_uuid')->constrained('stocks', 'uuid')->onDelete('cascade');
    $t->foreignUuid('invoice_item_uuid')->constrained('invoice_items', 'uuid')->onDelete('cascade');
    $t->string('batch_number')->nullable(); // Company batch number
    $t->decimal('purchase_price', 10, 2); // Actual purchase price from invoice
    $t->decimal('initial_quantity', 10, 2); // Initial quantity received
    $t->decimal('remaining_quantity', 10, 2); // Current remaining quantity
    $t->date('purchase_date'); // Date from invoice
    $t->date('expiry_date')->nullable();
    $t->enum('status', ['active', 'expired', 'depleted'])->default('active');
    $t->text('notes')->nullable();
    $t->foreignUuid('created_by')->nullable()->constrained('users', 'uuid')->onDelete('cascade');
    $t->foreignUuid('updated_by')->nullable()->constrained('users', 'uuid')->onDelete('cascade');
    $t->foreignUuid('deleted_by')->nullable()->constrained('users', 'uuid')->onDelete('cascade');
    $t->timestamps();
    $t->softDeletes();
    
    // Performance indexes
    $t->index(['stock_uuid']);
    $t->index(['batch_number']);
    $t->index(['purchase_date']);
    $t->index(['expiry_date']);
    $t->index(['status']);
    $t->index(['remaining_quantity']);
});

// REVISED: Stock movements (detailed tracking of all stock movements)
Schema::create('stock_movements', function (Blueprint $t) {
    $t->uuid('uuid')->primary();
    $t->enum('movement_type', [
        'receipt',           // Stock received from company
        'transfer',          // Transfer between locations
        'sale_guest',        // Sale to new guest
        'sale_customer',     // Sale to existing customer
        'personal_consumption', // Personal use
        'club_consumption',  // Club usage
        'loss',              // Lost/damaged
        'expired',           // Expired stock
        'adjustment',        // Manual adjustment
        'reservation',       // Reserve for order
        'unreservation'      // Cancel reservation
    ]);
    $t->foreignUuid('stock_uuid')->constrained('stocks', 'uuid')->onDelete('cascade');
    $t->foreignUuid('stock_item_uuid')->nullable()->constrained('stock_items', 'uuid')->onDelete('cascade'); // For FIFO tracking
    $t->foreignUuid('from_location_uuid')->nullable()->constrained('stock_locations', 'uuid')->onDelete('cascade');
    $t->foreignUuid('to_location_uuid')->nullable()->constrained('stock_locations', 'uuid')->onDelete('cascade');
    $t->decimal('quantity', 10, 2);
    $t->decimal('unit_price', 10, 2); // Price at time of movement
    $t->decimal('total_amount', 12, 2);
    $t->decimal('profit_loss', 12, 2)->nullable(); // For sales: selling_price - purchase_price
    
    // Reference tracking
    $t->string('reference_type', 100)->nullable(); // 'order', 'club_consumption', 'shake_counter', 'invoice'
    $t->string('reference_uuid', 36)->nullable();
    
    // Customer/User tracking for sales
    $t->foreignUuid('customer_uuid')->nullable()->constrained('users', 'uuid')->onDelete('cascade');
    $t->foreignUuid('club_uuid')->nullable()->constrained('clubs', 'uuid')->onDelete('cascade');
    
    // Approval workflow
    $t->foreignUuid('performed_by')->constrained('users', 'uuid')->onDelete('cascade');
    $t->foreignUuid('approved_by')->nullable()->constrained('users', 'uuid')->onDelete('cascade');
    $t->timestamp('approved_at')->nullable();
    $t->enum('status', ['pending', 'approved', 'rejected', 'cancelled'])->default('pending');
    
    $t->text('notes')->nullable();
    $t->foreignUuid('created_by')->nullable()->constrained('users', 'uuid')->onDelete('cascade');
    $t->foreignUuid('updated_by')->nullable()->constrained('users', 'uuid')->onDelete('cascade');
    $t->foreignUuid('deleted_by')->nullable()->constrained('users', 'uuid')->onDelete('cascade');
    $t->timestamps();
    $t->softDeletes();
    
    // Performance indexes
    $t->index(['movement_type']);
    $t->index(['stock_uuid']);
    $t->index(['stock_item_uuid']);
    $t->index(['from_location_uuid']);
    $t->index(['to_location_uuid']);
    $t->index(['customer_uuid']);
    $t->index(['club_uuid']);
    $t->index(['performed_by']);
    $t->index(['status']);
    $t->index(['reference_type', 'reference_uuid']);
});

// NEW: Stock reservations (for pending orders)
Schema::create('stock_reservations', function (Blueprint $t) {
    $t->uuid('uuid')->primary();
    $t->foreignUuid('stock_uuid')->constrained('stocks', 'uuid')->onDelete('cascade');
    $t->foreignUuid('customer_uuid')->nullable()->constrained('users', 'uuid')->onDelete('cascade');
    $t->decimal('quantity', 10, 2);
    $t->date('reserved_until');
    $t->enum('status', ['active', 'fulfilled', 'expired', 'cancelled'])->default('active');
    $t->text('notes')->nullable();
    $t->foreignUuid('created_by')->nullable()->constrained('users', 'uuid')->onDelete('cascade');
    $t->foreignUuid('updated_by')->nullable()->constrained('users', 'uuid')->onDelete('cascade');
    $t->foreignUuid('deleted_by')->nullable()->constrained('users', 'uuid')->onDelete('cascade');
    $t->timestamps();
    $t->softDeletes();
    
    // Performance indexes
    $t->index(['stock_uuid']);
    $t->index(['customer_uuid']);
    $t->index(['reserved_until']);
    $t->index(['status']);
});

// NEW: Stock valuation (for profit/loss tracking)
Schema::create('stock_valuations', function (Blueprint $t) {
    $t->uuid('uuid')->primary();
    $t->foreignUuid('location_uuid')->constrained('stock_locations', 'uuid')->onDelete('cascade');
    $t->foreignUuid('product_uuid')->constrained('products', 'uuid')->onDelete('cascade');
    $t->decimal('total_quantity', 10, 2);
    $t->decimal('average_cost', 10, 2); // Weighted average cost
    $t->decimal('total_value', 12, 2);
    $t->decimal('market_value', 12, 2); // Current market value
    $t->decimal('unrealized_profit_loss', 12, 2); // Market value - Total value
    $t->date('valuation_date');
    $t->text('notes')->nullable();
    $t->foreignUuid('created_by')->nullable()->constrained('users', 'uuid')->onDelete('cascade');
    $t->foreignUuid('updated_by')->nullable()->constrained('users', 'uuid')->onDelete('cascade');
    $t->foreignUuid('deleted_by')->nullable()->constrained('users', 'uuid')->onDelete('cascade');
    $t->timestamps();
    $t->softDeletes();
    
    // Performance indexes
    $t->index(['location_uuid']);
    $t->index(['product_uuid']);
    $t->index(['valuation_date']);
    $t->unique(['location_uuid', 'product_uuid', 'valuation_date']);
});
```

### Key Features

1. **Invoice-Based Stock Receipt**
   - Track complete invoices from company
   - Multiple products per invoice
   - Batch/lot tracking with expiry dates

2. **Stock-Level Inventory Management**
   - Main stock table tracks total quantity and average cost
   - Stock items track individual receipts with purchase prices
   - FIFO consumption from stock items for accurate profit calculation
   - Expiry date tracking at stock item level

3. **Detailed Movement Tracking**
   - All stock movements are tracked with source and destination
   - Profit/loss calculation for sales
   - Reference tracking for orders, club usage, etc.

4. **Reservation System**
   - Reserve stock for pending orders
   - Prevent overselling
   - Automatic expiration of reservations

5. **Valuation and Reporting**
   - Real-time stock valuation
   - Profit/loss tracking
   - Market value vs cost value

### Business Logic

1. **Stock Receipt Process**
   - Create company invoice
   - Add invoice items with quantities and prices
   - System creates stock batches automatically
   - Update location stock levels

2. **Stock Consumption (FIFO Method)**
   - System consumes oldest stock items first
   - Calculate profit based on actual purchase price from stock items
   - Update stock quantities and stock item quantities
   - Track movement history with stock item reference

3. **Profit Calculation**
   - Selling Price - Purchase Price = Profit/Loss
   - Track profit by batch
   - Aggregate profit reports

4. **Stock Transfers**
   - Move stock between locations
   - Maintain stock item integrity for FIFO tracking
   - Update location stock levels

### API Endpoints

```php
// Company Invoice Management
GET /api/company-invoices
POST /api/company-invoices
GET /api/company-invoices/{invoice}
PUT /api/company-invoices/{invoice}
DELETE /api/company-invoices/{invoice}
POST /api/company-invoices/{invoice}/verify

// Invoice Items Management
GET /api/invoice-items
POST /api/invoice-items
GET /api/invoice-items/{item}
PUT /api/invoice-items/{item}
DELETE /api/invoice-items/{item}
GET /api/invoice-items/by-invoice/{invoiceUuid}

// Stock Batch Management
GET /api/stock-batches
POST /api/stock-batches
GET /api/stock-batches/{batch}
PUT /api/stock-batches/{batch}
DELETE /api/stock-batches/{batch}
GET /api/stock-batches/by-location/{locationUuid}
GET /api/stock-batches/by-product/{productUuid}
GET /api/stock-batches/expiring-soon

// Stock Movement Management
GET /api/stock-movements
POST /api/stock-movements
GET /api/stock-movements/{movement}
PUT /api/stock-movements/{movement}
DELETE /api/stock-movements/{movement}
GET /api/stock-movements/by-location/{locationUuid}
GET /api/stock-movements/by-type/{type}
POST /api/stock-movements/{movement}/approve

// Stock Reservation Management
GET /api/stock-reservations
POST /api/stock-reservations
GET /api/stock-reservations/{reservation}
PUT /api/stock-reservations/{reservation}
DELETE /api/stock-reservations/{reservation}
GET /api/stock-reservations/by-customer/{customerUuid}
POST /api/stock-reservations/{reservation}/fulfill

// Stock Valuation Management
GET /api/stock-valuations
POST /api/stock-valuations
GET /api/stock-valuations/{valuation}
PUT /api/stock-valuations/{valuation}
DELETE /api/stock-valuations/{valuation}
GET /api/stock-valuations/by-location/{locationUuid}
GET /api/stock-valuations/profit-loss-report
```


## Club Management System

```php
Schema::create('clubs', function (Blueprint $t) {
    $t->uuid('uuid')->primary();
    $t->string('name');
    $t->string('tagline', 500)->nullable();
    $t->enum('club_type', ['UMS', 'Centralise', '2_Time_UMS', '2_Time_Centralise']);
    $t->enum('club_mode', ['Offline', 'Online', 'Hybrid', '2_Time_Offline', 'Hybrid_with_2_Time_Online']);
    $t->date('start_date');
    $t->boolean('is_active')->default(true);
    $t->foreignUuid('owner_id')->constrained('users', 'uuid')->onDelete('cascade');
    $t->foreignUuid('created_by')->nullable()->constrained('users', 'uuid')->onDelete('cascade');
    $t->foreignUuid('updated_by')->nullable()->constrained('users', 'uuid')->onDelete('cascade');
    $t->foreignUuid('deleted_by')->nullable()->constrained('users', 'uuid')->onDelete('cascade');
    $t->timestamps();
    $t->softDeletes();
    
    // Performance indexes
    $t->index(['owner_id']);
    $t->index(['is_active']);
});

Schema::create('club_memberships', function (Blueprint $t) {
    $t->uuid('uuid')->primary();
    $t->foreignUuid('club_uuid')->constrained('clubs', 'uuid')->onDelete('cascade');
    $t->foreignUuid('user_uuid')->constrained('users', 'uuid')->onDelete('cascade');
    $t->enum('association_type', ['Membership', 'Kit_Plus_Fee', 'Owner', 'Guest', 'Only_excercise', 'excercise_afresh']);
    $t->decimal('membership_fee', 10, 2)->nullable();
    $t->timestamp('joined_at')->useCurrent();
    $t->timestamp('expires_at')->nullable();
    $t->boolean('is_active')->default(true);
    $t->foreignUuid('created_by')->nullable()->constrained('users', 'uuid')->onDelete('cascade');
    $t->foreignUuid('updated_by')->nullable()->constrained('users', 'uuid')->onDelete('cascade');
    $t->foreignUuid('deleted_by')->nullable()->constrained('users', 'uuid')->onDelete('cascade');
    $t->timestamps();
    $t->softDeletes();
    
    // Performance indexes
    $t->index(['club_uuid']);
    $t->index(['user_uuid']);
    $t->index(['expires_at']);
});
```

#### Shake Counter Implementation:
```php
Schema::create('shake_combinations', function (Blueprint $t) {
    $t->uuid('uuid')->primary();
    $t->string('name');
    $t->unsignedInteger('serving_size')->default(1);
    $t->foreignUuid('flavor_1_product_id')->nullable()->constrained('products', 'uuid')->onDelete('cascade');
    $t->decimal('flavor_1_quantity', 5, 2)->nullable();
    $t->foreignUuid('flavor_2_product_id')->nullable()->constrained('products', 'uuid')->onDelete('cascade');
    $t->decimal('flavor_2_quantity', 5, 2)->nullable();
    $t->foreignUuid('flavor_3_product_id')->nullable()->constrained('products', 'uuid')->onDelete('cascade');
    $t->decimal('flavor_3_quantity', 5, 2)->nullable();
    $t->decimal('shakemate_quantity', 5, 2)->nullable();
    $t->decimal('protein_quantity', 5, 2)->nullable();
    $t->decimal('water_quantity', 5, 2)->nullable();
    $t->decimal('ice_quantity', 5, 2)->nullable();
    $t->text('notes')->nullable();
    $t->foreignUuid('created_by')->constrained('users', 'uuid')->onDelete('cascade');
    $t->foreignUuid('updated_by')->nullable()->constrained('users', 'uuid')->onDelete('cascade');
    $t->foreignUuid('deleted_by')->nullable()->constrained('users', 'uuid')->onDelete('cascade');
    $t->timestamps();
    $t->softDeletes();
    
    // Performance indexes
    $t->index(['created_by']);
});

Schema::create('club_shake_counter', function (Blueprint $t) {
    $t->uuid('uuid')->primary();
    $t->foreignUuid('club_id')->constrained('clubs', 'uuid')->onDelete('cascade');
    $t->foreignUuid('member_id')->constrained('users', 'uuid')->onDelete('cascade');
    $t->foreignUuid('shake_combination_id')->constrained('shake_combinations', 'uuid')->onDelete('cascade');
    $t->timestamp('first_serving_at')->nullable();
    $t->timestamp('refill_serving_at')->nullable();
    $t->foreignUuid('counter_operator_id')->constrained('users', 'uuid')->onDelete('cascade');
    $t->date('date');
    $t->foreignUuid('created_by')->nullable()->constrained('users', 'uuid')->onDelete('cascade');
    $t->foreignUuid('updated_by')->nullable()->constrained('users', 'uuid')->onDelete('cascade');
    $t->foreignUuid('deleted_by')->nullable()->constrained('users', 'uuid')->onDelete('cascade');
    $t->timestamps();
    $t->softDeletes();
    
    // Performance indexes
    $t->index(['club_id']);
    $t->index(['member_id']);
    $t->index(['date']);
    $t->index(['counter_operator_id']);
});
```


## Calendar Management System

### Core Concepts
The calendar management system provides comprehensive scheduling and time tracking capabilities for wellness coaches and members:

1. **Event Scheduling**: Create, manage, and track various types of events and activities
2. **Recurring Events**: Support for complex recurrence patterns (daily, weekly, monthly, yearly, custom intervals)
3. **External Calendar Integration**: Sync with Google Calendar, iCal, Outlook, and other calendar services
4. **Time Tracking**: Monitor daily activities and categorize time usage (major, minor, routine, wasted, other)
5. **Smart Notifications**: Automated reminders and follow-ups based on calendar events
6. **Activity Integration**: Direct links to prospects, members, clubs, and business activities

### Database Schema

#### Calendar Events
```php
Schema::create('calendar_events', function (Blueprint $t) {
    $t->uuid('uuid')->primary();
    $t->string('title')->notNull();
    $t->text('description')->nullable();
    $t->dateTime('start_datetime')->notNull()->index();
    $t->dateTime('end_datetime')->notNull()->index();
    $t->string('location')->nullable();
    $t->decimal('latitude', 11, 8)->nullable();
    $t->decimal('longitude', 11, 8)->nullable();
    $t->string('google_map_link')->nullable();
    
    // Event categorization
    $t->enum('event_type', [
        'activity',           // Business activities (TTP, Flyer, etc.)
        'follow_up',          // Prospect/member follow-ups
        'appointment',        // Health consultations
        'club_session',       // Club meetings/sessions
        'measurement',        // Measurement appointments
        'training',           // Training sessions
        'meeting',            // Business meetings
        'personal',           // Personal appointments
        'reminder',           // General reminders
        'custom'              // Custom event types
    ])->default('activity')->index();
    
    $t->enum('priority', ['low', 'medium', 'high', 'urgent'])->default('medium');
    $t->enum('status', ['scheduled', 'in_progress', 'completed', 'cancelled', 'postponed'])->default('scheduled');
    
    // Color coding and styling
    $t->string('color')->default('#1976D2'); // Material Design Blue
    $t->string('icon')->nullable(); // FontAwesome or custom icon
    $t->boolean('is_all_day')->default(false);
    $t->boolean('is_private')->default(false);
    
    // Recurrence settings
    $t->boolean('is_recurring')->default(false);
    $t->json('recurrence_rule')->nullable(); // RRULE format
    $t->dateTime('recurrence_end')->nullable();
    $t->uuid('recurrence_parent_uuid')->nullable(); // For recurring event series
    
    // External calendar integration
    $t->string('external_calendar_id')->nullable(); // Google Calendar ID, etc.
    $t->string('external_event_id')->nullable(); // External event ID
    $t->enum('sync_status', ['synced', 'pending_sync', 'sync_failed'])->default('synced');
    $t->timestamp('last_synced_at')->nullable();
    
    // Reminders and notifications
    $t->json('reminders')->nullable(); // Array of reminder times
    $t->boolean('send_notifications')->default(true);
    
    // Related entities (polymorphic)
    $t->string('related_type')->nullable(); // 'prospect', 'member', 'club', 'activity'
    $t->string('related_uuid', 36)->nullable();
    
    // Audit fields
    $t->foreignUuid('created_by')->constrained('users', 'uuid')->onDelete('cascade')->index();
    $t->foreignUuid('updated_by')->nullable()->constrained('users', 'uuid')->onDelete('cascade');
    $t->foreignUuid('deleted_by')->nullable()->constrained('users', 'uuid')->onDelete('cascade');
    $t->timestamps();
    $t->softDeletes();
    
    // Performance indexes
    $t->index(['start_datetime', 'end_datetime']);
    $t->index(['event_type', 'status']);
    $t->index(['is_recurring', 'recurrence_parent_uuid']);
    $t->index(['external_calendar_id', 'external_event_id']);
    $t->index(['related_type', 'related_uuid']);
    $t->index(['created_by', 'start_datetime']);
});
```

#### Calendar Participants
```php
Schema::create('calendar_participants', function (Blueprint $t) {
    $t->uuid('uuid')->primary();
    $t->foreignUuid('event_uuid')->constrained('calendar_events', 'uuid')->onDelete('cascade')->index();
    $t->foreignUuid('user_uuid')->constrained('users', 'uuid')->onDelete('cascade')->index();
    
    // Participant status
    $t->enum('status', ['invited', 'accepted', 'declined', 'tentative', 'no_response'])->default('invited');
    $t->enum('role', ['organizer', 'attendee', 'optional'])->default('attendee');
    
    // Response tracking
    $t->timestamp('responded_at')->nullable();
    $t->text('response_notes')->nullable();
    
    // Notification preferences
    $t->boolean('send_reminders')->default(true);
    $t->json('reminder_times')->nullable(); // Custom reminder times for this participant
    
    $t->timestamps();
    $t->softDeletes();
    
    // Performance indexes
    $t->unique(['event_uuid', 'user_uuid']);
    $t->index(['user_uuid', 'status']);
});
```

#### External Calendar Integrations
```php
Schema::create('external_calendars', function (Blueprint $t) {
    $t->uuid('uuid')->primary();
    $t->foreignUuid('user_uuid')->constrained('users', 'uuid')->onDelete('cascade')->index();
    
    // Calendar provider details
    $t->enum('provider', ['google', 'outlook', 'apple', 'ical', 'other'])->index();
    $t->string('calendar_id')->notNull(); // External calendar ID
    $t->string('calendar_name')->notNull();
    $t->string('calendar_color')->nullable();
    
    // Authentication and sync settings
    $t->text('access_token')->nullable();
    $t->text('refresh_token')->nullable();
    $t->timestamp('token_expires_at')->nullable();
    $t->boolean('is_active')->default(true);
    $t->boolean('two_way_sync')->default(true); // Sync both directions
    
    // Sync settings
    $t->enum('sync_frequency', ['realtime', 'hourly', 'daily', 'manual'])->default('hourly');
    $t->timestamp('last_synced_at')->nullable();
    $t->timestamp('next_sync_at')->nullable();
    
    // Event filtering
    $t->json('sync_filters')->nullable(); // Which events to sync
    $t->boolean('sync_past_events')->default(false);
    $t->integer('sync_days_future')->default(365); // How many days ahead to sync
    
    $t->timestamps();
    $t->softDeletes();
    
    // Performance indexes
    $t->unique(['user_uuid', 'provider', 'calendar_id']);
    $t->index(['provider', 'is_active']);
});
```

#### Time Tracking System
```php
Schema::create('time_blocks', function (Blueprint $t) {
    $t->uuid('uuid')->primary();
    $t->foreignUuid('user_uuid')->constrained('users', 'uuid')->onDelete('cascade')->index();
    $t->date('date')->notNull()->index();
    
    // Time block details
    $t->time('start_time')->notNull();
    $t->time('end_time')->notNull();
    $t->string('title')->notNull();
    $t->text('description')->nullable();
    
    // Time categorization
    $t->enum('category', ['major', 'minor', 'routine', 'wasted'])->default('routine');
    $t->enum('activity_type', [
        'prospect_work',      // Working with prospects
        'member_support',     // Supporting members
        'club_management',    // Club activities
        'product_sales',      // Product sales activities
        'training',           // Training and development
        'administration',     // Administrative tasks
        'personal',           // Personal time
        'exercise',           // Exercise and fitness
        'meal_prep',          // Meal preparation
        'rest',               // Rest and relaxation
        'travel',             // Travel time
        'other'               // Other activities
    ])->default('other');
    
    // Productivity metrics
    $t->integer('productivity_score')->nullable(); // 1-10 scale
    $t->boolean('was_productive')->default(false);
    $t->text('notes')->nullable();
    
    // Related entities
    $t->string('related_type')->nullable(); // 'prospect', 'member', 'club', 'activity'
    $t->string('related_uuid', 36)->nullable();
    
    // Audit fields
    $t->foreignUuid('created_by')->nullable()->constrained('users', 'uuid')->onDelete('cascade');
    $t->foreignUuid('updated_by')->nullable()->constrained('users', 'uuid')->onDelete('cascade');
    $t->foreignUuid('deleted_by')->nullable()->constrained('users', 'uuid')->onDelete('cascade');
    $t->timestamps();
    $t->softDeletes();
    
    // Performance indexes
    $t->index(['user_uuid', 'date']);
    $t->index(['category', 'activity_type']);
    $t->index(['related_type', 'related_uuid']);
});
```

#### Time Tracking Preferences
```php
Schema::create('time_tracking_preferences', function (Blueprint $t) {
    $t->uuid('uuid')->primary();
    $t->foreignUuid('user_uuid')->constrained('users', 'uuid')->onDelete('cascade')->unique();
    
    // Time block settings
    $t->enum('default_block_size', ['15min', '30min', '1hour', 'custom'])->default('30min');
    $t->integer('custom_block_minutes')->nullable();
    $t->boolean('auto_track_events')->default(true); // Auto-create time blocks from calendar events
    
    // Reminder settings
    $t->boolean('enable_time_tracking_reminders')->default(true);
    $t->json('reminder_times')->nullable(); // When to remind about time tracking
    $t->boolean('end_of_day_summary')->default(true);
    $t->boolean('weekly_productivity_report')->default(true);
    
    // Default categories
    $t->json('default_categories')->nullable(); // User's preferred activity categories
    $t->json('category_colors')->nullable(); // Color coding for categories
    
    // Integration settings
    $t->boolean('sync_with_calendar')->default(true);
    $t->boolean('auto_categorize_events')->default(true);
    
    $t->timestamps();
    $t->softDeletes();
});
```

### Recurrence Patterns

The system supports complex recurrence patterns using RRULE (RFC 5545) format:

#### Supported Recurrence Types:
1. **Hourly**: Every N hours
2. **Daily**: 
   - Every day
   - Every N days
   - Every weekday (Mon-Fri)
   - Every weekend (Sat-Sun)
   - Every alternate day
3. **Weekly**:
   - Every week
   - Every N weeks
   - Specific days of week (Mon, Wed, Fri)
   - Every alternate week
4. **Monthly**:
   - Every month
   - Every N months
   - Specific day of month (1st, 15th, last day)
   - Specific week of month (1st Monday, 3rd Friday)
5. **Quarterly**:
   - Every quarter
   - Every N quarters
   - Specific date (1st day of quarter)
   - Specific day of quarter (1st Monday of quarter)
6. **Half Yearly**:
   - Every half year
   - Specific date (1st day of half year)
   - Specific day of half year (1st Monday of half year)
7. **Yearly**:
   - Every year
   - Every N years
   - Specific date (March 15th)
   - Specific day of year (1st Monday of March)

#### Recurrence Rule Examples:
```php
// Every Monday and Wednesday at 9 AM
$rrule = 'FREQ=WEEKLY;BYDAY=MO,WE;BYHOUR=9';

// Every 2 weeks on Friday
$rrule = 'FREQ=WEEKLY;INTERVAL=2;BYDAY=FR';

// Every 3rd day of month
$rrule = 'FREQ=MONTHLY;BYMONTHDAY=3';

// Every alternate day
$rrule = 'FREQ=DAILY;INTERVAL=2';

// Every 2 hours during business hours
$rrule = 'FREQ=HOURLY;INTERVAL=2;BYHOUR=9,10,11,12,13,14,15,16,17';
```

### Key Features

#### 1. Calendar Integration
- **Google Calendar**: Full two-way sync with Google Calendar API
- **Outlook/Exchange**: Integration with Microsoft Graph API
- **Apple Calendar**: iCal format import/export
- **Other Calendars**: Generic iCal support for any calendar system

#### 2. Smart Scheduling
- **Conflict Detection**: Automatically detect scheduling conflicts
- **Availability Suggestions**: Suggest optimal meeting times
- **Travel Time**: Automatically add travel time between locations
- **Buffer Time**: Add buffer time before/after events
- **Recurring Events**: Complex recurrence patterns with exceptions

#### 3. Time Tracking
- **Flexible Time Blocks**: 15min, 30min, 1-hour, or custom intervals
- **Activity Categorization**: Major, minor, routine, wasted time
- **Productivity Scoring**: Rate productivity for each time block
- **Auto-tracking**: Automatically create time blocks from calendar events
- **Daily/Weekly Reports**: Productivity insights and time analysis

#### 4. Event Types and Integration
- **Activity Events**: Direct links to TTP, Flyer, Survey, etc.
- **Follow-up Events**: Prospect and member follow-up scheduling
- **Club Sessions**: Club meeting and session scheduling
- **Measurement Appointments**: Health measurement scheduling
- **Training Sessions**: Training and development events
- **Business Meetings**: Team and client meetings

#### 5. Notifications and Reminders
- **Multi-channel Notifications**: Email, SMS, push notifications
- **Smart Reminders**: Context-aware reminder timing
- **Follow-up Automation**: Automatic follow-up scheduling
- **Missed Event Alerts**: Notify about missed or incomplete events

#### 6. Reporting and Analytics
- **Time Analysis**: How time is spent across categories
- **Productivity Trends**: Track productivity over time
- **Event Completion Rates**: Success rate of scheduled events
- **Prospect Follow-up Metrics**: Follow-up effectiveness
- **Revenue Correlation**: Link time spent to business outcomes

### API Endpoints

#### Calendar Events
```php
// Calendar Event Management
GET /api/calendar-events
POST /api/calendar-events
GET /api/calendar-events/{event}
PUT /api/calendar-events/{event}
DELETE /api/calendar-events/{event}
GET /api/calendar-events/by-date-range
GET /api/calendar-events/by-type/{type}
POST /api/calendar-events/{event}/duplicate
POST /api/calendar-events/{event}/reschedule

// Recurring Events
POST /api/calendar-events/{event}/make-recurring
PUT /api/calendar-events/{event}/recurrence
DELETE /api/calendar-events/{event}/recurrence
GET /api/calendar-events/recurring-series/{parentUuid}

// Event Participants
GET /api/calendar-events/{event}/participants
POST /api/calendar-events/{event}/participants
PUT /api/calendar-events/{event}/participants/{participant}
DELETE /api/calendar-events/{event}/participants/{participant}
POST /api/calendar-events/{event}/participants/{participant}/respond
```

#### External Calendar Integration
```php
// External Calendar Management
GET /api/external-calendars
POST /api/external-calendars
GET /api/external-calendars/{calendar}
PUT /api/external-calendars/{calendar}
DELETE /api/external-calendars/{calendar}
POST /api/external-calendars/{calendar}/sync
GET /api/external-calendars/{calendar}/sync-status

// Calendar Provider OAuth
POST /api/external-calendars/google/authorize
POST /api/external-calendars/outlook/authorize
GET /api/external-calendars/oauth/callback
```

#### Time Tracking
```php
// Time Block Management
GET /api/time-blocks
POST /api/time-blocks
GET /api/time-blocks/{timeBlock}
PUT /api/time-blocks/{timeBlock}
DELETE /api/time-blocks/{timeBlock}
GET /api/time-blocks/by-date/{date}
GET /api/time-blocks/by-date-range
GET /api/time-blocks/productivity-report

// Time Tracking Preferences
GET /api/time-tracking-preferences
PUT /api/time-tracking-preferences
POST /api/time-tracking-preferences/auto-track
```

#### Calendar Views and Reports
```php
// Calendar Views
GET /api/calendar/agenda
GET /api/calendar/weekly
GET /api/calendar/monthly
GET /api/calendar/yearly
GET /api/calendar/search

// Analytics and Reports
GET /api/calendar/analytics/time-spent
GET /api/calendar/analytics/productivity
GET /api/calendar/analytics/event-completion
GET /api/calendar/analytics/follow-up-effectiveness
GET /api/calendar/reports/daily
GET /api/calendar/reports/weekly
GET /api/calendar/reports/monthly
```

### Business Logic

#### 1. Event Creation and Management
- **Smart Defaults**: Pre-fill event details based on type and context
- **Location Intelligence**: Auto-suggest locations based on history
- **Participant Management**: Easy invite and response tracking
- **Conflict Resolution**: Suggest alternative times for conflicts

#### 2. Recurrence Handling
- **Pattern Generation**: Convert user-friendly options to RRULE format
- **Exception Management**: Handle individual event modifications in series
- **Series Updates**: Update entire series or individual occurrences
- **End Date Handling**: Support for end dates and occurrence limits

#### 3. External Calendar Sync
- **Bidirectional Sync**: Keep local and external calendars in sync
- **Conflict Resolution**: Handle conflicts between local and external events
- **Selective Sync**: Choose which events to sync
- **Error Handling**: Graceful handling of sync failures

#### 4. Time Tracking Integration
- **Auto-block Creation**: Create time blocks from calendar events
- **Smart Categorization**: Auto-categorize based on event type
- **Productivity Insights**: Track productivity patterns
- **Goal Tracking**: Monitor time spent on goals vs. actual

#### 5. Notification System
- **Context-aware Reminders**: Remind based on event type and importance
- **Multi-channel Delivery**: Email, SMS, push notifications
- **Smart Timing**: Send reminders at optimal times
- **Follow-up Automation**: Auto-schedule follow-ups

### Frontend Integration

#### Calendar Views
1. **Agenda View**: List view with time slots
2. **Day View**: Hourly breakdown of single day
3. **Week View**: 7-day calendar view
4. **Month View**: Full month overview
5. **Year View**: Annual planning view

#### Time Tracking Interface
1. **Quick Time Entry**: Fast time block creation
2. **Drag-and-Drop**: Visual time block management
3. **Category Selection**: Easy activity categorization
4. **Productivity Rating**: Quick productivity scoring
5. **Daily Summary**: End-of-day time review

#### Integration Features
1. **Prospect Linking**: Direct links to prospect profiles
2. **Member Integration**: Connect events to member activities
3. **Club Management**: Schedule and track club sessions
4. **Activity Tracking**: Link to business activities
5. **Measurement Scheduling**: Health measurement appointments

### Security and Permissions

#### Access Control
- **Personal Events**: Users can only see their own events
- **Shared Events**: Participants can see events they're invited to
- **Team Events**: Coaches can see team member events (with permission)
- **Public Events**: Club sessions and activities visible to members

#### Data Privacy
- **Private Events**: Mark events as private (not visible to others)
- **Sensitive Information**: Encrypt sensitive event details
- **Audit Trail**: Track all calendar modifications
- **Data Retention**: Configurable data retention policies

### Implementation Phases

#### Phase 1: Core Calendar (Weeks 1-4)
- Basic calendar events CRUD
- Simple recurrence patterns
- Basic time tracking
- Calendar views (day, week, month)

#### Phase 2: Advanced Features (Weeks 5-8)
- Complex recurrence patterns
- External calendar integration
- Advanced time tracking
- Notification system

#### Phase 3: Integration & Analytics (Weeks 9-12)
- Full system integration
- Analytics and reporting
- Mobile optimization
- Performance optimization

## Implementation Status

### ✅ Completed Features

**Backend Infrastructure:**
- Laravel 12 with PHP 8.4 setup
- Laravel Sanctum authentication
- Spatie Laravel Permission for RBAC
- Laravel Auditing for audit trails
- Laravel Telescope for debugging
- Laravel Horizon for queue management
- Laravel Pulse for application monitoring
- Laravel Backup for data backup
- Development tools: Laravel Pint, Larastan, PHPUnit, Laravel IDE Helper

**Database Migrations:**
- Users table with UUID primary keys
- Prospects table with all required fields
- Profile Details table with marketing level hierarchy
- Health Data table for user/prospect health information
- Measurements table for tracking body metrics
- Daily Trackers table for wellness tracking
- Groups table for user categorization
- Marketing Levels table for business hierarchy
- Contacts table for multiple contact methods
- Addresses table for location management
- Photos table for media storage

**API Endpoints:**
- Authentication: register, login, logout, forgot-password, forgot-username, reset-password, email verification
- Prospect management: full CRUD with conversion functionality
- Profile details management: full CRUD with prospect conversion
- Health data management: full CRUD operations
- Measurement management: full CRUD operations
- Daily tracker management: full CRUD with validation

**Models & Relationships:**
- User model with UUID support and auditing
- Prospect model with polymorphic relationships
- ProfileDetail model with marketing level hierarchy
- HealthData model with polymorphic relationships
- Measurement model with polymorphic relationships
- DailyTracker model with validation support
- All models include proper relationships and soft deletes

**New Models Required:**
- Product model with UUID support and auditing
- CompanyInvoice model with UUID support and auditing
- InvoiceItem model with UUID support and auditing
- StockLocation model with UUID support and auditing
- Stock model with UUID support and auditing
- StockItem model with UUID support and auditing
- StockMovement model with UUID support and auditing
- StockReservation model with UUID support and auditing
- StockValuation model with UUID support and auditing
- Club model with UUID support and auditing
- ClubMembership model with UUID support and auditing
- ShakeCombination model with UUID support and auditing
- ClubShakeCounter model with UUID support and auditing

**Role-Based Access Control:**
- Super Admin: Full system access
- Mentor: Create/update/delete all records
- Coach: Create own, update own/all, delete own
- Member: Create/update/delete own records
- Guest: Read-only access (ABAC controlled)

**Frontend Development:**
- Quasar framework setup
- Vue.js 4 components
- Pinia store management
- User interface for all CRUD operations
- Profile switching functionality
- Dark mode support

### 🚧 In Progress / Planned Features

**Additional Backend Features:**
- Club management system (✅ Schema defined)
- Stock and inventory management (✅ Schema defined)
- Financial tracking and reporting
- Assessment and survey tools
- Product/Service/Program/Batch management (✅ Schema defined)
- Communication and notification systems
- Advanced ABAC implementation with tags

**Security Enhancements:**
- Password policy enforcement
- Token expiration management
- Rate limiting implementation
- Advanced audit trail features

**Integration Features:**
- Photo compression and storage optimization
- Address validation services
- Google Maps integration
- Email/SMS notification systems
