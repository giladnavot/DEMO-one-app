---
title: Performance Implications of Using hasChildRoutes in Large Applications
---
This document will cover the potential performance implications of using `hasChildRoutes` frequently in large applications. We'll cover:

1. What `hasChildRoutes` is and how it's used
2. How `hasChildRoutes` could impact performance
3. How to monitor performance in the application.

<SwmSnippet path="/src/universal/utils/hasChildRoutes.js" line="17">

---

# What is `hasChildRoutes` and how it's used

`hasChildRoutes` is a utility function that checks if a module has child routes. It's used to determine the structure of the application's routing.

```javascript
const hasChildRoutes = (module) => module && !!module.childRoutes;
```

---

</SwmSnippet>

# How `hasChildRoutes` could impact performance

Frequent use of `hasChildRoutes` in a large application could potentially impact performance. This is because each invocation of `hasChildRoutes` requires traversing the module's properties to check for child routes. In a large application with many modules and nested routes, this could add up to a significant amount of processing.

<SwmSnippet path="/__performance__/bin/k6/metrics.js" line="19">

---

# Monitoring performance in the application

The application includes a performance monitoring setup using k6. This setup measures various performance metrics such as request duration, time spent establishing TCP connection, time spent sending and receiving data, and more. These metrics can be used to monitor the performance impact of using `hasChildRoutes` frequently.

```javascript
const metrics = {
  http_reqs: {
    label: 'HTTP Requests',
    format: float,
    description: 'total HTTP requests k6 generated',
  },
  http_req_duration: {
    label: 'Request Duration',
    format: milliseconds,
    description: 'total time for the request. It\'s equal to http_req_sending + http_req_waiting + http_req_receiving (i.e. how long did the remote server take to process the request and respond, without the initial DNS lookup/connection times)',
  },
  http_req_blocked: {
    label: 'Request Blocked',
    format: milliseconds,
    description: 'time spent blocked (waiting for a free TCP connection slot) before initiating the request',
  },
  http_req_connecting: {
    label: 'Request Connecting',
    format: milliseconds,
    description: 'time spent establishing TCP connection to the remote host',
  },
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1vbmUtYXBwJTNBJTNBZ2lsYWRuYXZvdA==" repo-name="DEMO-one-app" doc-type="follow-up"><sup>Powered by [Swimm](/)</sup></SwmMeta>
