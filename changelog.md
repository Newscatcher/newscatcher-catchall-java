## 1.5.0 - 2026-04-23
* ## [1.5.0] - 2025
### Added
* **`JobsClient.deleteJob()` / `AsyncJobsClient.deleteJob()`** — soft-deletes a job by ID; the job is flagged as deleted and no longer appears in list results while underlying data is retained.
* **`MonitorsClient.deleteMonitor()` / `AsyncMonitorsClient.deleteMonitor()`** — soft-deletes a monitor by ID, immediately stopping scheduled job execution and hiding it from list results.
* **`MonitorsClient.getMonitorStatusHistory()` / `AsyncMonitorsClient.getMonitorStatusHistory()`** — retrieves the full ordered execution history of a monitor as a list of `MonitorStatusEntry` items.
* **`OwnershipFilter`** — new enum (`ALL`, `OWN`, `SHARED`) added as an optional `ownership` filter on `ListDatasetsRequest`, `GetUserJobsRequest`, and `ListMonitorsRequest`.
* **`search` query parameter** — new optional text-filter field on `GetUserJobsRequest` and `ListMonitorsRequest` for case-insensitive substring matching.
* **`SharingInfo` / `SharingInfoPermission`** — new types surfacing sharing metadata and permission level (`VIEW`, `EDIT`, `MANAGE`) on `UserJob`, `MonitorListItemDto`, and `PullJobResponseDto`.
* **`UnauthorizedError`** — new typed exception for HTTP 401 responses, raised from `deleteMonitor` and `getMonitorStatusHistory` endpoints.
* **New request/response types** (`DeleteJobRequest`, `DeleteJobResponseDto`, `DeleteMonitorRequest`, `DeleteMonitorResponseDto`, `GetMonitorStatusHistoryRequest`, `MonitorStatusHistoryResponseDto`, `MonitorStatusEntry`, `MonitorStatusEntryStatus`) added to support the new job and monitor operations.
### Fixed
* **`ValidationErrorDetailLocItem`** — deserializer now correctly handles integer location segments before attempting string conversion, preventing parse failures on numeric path components.

