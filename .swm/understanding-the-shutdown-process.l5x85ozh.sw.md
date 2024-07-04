---
title: Understanding the Shutdown Process
---
Shutdown in this codebase refers to the process of gracefully stopping the server. It is implemented as a function that sets a flag `shouldHaveShutDown` to true and then iterates over all servers, invoking their `close` method. If the server does not shut down within a specified timeout, a forcible shutdown is triggered, which immediately stops the Node.js process.

<SwmSnippet path="/src/server/shutdown.js" line="23">

---

# Shutdown Function

This is the definition of the 'shutdown' function. It sets the 'shouldHaveShutDown' flag to true, logs a message to the console, and then iterates over the 'servers' array to close each server.

```javascript
function shutdown() {
  shouldHaveShutDown = true;
  console.log('shutting down, closing servers');
  servers.forEach((s) => s.close());
}
```

---

</SwmSnippet>

<SwmSnippet path="/src/server/shutdown.js" line="36">

---

# Shutdown Trigger

This block of code sets up event listeners for the 'SIGINT' and 'SIGTERM' signals, which are typically sent when the application is being terminated. When one of these signals is received, the 'shutdown' function is called.

```javascript
[
  'SIGINT',
  'SIGTERM',
]
  .forEach((signalName) => process.on(signalName, () => {
    if (shouldHaveShutDown) {
      // second signal, something is keeping node up
      console.log('received %s, forcibly shutting down', signalName);
      shutdownForcibly();
      return;
    }

    console.log('received %s, shutting down', signalName);
    shutdown();
    // `shouldHaveShutDown` is now `true`, BUT
    // we never get additional signals when running in babel-node
    // https://github.com/babel/babel/issues/1062
    // at least, until https://github.com/babel/babel/pull/7902 is published
    const forcibleRef = setTimeout(() => {
      console.error('still running after %ds, forcibly shutting down', FORCIBLE_SHUTDOWN_TIMEOUT / 1e3);
      shutdownForcibly();
```

---

</SwmSnippet>

<SwmSnippet path="/__tests__/server/shutdown.spec.js" line="19">

---

# Shutdown Tests

These are the tests for the 'shutdown' function. They verify that the function correctly closes the servers and logs the appropriate messages to the console.

```javascript
describe('shutdown', () => {
  jest.spyOn(global, 'setTimeout').mockImplementation(() => {});
  jest.spyOn(global, 'setImmediate').mockImplementation(() => {});
  jest.spyOn(process, 'exit').mockImplementation(() => {});
  jest.spyOn(console, 'log').mockImplementation(util.format);
  jest.spyOn(console, 'error').mockImplementation(util.format);

  const signalNames = [
    'SIGINT',
    'SIGTERM',
  ];

  function load() {
    jest.resetModules();
    return require('../../src/server/shutdown');
  }

  beforeEach(() => {
    signalNames.forEach((signalName) => process.removeAllListeners(signalName));
  });

```

---

</SwmSnippet>

# Shutdown functions

Shutdown functions

<SwmSnippet path="/src/server/shutdown.js" line="21">

---

## addServer

The `addServer` function is used to add a server to the list of servers. It takes a server as an argument and adds it to the beginning of the servers array.

```javascript
function addServer(s) { servers.unshift(s); }
```

---

</SwmSnippet>

<SwmSnippet path="/src/server/shutdown.js" line="23">

---

## shutdown

The `shutdown` function is used to initiate the shutdown process. It sets the `shouldHaveShutDown` flag to true and then iterates over the servers array, calling the `close` method on each server.

```javascript
function shutdown() {
  shouldHaveShutDown = true;
  console.log('shutting down, closing servers');
  servers.forEach((s) => s.close());
}
```

---

</SwmSnippet>

<SwmSnippet path="/src/server/shutdown.js" line="29">

---

## shutdownForcibly

The `shutdownForcibly` function is used to forcibly shut down the application. It logs an error message and then immediately exits the process with a status code of 1.

```javascript
function shutdownForcibly() {
  console.error('shutting down, forcibly stopping node');
  // this is a forcible shutdown due to a babel-node bug of not forwarding the signal
  // eslint-disable-next-line unicorn/no-process-exit -- see above
  setImmediate(() => process.exit(1));
}
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1vbmUtYXBwJTNBJTNBZ2lsYWRuYXZvdA==" repo-name="DEMO-one-app" doc-type="overview"><sup>Powered by [Swimm](/)</sup></SwmMeta>
