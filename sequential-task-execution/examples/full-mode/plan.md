# Plan

Design source of truth:

- `tasks/YYYY-MM-DD-notification-preferences/design.md`

## Approved Task Split

### Task 1: Preference Storage

Status: approved for implementation.

Target behavior:

- store notification preference values per user;
- expose default preference values for new users.

Primary test layer:

- unit/service or persistence test.

Expected failing test:

- preference defaults cannot be resolved or stored.

Implementation scope:

- persistence object or table;
- enum/constants for preference keys;
- default resolver;
- test fixture/factory if useful.

Review criteria:

- preferences are scoped to one user;
- unsupported preference keys cannot be stored;
- no API changes yet.

#### Approved Implementation Plan

Primary test layer: unit/service persistence test.

Primary test file:

- Create `tests/Unit/NotificationPreferenceStorageTest.php`

Production files:

- Create `app/Models/NotificationPreference.php`
- Create `app/Enums/NotificationPreferenceKey.php`
- Create `app/Services/NotificationPreferenceDefaults.php`
- Create `database/migrations/YYYY_MM_DD_000001_create_notification_preferences_table.php`

Implementation steps:

1. Write the failing storage/defaults test.
2. Run focused red verification:
   - `php artisan test --filter=NotificationPreferenceStorageTest`
   - Expected before implementation: failure because storage/default resolver does not exist.
3. Create the preference key enum/constants.
4. Create the notification preferences table.
5. Create the model and user relationship.
6. Implement default preference resolution.
7. Run focused green verification:
   - `php artisan test --filter=NotificationPreferenceStorageTest`
8. Save implementation notes and update task tracking.

### Task 2: Preference Update API

Status: pending task-specific approach approval.

Target behavior:

- authenticated users can update allowed notification preferences;
- invalid keys and forbidden security-alert changes are rejected.

Primary test layer:

- API feature test.

Expected failing test:

- update endpoint does not exist or does not enforce constraints.

Implementation scope:

- request validation;
- update service;
- route/controller hook;
- response serialization.

Review criteria:

- cannot update another user's preferences;
- security alerts remain enabled;
- response returns current state.

### Task 3: Documentation

Status: pending task-specific approach approval.

Target behavior:

- document user-visible behavior and API contract.

Primary test layer:

- docs content review.

Expected failing test:

- not applicable; documentation task.

Implementation scope:

- product behavior notes;
- API contract documentation;
- docs index update.

Review criteria:

- docs match implemented behavior;
- docs do not include implementation-only internals in product docs.
