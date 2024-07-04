---
title: Basic Concepts of Server Side Rendering Metrics
---
Server Side Rendering Metrics, in the context of this repository, refers to the performance measurements taken during the server-side rendering process. This is crucial for understanding the efficiency of the rendering process and identifying potential bottlenecks.

In the DEMO-one-app, these metrics are specifically related to the time taken by React to render HTML. This is measured in seconds and is labeled by the method name used for rendering.

<SwmSnippet path="/src/server/metrics/ssr.js" line="21">

---

## Creating a Metric Namespace

Here, a metric namespace 'ssr' is created using the `createMetricNamespace` function. This namespace will be used to group all SSR related metrics.

```javascript
const ssrNamespace = createMetricNamespace('ssr');
```

---

</SwmSnippet>

<SwmSnippet path="/src/server/metrics/ssr.js" line="23">

---

## Creating a Summary Metric

A summary metric 'react_rendering' is created under the 'ssr' namespace. This metric measures the time taken by React to render HTML, in seconds. The `labelNames` property is used to provide additional dimensions for the metric, in this case, the method name used for rendering.

```javascript
createSummary({
  name: ssrNamespace('react_rendering', 'seconds'),
  help: 'time taken by React to render HTML',
  labelNames: ['renderMethodName'],
});
```

---

</SwmSnippet>

# Server Side Rendering Metrics Functions

This section will explain the functions `createSummary` and `createMetricNamespace` used in Server Side Rendering Metrics.

<SwmSnippet path="/src/server/metrics/ssr.js" line="19">

---

## createMetricNamespace

The `createMetricNamespace` function is used to create a namespace for the metrics. In this case, it creates a namespace 'ssr'.

```javascript
import createMetricNamespace from './create-metric-namespace';

const ssrNamespace = createMetricNamespace('ssr');
```

---

</SwmSnippet>

<SwmSnippet path="/src/server/metrics/ssr.js" line="23">

---

## createSummary

The `createSummary` function is used to create a summary of the metrics. It takes an object as an argument which includes the name of the metric, a help description, and label names. In this case, it creates a summary for 'react_rendering' metric which measures the time taken by React to render HTML.

```javascript
createSummary({
  name: ssrNamespace('react_rendering', 'seconds'),
  help: 'time taken by React to render HTML',
  labelNames: ['renderMethodName'],
});
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1vbmUtYXBwJTNBJTNBZ2lsYWRuYXZvdA==" repo-name="DEMO-one-app" doc-type="overview"><sup>Powered by [Swimm](/)</sup></SwmMeta>
