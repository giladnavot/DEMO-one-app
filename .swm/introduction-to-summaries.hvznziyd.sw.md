---
title: Introduction to Summaries
---
In the DEMO-one-app, 'Summaries' is an object that holds instances of the 'Summary' class from the 'prom-client' library. These instances are used to collect and record metrics data in the application.

The 'createSummary' function is used to create a new 'Summary' instance with the provided options and store it in the 'summaries' object. If a 'Summary' with the same name already exists, it will not create a new one.

The 'startSummaryTimer' function is used to start a timer for a specific 'Summary' instance. It throws an error if the 'Summary' instance does not exist in the 'summaries' object.

<SwmSnippet path="/src/server/metrics/summaries.js" line="21">

---

## Creating a Summary

The `createSummary` function is used to create a new Summary instance. It takes an options object as a parameter, which should include a unique name for the summary. If a summary with the same name already exists, the function will simply return, avoiding duplicate summaries.

```javascript
function createSummary(opts) {
  const { name } = opts;
  if (summaries[name]) {
    return;
  }
  summaries[name] = new Summary(opts);
}
```

---

</SwmSnippet>

<SwmSnippet path="/src/server/metrics/summaries.js" line="29">

---

## Starting a Summary Timer

The `startSummaryTimer` function is used to start the timer for a specific summary. It takes the name of the summary and additional arguments. If the summary with the provided name does not exist, it throws an error.

```javascript
function startSummaryTimer(name, ...args) {
  if (!summaries[name]) {
    throw new Error(`unable to find summary ${name}, please create it first`);
  }
  return summaries[name].startTimer(...args);
}
```

---

</SwmSnippet>

<SwmSnippet path="/src/server/metrics/index.js" line="36">

---

## Using Summaries in Other Modules

The `startSummaryTimer` function is exported and can be used in other modules. Here it is imported in the `index.js` file of the metrics module.

```javascript
  startSummaryTimer,
```

---

</SwmSnippet>

# Summaries Functions

This section covers the functions related to 'summaries'.

<SwmSnippet path="/src/server/metrics/summaries.js" line="21">

---

## createSummary Function

The 'createSummary' function is used to create a new summary. It takes an 'opts' object as a parameter, which contains the 'name' of the summary. If a summary with the same name already exists, the function does nothing. Otherwise, it creates a new summary with the given 'opts' and stores it in the 'summaries' object.

```javascript
function createSummary(opts) {
  const { name } = opts;
  if (summaries[name]) {
    return;
  }
  summaries[name] = new Summary(opts);
}
```

---

</SwmSnippet>

<SwmSnippet path="/src/server/metrics/summaries.js" line="29">

---

## startSummaryTimer Function

The 'startSummaryTimer' function is used to start a timer for a specific summary. It takes the 'name' of the summary and additional arguments. If the summary does not exist, it throws an error. Otherwise, it starts the timer for the specified summary.

```javascript
function startSummaryTimer(name, ...args) {
  if (!summaries[name]) {
    throw new Error(`unable to find summary ${name}, please create it first`);
  }
  return summaries[name].startTimer(...args);
}
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1vbmUtYXBwJTNBJTNBZ2lsYWRuYXZvdA==" repo-name="DEMO-one-app" doc-type="overview"><sup>Powered by [Swimm](/)</sup></SwmMeta>
