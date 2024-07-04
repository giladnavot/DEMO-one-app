---
title: Exploring the Server
---
In the DEMO-one-app repository, the server is a crucial component that serves up a single page app built using React components. It is a Node.js server that makes use of Holocron for code splitting. The server is responsible for handling requests, serving static files, and rendering the React components. It also includes a metrics server for monitoring performance and a development server for local development. The server's functionality is spread across multiple files, including `server.js`, `ssrServer.js`, `metricsServer.js`, and others.

<SwmSnippet path="/src/server/index.js" line="73">

---

# Server Setup

This is where the server is set up and started. The server is configured to listen on a specific port, and various server-side configurations are set up here.

```javascript
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

  if (process.env.OTEL_EXPORTER_OTLP_LOGS_ENDPOINT) {
```

---

</SwmSnippet>

<SwmSnippet path="/src/server/utils/stateConfig.js" line="125">

---

# Server-side Configurations

This is where server-side configurations are handled. The server configurations are set based on the environment and are used to manage the state of the app.

```javascript
    .reduce((acc, [key, { client, server }]) => {
      if (client) {
        if (typeof client === 'object') {
          if (!configEnv) {
            throw makeConfigEnvError();
          }
          acc.client[key] = client[configEnv];
        } else {
          acc.client[key] = client;
        }
      }
      if (server) {
        if (typeof server === 'object') {
          if (!configEnv) {
            throw makeConfigEnvError();
          }
          acc.server[key] = server[configEnv];
        } else {
          acc.server[key] = server;
        }
```

---

</SwmSnippet>

<SwmSnippet path="/prod-sample/api/server.js" line="23">

---

# Mock Server for Testing

This is where a mock server is created for testing purposes. The server is created using the jsonServer module and is used to simulate server-side operations during testing.

```javascript
const server = jsonServer.create();
```

---

</SwmSnippet>

# Server Endpoints

Server Endpoints

<SwmSnippet path="/src/server/ssrServer.js" line="100">

---

## /\_/status Endpoint

The '/\_/status' endpoint is a health check endpoint for the One App server. When a GET request is made to this endpoint, the server responds with a 200 status code and 'OK' message. This endpoint is useful for monitoring the health and availability of the server.

```javascript
  fastify.get('/_/status', (_request, reply) => reply.status(200).send('OK'));
```

---

</SwmSnippet>

<SwmSnippet path="/src/server/ssrServer.js" line="101">

---

## /\_/pwa/service-worker.js Endpoint

The '/\_/pwa/service-worker.js' endpoint serves the service worker script for the Progressive Web App (PWA). When a GET request is made to this endpoint, the 'serviceWorkerHandler' function is called to handle the request. This function checks if the service worker is enabled in the server's PWA configuration. If it is, it sends the service worker script with the appropriate headers. If not, it responds with a 404 status code and 'Not found' message.

```javascript
  fastify.get('/_/pwa/service-worker.js', serviceWorkerHandler);
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1vbmUtYXBwJTNBJTNBZ2lsYWRuYXZvdA==" repo-name="DEMO-one-app" doc-type="overview"><sup>Powered by [Swimm](/)</sup></SwmMeta>
