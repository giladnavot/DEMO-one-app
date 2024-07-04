---
title: Exploring Server Metrics
---
Server Metrics in the DEMO-one-app repository refers to the collection of data points that provide insights into the performance and health of the server. These metrics are implemented using various functions and modules such as counters, gauges, and summaries. They are used to monitor different aspects of the server's operation, such as the time taken by React to render HTML, the total times the module map has been polled or updated, and the delay until the next poll of the module map. The metrics are organized in a structured manner, making it easier to monitor and debug the server's performance.

<SwmSnippet path="/src/server/metrics/counters.js" line="17">

---

## Counters

Counters are a type of metric that can only increase or be reset to zero. They are used to count the occurrence of events. In the DEMO-one-app, counters are used to track events like the number of times the module map has been polled or updated.

```javascript
import { Counter } from 'prom-client';

const counters = {};

function createCounter(opts) {
  const { name } = opts;
  if (counters[name]) {
    return;
  }
  counters[name] = new Counter(opts);
}

function incrementCounter(name, ...args) {
  if (!counters[name]) {
    throw new Error(`unable to find counter ${name}, please create it first`);
  }
  counters[name].inc(...args);
}
```

---

</SwmSnippet>

<SwmSnippet path="/src/server/metrics/gauges.js" line="17">

---

## Gauges

Gauges are a type of metric that can arbitrarily go up and down. They are used to measure the current value of particular item. In the DEMO-one-app, gauges are used to track values like the delay until the next poll of the module map or the number of Holocron modules that have failed to load.

```javascript
import { Gauge } from 'prom-client';

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
```

---

</SwmSnippet>

<SwmSnippet path="/src/server/metrics/summaries.js" line="17">

---

## Summaries

Summaries are a type of metric that calculate quantiles over a sliding time window. They are used to track the size and duration of events. In the DEMO-one-app, a summary is used to measure the time taken by React to render HTML.

```javascript
import { Summary } from 'prom-client';

const summaries = {};

function createSummary(opts) {
  const { name } = opts;
  if (summaries[name]) {
    return;
  }
  summaries[name] = new Summary(opts);
}

function startSummaryTimer(name, ...args) {
  if (!summaries[name]) {
    throw new Error(`unable to find summary ${name}, please create it first`);
  }
  return summaries[name].startTimer(...args);
}
```

---

</SwmSnippet>

<SwmSnippet path="/src/server/metrics/create-metric-namespace.js" line="26">

---

## Metric Namespaces

Metric namespaces are used to group related metrics together. In the DEMO-one-app, each metric category (like 'holocron' or 'version') has its own namespace.

```javascript
export default function createMetricNamespace(namespace) {
  if (namespace.split('_').length > 1) {
    throw new Error('metric namespace/application should be a single word');
  }

  const names = {};
  const createMetricName = (middle, units) => {
    const nameParts = ['oneapp', namespace, middle];

    if (units) {
      const unitParts = units.split('_');

      if (unitParts.length > 2) {
        throw new Error(`should use only one type of units unless also using "total" (given "${units}")`);
      }

      // seconds_total is okay, seconds_bytes is not
      if (unitParts.length > 1) {
        if (unitParts[1] !== 'total') {
          throw new Error(`if combining two units the last should be "total" (given "${unitParts[1]}")`);
        }
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1vbmUtYXBwJTNBJTNBZ2lsYWRuYXZvdA==" repo-name="DEMO-one-app" doc-type="overview"><sup>Powered by [Swimm](/)</sup></SwmMeta>
