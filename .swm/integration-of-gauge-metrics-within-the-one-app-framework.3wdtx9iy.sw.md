---
title: Integration of Gauge Metrics within the One App Framework
---
This document will cover the integration of gauge metrics into the larger application. We'll cover:

1. The creation and usage of gauge metrics
2. How gauge metrics are used in different parts of the application
3. The role of gauge metrics in performance testing.

<SwmSnippet path="/src/server/metrics/gauges.js" line="19">

---

# Creation and usage of gauge metrics

The `gauges` object is used to store all the gauge metrics. The `createGauge` function is used to create a new gauge metric and store it in the `gauges` object. The `incrementGauge` and `setGauge` functions are used to manipulate the value of a gauge metric.

```javascript
const gauges = {};

function createGauge(opts) {
  const { name } = opts;
  if (gauges[name]) {
    return;
  }
  gauges[name] = new Gauge(opts);
}

function incrementGauge(name, ...args) {
  if (!gauges[name]) {
    throw new Error(`unable to find gauge ${name}, please create it first`);
  }
  gauges[name].inc(...args);
}

function setGauge(name, ...args) {
  if (!gauges[name]) {
    throw new Error(`unable to find gauge ${name}, please create it first`);
  }
```

---

</SwmSnippet>

<SwmSnippet path="/src/server/metrics/index.js" line="17">

---

# Usage of gauge metrics in different parts of the application

The `incrementGauge` function from `gauges.js` is imported and used here. This shows how gauge metrics are integrated into other parts of the application.

```javascript
import { incrementCounter } from './counters';
import { incrementGauge, setGauge, resetGauge } from './gauges';
import { startSummaryTimer } from './summaries';

import holocron from './holocron';
import intlCache from './intl-cache';
import * as appVersion from './app-version';
import ssr from './ssr';

export {
  // counters
  incrementCounter,

  // gauges
  incrementGauge,
```

---

</SwmSnippet>

<SwmSnippet path="/__performance__/bin/k6/metrics.js" line="19">

---

# Role of gauge metrics in performance testing

This file defines a set of metrics used for performance testing. It shows how gauge metrics are integrated into the larger application for performance monitoring.

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
