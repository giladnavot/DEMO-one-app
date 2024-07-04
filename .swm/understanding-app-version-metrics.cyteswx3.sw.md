---
title: Understanding App Version Metrics
---
App Version Metrics in the DEMO-one-app refers to the system of tracking and reporting the version information of the application. This is achieved by creating a gauge metric with labels for the version, major, minor, patch, prerelease, and build information. The version information is parsed from the build metadata and set to the gauge for reporting. This allows for monitoring and tracking the versions of the application that are in use.

<SwmSnippet path="/src/server/metrics/app-version.js" line="22">

---

## App Version Metrics Creation

Here, the App Version Metrics are created. The `versionNamespace` is defined and a gauge is created with labels for each versioning component. The `help` property provides a description of the metric.

```javascript
const { buildVersion: version } = readJsonFile('../../../.build-meta.json');
const versionNamespace = createMetricNamespace('version');

createGauge({
  name: versionNamespace('info'),
  labelNames: ['version', 'major', 'minor', 'patch', 'prerelease', 'build'],
  help: 'One App version info',
});
```

---

</SwmSnippet>

<SwmSnippet path="/src/server/metrics/app-version.js" line="32">

---

## Parsing and Setting the Gauge

In the `parseVersionAndSetGaugue` function, the version is parsed and the gauge is set with the versioning components. This function is called immediately to set the initial values.

```javascript
function parseVersionAndSetGaugue() {
  // semver expects build to be qualified with '+'
  // current version of one app bundler appends build with '-'
  const parts = version.split('-');
  const build = parts.pop();
  const {
    major, minor, patch, prerelease,
  } = semverParse(parts.join('-'));

  setGauge(versionNamespace.getMetricNames().info, {
    version,
    major,
    minor,
    patch,
    prerelease: prerelease.join('.') || null,
    build,
  }, 1);
}
```

---

</SwmSnippet>

<SwmSnippet path="/src/server/metrics/app-version.js" line="52">

---

## Accessing the Metrics

The `versionNamespace.getMetricNames()` function is exported, allowing other parts of the codebase to access the App Version Metrics.

```javascript
export default versionNamespace.getMetricNames();
```

---

</SwmSnippet>

# App Version Metrics Functions

This section will cover the main functions related to App Version Metrics.

<SwmSnippet path="/src/server/metrics/app-version.js" line="25">

---

## createGauge Function

The `createGauge` function is used to create a new gauge metric. It takes an object as an argument, which includes the name of the metric, labels, and a help description. In this case, it's creating a gauge for the One App version info.

```javascript
createGauge({
  name: versionNamespace('info'),
  labelNames: ['version', 'major', 'minor', 'patch', 'prerelease', 'build'],
  help: 'One App version info',
});
```

---

</SwmSnippet>

<SwmSnippet path="/src/server/metrics/app-version.js" line="32">

---

## parseVersionAndSetGaugue Function

The `parseVersionAndSetGaugue` function is responsible for parsing the version and setting the gauge. It splits the version into parts, parses it using semver, and then sets the gauge with the parsed version info.

```javascript
function parseVersionAndSetGaugue() {
  // semver expects build to be qualified with '+'
  // current version of one app bundler appends build with '-'
  const parts = version.split('-');
  const build = parts.pop();
  const {
    major, minor, patch, prerelease,
  } = semverParse(parts.join('-'));

  setGauge(versionNamespace.getMetricNames().info, {
    version,
    major,
    minor,
    patch,
    prerelease: prerelease.join('.') || null,
    build,
  }, 1);
}
```

---

</SwmSnippet>

<SwmSnippet path="/src/server/metrics/gauges.js" line="36">

---

## setGauge Function

The `setGauge` function is used to set the value of a gauge. It takes the name of the gauge and the value as arguments. If the gauge does not exist, it throws an error.

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

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1vbmUtYXBwJTNBJTNBZ2lsYWRuYXZvdA==" repo-name="DEMO-one-app" doc-type="overview"><sup>Powered by [Swimm](/)</sup></SwmMeta>
