---
title: Understanding index.js
---
In the context of the DEMO-one-app repository, `index.js` is a common entry point file for various parts of the application. It is typically where modules are exported for use in other parts of the application. For instance, in `src/server/index.js`, the server setup and initialization happens, and the server chain is exported for use elsewhere. Similarly, in `src/universal/index.js`, the application's routes and reducers are exported for use in the universal application. In the context of test files, `index.spec.js` is often used to group and run related test suites.

<SwmSnippet path="/src/server/metrics/index.js" line="1">

---

# `index.js` in `src/server/metrics`

In this `index.js` file, several functions and modules related to metrics are exported. These include functions for incrementing counters and gauges, starting summary timers, and modules for holocron, app version, international cache, and server-side rendering. These can be imported in other parts of the application where metrics functionality is needed.

```javascript
/*
 * Copyright 2019 American Express Travel Related Services Company, Inc.
 *
 * Licensed under the Apache License, Version 2.0 (the "License");
 * you may not use this file except in compliance with the License.
 * You may obtain a copy of the License at
 *
 * http://www.apache.org/licenses/LICENSE-2.0
 *
 * Unless required by applicable law or agreed to in writing, software
 * distributed under the License is distributed on an "AS IS" BASIS,
 * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express
 * or implied. See the License for the specific language governing
 * permissions and limitations under the License.
 */

import { incrementCounter } from './counters';
import { incrementGauge, setGauge, resetGauge } from './gauges';
import { startSummaryTimer } from './summaries';

import holocron from './holocron';
```

---

</SwmSnippet>

<SwmSnippet path="/src/universal/index.js" line="1">

---

# `index.js` in `src/universal`

In this `index.js` file, the `reducers` and `createRoutes` modules are exported. These are core parts of the application's state management and routing functionality, and can be imported wherever they are needed in the application.

```javascript
/*
 * Copyright 2019 American Express Travel Related Services Company, Inc.
 *
 * Licensed under the Apache License, Version 2.0 (the "License");
 * you may not use this file except in compliance with the License.
 * You may obtain a copy of the License at
 *
 * http://www.apache.org/licenses/LICENSE-2.0
 *
 * Unless required by applicable law or agreed to in writing, software
 * distributed under the License is distributed on an "AS IS" BASIS,
 * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express
 * or implied. See the License for the specific language governing
 * permissions and limitations under the License.
 */

import reducers from './reducers';
import createRoutes from './routes';

export default {
  createRoutes,
```

---

</SwmSnippet>

<SwmSnippet path="/src/server/plugins/reactHtml/index.jsx" line="447">

---

# `index.js` in `src/server/plugins/reactHtml`

In this `index.js` file, an HTML template for the application is defined and sent as a response. This includes setting the HTML attributes, head, and body of the application's HTML document.

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

The server index.js file contains functions for starting the server, including the ssrServerStart, metricsServerStart, devHolocronCDNStart, and oneAppDevProxyStart functions. These functions are responsible for starting the server-side rendering server, the metrics server, the development Holocron CDN, and the One App development proxy respectively.

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

The universal index.js file exports the createRoutes and reducers functions. These functions are used to create the routes for the application and define the reducers for the application state.

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

# Endpoints Explanation

Endpoint Overview

<SwmSnippet path="/src/server/ssrServer.js" line="100">

---

## /\_/status Endpoint

The `/_/status` endpoint is a simple health check endpoint. When a GET request is made to this endpoint, the server responds with a 200 status code and a body of 'OK'. This can be used to quickly check if the server is running and able to respond to requests.

```javascript
  fastify.get('/_/status', (_request, reply) => reply.status(200).send('OK'));
```

---

</SwmSnippet>

<SwmSnippet path="/src/server/ssrServer.js" line="101">

---

## /\_/pwa/service-worker.js Endpoint

The `/_/pwa/service-worker.js` endpoint is used to serve the service worker file for Progressive Web Apps (PWA). When a GET request is made to this endpoint, the `serviceWorkerHandler` function is called to handle the request. This function checks if the service worker is enabled and if so, it responds with the service worker script. If not, it responds with a 404 status code.

```javascript
  fastify.get('/_/pwa/service-worker.js', serviceWorkerHandler);
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1vbmUtYXBwJTNBJTNBZ2lsYWRuYXZvdA==" repo-name="DEMO-one-app" doc-type="overview"><sup>Powered by [Swimm](/)</sup></SwmMeta>
