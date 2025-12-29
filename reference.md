# Reference
## Jobs
<details><summary><code>client.jobs.createJob(request) -> SubmitResponseBody</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Submit a natural language query to create a new processing job.
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
        .query("AI company acquisitions")
        .context("Focus on deal size and acquiring company details")
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

**schema:** `Optional<String>` 
    
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
    "af7a26d6-cf0b-458c-a6ed-4b6318c74da3",
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

**jobId:** `String` — Unique job identifier returned from the `/catchAll/submit` endpoint.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.jobs.getUserJobs() -> List&lt;ListUserJobsResponseDto&gt;</code></summary>
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
client.jobs().getUserJobs();
```
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
    "af7a26d6-cf0b-458c-a6ed-4b6318c74da3",
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

**jobId:** `String` — Unique job identifier returned from the `/catchAll/submit` endpoint.
    
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

## Monitors
<details><summary><code>client.monitors.createMonitor(request) -> CreateMonitorResponseDto</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Create a monitor that runs jobs based on a reference job with a specified schedule.

**Schedule requirements:**
- Minimum 24-hour interval between executions
- Natural language format (e.g., "every day at 12 PM UTC", "every 48 hours")

**Validation:**
- Schedules below minimum frequency return error with descriptive message.
- Invalid job IDs return 400 Bad Request.
- Duplicate monitors (same job already monitored) return error.
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
        .referenceJobId("reference_job_id")
        .schedule("every day at 12 PM UTC")
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

**referenceJobId:** `String` — Job ID to use as template for scheduled runs.
    
</dd>
</dl>

<dl>
<dd>

**schedule:** `String` 

Natural language schedule (e.g. 'every day at 12 AM EST').

**Minimum frequency:** Monitors must be scheduled at least 24 hours apart.
    
</dd>
</dl>

<dl>
<dd>

**webhook:** `Optional<WebhookDto>` — Optional webhook to receive notifications when jobs complete.
    
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

Returns all jobs associated with a monitor, sorted by start_date.
Each job includes job_id, start_date, and end_date.
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

<details><summary><code>client.monitors.pullMonitorResults(monitorId) -> PullMonitorResponseDto</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Retrieve aggregated results from all jobs executed by this monitor.
Includes monitor configuration, execution history, and all records collected.
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

<details><summary><code>client.monitors.disableMonitor(monitorId) -> DisableMonitorResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Disables a monitor to stop executing scheduled jobs.
Validates that the provided API key is associated with the monitor.
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

<details><summary><code>client.monitors.enableMonitor(monitorId) -> EnableMonitorResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Enables a monitor to resume executing scheduled jobs.
Validates that the provided API key is associated with the monitor.
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
    EnableMonitorRequest
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
client.monitors().listMonitors();
```
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
