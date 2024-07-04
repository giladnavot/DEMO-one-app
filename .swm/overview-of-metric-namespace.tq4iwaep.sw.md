---
title: Overview of Metric Namespace
---
A Metric Namespace in the DEMO-one-app refers to a unique identifier used to group and categorize metrics. It is used to organize the metrics data and make it easier to manage. The namespace is created using the `createMetricNamespace` function, which ensures that the namespace is a single word. This function also creates metric names, which are composed of the application name, the namespace, and optional units. The units must be one of the predefined valid units: 'seconds', 'bytes', or 'total'.

<SwmSnippet path="/src/server/metrics/create-metric-namespace.js" line="26">

---

## Using the `createMetricNamespace` function

This is the `createMetricNamespace` function. It takes a namespace as an argument and checks if it is a single word. It then creates a `createMetricName` function that can be used to create new metrics within that namespace.

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

<SwmSnippet path="/src/server/metrics/create-metric-namespace.js" line="32">

---

## Creating a new metric

This is the `createMetricName` function. It takes a middle and units as arguments and creates a new metric name. It also checks if the units are valid and if they are used correctly.

```javascript
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

        if (unitParts[0] === 'total') {
          throw new Error(`should use "total" only once (given "${units}")`);
        }
      }

```

---

</SwmSnippet>

<SwmSnippet path="/src/server/metrics/create-metric-namespace.js" line="67">

---

## Getting the metric names

The `getMetricNames` function is used to get all the metric names within a namespace. It can be used to retrieve the names of all the metrics that have been created within a namespace.

```javascript
  createMetricName.getMetricNames = () => names;
  return createMetricName;
```

---

</SwmSnippet>

# Metric Namespace Functions

Metric Namespace Function

<SwmSnippet path="/src/server/metrics/create-metric-namespace.js" line="26">

---

## createMetricNamespace

The `createMetricNamespace` function is used to create a namespace for metrics. It ensures that the namespace is a single word and throws an error if it is not. It also creates metric names within that namespace, ensuring that the units used are valid and that the 'total' unit is used correctly. The function returns a 'createMetricName' function, which can be used to create additional metric names within the namespace.

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

<SwmSnippet path="/src/server/metrics/create-metric-namespace.js" line="32">

---

## createMetricName

The `createMetricName` function is a nested function within `createMetricNamespace`. It is used to create a metric name within the namespace. It checks the units used in the metric name, ensuring they are valid and that the 'total' unit is used correctly. The function returns the created metric name.

```javascript
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

        if (unitParts[0] === 'total') {
          throw new Error(`should use "total" only once (given "${units}")`);
        }
      }

```

---

</SwmSnippet>

<SwmSnippet path="/src/server/metrics/create-metric-namespace.js" line="67">

---

## getMetricNames

The `getMetricNames` function is a method of `createMetricName`. It returns all the metric names created within the namespace.

```javascript
  createMetricName.getMetricNames = () => names;
  return createMetricName;
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1vbmUtYXBwJTNBJTNBZ2lsYWRuYXZvdA==" repo-name="DEMO-one-app" doc-type="overview"><sup>Powered by [Swimm](/)</sup></SwmMeta>
