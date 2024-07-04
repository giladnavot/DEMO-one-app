---
title: Understanding index.js
---
In the context of the DEMO-one-app repository, `index.js` is a crucial entry point file for various modules and server configurations. It's where the main server logic resides, including server start-up, server listening, and error handling. It also sets up the development and production environments and initiates module-map polling.

<SwmSnippet path="/src/server/index.js" line="71">

---

This section of the `index.js` file in the `src/server` directory outlines the server start-up process. It sets up the server for both development and production environments, initiates module-map polling, and handles any errors that occur during server start-up.

```javascript
/*    🚀 STARTERS 🚀    */

async function ssrServerStart() {
  // need to load _some_ locale so that react-intl does not prevent modules from loading
  Intl.__addLocaleData(enData);

  await loadModules();

  const isHttps = !!process.env.HTTPS_PORT;
  const port = isHttps ? process.env.HTTPS_PORT : process.env.HTTP_PORT || 3000;

  await listen({
    context: '🌎 One App server',
    instance: await ssrServer({
      https: isHttps ? getHttpsConfig() : null,
    }),
    host: process.env.IP_ADDRESS,
    port,
  });

  await pollModuleMap();
```

---

</SwmSnippet>

<SwmSnippet path="/__tests__/server/index.spec.js" line="37">

---

The `index.spec.js` file in the `__tests__/server` directory contains tests for the `index.js` file. It ensures that the server starts correctly in different environments and handles errors as expected.

```javascript
describe('server index', () => {
  const origFsExistsSync = fs.existsSync;

  let addServer;
  let shutdown;
  let ssrServer;
  let ssrServerListen;
  let devHolocronCDNListen;
  let metricsServerListen;

  async function load({
    ssrServerError,
    metricsServerError,
    getModuleImplementation = (/* name */) => (/* props */) => 'a react component',
    getModulesImplementation = new ImmutableMap({}),
    devHolocronCdnError,
    oneAppDevProxyError,
  } = {}) {
    process.argv = [
      '',
      '',
```

---

</SwmSnippet>

<SwmSnippet path="/src/server/metrics/index.js" line="26">

---

# `index.js` in `src/server/metrics`

In this `index.js` file, several functions and objects related to metrics are exported. These exports can be used in other parts of the application to increment counters, set gauges, start timers, and access metrics related to holocron, app version, international cache, and server-side rendering.

```javascript
export {
  // counters
  incrementCounter,

  // gauges
  incrementGauge,
  setGauge,
  resetGauge,

  // summaries
  startSummaryTimer,

  // metrics
  holocron,
  appVersion,
  intlCache,
  ssr,
};
```

---

</SwmSnippet>

<SwmSnippet path="/src/universal/index.js" line="17">

---

# `index.js` in `src/universal`

In this `index.js` file, the `createRoutes` function and `reducers` object are exported. These exports can be used in other parts of the application to create routes and manage state.

```javascript
import reducers from './reducers';
import createRoutes from './routes';

export default {
  createRoutes,
  reducers,
};
```

---

</SwmSnippet>

<SwmSnippet path="/src/server/plugins/reactHtml/index.jsx" line="447">

---

# `index.js` in `src/server/plugins/reactHtml`

In this `index.js` file, an HTML template is defined and sent as a response. This template is used to render the application's HTML structure.

```javascript
      request,
    };

    const html = `
      <!DOCTYPE html>
      <html${getHtmlAttributesString(helmetInfo)}>
        ${getHead(headSectionArgs)}
        ${getBody(bodySectionArgs)}
      </html>
    `;

    return reply.header('content-type', 'text/html; charset=utf-8').send(html);
  } catch (err) {
    span.recordException(err);
    request.log.error('sendHtml had an error, sending static error page', err);
    return renderStaticErrorPage(request, reply);
  } finally {
    span.end();
  }
};
```

---

</SwmSnippet>

# Index.js Functions

This section covers the main functions in the index.js files of the server and universal directories.

<SwmSnippet path="/src/server/index.js" line="147">

---

## Server Index Functions

This section of the server index.js file contains the main server initialization logic. It sets up the server depending on the environment (development or production), starts the server, and handles any errors that occur during startup.

```javascript

// INIT

const serverChain = process.env.NODE_ENV === 'development'
  ? Promise.resolve()
    .then(devHolocronCDNStart)
    .then(oneAppDevProxyStart)
    .then(appServersStart)
    .then(require('./utils/watchLocalModules').default)
  : appServersStart();

export default serverChain.catch((err) => {
  console.error(err);
  if (process.env.OTEL_EXPORTER_OTLP_LOGS_ENDPOINT) {
    process.stderr.write(util.format('\none-app failed to start. Logs are being sent to OTel via gRPC at %s\n\n%s', process.env.OTEL_EXPORTER_OTLP_LOGS_ENDPOINT, err.stack));
  }
  shutdown();
});
```

---

</SwmSnippet>

<SwmSnippet path="/src/universal/index.js" line="17">

---

## Universal Index Functions

The universal index.js file exports the createRoutes and reducers functions. These functions are used to set up the routing and state management for the universal code.

```javascript
import reducers from './reducers';
import createRoutes from './routes';

export default {
  createRoutes,
  reducers,
};
```

---

</SwmSnippet>

# Endpoints Overview

Understanding the Endpoints

<SwmSnippet path="/src/server/ssrServer.js" line="100">

---

## /\_/status Endpoint

The `/_/status` endpoint is a simple health check endpoint. When a GET request is made to this endpoint, it responds with a 200 status code and sends 'OK' as the response body. This is typically used for monitoring the health of the application.

```javascript
  fastify.get('/_/status', (_request, reply) => reply.status(200).send('OK'));
```

---

</SwmSnippet>

<SwmSnippet path="/src/server/ssrServer.js" line="101">

---

## /\_/pwa/service-worker.js Endpoint

The `/_/pwa/service-worker.js` endpoint is used to serve the service worker file for Progressive Web Apps (PWA). When a GET request is made to this endpoint, the `serviceWorkerHandler` function is called to handle the request.

```javascript
  fastify.get('/_/pwa/service-worker.js', serviceWorkerHandler);
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1vbmUtYXBwJTNBJTNBZ2lsYWRuYXZvdA==" repo-name="DEMO-one-app" doc-type="overview"><sup>Powered by [Swimm](/)</sup></SwmMeta>
