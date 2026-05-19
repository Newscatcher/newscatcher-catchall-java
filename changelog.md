## 2.0.0 - 2026-05-19
### Breaking Changes
* **`ListEntitiesInDatasetRequest`** — renamed to **`ListDatasetEntitiesRequest`**; update all imports and usages to the new class name.
* **`listEntitiesInDataset()`** — now sends `POST /catchAll/datasets/{id}/entities/list` instead of `GET /catchAll/datasets/{id}/entities`; recompile against the new SDK and update any HTTP-level integrations.
* **`ConnectedEntity.Builder`** — a new mandatory `.type(entityType)` step is now required between `.relation()` and `.build()`; update all existing builder chains accordingly.
### Added
* **`ConnectedEntity.getType()`** — returns the entity type string for a connected entity.
* **`ConnectedEntity.getCompany()`** — returns an `Optional<CompanyAttributes>` with stored company attributes when present.
* **`SubmitRequestDto.getEdScoreMin()`** — new optional `ed_score_min` field to set a minimum relevance score threshold for Company Watchlist mode results.
* **`CreateMonitorRequestDto.getTimezone()`** — new optional `timezone` field accepting an IANA timezone identifier used as a fallback when the schedule string lacks an explicit timezone.
### Changed
* **`addEntitiesToDataset()`** — now throws `UnprocessableEntityError` on HTTP 422 responses in addition to the existing `NotFoundError` on 404.
* **`createDataset()` / `uploadCsvToDataset()` documentation** — clarifies that both `name` and `domain` columns are required in the uploaded CSV (previously only `name` was documented as required).
### Fixed
* **`ListDatasetsRequest` and `ListDatasetEntitiesRequest`** — getter methods were incorrectly annotated with `@JsonIgnore` instead of `@JsonProperty`, causing query parameters (`page`, `page_size`, `search`, etc.) to be silently dropped during serialization; now correctly annotated.

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

