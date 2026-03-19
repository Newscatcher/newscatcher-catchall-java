## 1.2.0 - 2026-03-19
* ### Added
* **Job processing modes** now support "base" (full pipeline) and "lite" (lightweight extraction) options via the optional `mode` parameter when creating jobs, with job response objects including the processing mode used.
* **`getPlanLimits()` method** on `MetaClient` retrieves plan features and current usage information for the authenticated organization.

