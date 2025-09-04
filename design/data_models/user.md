# User Data Model

This document defines the data model for a "user" object in The Fellowship of the Cider application.

## Object Structure

```json
{
  "id": "uuid",
  "username": "string",
  "created_at": "timestamp"
}
```

## Field Descriptions

*   **`id`**: The user's unique identifier from Supabase Authentication.
*   **`username`**: The user's chosen display name.
*   **`created_at`**: The timestamp of when the user account was created.
