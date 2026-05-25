# Notification Preferences Design

## Goal

Let users control which product notifications they receive without changing unrelated account behavior.

## Context

The product currently sends several notification types through shared delivery channels. Users need simple preferences that can be read by delivery code and updated through an API.

## Approved Product Behavior

### Default Preferences

When a user account is created, default notification preferences are available.

Default values:

- product updates: enabled
- security alerts: enabled
- weekly summary: disabled

### Preference Updates

Users can update notification preferences after login.

Accepted behavior:

- only authenticated users can update their own preferences;
- unknown preference keys are rejected;
- security alerts cannot be disabled in this feature version;
- successful updates return the current preference state.

### Notification Delivery Reads

Notification delivery code should be able to check the current preference state before sending optional notifications.

Security alerts are always sendable regardless of stored preferences.

## Open Implementation Details

These must be locked during task planning:

- exact persistence shape;
- exact API route and request schema;
- whether preference keys are represented as enum values or constants;
- whether defaults are materialized on account creation or lazily resolved.
