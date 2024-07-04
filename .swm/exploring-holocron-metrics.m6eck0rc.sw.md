---
title: Exploring Holocron Metrics
---
Holocron Metrics in the DEMO-one-app repository refers to the system of measurement and monitoring of the Holocron module's performance and behavior. It is implemented using counters and gauges to track various aspects such as the total times the module map has been polled, updated, or restarted, the delay until the next poll of the module map, the number of times the server encountered errors polling the module map, the number of Holocron modules that have failed to load, and the number of Holocron modules which require external fallbacks. These metrics provide valuable insights into the operation of the Holocron modules, helping to identify potential issues and optimize performance.

<SwmSnippet path="/src/server/metrics/holocron.js" line="22">

---

# Creating Holocron Metrics

Here, a metric namespace 'holocron' is created using the `createMetricNamespace` function. This namespace is used to group related metrics. A counter metric 'module_map_poll' is then created using the `createCounter` function. This metric tracks the total times the module map has been polled.

```javascript
const holocronNamespace = createMetricNamespace('holocron');

createCounter({
  name: holocronNamespace('module_map_poll', 'total'),
  help: 'total times the module map has been polled',
});
```

---

</SwmSnippet>

<SwmSnippet path="/src/server/metrics/holocron.js" line="39">

---

# Using Holocron Metrics

Here, a gauge metric 'module_map_poll_wait' is created using the `createGauge` function. This metric measures the delay until the next poll of the module map. Gauge metrics are used when you want to measure the current value of a particular entity, rather than counting occurrences.

```javascript
createGauge({
  name: holocronNamespace('module_map_poll_wait', 'seconds'),
  help: 'delay until the next poll of the module map',
});
```

---

</SwmSnippet>

<SwmSnippet path="/src/server/metrics/holocron.js" line="54">

---

Another gauge metric 'modules_requiring_fallbacks' is created here. This metric measures how many Holocron modules require external fallbacks.

```javascript
createGauge({
  name: holocronNamespace('modules_requiring_fallbacks', 'total'),
  help: 'how many Holocron modules which require external fallbacks',
});
```

---

</SwmSnippet>

# Holocron Metrics Functions

This section covers the main functions used in the Holocron metrics module.

<SwmSnippet path="/src/server/metrics/holocron.js" line="20">

---

## createMetricNamespace

The `createMetricNamespace` function is used to create a namespace for Holocron metrics. This namespace is used as a prefix for the names of the metrics created in this module.

```javascript
import createMetricNamespace from './create-metric-namespace';

const holocronNamespace = createMetricNamespace('holocron');
```

---

</SwmSnippet>

<SwmSnippet path="/src/server/metrics/holocron.js" line="24">

---

## createCounter

The `createCounter` function is used to create counter metrics. These metrics are used to count the number of times certain events occur, such as the module map being polled, updated, or restarted.

```javascript
createCounter({
  name: holocronNamespace('module_map_poll', 'total'),
  help: 'total times the module map has been polled',
});

createCounter({
  name: holocronNamespace('module_map_updated', 'total'),
  help: 'total times the module map has been updated',
});

createCounter({
  name: holocronNamespace('module_map_poll_restarted', 'total'),
  help: 'total times the module map poll has been restarted',
});
```

---

</SwmSnippet>

<SwmSnippet path="/src/server/metrics/holocron.js" line="39">

---

## createGauge

The `createGauge` function is used to create gauge metrics. These metrics are used to measure the current value of specific parameters, such as the delay until the next poll of the module map, the number of consecutive errors encountered while polling the module map, the number of failed module loads, and the number of modules requiring external fallbacks.

```javascript
createGauge({
  name: holocronNamespace('module_map_poll_wait', 'seconds'),
  help: 'delay until the next poll of the module map',
});

createGauge({
  name: holocronNamespace('module_map_poll_consecutive_errors', 'total'),
  help: 'how many times has the server encountered errors polling the module map',
});

createGauge({
  name: holocronNamespace('rejected_modules', 'total'),
  help: 'how many Holocron modules have failed to load',
});

createGauge({
  name: holocronNamespace('modules_requiring_fallbacks', 'total'),
  help: 'how many Holocron modules which require external fallbacks',
});
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1vbmUtYXBwJTNBJTNBZ2lsYWRuYXZvdA==" repo-name="DEMO-one-app" doc-type="overview"><sup>Powered by [Swimm](/)</sup></SwmMeta>
