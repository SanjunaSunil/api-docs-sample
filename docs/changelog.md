# Changelog

All notable changes to the Taskly API are documented here.

---

## v1.2.0 — 2026-10-01

### Added
- `tags` field on the task object — pass an array of strings to categorize tasks.
- `GET /v1/tasks` now supports filtering by `assignee_id`.

### Changed
- The `due_date` field now accepts ISO 8601 datetime strings (`YYYY-MM-DDTHH:MM:SSZ`) in addition to date strings (`YYYY-MM-DD`). Existing date-only values are preserved.

### Fixed
- `PATCH /v1/tasks/{task_id}` no longer returns a `500` error when `assignee_id` is set to `null` to unassign a task.

---

## v1.1.0 — 2026-08-15

### Added
- `GET /v1/users` and `GET /v1/users/{user_id}` endpoints.
- `assignee_id` field on task objects.
- `per_page` query parameter on list endpoints (max `100`, default `20`).

### Changed
- Rate limit response now includes a `Retry-After` header.

---

## v1.0.0 — 2026-07-01

Initial release.

### Added
- `GET /v1/tasks` — list tasks
- `GET /v1/tasks/{task_id}` — get a task
- `POST /v1/tasks` — create a task
- `PATCH /v1/tasks/{task_id}` — update a task
- `DELETE /v1/tasks/{task_id}` — delete a task
- API key authentication with scope support
