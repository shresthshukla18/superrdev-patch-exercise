# Patch Notes

## 1. Task search status filter

- **Location:** `backend/src/main/java/com/internal/tasktracker/TaskRepository.java`
- **Discovery:** `GET /api/tasks?q=rate&status=DONE` returned OPEN and IN_PROGRESS tasks.
- **Root cause:** SQL `AND`/`OR` operator precedence caused the status condition to apply only to the description branch.
- **Fix:** Grouped the title/description search with parentheses and applied the status filter to the complete search condition.
- **Why:** Ensures archived filtering, text matching, and status filtering are all applied consistently.

## 2. Frontend request error state

- **Location:** `frontend/src/hooks/useTasks.js`
- **Discovery:** The request error handler set `error` but never reset `loading`.
- **Root cause:** The failure path omitted `setLoading(false)`, and successful requests did not clear a previous error.
- **Fix:** Clear the previous error when starting a request and set loading to false when a request fails.
- **Why:** Prevents the UI from remaining indefinitely in a loading state after an API failure.
