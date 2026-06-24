# Reference
## Jobs
<details><summary><code>client.jobs.getUserJobs() -> ListUserJobsResponseDto</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns all jobs created by the authenticated user.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.jobs().getUserJobs(
    GetUserJobsRequest
        .builder()
        .projectId("60a85db4-78ec-4b78-876a-bc7d9cdadd04")
        .build()
);
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**page:** `Optional<Integer>` — Page number to retrieve.
    
</dd>
</dl>

<dl>
<dd>

**pageSize:** `Optional<Integer>` — Number of records per page.
    
</dd>
</dl>

<dl>
<dd>

**search:** `Optional<String>` — Filter results by text (case-insensitive substring match).
    
</dd>
</dl>

<dl>
<dd>

**ownership:** `Optional<OwnershipFilter>` 
    
</dd>
</dl>

<dl>
<dd>

**projectId:** `Optional<String>` — Filter results to resources belonging to this project.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.jobs.validateQuery(request) -> ValidateQueryResponseDto</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Checks whether a query is well-formed and likely to produce good results before submitting a job.

Returns a quality assessment with a status level, identified issues, and actionable suggestions.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.jobs().validateQuery(
    ValidateQueryRequestDto
        .builder()
        .query("Series B funding rounds for SaaS startups")
        .build()
);
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**query:** `String` — Plain text query to validate.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.jobs.initialize(request) -> InitializeResponseDto</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Get suggested validators, enrichments, and date ranges for a query.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.jobs().initialize(
    InitializeRequestDto
        .builder()
        .query("Series B funding rounds for SaaS startups")
        .context("Focus on funding amount and company name")
        .build()
);
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**query:** `String` 
    
</dd>
</dl>

<dl>
<dd>

**context:** `Optional<String>` 
    
</dd>
</dl>

<dl>
<dd>

**connectedDatasetIds:** `Optional<List<String>>` — Optional list of watchlist dataset IDs connected to this job.
    
</dd>
</dl>

<dl>
<dd>

**fetchAllWatchlistNews:** `Optional<Boolean>` 

When true, returns generic news validators and enrichments suitable for
watchlist-based article collection instead of query-specific fields.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.jobs.createJob(request) -> SubmitResponseDto</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Submit a query to create a new processing job.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.jobs().createJob(
    SubmitRequestDto
        .builder()
        .query("Series B funding rounds for SaaS startups")
        .context("Focus on funding amount and company name")
        .limit(10)
        .startDate(OffsetDateTime.parse("2026-02-18T00:00:00Z"))
        .endDate(OffsetDateTime.parse("2026-02-23T00:00:00Z"))
        .mode(SubmitRequestDtoMode.BASE)
        .build()
);
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**query:** `String` 
    
</dd>
</dl>

<dl>
<dd>

**context:** `Optional<String>` 
    
</dd>
</dl>

<dl>
<dd>

**limit:** `Optional<Integer>` 
    
</dd>
</dl>

<dl>
<dd>

**startDate:** `Optional<OffsetDateTime>` 
    
</dd>
</dl>

<dl>
<dd>

**endDate:** `Optional<OffsetDateTime>` 
    
</dd>
</dl>

<dl>
<dd>

**validators:** `Optional<List<ValidatorSchema>>` 

Custom validators for filtering web page clusters.

If not provided, validators are generated automatically based on the query.
    
</dd>
</dl>

<dl>
<dd>

**enrichments:** `Optional<List<EnrichmentSchema>>` 

Custom enrichment fields for data extraction.

If not provided, enrichments are generated automatically based on the query.
    
</dd>
</dl>

<dl>
<dd>

**mode:** `Optional<SubmitRequestDtoMode>` 

Job processing mode.

- `base`: Full pipeline with validation and enrichment.
- `lite`: Lightweight extraction with faster processing. Returns titles and citations only.
    
</dd>
</dl>

<dl>
<dd>

**connectedDatasetIds:** `Optional<List<String>>` 

Dataset IDs to connect to the job. When provided, this enables Company Watchlist mode — the job returns only events relevant to companies in the connected datasets. To set the minimum relevance threshold, use `ed_score_min`.

The dataset must have `latest_status: ready` before the job is submitted. Submitting with a non-existent or inaccessible dataset ID returns `400`.
    
</dd>
</dl>

<dl>
<dd>

**edScoreMin:** `Optional<Integer>` 

The minimum relevance score a connected entity must reach for its record to be included in results.

Only valid when `connected_dataset_ids` is set; otherwise ignored. Records where no connected entity meets the threshold are excluded entirely.
    
</dd>
</dl>

<dl>
<dd>

**projectId:** `Optional<String>` — Project to assign this job to. The job appears in the project's resource list immediately after submission.
    
</dd>
</dl>

<dl>
<dd>

**webhookIds:** `Optional<List<String>>` — IDs of webhooks to notify when the job completes. Maximum 5 per job.
    
</dd>
</dl>

<dl>
<dd>

**fetchAllWatchlistNews:** `Optional<Boolean>` 

When true, retrieves all news for connected Company Watchlist entities
without topic filtering. Requires connected_dataset_ids to be set.
    
</dd>
</dl>

<dl>
<dd>

**edAssociationType:** `Optional<EntityAssociationType>` 

