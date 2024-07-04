---
title: Use Cases for Gauge Metrics in One App
---
This document will cover the topic of gauge metrics in the DEMO-one-app application. We'll cover:

1. What are gauge metrics
2. How gauge metrics are used in the codebase
3. Typical use cases for gauge metrics in the application.

# What are gauge metrics

Gauge metrics are a type of metric that can be used to record a value that can arbitrarily go up and down. They are typically used to measure values such as memory usage, number of concurrent requests, queue size, etc. In the context of DEMO-one-app, gauge metrics are implemented using the `prom-client` library, specifically the `Gauge` class.

<SwmSnippet path="/src/server/metrics/gauges.js" line="19">

---

# How gauge metrics are used in the codebase

In the `gauges.js` file, we can see the `gauges` object which stores all the gauge metrics. There are also several functions such as `createGauge`, `incrementGauge`, `setGauge`, and `resetGauge` which are used to manipulate these gauge metrics. These functions are exported and used in other parts of the codebase.

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

<SwmSnippet path="/src/server/metrics/holocron.js" line="18">

---

# Typical use cases for gauge metrics in the application

Here, the `createGauge` function is imported and used to create gauge metrics related to Holocron.

```javascript
import { createGauge } from './gauges';
```

---

</SwmSnippet>

<SwmSnippet path="/src/server/metrics/intl-cache.js" line="18">

---

In this file, `createGauge` is used to create gauge metrics related to the internationalization cache.

```javascript
import { createGauge } from './gauges';
```

---

</SwmSnippet>

<SwmSnippet path="/src/server/metrics/app-version.js" line="18">

---

Here, `createGauge` and `setGauge` are used to create and set gauge metrics related to the application version.

```javascript
import { createGauge, setGauge } from './gauges';
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1vbmUtYXBwJTNBJTNBZ2lsYWRuYXZvdA==" repo-name="DEMO-one-app" doc-type="follow-up"><sup>Powered by [Swimm](/)</sup></SwmMeta>
