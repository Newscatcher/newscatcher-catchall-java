# Newscatcher CatchAll Java Library

[![fern shield](https://img.shields.io/badge/%F0%9F%8C%BF-Built%20with%20Fern-brightgreen)](https://buildwithfern.com?utm_source=github&utm_medium=github&utm_campaign=readme&utm_source=https%3A%2F%2Fgithub.com%2FNewscatcher%2Fnewscatcher-catchall-java)
[![Maven Central](https://img.shields.io/maven-central/v/com.newscatcherapi/newscatcher-catchall-sdk)](https://central.sonatype.com/artifact/com.newscatcherapi/newscatcher-catchall-sdk)

The Newscatcher CatchAll Java library provides access to the [CatchAll API](https://www.newscatcherapi.com/docs/v3/catch-all/overview/introduction), which transforms natural language queries into structured data extracted from web sources.

## Installation

### Gradle

Add the dependency in your `build.gradle` file:

```groovy
dependencies {
    implementation 'com.newscatcherapi:newscatcher-catchall-sdk:0.3.1'
}
```

### Maven

Add the dependency in your `pom.xml` file:

```xml
<dependency>
    <groupId>com.newscatcherapi</groupId>
    <artifactId>newscatcher-catchall-sdk</artifactId>
    <version>0.3.1</version>
</dependency>
```

## Reference

A full reference for this library is available [here](./reference.md).

## Usage

### Jobs

Submit a query and retrieve structured results:

```java
import com.newscatcher.catchall.CatchAllApi;
import com.newscatcher.catchall.resources.jobs.requests.SubmitRequestDto;
import com.newscatcher.catchall.resources.jobs.requests.ContinueRequestDto;
import com.newscatcher.catchall.types.StatusResponseDto;
import com.newscatcher.catchall.types.PullJobResponseDto;
import com.newscatcher.catchall.types.SubmitResponseBody;
import com.newscatcher.catchall.types.ContinueResponseDto;
import com.newscatcher.catchall.types.JobStatus;

public class JobsExample {
    public static void main(String[] args) throws InterruptedException {
        CatchAllApi client = CatchAllApi.builder()
            .apiKey("YOUR_API_KEY")
            .build();

        // Create a job with optional limit for testing
        SubmitResponseBody job = client.jobs().createJob(
            SubmitRequestDto.builder()
                .query("Tech company earnings this quarter")
                .context("Focus on revenue and profit margins")
                .limit(10) // Start with 10 records for quick testing
                .build()
        );
        System.out.println("Job created: " + job.getJobId());

        // Poll for completion with progress updates
        String jobId = job.getJobId();
        while (true) {
            StatusResponseDto status = client.jobs().getJobStatus(jobId);
            
            // Check if completed or enriching (early access)
            if (JobStatus.COMPLETED.equals(status.getStatus().orElse(null)) || 
                JobStatus.ENRICHING.equals(status.getStatus().orElse(null))) {
                System.out.println("Job " + status.getStatus().orElse(null) + "!");
                break;
            }

            // Show current processing step
            status.getSteps().stream()
                .filter(step -> !step.getCompleted().orElse(false))
                .findFirst()
                .ifPresent(step -> System.out.println(String.format(
                    "Processing: %s (step %d/7)",
                    step.getStatus(),
                    step.getOrder()
                )));

            Thread.sleep(60000); // Wait 60 seconds
        }

        // Retrieve initial results (available during enriching stage)
        PullJobResponseDto results = client.jobs().getJobResults(jobId);
        System.out.println("Found " + results.getValidRecords().orElse(0) + " valid records");
        System.out.println(String.format(
            "Progress: %d/%d validated",
            results.getProgressValidated().orElse(0),
            results.getCandidateRecords().orElse(0)
        ));

        // Continue job to process more records
        if (results.getValidRecords().orElse(0) >= 10) {
            ContinueResponseDto continued = client.jobs().continueJob(
                ContinueRequestDto.builder()
                    .jobId(jobId)
                    .newLimit(50) // Increase to 50 records
                    .build()
            );
            System.out.println("Job continued: " + continued.getJobId());

            // Wait for completion
            while (true) {
                StatusResponseDto status = client.jobs().getJobStatus(jobId);
                if (JobStatus.COMPLETED.equals(status.getStatus().orElse(null))) {
                    break;
                }
                Thread.sleep(60000);
            }

            // Get final results
            PullJobResponseDto finalResults = client.jobs().getJobResults(jobId);
            System.out.println("Final: " + finalResults.getValidRecords().orElse(0) + " records");
        }

        results.getAllRecords().ifPresent(records ->
            records.forEach(record ->
                System.out.println(record.getRecordTitle())
            )
        );
    }
}
```

Jobs process asynchronously and typically complete in 10-15 minutes. To learn more, see the [Quickstart](https://www.newscatcherapi.com/docs/v3/catch-all/overview/quickstart).

### Monitors

Automate recurring queries with scheduled execution:

```java
import com.newscatcher.catchall.CatchAllApi;
import com.newscatcher.catchall.resources.monitors.requests.CreateMonitorRequestDto;
import com.newscatcher.catchall.resources.monitors.requests.UpdateMonitorRequestDto;
import com.newscatcher.catchall.resources.monitors.requests.ListMonitorJobsRequest;
import com.newscatcher.catchall.types.WebhookDto;
import com.newscatcher.catchall.types.WebhookDtoMethod;
import com.newscatcher.catchall.types.CreateMonitorResponseDto;
import com.newscatcher.catchall.types.UpdateMonitorResponseDto;
import com.newscatcher.catchall.types.ListMonitorJobsResponse;
import com.newscatcher.catchall.types.PullMonitorResponseDto;
import com.newscatcher.catchall.types.DisableMonitorResponse;
import com.newscatcher.catchall.types.EnableMonitorResponse;
import java.util.Map;

public class MonitorsExample {
    public static void main(String[] args) {
        CatchAllApi client = CatchAllApi.builder()
            .apiKey("YOUR_API_KEY")
            .build();

        // Create a monitor from a completed job
        CreateMonitorResponseDto monitor = client.monitors().createMonitor(
            CreateMonitorRequestDto.builder()
                .referenceJobId(jobId) // Use job ID from previous example
                .schedule("every day at 12 PM UTC")
                .webhook(WebhookDto.builder()
                    .url("https://your-endpoint.com/webhook")
                    .method(WebhookDtoMethod.POST)
                    .headers(Map.of("Authorization", "Bearer YOUR_TOKEN"))
                    .build())
                .build()
        );
        System.out.println("Monitor created: " + monitor.getMonitorId().orElse("N/A"));

        String monitorId = monitor.getMonitorId().orElseThrow();

        // Update webhook configuration without recreating monitor
        UpdateMonitorResponseDto updated = client.monitors().updateMonitor(
            monitorId,
            UpdateMonitorRequestDto.builder()
                .webhook(WebhookDto.builder()
                    .url("https://new-endpoint.com/webhook")
                    .method(WebhookDtoMethod.POST)
                    .headers(Map.of("Authorization", "Bearer NEW_TOKEN"))
                    .build())
                .build()
        );

        // Pause monitor execution
        DisableMonitorResponse disabled = client.monitors().disableMonitor(monitorId);
        System.out.println("Monitor paused");

        // Resume monitor execution
        EnableMonitorResponse enabled = client.monitors().enableMonitor(monitorId);
        System.out.println("Monitor resumed");

        // List monitor execution history
        ListMonitorJobsResponse jobs = client.monitors().listMonitorJobs(
            monitorId,
            ListMonitorJobsRequest.builder()
                .sort("desc") // Most recent first
                .build()
        );
        System.out.println("Monitor has executed " + jobs.getTotalJobs() + " jobs");
        jobs.getJobs().forEach(job ->
            System.out.println(String.format(
                "  Job %s: %s to %s",
                job.getJobId(),
                job.getStartDate(),
                job.getEndDate()
            ))
        );

        // Get aggregated results
        PullMonitorResponseDto results = client.monitors().pullMonitorResults(monitorId);
        System.out.println("Collected " + results.getRecords().orElse(0) + " records across all executions");
    }
}
```

Monitors run jobs on your schedule and send webhook notifications when complete. See the [Monitors documentation](https://www.newscatcherapi.com/docs/v3/catch-all/overview/monitors) for setup and configuration.

## Exception handling

Handle API errors with the `CatchAllApiApiException`:

```java
import com.newscatcher.catchall.core.CatchAllApiApiException;

try {
    client.jobs().createJob(
        SubmitRequestDto.builder()
            .query("Tech company earnings this quarter")
            .build()
    );
} catch (CatchAllApiApiException e) {
    System.out.println("Status: " + e.statusCode());
    System.out.println("Error: " + e.body());
}
```

## Advanced

### Pagination

Retrieve large result sets with pagination:

```java
import com.newscatcher.catchall.resources.jobs.requests.GetJobResultsRequest;

// Retrieve large result sets with pagination
int page = 1;
while (true) {
    PullJobResponseDto results = client.jobs().getJobResults(
        jobId,
        GetJobResultsRequest.builder()
            .page(page)
            .pageSize(100)
            .build()
    );

    System.out.println(String.format(
        "Page %d/%d: %d records",
        results.getPage().orElse(0),
        results.getTotalPages().orElse(0),
        results.getAllRecords().map(List::size).orElse(0)
    ));

    results.getAllRecords().ifPresent(records ->
        records.forEach(record -> {
            // Process each record
            System.out.println("  - " + record.getRecordTitle());
        })
    );

    if (results.getPage().orElse(0) >= results.getTotalPages().orElse(1)) {
        break;
    }
    page++;
}

System.out.println("Processed " + results.getValidRecords().orElse(0) + " total records");
```

### Access raw response data

Access response headers and raw data using the `withRawResponse()` method:

```java
import com.newscatcher.catchall.core.ApiResponse;

ApiResponse<SubmitResponseBody> response = client.jobs()
    .withRawResponse()
    .createJob(
        SubmitRequestDto.builder()
            .query("Tech company earnings this quarter")
            .build()
    );

System.out.println(response.body());
System.out.println(response.headers().get("X-Custom-Header"));
```

### Timeouts

Set custom timeouts at the client or request level:

```java
import com.newscatcher.catchall.core.RequestOptions;

// Client-level timeout
CatchAllApi client = CatchAllApi.builder()
    .apiKey("YOUR_API_KEY")
    .timeout(30) // 30 seconds
    .build();

// Request-level timeout
client.jobs().createJob(
    SubmitRequestDto.builder()
        .query("Tech company earnings this quarter")
        .build(),
    RequestOptions.builder()
        .timeout(10) // 10 seconds
        .build()
);
```

### Retries

The SDK retries failed requests automatically with exponential backoff. A request is retried when any of these HTTP status codes is returned:

- [408](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/408) (Timeout)
- [429](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/429) (Too Many Requests)
- [5XX](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/500) (Internal Server Errors)

Configure retry behavior at the client or request level:

```java
import com.newscatcher.catchall.core.RequestOptions;

// Client-level retries
CatchAllApi client = CatchAllApi.builder()
    .apiKey("YOUR_API_KEY")
    .maxRetries(3)
    .build();

// Request-level retries
client.jobs().createJob(
    SubmitRequestDto.builder()
        .query("Tech company earnings this quarter")
        .build(),
    RequestOptions.builder()
        .maxRetries(3)
        .build()
);
```

The SDK respects the `Retry-After` header and `X-RateLimit-Reset` header before falling back to exponential backoff.

### Custom headers

Add custom headers at the client or request level:

```java
import com.newscatcher.catchall.core.RequestOptions;

// Client-level headers
CatchAllApi client = CatchAllApi.builder()
    .apiKey("YOUR_API_KEY")
    .addHeader("X-Custom-Header", "custom-value")
    .addHeader("X-Request-Id", "abc-123")
    .build();

// Request-level headers
client.jobs().createJob(
    SubmitRequestDto.builder()
        .query("Tech company earnings this quarter")
        .build(),
    RequestOptions.builder()
        .addHeader("X-Request-Header", "request-value")
        .build()
);
```

### Custom HTTP client

Customize the underlying `OkHttpClient` for advanced configuration:

```java
import okhttp3.OkHttpClient;
import java.net.InetSocketAddress;
import java.net.Proxy;

OkHttpClient customClient = new OkHttpClient.Builder()
    .proxy(new Proxy(Proxy.Type.HTTP,
        new InetSocketAddress("proxy.example.com", 8080)))
    .build();

CatchAllApi client = CatchAllApi.builder()
    .apiKey("YOUR_API_KEY")
    .httpClient(customClient)
    .build();
```

## Beta status

CatchAll API is in beta. Breaking changes may occur in minor version updates. See the [Changelog](https://www.newscatcherapi.com/docs/v3/catch-all/overview/changelog) for updates.

## Contributing

This library is generated programmatically from our API specification. Direct contributions to the generated code cannot be merged, but README improvements are welcome. To suggest SDK changes, please [open an issue](https://github.com/Newscatcher/newscatcher-catchall-java/issues).

## Support

- Documentation: [https://www.newscatcherapi.com/docs/v3/catch-all](https://www.newscatcherapi.com/docs/v3/catch-all)
- Support: <support@newscatcherapi.com>
