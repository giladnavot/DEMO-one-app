---
title: Overview of Counters
---
Counters in this codebase are instances of the Counter class imported from the 'prom-client' library. They are used to track and increment numerical metrics within the application.

The 'counters' object is a collection of these Counter instances, indexed by name. This allows for the creation and retrieval of counters by their unique names.

The 'createCounter' function is used to create a new Counter instance and add it to the 'counters' object. If a counter with the given name already exists, the function simply returns.

The 'incrementCounter' function is used to increment the value of a specific counter. If the counter does not exist, an error is thrown.

<SwmSnippet path="/src/server/metrics/counters.js" line="21">

---

# Creating a Counter

The `createCounter` function is used to create a new counter. It takes an `opts` object as a parameter, which must contain a unique `name` property. If a counter with the same name already exists, the function will simply return.

```javascript
function createCounter(opts) {
  const { name } = opts;
  if (counters[name]) {
    return;
  }
  counters[name] = new Counter(opts);
}
```

---

</SwmSnippet>

<SwmSnippet path="/src/server/metrics/counters.js" line="29">

---

# Incrementing a Counter

The `incrementCounter` function is used to increment a counter. It takes the `name` of the counter and an optional `args` parameter. If the counter does not exist, it throws an error.

```javascript
function incrementCounter(name, ...args) {
  if (!counters[name]) {
    throw new Error(`unable to find counter ${name}, please create it first`);
  }
  counters[name].inc(...args);
}
```

---

</SwmSnippet>

# Counter Functions

This section discusses the main functions related to counters in the 'counters.js' file.

<SwmSnippet path="/src/server/metrics/counters.js" line="21">

---

## createCounter

The `createCounter` function is used to create a new counter. It takes an 'opts' object as a parameter, which contains the name of the counter. If a counter with the same name already exists, the function does nothing. Otherwise, it creates a new counter with the given options and stores it in the 'counters' object.

```javascript
function createCounter(opts) {
  const { name } = opts;
  if (counters[name]) {
    return;
  }
  counters[name] = new Counter(opts);
}
```

---

</SwmSnippet>

<SwmSnippet path="/src/server/metrics/counters.js" line="29">

---

## incrementCounter

The `incrementCounter` function is used to increment a counter. It takes the name of the counter and an optional list of arguments. If the counter does not exist, it throws an error. Otherwise, it increments the counter.

```javascript
function incrementCounter(name, ...args) {
  if (!counters[name]) {
    throw new Error(`unable to find counter ${name}, please create it first`);
  }
  counters[name].inc(...args);
}
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1vbmUtYXBwJTNBJTNBZ2lsYWRuYXZvdA==" repo-name="DEMO-one-app" doc-type="overview"><sup>Powered by [Swimm](/)</sup></SwmMeta>
