---
title: Overview of Transit
---
Transit is a library used in this project for serializing and deserializing JavaScript data structures. It's particularly useful for handling data that's being sent over the network or stored in a file. The library is used in this project to handle data in a way that maintains its original structure and type information. This is especially important for complex data types that are not natively supported by JSON, such as Error objects and Promises. The Transit library provides a way to define custom handlers for these types, ensuring that they are correctly serialized and deserialized.

<SwmSnippet path="/src/universal/utils/transit.js" line="38">

---

## Defining Handlers

Here we define `extraHandlers` for Error, Promise, and URL data types. Each handler has a `tag` (a string to identify the data type), a `class` (the JavaScript class of the data type), and `write` and `read` functions for serialization and deserialization respectively.

```javascript
const extraHandlers = [
  {
    tag: 'error',
    class: Error,
    write: writeError,
    // eslint-disable-next-line unicorn/error-message -- not applicable for deserialization
    read: (value) => Object.assign(new Error(), { stack: undefined }, value),
  },
  {
    tag: 'promise',
    class: Promise,
    write: () => null,
    read: () => null,
  },
  {
    tag: 'url',
    class: global.BROWSER ? URL : require('node:url').Url,
    write: (value) => value.href,
    read: (value) => (global.BROWSER
      ? new URL(value, global.location.href)
      : require('node:url').parse(value)
```

---

</SwmSnippet>

<SwmSnippet path="/src/universal/utils/transit.js" line="63">

---

## Using Handlers

The handlers are used in the Transit reader and writer. The reader and writer are then used to serialize and deserialize data with the `toJSON` and `fromJSON` functions respectively.

```javascript
const modifiedHandlers = handlers.withExtraHandlers(extraHandlers);

const reader = transit.reader('json', {
  handlers: modifiedHandlers.read,
  cache: false,
  // Taken from transit-immutable-js. Without this, Error fails to read.
  // https://github.com/glenjamin/transit-immutable-js/blob/cb0ac0799d730080ea2403dba4061cf9c9d7b9bd/index.js#L6
  mapBuilder: {
    init() {
      return {};
    },
    add(m, k, v) {
      // eslint-disable-next-line no-param-reassign -- Keeping what is done by transit-immutable-js
      m[k] = v;
      return m;
    },
    finalize(m) {
      return m;
    },
  },
});
```

---

</SwmSnippet>

# Transit Functions Overview

Transit Functions

<SwmSnippet path="/src/universal/utils/transit.js" line="27">

---

## writeError Function

The `writeError` function is used to serialize an error object. It uses the `serializeError` function to convert the error into a plain object, removes the stack trace, and conceals the origin of the error message and response URL.

```javascript
export function writeError(value) {
  const error = serializeError(value);
  delete error.stack;
  error.message = concealOrigin(error.message);
  if (error.response) {
    error.response.url = concealOrigin(error.response.url);
  }

  return error;
}
```

---

</SwmSnippet>

<SwmSnippet path="/src/universal/utils/transit.js" line="38">

---

## extraHandlers Constant

The `extraHandlers` constant is an array of handler objects for different data types (Error, Promise, URL). Each handler object contains a `tag` to identify the data type, a `class` to specify the JavaScript class, and `write` and `read` functions for serialization and deserialization.

```javascript
const extraHandlers = [
  {
    tag: 'error',
    class: Error,
    write: writeError,
    // eslint-disable-next-line unicorn/error-message -- not applicable for deserialization
    read: (value) => Object.assign(new Error(), { stack: undefined }, value),
  },
  {
    tag: 'promise',
    class: Promise,
    write: () => null,
    read: () => null,
  },
  {
    tag: 'url',
    class: global.BROWSER ? URL : require('node:url').Url,
    write: (value) => value.href,
    read: (value) => (global.BROWSER
      ? new URL(value, global.location.href)
      : require('node:url').parse(value)
```

---

</SwmSnippet>

<SwmSnippet path="/src/universal/utils/transit.js" line="65">

---

## reader Constant

The `reader` constant is a Transit reader object configured to read JSON data. It uses the `modifiedHandlers` for deserialization, disables caching, and provides a custom `mapBuilder` to handle the deserialization of map data.

```javascript
const reader = transit.reader('json', {
  handlers: modifiedHandlers.read,
  cache: false,
  // Taken from transit-immutable-js. Without this, Error fails to read.
  // https://github.com/glenjamin/transit-immutable-js/blob/cb0ac0799d730080ea2403dba4061cf9c9d7b9bd/index.js#L6
  mapBuilder: {
    init() {
      return {};
    },
    add(m, k, v) {
      // eslint-disable-next-line no-param-reassign -- Keeping what is done by transit-immutable-js
      m[k] = v;
      return m;
    },
    finalize(m) {
      return m;
    },
  },
});
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1vbmUtYXBwJTNBJTNBZ2lsYWRuYXZvdA==" repo-name="DEMO-one-app" doc-type="overview"><sup>Powered by [Swimm](/)</sup></SwmMeta>
