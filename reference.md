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

Dataset IDs to connect to this job. When provided, activates Company Search mode — the job returns only events relevant to companies in the connected datasets with each record including a `connected_entities` array scored per company.

The dataset must have `latest_status: ready` before the job is submitted. Submitting with a non-existent or inaccessible dataset ID returns `400`.
    
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
        .schedule("every day at 12 PM UTC")
        .webhook(
            WebhookDto
                .builder()
                .url("https://your-endpoint.com/webhook")
                .method(WebhookDtoMethod.POST)
                .headers(
                    new HashMap<String, String>() {{
                        put("Authorization", "Bearer your_token_here");
                    }}
                )
                .build()
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

**schedule:** `String` 

Monitor schedule in plain text format (e.g. 'every day at 12 PM UTC', 'every 48 hours').

Minimum frequency depends on your plan.
    
</dd>
</dl>

<dl>
<dd>

**webhook:** `Optional<WebhookDto>` — Optional webhook to receive notifications when jobs complete.
    
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
        .webhook(
            WebhookDto
                .builder()
                .url("https://new-endpoint.com/webhook")
                .method(WebhookDtoMethod.POST)
                .headers(
                    new HashMap<String, String>() {{
                        put("Authorization", "Bearer new_token_xyz");
                    }}
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

**monitorId:** `String` — Monitor identifier.
    
</dd>
</dl>

<dl>
<dd>

**webhook:** `Optional<WebhookDto>` — Updated webhook configuration.
    
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

## Entities
<details><summary><code>client.entities.listEntities() -> EntityListResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns a paginated list of entities belonging to the authenticated
organization. Supports filtering by status and entity type, and
sorting by name, status, or creation date.
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

Creates multiple entities in a single request. Each entity is
processed independently — a failure in one does not affect others.

Returns an array of `{id, status}` objects in the same order as
the input array.
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

Returns a paginated list of datasets belonging to the authenticated
organization. Supports filtering by status and sorting by name,
status, or creation date.
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

Creates a new dataset by uploading a CSV file. Each row in the CSV
becomes an entity. The `name` column is required; all other columns
are optional.

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

<details><summary><code>client.datasets.listEntitiesInDataset(datasetId) -> DatasetEntityListResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns a paginated list of entities in a dataset. Supports filtering by status and entity type.
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
    ListEntitiesInDatasetRequest
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

**search:** `Optional<String>` — Filter entities by name.
    
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

Removes one or more entities from a dataset. The entities themselves
are not deleted — they are only removed from this dataset. Returns
the number of entities removed.
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

<details><summary><code>client.datasets.getDatasetStatusHistory(datasetId) -> DatasetStatusHistoryResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns the full status change history for a dataset, ordered
chronologically from oldest to newest.
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

Appends new companies to an existing dataset by uploading a CSV file.
Uses the same CSV format as the dataset creation endpoint.

The response omits `dataset_name` compared to the create-from-CSV
endpoint since the dataset already exists.
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