Filter events by entity association type. `event_associated` keeps only
events where the entity is a direct actor. `mention` keeps only events
where the entity is merely referenced. Only relevant when
connected_dataset_ids is set.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.jobs.getJobStatus(jobId) -> StatusResponseDto</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Retrieve the current processing status of a job.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.jobs().getJobStatus(
    "5f0c9087-85cb-4917-b3c7-e5a5eff73a0c",
    GetJobStatusRequest
        .builder()
        .build()
);
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**jobId:** `String` — Unique job identifier returned from [`POST /catchAll/submit`](https://www.newscatcherapi.com/docs/web-search-api/api-reference/jobs/create-job).
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.jobs.getJobResults(jobId) -> PullJobResponseDto</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Retrieve the final results for a completed job.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.jobs().getJobResults(
    "5f0c9087-85cb-4917-b3c7-e5a5eff73a0c",
    GetJobResultsRequest
        .builder()
        .build()
);
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**jobId:** `String` — Unique job identifier returned from [`POST /catchAll/submit`](https://www.newscatcherapi.com/docs/web-search-api/api-reference/jobs/create-job).
    
</dd>
</dl>

<dl>
<dd>

**page:** `Optional<Integer>` — Page number to retrieve.
    
</dd>
</dl>

<dl>
<dd>

**pageSize:** `Optional<Integer>` — Number of records per page.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.jobs.getJobResultsCsv(jobId) -> String</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns a completed job's result records as a CSV download. One row per record, with enrichment fields as columns, citations as a JSON column, and connected entities split into `event_associated_entities` and `mention_entities` JSON columns.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.jobs().getJobResultsCsv(
    "job_id",
    GetJobResultsCsvRequest
        .builder()
        .build()
);
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**jobId:** `String` — Unique job identifier returned from [`POST /catchAll/submit`](https://www.newscatcherapi.com/docs/web-search-api/api-reference/jobs/create-job).
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.jobs.continueJob(request) -> ContinueResponseDto</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Continue an existing job to process more records beyond the initial limit.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.jobs().continueJob(
    ContinueRequestDto
        .builder()
        .jobId("5f0c9087-85cb-4917-b3c7-e5a5eff73a0c")
        .newLimit(100)
        .build()
);
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**jobId:** `String` — Job identifier of the completed job to continue.
    
</dd>
</dl>

<dl>
<dd>

**newLimit:** `Optional<Integer>` — New record limit for continued processing. Must be greater than the previous limit. If not provided, defaults to the plan maximum.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.jobs.deleteJob(jobId) -> DeleteJobResponseDto</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Soft-deletes a job. The job is flagged as deleted and no longer appears in list results. The underlying data is retained.

Only the job owner can delete a job. Returns `404` if the job is not found or does not belong to the authenticated user.

Deleting an already-deleted job returns `200`.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.jobs().deleteJob(
    "5f0c9087-85cb-4917-b3c7-e5a5eff73a0c",
    DeleteJobRequest
        .builder()
        .build()
);
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**jobId:** `String` — Unique job identifier returned from [`POST /catchAll/submit`](https://www.newscatcherapi.com/docs/web-search-api/api-reference/jobs/create-job).
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## Monitors
<details><summary><code>client.monitors.listMonitors() -> ListMonitorsResponseDto</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns all monitors created by the authenticated user.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.monitors().listMonitors(
    ListMonitorsRequest
        .builder()
        .projectId("60a85db4-78ec-4b78-876a-bc7d9cdadd04")
        .build()
);
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**page:** `Optional<Integer>` — Page number to retrieve.
    
</dd>
</dl>

<dl>
<dd>

**pageSize:** `Optional<Integer>` — Number of records per page.
    
</dd>
</dl>

<dl>
<dd>

**search:** `Optional<String>` — Filter results by text (case-insensitive substring match).
    
</dd>
</dl>

<dl>
<dd>

**ownership:** `Optional<OwnershipFilter>` 
    
</dd>
</dl>

<dl>
<dd>

**projectId:** `Optional<String>` — Filter results to resources belonging to this project.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.monitors.createMonitor(request) -> CreateMonitorResponseDto</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Create a scheduled monitor based on a reference job.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.monitors().createMonitor(
    CreateMonitorRequestDto
        .builder()
        .referenceJobId("5f0c9087-85cb-4917-b3c7-e5a5eff73a0c")
        .schedule("every day at 12 PM")
        .timezone("UTC")
        .webhookIds(
            Optional.of(
                Arrays.asList("a1b2c3d4-e5f6-7890-abcd-ef1234567890")
            )
        )
        .limit(10)
        .backfill(true)
        .build()
);
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**referenceJobId:** `String` 

Job ID to use as template for scheduled runs. Defines the query, validators, and enrichments used for each scheduled run.

If [`backfill`](https://www.newscatcherapi.com/docs/web-search-api/api-reference/monitors/create-monitor#body-backfill) is true, the job's `end_date` must be within the last 7 days.
    
</dd>
</dl>

<dl>
<dd>

**schedule:** `String` — Monitor schedule in plain text format. Minimum frequency depends on your plan.
    
</dd>
</dl>

<dl>
<dd>

**timezone:** `Optional<String>` 

The IANA timezone identifier used as the fallback when the `schedule` string does not include an explicit timezone.

If the schedule includes a timezone abbreviation (for example, `"every day at 9am EST"`), the parsed timezone takes priority and this value is ignored. 
    
</dd>
</dl>

<dl>
<dd>

**webhookIds:** `Optional<List<String>>` 

IDs of centralized webhooks to notify on each run completion.
Passing IDs here is equivalent to calling
`POST /catchAll/webhooks/{webhook_id}/resources` for each ID after creation.
Maximum 5 per monitor.
    
</dd>
</dl>

<dl>
<dd>

**limit:** `Optional<Integer>` — Maximum number of records per monitor run. If not provided, defaults to the plan limit.
    
</dd>
</dl>

<dl>
<dd>

**backfill:** `Optional<Boolean>` 

If true, fills the data gap between the reference job's `end_date` and the first scheduled run. The reference job's `end_date` must be within the last 7 days. 

If false, no gap filling occurs and the first run uses the current cron window only — the reference job's age does not matter.
    
</dd>
</dl>

<dl>
<dd>

**projectId:** `Optional<String>` — Project to assign this monitor to. The monitor appears in the project's resource list after creation.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.monitors.pullMonitorResults(monitorId) -> PullMonitorResponseDto</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Retrieve aggregated results from all jobs executed by a monitor.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.monitors().pullMonitorResults(
    "monitor_id",
    PullMonitorResultsRequest
        .builder()
        .build()
);
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**monitorId:** `String` — Monitor identifier.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.monitors.pullMonitorResultsCsv(monitorId) -> String</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns the most recent run's records as a CSV download. One row per record, with enrichment fields as columns, citations as a JSON column, and connected entities split into `event_associated_entities` and `mention_entities` JSON columns.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.monitors().pullMonitorResultsCsv(
    "monitor_id",
    PullMonitorResultsCsvRequest
        .builder()
        .build()
);
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**monitorId:** `String` — Monitor identifier.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.monitors.listMonitorJobs(monitorId) -> ListMonitorJobsResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Return all jobs executed by a monitor.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.monitors().listMonitorJobs(
    "monitor_id",
    ListMonitorJobsRequest
        .builder()
        .build()
);
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**monitorId:** `String` — Monitor identifier.
    
</dd>
</dl>

<dl>
<dd>

**sort:** `Optional<ListMonitorJobsRequestSort>` — Sort by start_date (asc or desc).
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.monitors.getMonitorStatusHistory(monitorId) -> MonitorStatusHistoryResponseDto</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns the full execution history of a monitor as a list of status entries, ordered from newest to oldest.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.monitors().getMonitorStatusHistory(
    "monitor_id",
    GetMonitorStatusHistoryRequest
        .builder()
        .build()
);
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**monitorId:** `String` — Monitor identifier.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.monitors.enableMonitor(monitorId, request) -> EnableMonitorResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Resume scheduled job execution for a monitor.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.monitors().enableMonitor(
    "monitor_id",
    EnableMonitorRequestDto
        .builder()
        .backfill(true)
        .build()
);
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**monitorId:** `String` — Monitor identifier.
    
</dd>
</dl>

<dl>
<dd>

**backfill:** `Optional<Boolean>` 

If true, fills the data gap between the last job's `end_date` and the first scheduled run after enabling. The last job's `end_date` must be within the last 7 days. 

If false, no gap filling occurs and the first run uses the current cron window only — the last job's age does not matter.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.monitors.disableMonitor(monitorId) -> DisableMonitorResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Stop scheduled job execution for a monitor.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.monitors().disableMonitor(
    "monitor_id",
    DisableMonitorRequest
        .builder()
        .build()
);
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**monitorId:** `String` — Monitor identifier.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.monitors.deleteMonitor(monitorId) -> DeleteMonitorResponseDto</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Soft-deletes a monitor. The monitor is flagged as deleted, stops
executing scheduled jobs immediately, and no longer appears in list
results.

Only the monitor owner can delete a monitor. Returns `404` if the
monitor is not found or does not belong to the authenticated user.

Deleting an already-deleted monitor returns `200`.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.monitors().deleteMonitor(
    "monitor_id",
    DeleteMonitorRequest
        .builder()
        .build()
);
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**monitorId:** `String` — Monitor identifier.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.monitors.updateMonitor(monitorId, request) -> UpdateMonitorResponseDto</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Update the webhook configuration for an existing monitor.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.monitors().updateMonitor(
    "monitor_id",
    UpdateMonitorRequestDto
        .builder()
        .webhookIds(
            Optional.of(
                Arrays.asList("a1b2c3d4-e5f6-7890-abcd-ef1234567890")
            )
        )
        .build()
);
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**monitorId:** `String` — Monitor identifier.
    
</dd>
</dl>

<dl>
<dd>

**webhookIds:** `Optional<List<String>>` 

Updated list of centralized webhook IDs for this monitor. 

Replaces all existing webhook assignments. Pass an empty array `[]` to clear all assignments. Omit to leave existing assignments unchanged.
    
</dd>
</dl>

<dl>
<dd>

**limit:** `Optional<Integer>` — Updated maximum number of records per monitor run.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## Webhooks
<details><summary><code>client.webhooks.listWebhooks() -> ListWebhooksResponseDto</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns a paginated list of webhooks belonging to the organization.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.webhooks().listWebhooks(
    ListWebhooksRequest
        .builder()
        .build()
);
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**page:** `Optional<Integer>` — Page number to retrieve.
    
</dd>
</dl>

<dl>
<dd>

**pageSize:** `Optional<Integer>` — Number of webhooks per page.
    
</dd>
</dl>

<dl>
<dd>

**search:** `Optional<String>` — Filter results by text (case-insensitive substring match).
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.webhooks.createWebhook(request) -> CreateWebhookResponseDto</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Creates a new webhook endpoint for the organization.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.webhooks().createWebhook(
    CreateWebhookRequestDto
        .builder()
        .name("Layoffs Alert")
        .url("https://hooks.slack.com/services/T000/B000/xxx")
        .type(WebhookType.SLACK)
        .deliveryMode(DeliveryMode.FULL)
        .build()
);
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**name:** `String` — Human-readable label for this webhook.
    
</dd>
</dl>

<dl>
<dd>

**url:** `String` 

Destination URL that receives the payload. Must use HTTPS. IP addresses are not accepted.

Type-specific URL requirements:
- `slack`: Must start with `https://hooks.slack.com/`.
- `teams`: Hostname must match `*.webhook.office.com` or `*.webhook.office365.com`.
- `generic`: Any valid HTTPS domain.
- `custom`: Any valid HTTPS domain.

When `type` is omitted, it is auto-detected from the URL.
    
</dd>
</dl>

<dl>
<dd>

**type:** `Optional<WebhookType>` 
    
</dd>
</dl>

<dl>
<dd>

**deliveryMode:** `Optional<DeliveryMode>` 
    
</dd>
</dl>

<dl>
<dd>

**method:** `Optional<HttpMethod>` 
    
</dd>
</dl>

<dl>
<dd>

**headers:** `Optional<Map<String, String>>` — Custom HTTP headers forwarded with each delivery.
    
</dd>
</dl>

<dl>
<dd>

**params:** `Optional<Map<String, String>>` — Query parameters appended to the webhook URL.
    
</dd>
</dl>

<dl>
<dd>

**auth:** `Optional<CreateWebhookRequestDtoAuth>` 

Authentication forwarded with each delivery. Supported types:
- `bearer`: Adds an `Authorization: Bearer <token>` header.
- `api_key`: Adds a custom header with the specified name and value.
- `basic`: Adds an `Authorization: Basic <credentials>` header.
    
</dd>
</dl>

<dl>
<dd>

**formatterConfig:** `Optional<Map<String, Object>>` — Custom payload transformation configuration. Required only when `type` is `custom`.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.webhooks.getWebhook(webhookId) -> GetWebhookResponseDto</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns the full configuration of a single webhook by ID.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.webhooks().getWebhook(
    "a1b2c3d4-e5f6-7890-abcd-ef1234567890",
    GetWebhookRequest
        .builder()
        .build()
);
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**webhookId:** `String` — Unique webhook identifier.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.webhooks.deleteWebhook(webhookId)</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Permanently deletes a webhook and removes all resource assignments. 

Assigned jobs and monitors no longer trigger delivery to this webhook. This operation cannot be undone.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.webhooks().deleteWebhook(
    "a1b2c3d4-e5f6-7890-abcd-ef1234567890",
    DeleteWebhookRequest
        .builder()
        .build()
);
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**webhookId:** `String` — Unique webhook identifier.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.webhooks.updateWebhook(webhookId, request) -> UpdateWebhookResponseDto</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Updates one or more fields of an existing webhook.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.webhooks().updateWebhook(
    "a1b2c3d4-e5f6-7890-abcd-ef1234567890",
    UpdateWebhookRequestDto
        .builder()
        .name("Layoffs Alert (EU)")
        .isActive(false)
        .build()
);
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**webhookId:** `String` — Unique webhook identifier.
    
</dd>
</dl>

<dl>
<dd>

**name:** `Optional<String>` — Updated webhook name.
    
</dd>
</dl>

<dl>
<dd>

**url:** `Optional<String>` — Updated destination URL. Must use HTTPS. Type-specific URL rules apply.
    
</dd>
</dl>

<dl>
<dd>

**type:** `Optional<WebhookType>` 
    
</dd>
</dl>

<dl>
<dd>

**deliveryMode:** `Optional<DeliveryMode>` 
    
</dd>
</dl>

<dl>
<dd>

**method:** `Optional<HttpMethod>` 
    
</dd>
</dl>

<dl>
<dd>

**headers:** `Optional<Map<String, String>>` — Updated HTTP headers. Replaces existing headers entirely.
    
</dd>
</dl>

<dl>
<dd>

**params:** `Optional<Map<String, String>>` — Updated query parameters. Replaces existing params entirely.
    
</dd>
</dl>

<dl>
<dd>

**auth:** `Optional<UpdateWebhookRequestDtoAuth>` — Updated authentication configuration. Replaces existing auth entirely.
    
</dd>
</dl>

<dl>
<dd>

**formatterConfig:** `Optional<Map<String, Object>>` — Updated formatter configuration.
    
</dd>
</dl>

<dl>
<dd>

**isActive:** `Optional<Boolean>` — Set to `false` to disable delivery without deleting the webhook.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.webhooks.testWebhook(webhookId, request) -> TestWebhookResponseDto</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Sends a test HTTP request to the webhook URL using the webhook's configured method, headers, and auth. Returns the response from the target endpoint.

Use this to verify URL reachability and authentication before attaching the webhook to a live job or monitor.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.webhooks().testWebhook(
    "a1b2c3d4-e5f6-7890-abcd-ef1234567890",
    TestWebhookRequestDto
        .builder()
        .payload(
            new HashMap<String, Object>() {{
                put("test", true);
                put("message", "CatchAll webhook test");
            }}
        )
        .build()
);
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**webhookId:** `String` — Unique webhook identifier.
    
</dd>
</dl>

<dl>
<dd>

**payload:** `Optional<Map<String, Object>>` 

Custom payload to send in the test request. If omitted, a synthetic
test payload is sent.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.webhooks.listWebhookResources(webhookId) -> ListWebhookResourcesResponseDto</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns a paginated list of resources currently assigned to this webhook.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.webhooks().listWebhookResources(
    "a1b2c3d4-e5f6-7890-abcd-ef1234567890",
    ListWebhookResourcesRequest
        .builder()
        .build()
);
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**webhookId:** `String` — Unique webhook identifier.
    
</dd>
</dl>

<dl>
<dd>

**resourceType:** `Optional<MappableResourceType>` 
    
</dd>
</dl>

<dl>
<dd>

**page:** `Optional<Integer>` — Page number to retrieve.
    
</dd>
</dl>

<dl>
<dd>

**pageSize:** `Optional<Integer>` — Number of assignments per page.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.webhooks.assignWebhookResource(webhookId, request) -> AssignWebhookResourceResponseDto</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Attaches a job, monitor, or monitor group to the webhook. When the
resource completes, the webhook receives a delivery.

A single webhook can be assigned to multiple resources. Each resource
can have up to 5 webhooks assigned.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.webhooks().assignWebhookResource(
    "a1b2c3d4-e5f6-7890-abcd-ef1234567890",
    AssignWebhookResourceRequestDto
        .builder()
        .resourceType(MappableResourceType.MONITOR)
        .resourceId("3fec5b07-8786-46d7-9486-d43ff67eccd4")
        .build()
);
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**webhookId:** `String` — Unique webhook identifier.
    
</dd>
</dl>

<dl>
<dd>

**resourceType:** `MappableResourceType` — Type of resource to assign.
    
</dd>
</dl>

<dl>
<dd>

**resourceId:** `String` — ID of the resource to assign.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.webhooks.removeWebhookResource(webhookId, resourceType, resourceId)</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Detaches a resource from this webhook. Completions of the resource no longer trigger delivery to this webhook.

The webhook and the resource itself are not deleted.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.webhooks().removeWebhookResource(
    "a1b2c3d4-e5f6-7890-abcd-ef1234567890",
    MappableResourceType.JOB,
    "3fec5b07-8786-46d7-9486-d43ff67eccd4",
    RemoveWebhookResourceRequest
        .builder()
        .build()
);
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**webhookId:** `String` — Unique webhook identifier.
    
</dd>
</dl>

<dl>
<dd>

**resourceType:** `MappableResourceType` 
    
</dd>
</dl>

<dl>
<dd>

**resourceId:** `String` — Unique resource identifier.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.webhooks.listWebhooksForResource(resourceType, resourceId) -> ListWebhooksResponseDto</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns all webhooks currently assigned to the given resource.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.webhooks().listWebhooksForResource(
    MappableResourceType.JOB,
    "3fec5b07-8786-46d7-9486-d43ff67eccd4",
    ListWebhooksForResourceRequest
        .builder()
        .build()
);
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**resourceType:** `MappableResourceType` 
    
</dd>
</dl>

<dl>
<dd>

**resourceId:** `String` — Unique resource identifier.
    
</dd>
</dl>

<dl>
<dd>

**isActive:** `Optional<Boolean>` — Filter by active status. Omit to return webhooks regardless of status.
    
</dd>
</dl>

<dl>
<dd>

**page:** `Optional<Integer>` — Page number to retrieve.
    
</dd>
</dl>

<dl>
<dd>

**pageSize:** `Optional<Integer>` — Number of webhooks per page.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.webhooks.getWebhookDeliveryHistory() -> DeliveryHistoryResponseDto</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns a paginated delivery log for a given resource, ordered by timestamp descending. 

Each record shows the webhook dispatched, the HTTP status code returned, delivery outcome, and any error or warning messages. Use this to debug failed deliveries or audit dispatch activity.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.webhooks().getWebhookDeliveryHistory(
    GetWebhookDeliveryHistoryRequest
        .builder()
        .resourceType(MappableResourceType.JOB)
        .resourceId("3fec5b07-8786-46d7-9486-d43ff67eccd4")
        .build()
);
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**resourceType:** `MappableResourceType` — Type of the resource to retrieve delivery history for.
    
</dd>
</dl>

<dl>
<dd>

**resourceId:** `String` — Identifier of the resource to retrieve delivery history for.
    
</dd>
</dl>

<dl>
<dd>

**page:** `Optional<Integer>` — Page number to retrieve.
    
</dd>
</dl>

<dl>
<dd>

**pageSize:** `Optional<Integer>` — Number of records per page.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## Entities
<details><summary><code>client.entities.listEntities() -> EntityListResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns a paginated list of entities belonging to the authenticated organization. Supports filtering by status and entity type, and sorting by name, status, or creation date.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.entities().listEntities(
    ListEntitiesRequest
        .builder()
        .search("NewsCatcher")
        .build()
);
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**page:** `Optional<Integer>` — Page number to retrieve.
    
</dd>
</dl>

<dl>
<dd>

**pageSize:** `Optional<Integer>` — Number of entities per page.
    
</dd>
</dl>

<dl>
<dd>

**search:** `Optional<String>` — Filter entities by name (case-insensitive substring match).
    
</dd>
</dl>

<dl>
<dd>

**status:** `Optional<EntityStatus>` 
    
</dd>
</dl>

<dl>
<dd>

**entityType:** `Optional<EntityType>` 
    
</dd>
</dl>

<dl>
<dd>

**sortBy:** `Optional<EntitySortBy>` 
    
</dd>
</dl>

<dl>
<dd>

**sortOrder:** `Optional<SortOrder>` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.entities.createEntity(request) -> CreateEntityResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Creates a new company entity and begins background enrichment.

The entity status starts as `pending` and transitions to `ready` once
enrichment completes. Provide as much identifying information as
possible — `domain` is the highest-signal field because it is
unambiguous.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.entities().createEntity(
    CreateEntityRequest
        .builder()
        .name("NewsCatcher")
        .entityType(EntityType.COMPANY)
        .description("AI-powered news data provider")
        .additionalAttributes(
            AdditionalAttributes
                .builder()
                .companyAttributes(
                    CompanyAttributes
                        .builder()
                        .domain("newscatcherapi.com")
                        .keyPersons(
                            Optional.of(
                                Arrays.asList("Artem Bugara", "Maksym Sugonyaka")
                            )
                        )
                        .alternativeNames(
                            Optional.of(
                                Arrays.asList("NC", "NewsCatcher API")
                            )
                        )
                        .build()
                )
                .build()
        )
        .build()
);
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `CreateEntityRequest` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.entities.createEntitiesBatch(request) -> CreateEntitiesBatchResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Creates multiple entities in a single request. Each entity is processed independently — a failure in one does not affect others.

Returns an array of `{id, status}` objects in the same order as the input array.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.entities().createEntitiesBatch(
    CreateEntitiesBatchRequest
        .builder()
        .entities(
            Arrays.asList(
                CreateEntityRequest
                    .builder()
                    .name("OpenAI")
                    .entityType(EntityType.COMPANY)
                    .description("Artificial intelligence research company")
                    .additionalAttributes(
                        AdditionalAttributes
                            .builder()
                            .companyAttributes(
                                CompanyAttributes
                                    .builder()
                                    .domain("openai.com")
                                    .keyPersons(
                                        Optional.of(
                                            Arrays.asList("Sam Altman")
                                        )
                                    )
                                    .alternativeNames(
                                        Optional.of(
                                            Arrays.asList("Open AI")
                                        )
                                    )
                                    .build()
                            )
                            .build()
                    )
                    .build(),
                CreateEntityRequest
                    .builder()
                    .name("Stripe")
                    .entityType(EntityType.COMPANY)
                    .description("Online payment processing platform")
                    .additionalAttributes(
                        AdditionalAttributes
                            .builder()
                            .companyAttributes(
                                CompanyAttributes
                                    .builder()
                                    .domain("stripe.com")
                                    .keyPersons(
                                        Optional.of(
                                            Arrays.asList("Patrick Collison", "John Collison")
                                        )
                                    )
                                    .build()
                            )
                            .build()
                    )
                    .build()
            )
        )
        .build()
);
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**entities:** `List<CreateEntityRequest>` 

Array of entities to create. Each item follows the same schema
as single entity creation.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.entities.getEntity(entityId) -> EntityResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns a single entity by ID with all attributes and current status.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.entities().getEntity(
    "854198fa-f702-49db-a381-0427fa87f173",
    GetEntityRequest
        .builder()
        .build()
);
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**entityId:** `String` — Unique entity identifier.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.entities.deleteEntity(entityId)</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Permanently deletes an entity. The entity is removed from all
datasets and the search index. This operation cannot be undone.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.entities().deleteEntity(
    "854198fa-f702-49db-a381-0427fa87f173",
    DeleteEntityRequest
        .builder()
        .build()
);
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**entityId:** `String` — Unique entity identifier.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.entities.updateEntity(entityId, request) -> EntityResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Updates one or more fields of an existing entity.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.entities().updateEntity(
    "854198fa-f702-49db-a381-0427fa87f173",
    UpdateEntityRequest
        .builder()
        .description("Updated description")
        .additionalAttributes(
            AdditionalAttributes
                .builder()
                .companyAttributes(
                    CompanyAttributes
                        .builder()
                        .alternativeNames(
                            Optional.of(
                                Arrays.asList("NC", "NewsCatcher API", "NCA")
                            )
                        )
                        .build()
                )
                .build()
        )
        .build()
);
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**entityId:** `String` — Unique entity identifier.
    
</dd>
</dl>

<dl>
<dd>

**name:** `Optional<String>` — Updated entity name.
    
</dd>
</dl>

<dl>
<dd>

**description:** `Optional<String>` — Updated description.
    
</dd>
</dl>

<dl>
<dd>

**externalEntityId:** `Optional<String>` — Updated external identifier for this entity.
    
</dd>
</dl>

<dl>
<dd>

**additionalAttributes:** `Optional<AdditionalAttributes>` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## Datasets
<details><summary><code>client.datasets.listDatasets() -> DatasetListResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns a paginated list of datasets belonging to the authenticated organization. Supports filtering by status and sorting by name, status, or creation date.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.datasets().listDatasets(
    ListDatasetsRequest
        .builder()
        .search("Portfolio")
        .projectId("60a85db4-78ec-4b78-876a-bc7d9cdadd04")
        .build()
);
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**page:** `Optional<Integer>` — Page number to retrieve.
    
</dd>
</dl>

<dl>
<dd>

**pageSize:** `Optional<Integer>` — Number of datasets per page.
    
</dd>
</dl>

<dl>
<dd>

**search:** `Optional<String>` — Filter datasets by name (case-insensitive substring match).
    
</dd>
</dl>

<dl>
<dd>

**latestStatus:** `Optional<DatasetStatus>` — Filter by dataset status.
    
</dd>
</dl>

<dl>
<dd>

**sortBy:** `Optional<DatasetSortBy>` 
    
</dd>
</dl>

<dl>
<dd>

**sortOrder:** `Optional<SortOrder>` 
    
</dd>
</dl>

<dl>
<dd>

**ownership:** `Optional<OwnershipFilter>` 
    
</dd>
</dl>

<dl>
<dd>

**projectId:** `Optional<String>` — Filter results to resources belonging to this project.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.datasets.createDataset(request) -> DatasetResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Creates a new dataset from a list of existing entity IDs.

If any of the provided entity IDs do not exist or do not belong to
your organization, the request fails with `400`. All entity IDs must
be valid before the dataset is created.

To create a dataset and entities in one step, use the [`Create dataset from CSV`](https://www.newscatcherapi.com/docs/web-search-api/api-reference/datasets/create-dataset-from-csv)
endpoint instead.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.datasets().createDataset(
    CreateDatasetRequest
        .builder()
        .name("My Portfolio")
        .description("Companies in our investment portfolio")
        .entityIds(
            Optional.of(
                Arrays.asList("854198fa-f702-49db-a381-0427fa87f173", "a1b2c3d4-e5f6-7890-abcd-ef1234567890")
            )
        )
        .build()
);
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**name:** `String` — Name for the dataset.
    
</dd>
</dl>

<dl>
<dd>

**description:** `Optional<String>` — Optional description.
    
</dd>
</dl>

<dl>
<dd>

**entityIds:** `Optional<List<String>>` — IDs of existing entities to include in the dataset. All IDs must belong to the authenticated organization. If any ID is invalid or not found, the request fails with `400`.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.datasets.createDatasetFromCsv(request) -> CreateDatasetCsvResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Creates a new dataset by uploading a CSV file. Each row in the CSV becomes an entity. The `name` and `domain`columns are required; all other columns are optional.

**CSV format:**
```csv
name,description,domain,alternative_names,key_persons
NewsCatcher,"AI-powered news data provider",newscatcherapi.com,"NC;NewsCatcher API","Artem Bugara;Maksym Sugonyaka"
OpenAI,"Artificial intelligence research company",openai.com,"Open AI","Sam Altman"
```

Use semicolons (`;`) to separate multiple values in `alternative_names` and `key_persons`. Rows with empty `name` are skipped and reported in `validation_report`. 

**Note**: The response shape differs from the JSON dataset creation endpoint: it returns `dataset_id` (not `id`) and includes a `validation_report` with details on skipped rows.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.datasets().createDatasetFromCsv(
    null,
    CreateDatasetFromCsvRequest
        .builder()
        .name("name")
        .build()
);
```
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.datasets.getDataset(datasetId) -> DatasetResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns a single dataset by ID including entity count and current status.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.datasets().getDataset(
    "ccabb755-afc2-4047-b84c-78d1f23d49b2",
    GetDatasetRequest
        .builder()
        .build()
);
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**datasetId:** `String` — Unique dataset identifier.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.datasets.deleteDataset(datasetId)</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Permanently deletes a dataset. The entities within the dataset are
not deleted — only the dataset itself. This operation cannot be
undone.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.datasets().deleteDataset(
    "ccabb755-afc2-4047-b84c-78d1f23d49b2",
    DeleteDatasetRequest
        .builder()
        .build()
);
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**datasetId:** `String` — Unique dataset identifier.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.datasets.updateDataset(datasetId, request) -> DatasetResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Updates the name or description of a dataset.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.datasets().updateDataset(
    "ccabb755-afc2-4047-b84c-78d1f23d49b2",
    UpdateDatasetRequest
        .builder()
        .name("My Portfolio (updated)")
        .description("Updated Q1 2026 watchlist")
        .build()
);
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**datasetId:** `String` — Unique dataset identifier.
    
</dd>
</dl>

<dl>
<dd>

**name:** `Optional<String>` — Updated dataset name.
    
</dd>
</dl>

<dl>
<dd>

**description:** `Optional<String>` — Updated description.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.datasets.addEntitiesToDataset(datasetId, request) -> ManageEntitiesResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Adds one or more existing entities to a dataset. Returns the number of entities added.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.datasets().addEntitiesToDataset(
    "ccabb755-afc2-4047-b84c-78d1f23d49b2",
    AddEntitiesToDatasetRequest
        .builder()
        .body(
            DatasetEntityIdsRequest
                .builder()
                .entityIds(
                    Arrays.asList("854198fa-f702-49db-a381-0427fa87f173")
                )
                .build()
        )
        .build()
);
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**datasetId:** `String` — Unique dataset identifier.
    
</dd>
</dl>

<dl>
<dd>

**request:** `DatasetEntityIdsRequest` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.datasets.removeEntitiesFromDataset(datasetId, request) -> ManageEntitiesResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Removes one or more entities from a dataset. The entities themselves are not deleted — they are only removed from this dataset. Returns the number of entities removed.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.datasets().removeEntitiesFromDataset(
    "ccabb755-afc2-4047-b84c-78d1f23d49b2",
    RemoveEntitiesFromDatasetRequest
        .builder()
        .body(
            DatasetEntityIdsRequest
                .builder()
                .entityIds(
                    Arrays.asList("854198fa-f702-49db-a381-0427fa87f173")
                )
                .build()
        )
        .build()
);
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**datasetId:** `String` — Unique dataset identifier.
    
</dd>
</dl>

<dl>
<dd>

**request:** `DatasetEntityIdsRequest` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.datasets.listEntitiesInDataset(datasetId, request) -> DatasetEntityListResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns a paginated list of entities in a dataset. Supports filtering by status, entity type, and name search.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.datasets().listEntitiesInDataset(
    "ccabb755-afc2-4047-b84c-78d1f23d49b2",
    ListDatasetEntitiesRequest
        .builder()
        .page(1)
        .pageSize(100)
        .search("OpenAI")
        .status(EntityStatus.READY)
        .entityType(EntityType.COMPANY)
        .sortBy(EntitySortBy.CREATED_AT)
        .sortOrder(SortOrder.DESC)
        .build()
);
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**datasetId:** `String` — Unique dataset identifier.
    
</dd>
</dl>

<dl>
<dd>

**page:** `Optional<Integer>` — The page number to retrieve.
    
</dd>
</dl>

<dl>
<dd>

**pageSize:** `Optional<Integer>` — The number of entities per page.
    
</dd>
</dl>

<dl>
<dd>

**search:** `Optional<String>` — Filters entities by name using a case-insensitive substring match.
    
</dd>
</dl>

<dl>
<dd>

**status:** `Optional<EntityStatus>` 
    
</dd>
</dl>

<dl>
<dd>

**entityType:** `Optional<EntityType>` 
    
</dd>
</dl>

<dl>
<dd>

**sortBy:** `Optional<EntitySortBy>` 
    
</dd>
</dl>

<dl>
<dd>

**sortOrder:** `Optional<SortOrder>` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.datasets.getDatasetStatusHistory(datasetId) -> DatasetStatusHistoryResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns the full status change history for a dataset, ordered chronologically from oldest to newest.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.datasets().getDatasetStatusHistory(
    "ccabb755-afc2-4047-b84c-78d1f23d49b2",
    GetDatasetStatusHistoryRequest
        .builder()
        .build()
);
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**datasetId:** `String` — Unique dataset identifier.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.datasets.uploadCsvToDataset(datasetId, request) -> UploadCsvToDatasetResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Appends new companies to an existing dataset by uploading a CSV file. Uses the same CSV format as the dataset creation endpoint.

The response omits `dataset_name` compared to the create-from-CSV endpoint since the dataset already exists.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.datasets().uploadCsvToDataset(
    "ccabb755-afc2-4047-b84c-78d1f23d49b2",
    null,
    UploadCsvToDatasetRequest
        .builder()
        .build()
);
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**datasetId:** `String` — Unique dataset identifier.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## Projects
<details><summary><code>client.projects.listProjects() -> ProjectListResponseDto</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns all projects visible to the authenticated user.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.projects().listProjects(
    ListProjectsRequest
        .builder()
        .search("M&A")
        .build()
);
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**page:** `Optional<Integer>` — Page number to retrieve.
    
</dd>
</dl>

<dl>
<dd>

**pageSize:** `Optional<Integer>` — Number of records per page.
    
</dd>
</dl>

<dl>
<dd>

**search:** `Optional<String>` — Filter by project name (case-insensitive substring match).
    
</dd>
</dl>

<dl>
<dd>

**ownership:** `Optional<OwnershipFilter>` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.projects.createProject(request) -> CreateProjectResponseDto</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Creates a new project.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.projects().createProject(
    CreateProjectRequestDto
        .builder()
        .name("AI M&A Tracking")
        .description("Tracks AI-related M&A activity for our investment team.")
        .build()
);
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**name:** `String` — Name for the project.
    
</dd>
</dl>

<dl>
<dd>

**description:** `Optional<String>` — Optional description.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.projects.getProject(projectId) -> ProjectResponseDto</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns a single project by ID.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.projects().getProject(
    "60a85db4-78ec-4b78-876a-bc7d9cdadd04",
    GetProjectRequest
        .builder()
        .build()
);
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**projectId:** `String` — Unique project identifier.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.projects.deleteProject(projectId)</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Deletes a project. By default, assigned resources are unassigned but not deleted. 
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.projects().deleteProject(
    "60a85db4-78ec-4b78-876a-bc7d9cdadd04",
    DeleteProjectRequest
        .builder()
        .build()
);
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**projectId:** `String` — Unique project identifier.
    
</dd>
</dl>

<dl>
<dd>

**deleteResources:** `Optional<Boolean>` — If true, permanently deletes all resources (jobs, monitors, datasets) assigned to the project. If false, the project is deleted and its resources are unassigned but not deleted.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.projects.updateProject(projectId, request) -> UpdateProjectResponseDto</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Updates the name or description of an existing project.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.projects().updateProject(
    "60a85db4-78ec-4b78-876a-bc7d9cdadd04",
    UpdateProjectRequestDto
        .builder()
        .name("AI M&A Tracking (Q2 2026)")
        .build()
);
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**projectId:** `String` — Unique project identifier.
    
</dd>
</dl>

<dl>
<dd>

**name:** `Optional<String>` — New name for the project.
    
</dd>
</dl>

<dl>
<dd>

**description:** `Optional<String>` — New description for the project.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.projects.getProjectOverview(projectId) -> ProjectOverviewResponseDto</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns resource counts for a project, grouped by type and status.

For `jobs` and `monitors`, counts are broken down by status (for example, `completed`, `failed`). For `datasets` and `monitor_groups`, only a `total` count is returned.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.projects().getProjectOverview(
    "60a85db4-78ec-4b78-876a-bc7d9cdadd04",
    GetProjectOverviewRequest
        .builder()
        .build()
);
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**projectId:** `String` — Unique project identifier.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.projects.listProjectResources(projectId) -> ProjectResourceListResponseDto</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns all resources assigned to a project, with optional filtering by type.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.projects().listProjectResources(
    "60a85db4-78ec-4b78-876a-bc7d9cdadd04",
    ListProjectResourcesRequest
        .builder()
        .build()
);
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**projectId:** `String` — Unique project identifier.
    
</dd>
</dl>

<dl>
<dd>

**resourceType:** `Optional<ProjectResourceType>` 
    
</dd>
</dl>

<dl>
<dd>

**page:** `Optional<Integer>` — Page number to retrieve.
    
</dd>
</dl>

<dl>
<dd>

**pageSize:** `Optional<Integer>` — Number of records per page.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.projects.addResourceToProject(projectId, request) -> AddResourceResponseDto</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Assigns one or more existing resources to a project in a single request.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.projects().addResourceToProject(
    "60a85db4-78ec-4b78-876a-bc7d9cdadd04",
    AddResourceRequestDto
        .builder()
        .resources(
            Arrays.asList(
                ResourceItemDto
                    .builder()
                    .resourceType(ProjectResourceType.JOB)
                    .resourceId("48421e16-1f50-4048-b62c-d3bc0789d30d")
                    .build()
            )
        )
        .build()
);
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**projectId:** `String` — Unique project identifier.
    
</dd>
</dl>

<dl>
<dd>

**resources:** `List<ResourceItemDto>` — Resources to assign to the project.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.projects.removeResourceFromProject(projectId, resourceType, resourceId) -> RemoveResourceResponseDto</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Removes a resource from a project. The resource itself is not
deleted — it becomes unassigned and continues to exist.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.projects().removeResourceFromProject(
    "60a85db4-78ec-4b78-876a-bc7d9cdadd04",
    ProjectResourceType.JOB,
    "48421e16-1f50-4048-b62c-d3bc0789d30d",
    RemoveResourceFromProjectRequest
        .builder()
        .build()
);
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**projectId:** `String` — Unique project identifier.
    
</dd>
</dl>

<dl>
<dd>

**resourceType:** `ProjectResourceType` 
    
</dd>
</dl>

<dl>
<dd>

**resourceId:** `String` — ID of the resource to remove.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## Meta
<details><summary><code>client.meta.healthCheck() -> HealthCheckResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Check API availability.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.meta().healthCheck();
```
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.meta.getVersion() -> GetVersionResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns current API version.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.meta().getVersion();
```
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.meta.getPlanLimits() -> GetPlanLimitsResponseDto</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns plan features and current usage for the authenticated organization.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```java
client.meta().getPlanLimits();
```
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

