# Tasks

The `/tasks` endpoints let you create, retrieve, update, and delete tasks.

---

## The task object

```json
{
  "id": "task_01j8k2m",
  "title": "Write API documentation",
  "description": "Create OpenAPI spec and Markdown reference docs.",
  "status": "in_progress",
  "priority": "high",
  "due_date": "2026-10-20",
  "assignee_id": "user_7xp9q",
  "tags": ["docs", "api"],
  "created_at": "2026-10-10T08:30:00Z",
  "updated_at": "2026-10-13T14:20:00Z"
}
```

### Task object fields

| Field | Type | Description |
|---|---|---|
| `id` | string | Unique identifier for the task |
| `title` | string | Short title of the task (max 255 characters) |
| `description` | string \| null | Optional longer description |
| `status` | string | Current status: `todo`, `in_progress`, or `done` |
| `priority` | string | Priority level: `low`, `medium`, or `high` |
| `due_date` | string \| null | ISO 8601 date (`YYYY-MM-DD`) |
| `assignee_id` | string \| null | ID of the assigned user |
| `tags` | array of strings | Optional list of tags |
| `created_at` | string | ISO 8601 timestamp of creation |
| `updated_at` | string | ISO 8601 timestamp of last update |

---

## List tasks

Retrieves a paginated list of tasks.

```
GET /v1/tasks
```

### Query parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `status` | string | No | Filter by status: `todo`, `in_progress`, `done` |
| `priority` | string | No | Filter by priority: `low`, `medium`, `high` |
| `assignee_id` | string | No | Filter by assignee |
| `page` | integer | No | Page number (default: `1`) |
| `per_page` | integer | No | Results per page, max `100` (default: `20`) |

### Example request

```bash
curl "https://api.taskly.io/v1/tasks?status=in_progress&priority=high" \
  -H "Authorization: Bearer YOUR_API_KEY"
```

### Example response

```json
{
  "data": [
    {
      "id": "task_01j8k2m",
      "title": "Write API documentation",
      "status": "in_progress",
      "priority": "high",
      "due_date": "2026-10-20",
      "assignee_id": "user_7xp9q",
      "created_at": "2026-10-10T08:30:00Z",
      "updated_at": "2026-10-13T14:20:00Z"
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

## Get a task

Retrieves a single task by ID.

```
GET /v1/tasks/{task_id}
```

### Path parameters

| Parameter | Type | Description |
|---|---|---|
| `task_id` | string | The ID of the task |

### Example request

```bash
curl https://api.taskly.io/v1/tasks/task_01j8k2m \
  -H "Authorization: Bearer YOUR_API_KEY"
```

### Example response

```json
{
  "data": {
    "id": "task_01j8k2m",
    "title": "Write API documentation",
    "description": "Create OpenAPI spec and Markdown reference docs.",
    "status": "in_progress",
    "priority": "high",
    "due_date": "2026-10-20",
    "assignee_id": "user_7xp9q",
    "tags": ["docs", "api"],
    "created_at": "2026-10-10T08:30:00Z",
    "updated_at": "2026-10-13T14:20:00Z"
  }
}
```

---

## Create a task

Creates a new task.

```
POST /v1/tasks
```

### Request body

| Field | Type | Required | Description |
|---|---|---|---|
| `title` | string | Yes | Title of the task (max 255 characters) |
| `description` | string | No | Longer description |
| `priority` | string | No | `low`, `medium`, or `high` (default: `medium`) |
| `due_date` | string | No | ISO 8601 date (`YYYY-MM-DD`) |
| `assignee_id` | string | No | ID of the user to assign |
| `tags` | array | No | List of tag strings |

### Example request

```bash
curl -X POST https://api.taskly.io/v1/tasks \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "title": "Review pull request",
    "priority": "medium",
    "due_date": "2026-10-22",
    "assignee_id": "user_7xp9q"
  }'
```

### Example response

```json
{
  "data": {
    "id": "task_02k9n3p",
    "title": "Review pull request",
    "description": null,
    "status": "todo",
    "priority": "medium",
    "due_date": "2026-10-22",
    "assignee_id": "user_7xp9q",
    "tags": [],
    "created_at": "2026-10-14T11:00:00Z",
    "updated_at": "2026-10-14T11:00:00Z"
  }
}
```

---

## Update a task

Updates one or more fields on an existing task. Only the fields you include are changed.

```
PATCH /v1/tasks/{task_id}
```

### Path parameters

| Parameter | Type | Description |
|---|---|---|
| `task_id` | string | The ID of the task to update |

### Request body

Include only the fields you want to update. All fields are optional.

| Field | Type | Description |
|---|---|---|
| `title` | string | New title |
| `description` | string | New description |
| `status` | string | New status: `todo`, `in_progress`, or `done` |
| `priority` | string | New priority: `low`, `medium`, or `high` |
| `due_date` | string | New due date (ISO 8601) |
| `assignee_id` | string \| null | New assignee, or `null` to unassign |
| `tags` | array | Replaces the existing tag list |

### Example request

```bash
curl -X PATCH https://api.taskly.io/v1/tasks/task_02k9n3p \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "status": "done"
  }'
```

### Example response

```json
{
  "data": {
    "id": "task_02k9n3p",
    "title": "Review pull request",
    "status": "done",
    "priority": "medium",
    "due_date": "2026-10-22",
    "assignee_id": "user_7xp9q",
    "tags": [],
    "created_at": "2026-10-14T11:00:00Z",
    "updated_at": "2026-10-14T15:45:00Z"
  }
}
```

---

## Delete a task

Permanently deletes a task. This action cannot be undone.

```
DELETE /v1/tasks/{task_id}
```

### Path parameters

| Parameter | Type | Description |
|---|---|---|
| `task_id` | string | The ID of the task to delete |

### Example request

```bash
curl -X DELETE https://api.taskly.io/v1/tasks/task_02k9n3p \
  -H "Authorization: Bearer YOUR_API_KEY"
```

### Response

Returns `204 No Content` on success. No response body.

---

## Error responses

| HTTP status | Error code | Meaning |
|---|---|---|
| `400` | `validation_error` | A required field is missing or invalid |
| `401` | `invalid_api_key` | Authentication failed |
| `403` | `insufficient_scope` | API key lacks `tasks:write` scope |
| `404` | `task_not_found` | No task exists with that ID |

See [Error codes](../errors.md) for full details.
