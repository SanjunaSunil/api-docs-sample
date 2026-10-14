# Getting Started with the Taskly API

The Taskly API lets you create, read, update, and delete tasks and users programmatically. This guide walks you through making your first API call in under five minutes.

---

## Prerequisites

- A Taskly account ([sign up at taskly.io](https://taskly.io))
- An API key (see [Authentication](authentication.md))
- A tool for making HTTP requests — curl, Postman, or any HTTP client

---

## Base URL

All API requests use the following base URL:

```
https://api.taskly.io/v1
```

All requests must be made over **HTTPS**. HTTP requests are rejected.

---

## Step 1: Get your API key

1. Log in to your Taskly account.
2. Go to **Settings → API Keys**.
3. Select **Generate new key**.
4. Copy the key and store it securely. It is only shown once.

---

## Step 2: Make your first request

The following example retrieves your task list.

**Request**

```bash
curl https://api.taskly.io/v1/tasks \
  -H "Authorization: Bearer YOUR_API_KEY"
```

**Response**

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
      "created_at": "2026-10-10T08:30:00Z"
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

## Step 3: Create a task

**Request**

```bash
curl -X POST https://api.taskly.io/v1/tasks \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "title": "Review pull request",
    "priority": "medium",
    "due_date": "2026-10-22"
  }'
```

**Response**

```json
{
  "data": {
    "id": "task_02k9n3p",
    "title": "Review pull request",
    "status": "todo",
    "priority": "medium",
    "due_date": "2026-10-22",
    "assignee_id": null,
    "created_at": "2026-10-14T11:00:00Z"
  }
}
```

---

## Next steps

- [Authentication](authentication.md) — learn about API key scopes and best practices
- [Tasks reference](endpoints/tasks.md) — full endpoint reference for `/tasks`
- [Users reference](endpoints/users.md) — manage users and assignees
- [Error codes](errors.md) — understand and handle API errors
