---
title: Basic Concepts of Request Handling
---
Request handling in the DEMO-one-app refers to the process of receiving, processing, and responding to HTTP requests sent by the client. This process is primarily handled in the `src/server/plugins/requestLogging.js` file. The request object (`req`) contains details about the incoming request such as the method, protocol, and referrer. The response is built using various symbols like `$RouteHandler`, `$ResponseBuilder`, `$RequestOverhead`, and `$ExternalRequestDuration` which are used to measure and log the duration of different parts of the request handling process. The `requestLogging.js` file also contains utility functions (`UTILS`) for configuring the request log.

<SwmSnippet path="/src/server/plugins/requestLogging.js" line="78">

---

## The 'method' Property

The 'method' property is used to identify the HTTP method (GET, POST, PUT, DELETE, etc.) of the incoming request. This information is crucial for determining how to process the request.

```javascript
  return {
    method: request.method,
    correlationId: headers['correlation-id'],
    // null to explicitly signal no value, undefined if not expected for every request
```

---

</SwmSnippet>

<SwmSnippet path="/src/server/plugins/requestLogging.js" line="120">

---

## The 'req' Property

The 'req' property is used to access the request object. This object contains all the information about the incoming request, including headers, query parameters, and body data. It is used throughout the request handling process to extract necessary information.

```javascript
  const configuredLog = UTILS.configureRequestLog({
    req: request,
    res: reply,
    log,
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1vbmUtYXBwJTNBJTNBZ2lsYWRuYXZvdA==" repo-name="DEMO-one-app" doc-type="overview"><sup>Powered by [Swimm](/)</sup></SwmMeta>
