---
title: Exploring Internationalization Cache Metrics
---
Internationalization Cache Metrics in the DEMO-one-app refers to the metrics related to the internationalization (i18n) cache. This cache is used to store internationalization data, such as translations, to improve the performance of the application. The metrics provide insights into the usage and performance of this cache.

The `intlServerCacheNamespace` is a namespace created specifically for these metrics. It is used to group related metrics together, making them easier to manage and understand.

The `createGauge` function is used to create a gauge metric, which is a type of metric that represents a single numerical value that can arbitrarily go up and down. In this case, it is used to track the estimated size of the internationalization server cache.

The `collect` method is part of the gauge metric. It is called to collect the current value of the metric. In this case, it retrieves the estimated size of the internationalization server cache and sets it as the current value of the gauge.

Finally, the `metricNames` constant is an array of the names of all the metrics in the `intlServerCacheNamespace`. This can be used to easily access all the metrics in this namespace.

<SwmSnippet path="/src/server/metrics/intl-cache.js" line="23">

---

## Creating the Gauge Metric

Here, a gauge metric is created with the name 'cache_size' under the 'intl' namespace. The `collect` method is used to get the estimated size of the internationalization server cache and set it as the value of the gauge.

```javascript
createGauge({
  name: intlServerCacheNamespace('cache_size', 'total'),
  help: 'estimated intl server cache size',
  collect() {
    const size = getEstimatedSize();
    this.set(size);
  },
});
```

---

</SwmSnippet>

<SwmSnippet path="/src/server/metrics/intl-cache.js" line="32">

---

## Exporting the Metric Names

The metric names under the 'intl' namespace are retrieved and exported for use in other parts of the application.

```javascript
const metricNames = intlServerCacheNamespace.getMetricNames();

export default metricNames;
```

---

</SwmSnippet>

# Internationalization Cache Metrics

Internationalization Cache Metrics Functions

<SwmSnippet path="/src/server/metrics/intl-cache.js" line="23">

---

## createGauge

The `createGauge` function is used to create a new gauge metric. It takes an object as an argument, which includes the name of the metric, a help description, and a collect method. The collect method is responsible for setting the value of the gauge metric, which in this case is the estimated size of the internationalization cache.

```javascript
createGauge({
  name: intlServerCacheNamespace('cache_size', 'total'),
  help: 'estimated intl server cache size',
  collect() {
    const size = getEstimatedSize();
    this.set(size);
  },
});
```

---

</SwmSnippet>

<SwmSnippet path="/src/server/metrics/intl-cache.js" line="27">

---

## getEstimatedSize

The `getEstimatedSize` function is used within the collect method of the `createGauge` function. It retrieves the estimated size of the internationalization cache, which is then set as the value of the gauge metric.

```javascript
    const size = getEstimatedSize();
    this.set(size);
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1vbmUtYXBwJTNBJTNBZ2lsYWRuYXZvdA==" repo-name="DEMO-one-app" doc-type="overview"><sup>Powered by [Swimm](/)</sup></SwmMeta>
