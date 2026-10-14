# Authentication

The Taskly API uses **API key authentication**. All requests must include a valid API key in the request header.

---

## API key format

Include your API key in the `Authorization` header using the `Bearer` scheme:

```
Authorization: Bearer YOUR_API_KEY
```

**Example:**

```bash
curl https://api.taskly.io/v1/tasks \
  -H "Authorization: Bearer tsk_live_4xKj9mP2nQr8vLw1"
```

---

## API key types

| Type | Prefix | Use |
|---|---|---|
| Live key | `tsk_live_` | Production environments |
| Test key | `tsk_test_` | Development and testing — no real data affected |

Use test keys during development to avoid affecting production data.

---

## Key scopes

When generating a key, you can restrict it to specific scopes:

| Scope | Access |
|---|---|
| `tasks:read` | Read tasks only |
| `tasks:write` | Create, update, and delete tasks |
| `users:read` | Read user profiles |
| `users:write` | Create and update users |
| `admin` | Full access to all endpoints |

Use the minimum scopes your integration requires.

---

## Security best practices

- **Never expose your API key** in client-side code, public repositories, or logs.
- **Rotate keys regularly.** Go to **Settings → API Keys** to revoke and regenerate keys.
- **Use environment variables** to store keys in your application:

  ```bash
  export TASKLY_API_KEY=tsk_live_4xKj9mP2nQr8vLw1
  ```

  ```python
  import os
  api_key = os.environ.get("TASKLY_API_KEY")
  ```

- **Use test keys in CI/CD pipelines** — never commit live keys.

---

## Authentication errors

| HTTP status | Error code | Meaning |
|---|---|---|
| `401` | `invalid_api_key` | The API key is missing or invalid |
| `401` | `expired_api_key` | The API key has been revoked |
| `403` | `insufficient_scope` | The key lacks permission for this action |

See [Error codes](../errors.md) for the full list.
