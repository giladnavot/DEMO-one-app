---
title: Getting Started with Gauges
---
Gauges in this codebase are a type of metric used to measure a specific value at a particular point in time. They are implemented using the 'prom-client' library. Gauges are stored in a global object and can be created, incremented, set to a specific value, or reset. They are used throughout the codebase to monitor various aspects of the application's performance and state.

<SwmSnippet path="/src/server/metrics/gauges.js" line="21">

---

# Creating a Gauge

The `createGauge` function is used to create a new gauge. It takes an options object as a parameter, which must include a unique name for the gauge. If a gauge with the same name already exists, the function will simply return.

```javascript
function createGauge(opts) {
  const { name } = opts;
  if (gauges[name]) {
    return;
  }
  gauges[name] = new Gauge(opts);
}
```

---

</SwmSnippet>

<SwmSnippet path="/src/server/metrics/gauges.js" line="29">

---

# Incrementing a Gauge

The `incrementGauge` function is used to increment the value of a gauge. It takes the name of the gauge and any additional arguments. If the gauge does not exist, it throws an error.

```javascript
function incrementGauge(name, ...args) {
  if (!gauges[name]) {
    throw new Error(`unable to find gauge ${name}, please create it first`);
  }
  gauges[name].inc(...args);
}
```

---

</SwmSnippet>

<SwmSnippet path="/src/server/metrics/gauges.js" line="36">

---

# Setting a Gauge

The `setGauge` function is used to set the value of a gauge to a specific value. It takes the name of the gauge and any additional arguments. If the gauge does not exist, it throws an error.

```javascript
function setGauge(name, ...args) {
  if (!gauges[name]) {
    throw new Error(`unable to find gauge ${name}, please create it first`);
  }
  gauges[name].set(...args);
}
```

---

</SwmSnippet>

<SwmSnippet path="/src/server/metrics/gauges.js" line="43">

---

# Resetting a Gauge

The `resetGauge` function is used to reset the value of a gauge. It takes the name of the gauge as a parameter. If the gauge does not exist, it throws an error.

```javascript
function resetGauge(name) {
  if (!gauges[name]) {
    throw new Error(`unable to find gauge ${name}, please create it first`);
  }
  gauges[name].reset();
}
```

---

</SwmSnippet>

# Functions of Gauges

This section covers the main functions related to Gauges in the application.

<SwmSnippet path="/src/server/metrics/gauges.js" line="21">

---

## createGauge

The `createGauge` function is used to create a new Gauge. It takes an options object as a parameter, which includes the name of the Gauge. If a Gauge with the same name already exists, it does not create a new one.

```javascript
function createGauge(opts) {
  const { name } = opts;
  if (gauges[name]) {
    return;
  }
  gauges[name] = new Gauge(opts);
}
```

---

</SwmSnippet>

<SwmSnippet path="/src/server/metrics/gauges.js" line="29">

---

## incrementGauge

The `incrementGauge` function is used to increment the value of a Gauge. It takes the name of the Gauge and additional arguments. If the Gauge does not exist, it throws an error.

```javascript
function incrementGauge(name, ...args) {
  if (!gauges[name]) {
    throw new Error(`unable to find gauge ${name}, please create it first`);
  }
  gauges[name].inc(...args);
}
```

---

</SwmSnippet>

<SwmSnippet path="/src/server/metrics/gauges.js" line="36">

---

## setGauge

The `setGauge` function is used to set the value of a Gauge. It takes the name of the Gauge and additional arguments. If the Gauge does not exist, it throws an error.

```javascript
function setGauge(name, ...args) {
  if (!gauges[name]) {
    throw new Error(`unable to find gauge ${name}, please create it first`);
  }
  gauges[name].set(...args);
}
```

---

</SwmSnippet>

<SwmSnippet path="/src/server/metrics/gauges.js" line="43">

---

## resetGauge

The `resetGauge` function is used to reset the value of a Gauge. It takes the name of the Gauge as a parameter. If the Gauge does not exist, it throws an error.

```javascript
function resetGauge(name) {
  if (!gauges[name]) {
    throw new Error(`unable to find gauge ${name}, please create it first`);
  }
  gauges[name].reset();
}
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1vbmUtYXBwJTNBJTNBZ2lsYWRuYXZvdA==" repo-name="DEMO-one-app" doc-type="overview"><sup>Powered by [Swimm](/)</sup></SwmMeta>
