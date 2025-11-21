# Newscatcher CatchAll Java Library

[![fern shield](https://img.shields.io/badge/%F0%9F%8C%BF-Built%20with%20Fern-brightgreen)](https://buildwithfern.com?utm_source=github&utm_medium=github&utm_campaign=readme&utm_source=https%3A%2F%2Fgithub.com%2FNewscatcher%2Fnewscatcher-catchall-java)
[![Maven Central](https://img.shields.io/maven-central/v/com.newscatcherapi/newscatcher-catchall-sdk)](https://central.sonatype.com/artifact/com.newscatcherapi/newscatcher-catchall-sdk)

The Newscatcher CatchAll Java library provides access to the [CatchAll API](https://www.newscatcherapi.com/docs/v3/catch-all/overview/introduction), which transforms natural language queries into structured data extracted from web sources.

## Installation

### Gradle

Add the dependency in your `build.gradle` file:

```groovy
dependencies {
    implementation 'com.newscatcherapi:newscatcher-catchall-sdk:0.1.0'
}
```

### Maven

Add the dependency in your `pom.xml` file:

```xml
<dependency>
    <groupId>com.newscatcherapi</groupId>
    <artifactId>newscatcher-catchall-sdk</artifactId>
    <version>0.1.0</version>
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
import com.newscatcher.catchall.types.StatusResponseDto;
import com.newscatcher.catchall.types.PullJobResponseDto;
import com.newscatcher.catchall.types.SubmitResponseBody;

public class JobsExample {
    public static void main(String[] args) throws InterruptedException {
        CatchAllApi client = CatchAllApi.builder()
            .apiKey("YOUR_API_KEY")
            .build();

        // Create a job
        SubmitResponseBody job = client.jobs().createJob(
            SubmitRequestDto.builder()
                .query("Tech company earnings this quarter")
                .context("Focus on revenue and profit margins")
                .schema("Company [NAME] earned [REVENUE] in [QUARTER]")
                .build()
        );
        System.out.println("Job created: " + job.getJobId());

        // Poll for completion
        String jobId = job.getJobId();
        while (true) {
            StatusResponseDto status = client.jobs().getJobStatus(jobId);

            System.out.println("Current status: " + status.getStatus());

            // Check if job is completed
            if ("job_completed".equals(status.getStatus())) {
                System.out.println("Job completed!");
                break;
            }

            Thread.sleep(60000); // Wait 60 seconds
        }

        // Retrieve results
        PullJobResponseDto results = client.jobs().getJobResults(jobId);
        System.out.println(String.format(
            "Found %d valid records from %d candidates",
            results.getValidRecords(),
            results.getCandidateRecords()
        ));

        results.getAllRecords().forEach(record ->
            System.out.println(record.getRecordTitle())
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
import com.newscatcher.catchall.types.WebhookDto;
import com.newscatcher.catchall.types.CreateMonitorResponseDto;
import com.newscatcher.catchall.types.ListMonitorsResponseDto;
import com.newscatcher.catchall.types.PullMonitorResponseDto;
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
                    .method("POST")
                    .headers(Map.of("Authorization", "Bearer YOUR_TOKEN"))
                    .build())
                .build()
        );
        System.out.println("Monitor created: " + monitor.getMonitorId().orElse("N/A"));

        // List all monitors
        ListMonitorsResponseDto monitors = client.monitors().listMonitors();
        System.out.println("Total monitors: " + monitors.getTotalMonitors());

        // Get aggregated results
        monitor.getMonitorId().ifPresent(monitorId -> {
            PullMonitorResponseDto results = client.monitors().pullMonitorResults(monitorId);
            System.out.println("Collected " + results.getRecords() + " records");
        });
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
        results.getPage(),
        results.getTotalPages(),
        results.getAllRecords().size()
    ));

    results.getAllRecords().forEach(record -> {
        // Process each record
        System.out.println("  - " + record.getRecordTitle());
    });

    if (results.getPage() >= results.getTotalPages()) {
        break;
    }
    page++;
}

System.out.println("Processed " + results.getValidRecords() + " total records");
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
