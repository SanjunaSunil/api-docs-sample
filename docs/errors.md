# Error Codes

When a request fails, the Taskly API returns a JSON error response with a machine-readable error code and a human-readable message.

---

## Error response format

```json
{
  "error": {
    "code": "task_not_found",
    "message": "No task exists with the ID 'task_99xyz'.",
    "status": 404
  }
}
```

| Field | Type | Description |
|---|---|---|
| `code` | string | Machine-readable error identifier |
| `message` | string | Human-readable description of the error |
| `status` | integer | HTTP status code |

---

## HTTP status codes

| Status | Meaning |
|---|---|
| `200 OK` | Request succeeded |
| `201 Created` | Resource was created successfully |
| `204 No Content` | Request succeeded with no response body (used for DELETE) |
| `400 Bad Request` | The request was malformed or missing required fields |
| `401 Unauthorized` | Authentication failed |
| `403 Forbidden` | The API key lacks permission for this action |
| `404 Not Found` | The requested resource does not exist |
| `409 Conflict` | The request conflicts with existing data |
| `422 Unprocessable Entity` | The request body was valid JSON but failed validation |
| `429 Too Many Requests` | Rate limit exceeded |
| `500 Internal Server Error` | An unexpected error occurred on the server |

---

## Error codes reference

### Authentication errors

| Code | Status | Description |
|---|---|---|
| `invalid_api_key` | `401` | The API key is missing, malformed, or invalid |
| `expired_api_key` | `401` | The API key has been revoked |
| `insufficient_scope` | `403` | The API key does not have the required scope |

### Validation errors

| Code | Status | Description |
|---|---|---|
| `validation_error` | `400` | One or more fields failed validation. The `message` field identifies which field and why |
| `missing_required_field` | `422` | A required field was not included in the request body |
| `invalid_field_value` | `422` | A field value is outside the allowed range or format |

### Resource errors

| Code | Status | Description |
|---|---|---|
| `task_not_found` | `404` | No task exists with the provided ID |
| `user_not_found` | `404` | No user exists with the provided ID |
| `duplicate_entry` | `409` | A resource with the same unique field already exists |

### Rate limiting

| Code | Status | Description |
|---|---|---|
| `rate_limit_exceeded` | `429` | You have exceeded the allowed number of requests per minute |

### Server errors

| Code | Status | Description |
|---|---|---|
| `internal_error` | `500` | An unexpected error occurred. If this persists, contact support |

---

## Rate limits

The Taskly API enforces the following rate limits:

| Plan | Requests per minute |
|---|---|
| Free | 60 |
| Pro | 300 |
| Enterprise | Unlimited |

When you exceed the rate limit, the API returns a `429` response with a `Retry-After` header indicating how many seconds to wait before retrying:

```
HTTP/1.1 429 Too Many Requests
Retry-After: 30
```

---

## Handling errors

### Recommended approach

1. Always check the HTTP status code before processing the response body.
2. On `4xx` errors, read the `error.code` field to identify the specific problem.
3. On `5xx` errors, implement exponential backoff and retry.
4. On `429`, wait for the number of seconds in `Retry-After` before retrying.

### Example error handling (Python)

```python
import requests
import time

def get_task(api_key, task_id):
    response = requests.get(
        f"https://api.taskly.io/v1/tasks/{task_id}",
        headers={"Authorization": f"Bearer {api_key}"}
    )

    if response.status_code == 200:
        return response.json()["data"]
    elif response.status_code == 404:
        raise ValueError(f"Task {task_id} not found")
    elif response.status_code == 429:
        retry_after = int(response.headers.get("Retry-After", 60))
        time.sleep(retry_after)
        return get_task(api_key, task_id)
    else:
        error = response.json().get("error", {})
        raise Exception(f"API error {error.get('code')}: {error.get('message')}")
```
