## 1.1.0 - 2026-02-23
* feat: add Server-Sent Events support with event-level discrimination
* Enhance the Stream API to support Server-Sent Events (SSE) with advanced discrimination capabilities, enabling more sophisticated real-time data processing patterns.
* Key changes:
* Add SseEvent<T> generic class for representing SSE data with standard fields (event, data, id, retry)
* Add SseEventParser utility with support for both data-level and event-level discrimination patterns
* Extend Stream class with SSE_EVENT_DISCRIMINATED type and new factory methods for event-level discrimination
* Add SSEEventDiscriminatedIterator for parsing SSE streams with envelope-level discriminators
* Update CLI version from 3.65.0 to 3.76.0 and generator version from 3.34.8 to 3.38.0
* Add additionalProperty and additionalProperties methods to all Builder classes for enhanced flexibility
* Add comprehensive test coverage for SSE event discrimination functionality
* 🌿 Generated with Fern

