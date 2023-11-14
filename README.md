# Audit Trail - General Guide

## Overview

This guide outlines the creation of Audit Trails, security-relevant chronological records that provide documentary evidence of activities affecting a specific system. Adhering to the JSON schema detailed below ensures consistency and integrity in generating these records.

## Audit Trail Schema

The schema, accessible [here](./schema.json), structures the Audit Trail as an object with the following key properties:

### `time`

- **`when` (mandatory):** Timestamp in ISO 8601 format when the activity occurred.
- **`duration` (optional):** Duration of the activity in ISO 8601 duration format (e.g., 'PT2H30M' for 2 hours and 30 minutes).

### `subject`

- **`kind` (mandatory):** Type of user or system (user, machine, system, other).
- **`id` (mandatory):** Unique identifier of the actor.
- **`agent` (optional):** Software or tool used by the actor to perform the action.

### `action`

- **`kind` (mandatory):** Type of action: 'dispositive' for changing functional state, 'informative' for accessing functional state.
- **`operation` (mandatory):** Description of the specific action or operation performed.
- **`status` (mandatory):** Result status of the action: 'succeeded' or 'failed'.
- **`changes` (optional):** Details of changes made during the action.

   - **`param` (mandatory):** Name of the parameter or attribute that was changed.
   - **`oldValue` (optional):** The previous value of the parameter.
   - **`newValue` (optional):** The new value of the parameter.

### `targets`

- **`kind` (mandatory):** Description of the resource type in URN format (e.g., 'urn:app/user').
- **`id` (mandatory):** Resource identifier.

**Note:** `targets` is an array of objects describing targets or resources affected by the action.

## Usage Examples

### Example 1: Successful Login

```json
{
  "time": {
    "when": "2023-11-10T08:30:00Z"
  },
  "subject": {
    "kind": "user",
    "id": "john.doe",
    "agent": "Web App"
  },
  "action": {
    "kind": "dispositive",
    "operation": "Login",
    "status": {
      "result": "succeeded"
    }
  },
  "targets": null
}
```

### Example 2: User Profile Update

```json
{
  "time": {
    "when": "2023-11-12T14:45:00Z"
  },
  "subject": {
    "kind": "user",
    "id": "alice.smith",
    "agent": "Mobile App"
  },
  "action": {
    "kind": "dispositive",
    "operation": "Update Profile",
    "status": {
      "result": "succeeded"
    },
    "changes": [
      {
        "param": "name",
        "oldValue": "Alice",
        "newValue": "Alice Smith"
      },
      {
        "param": "email",
        "oldValue": "alice@example.com",
        "newValue": "alice.smith@example.com"
      }
    ]
  },
  "targets": [
    {
      "kind": "urn:company:user",
      "id": "12345"
    }
  ]
}
```

### Example 3: Failed Login Attempt

```json
{
  "time": {
    "when": "2023-11-15T10:00:00Z"
  },
  "subject": {
    "kind": "user",
    "id": "bob.jones",
    "agent": "Desktop App"
  },
  "action": {
    "kind": "dispositive",
    "operation": "Login",
    "status": {
      "result": "failed",
      "code": 401,
      "reason": "Invalid credentials"
    }
  },
  "targets": null
}
```

These examples demonstrate different scenarios of Audit Trail usage, covering successful logins, user profile updates, and failed login attempts. Ensure compliance with the schema for consistency and integrity in recorded information.