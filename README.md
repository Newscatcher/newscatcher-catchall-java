# Newscatcher Java Library

[![fern shield](https://img.shields.io/badge/%F0%9F%8C%BF-Built%20with%20Fern-brightgreen)](https://buildwithfern.com?utm_source=github&utm_medium=github&utm_campaign=readme&utm_source=https%3A%2F%2Fgithub.com%2FNewscatcher%2Fnewscatcher-catchall-java)
[![Maven Central](https://img.shields.io/maven-central/v/com.newscatcherapi/newscatcher-catchall-sdk)](https://central.sonatype.com/artifact/com.newscatcherapi/newscatcher-catchall-sdk)

The Newscatcher Java library provides convenient access to the Newscatcher APIs from Java.

## Table of Contents

- [Documentation](#documentation)
- [Installation](#installation)
- [Reference](#reference)
- [Usage](#usage)
- [Environments](#environments)
- [Base Url](#base-url)
- [Exception Handling](#exception-handling)
- [Advanced](#advanced)
  - [Custom Client](#custom-client)
  - [Retries](#retries)
  - [Timeouts](#timeouts)
  - [Custom Headers](#custom-headers)
  - [Access Raw Response Data](#access-raw-response-data)
- [Contributing](#contributing)

## Documentation

API reference documentation is available [here](https://www.newscatcherapi.com/docs/v3/catch-all/endpoints/create-job).

## Installation

### Gradle

Add the dependency in your `build.gradle` file:

```groovy
dependencies {
  implementation 'com.newscatcherapi:newscatcher-catchall-sdk'
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

A full reference for this library is available [here](https://github.com/Newscatcher/newscatcher-catchall-java/blob/HEAD/./reference.md).

## Usage

Instantiate and use the client with the following:

```java
package com.example.usage;

import com.newscatcher.catchall.CatchAllApi;
import com.newscatcher.catchall.resources.jobs.requests.SubmitRequestDto;

public class Example {
    public static void main(String[] args) {
        CatchAllApi client = CatchAllApi
            .builder()
            .apiKey("<value>")
            .build();

        client.jobs().createJob(
            SubmitRequestDto
                .builder()
                .query("Tech company earnings this quarter")
                .schema("Company [NAME] earned [REVENUE] in [QUARTER]")
                .context("Focus on revenue and profit margins")
                .build()
        );
    }
}
```

## Environments

This SDK allows you to configure different environments for API requests.

```java
import com.newscatcher.catchall.CatchAllApi;
import com.newscatcher.catchall.core.Environment;

CatchAllApi client = CatchAllApi
    .builder()
    .environment(Environment.Default)
    .build();
```

## Base Url

You can set a custom base URL when constructing the client.

```java
import com.newscatcher.catchall.CatchAllApi;

CatchAllApi client = CatchAllApi
    .builder()
    .url("https://example.com")
    .build();
```

## Exception Handling

When the API returns a non-success status code (4xx or 5xx response), an API exception will be thrown.

```java
import com.newscatcher.catchall.core.NewscatcherApiApiException;

try{
    client.jobs().createJob(...);
} catch (NewscatcherApiApiException e){
    // Do something with the API exception...
}
```

## Advanced

### Custom Client

This SDK is built to work with any instance of `OkHttpClient`. By default, if no client is provided, the SDK will construct one.
However, you can pass your own client like so:

```java
import com.newscatcher.catchall.CatchAllApi;
import okhttp3.OkHttpClient;

OkHttpClient customClient = ...;

CatchAllApi client = CatchAllApi
    .builder()
    .httpClient(customClient)
    .build();
```

### Retries

The SDK is instrumented with automatic retries with exponential backoff. A request will be retried as long
as the request is deemed retryable and the number of retry attempts has not grown larger than the configured
retry limit (default: 2). Before defaulting to exponential backoff, the SDK will first attempt to respect
the `Retry-After` header (as either in seconds or as an HTTP date), and then the `X-RateLimit-Reset` header
(as a Unix timestamp in epoch seconds); failing both of those, it will fall back to exponential backoff.

A request is deemed retryable when any of the following HTTP status codes is returned:

- [408](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/408) (Timeout)
- [429](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/429) (Too Many Requests)
- [5XX](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/500) (Internal Server Errors)

Use the `maxRetries` client option to configure this behavior.

```java
import com.newscatcher.catchall.CatchAllApi;

CatchAllApi client = CatchAllApi
    .builder()
    .maxRetries(1)
    .build();
```

### Timeouts

The SDK defaults to a 60 second timeout. You can configure this with a timeout option at the client or request level.

```java
import com.newscatcher.catchall.CatchAllApi;
import com.newscatcher.catchall.core.RequestOptions;

// Client level
CatchAllApi client = CatchAllApi
    .builder()
    .timeout(10)
    .build();

// Request level
client.jobs().createJob(
    ...,
    RequestOptions
        .builder()
        .timeout(10)
        .build()
);
```

### Custom Headers

The SDK allows you to add custom headers to requests. You can configure headers at the client level or at the request level.

```java
import com.newscatcher.catchall.CatchAllApi;
import com.newscatcher.catchall.core.RequestOptions;

// Client level
CatchAllApi client = CatchAllApi
    .builder()
    .addHeader("X-Custom-Header", "custom-value")
    .addHeader("X-Request-Id", "abc-123")
    .build();
;

// Request level
client.jobs().createJob(
    ...,
    RequestOptions
        .builder()
        .addHeader("X-Request-Header", "request-value")
        .build()
);
```

### Access Raw Response Data

The SDK provides access to raw response data, including headers, through the `withRawResponse()` method.
The `withRawResponse()` method returns a raw client that wraps all responses with `body()` and `headers()` methods.
(A normal client's `response` is identical to a raw client's `response.body()`.)

```java
CreateJobHttpResponse response = client.jobs().withRawResponse().createJob(...);

System.out.println(response.body());
System.out.println(response.headers().get("X-My-Header"));
```

## Contributing

While we value open-source contributions to this SDK, this library is generated programmatically.
Additions made directly to this library would have to be moved over to our generation code,
otherwise they would be overwritten upon the next generated release. Feel free to open a PR as
a proof of concept, but know that we will not be able to merge it as-is. We suggest opening
an issue first to discuss with us!

On the other hand, contributions to the README are always very welcome!