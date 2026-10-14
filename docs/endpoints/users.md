# Users

The `/users` endpoints let you retrieve and manage user profiles in your Taskly workspace.

---

## The user object

```json
{
  "id": "user_7xp9q",
  "name": "Sanjuna Sunil",
  "email": "sanjuna@example.com",
  "role": "member",
  "created_at": "2026-09-01T09:00:00Z"
}
```

### User object fields

| Field | Type | Description |
|---|---|---|
| `id` | string | Unique identifier for the user |
| `name` | string | Display name |
| `email` | string | Email address |
| `role` | string | Workspace role: `admin` or `member` |
| `created_at` | string | ISO 8601 timestamp of account creation |

---

## List users

Retrieves all users in the workspace.

```
GET /v1/users
```

### Query parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `role` | string | No | Filter by role: `admin` or `member` |
| `page` | integer | No | Page number (default: `1`) |
| `per_page` | integer | No | Results per page, max `100` (default: `20`) |

### Example request

```bash
curl https://api.taskly.io/v1/users \
  -H "Authorization: Bearer YOUR_API_KEY"
```

### Example response

```json
{
  "data": [
    {
      "id": "user_7xp9q",
      "name": "Sanjuna Sunil",
      "email": "sanjuna@example.com",
      "role": "member",
      "created_at": "2026-09-01T09:00:00Z"
    }
  ],
  "meta": {
    "total": 1,
    "page": 1,
    "per_page": 20
  }
}
```

---

## Get a user

Retrieves a single user by ID.

```
GET /v1/users/{user_id}
```

### Path parameters

| Parameter | Type | Description |
|---|---|---|
| `user_id` | string | The ID of the user |

### Example request

```bash
curl https://api.taskly.io/v1/users/user_7xp9q \
  -H "Authorization: Bearer YOUR_API_KEY"
```

### Example response

```json
{
  "data": {
    "id": "user_7xp9q",
    "name": "Sanjuna Sunil",
    "email": "sanjuna@example.com",
    "role": "member",
    "created_at": "2026-09-01T09:00:00Z"
  }
}
```

---

## Error responses

| HTTP status | Error code | Meaning |
|---|---|---|
| `401` | `invalid_api_key` | Authentication failed |
| `403` | `insufficient_scope` | API key lacks `users:read` scope |
| `404` | `user_not_found` | No user exists with that ID |

See [Error codes](../errors.md) for full details.
