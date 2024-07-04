---
title: Exploring Service Worker Core
---
Service Worker Core in DEMO-one-app refers to the core functionalities of service workers in the application. It includes the initialization of service workers, handling of service worker events, and the management of service worker clients. The core functionality is primarily handled by the `initializeServiceWorker` function, which checks for service worker support, manages registration, and handles different operational modes. The `serviceWorkerClient` function is responsible for listening to messages from the service worker and managing the registration of the service worker. The service worker core also includes utility functions and middleware for handling service worker lifecycle events and fetch events.

<SwmSnippet path="/src/client/service-worker/index.js" line="34">

---

# Service Worker Registration

The `initializeServiceWorker` function is responsible for registering the service worker. It checks if the service worker is enabled and if the browser supports it. If the service worker is not enabled or the browser doesn't support it, the function returns. If the service worker is enabled, it calls the `importServiceWorkerClient` function to register the service worker.

```javascript
export function initializeServiceWorker({
  serviceWorker,
  recoveryMode,
  scope,
  scriptUrl,
  webManifestUrl,
  offlineUrl,
  onError,
}) {
  // If the service worker is unavailable, we would not need
  // to call in the chunk since it is not supported in the given browser.
  if ('serviceWorker' in navigator === false) return Promise.resolve();

  // Before we load in the pwa chunk, we can make a few checks to avoid loading it, if not needed.
  return navigator.serviceWorker.getRegistration()
    .then((registration) => {
      // When the service Worker is not enabled (default)
      if (!serviceWorker) {
        // If by any chance a service worker is present, we clear the cache and remove it.
        if (registration) {
          return registration.unregister().then(clearCache).then(() => registration);
```

---

</SwmSnippet>

<SwmSnippet path="/src/client/service-worker/index.js" line="17">

---

# Service Worker Client Import

The `importServiceWorkerClient` function imports the service worker client and invokes it with the provided settings. This function is called during the registration of the service worker.

```javascript
export function importServiceWorkerClient(settings) {
  return import(/* webpackChunkName: "service-worker-client" */ './client')
    // get the default export and invoke with settings
    .then(({ default: serviceWorkerClient }) => serviceWorkerClient(settings));
}
```

---

</SwmSnippet>

<SwmSnippet path="/src/client/service-worker/client.js" line="27">

---

# Service Worker Client

The `serviceWorkerClient` function sets up listeners for messages from the service worker and for the 'register' event. It preloads the offline shell and manifest into the cache and registers the service worker.

```javascript
export default function serviceWorkerClient({
  scope,
  scriptUrl,
  webManifestUrl,
  offlineUrl,
  onError,
}) {
  // We listen for any messages that come in from the service worker
  on('message', [
    messageContext(),
    messenger({
      [ERROR_MESSAGE_ID_KEY]: onError,
    }),
  ]);

  on('register', () => {
    // to preload the the offline shell and manifest into the cache,
    // we only need to fetch them if they are missing since the
    // service worker will cache them
    match(offlineUrl).then((response) => {
      if (!response) fetch(offlineUrl);
```

---

</SwmSnippet>

<SwmSnippet path="/src/client/service-worker/worker.js" line="26">

---

# Service Worker Lifecycle Events

The service worker listens for 'install', 'activate', and 'fetch' events and executes the corresponding middleware functions. In case of any error during setup, it posts a message to the main thread and unregisters itself.

```javascript
try {
  on('install', createInstallMiddleware());
  on('activate', createActivateMiddleware());
  on('fetch', createFetchMiddleware());
} catch (error) {
  // due to the script body being able to terminate and cut off any
  // asynchronous behavior before it finishes executing, we need to rely on
  // synchronous calls to the main thread like a postMessage
  // or rely on invoking a global or 'error' event handler to triage.
  self.postMessage({ id: ERROR_MESSAGE_ID_KEY, error });
  // in the event of any failure during setup, we immediately
  // unregister this service worker.
  self.unregister();
}
```

---

</SwmSnippet>

# Service Worker Core Functions

This section provides an overview of the main functions in the Service Worker Core.

## Service Worker Events

The Service Worker Core handles three main events: `install`, `activate`, and `fetch`. The `install` event initializes the service worker and skips waiting during service worker installation. The `activate` event claims all the open window clients when the service worker is activated. The `fetch` event supports caching One App and Holocron module resources in tandem to supporting offline navigation.

<SwmSnippet path="/src/client/service-worker/index.js" line="17">

---

## importServiceWorkerClient

The `importServiceWorkerClient` function is used to import the service worker client. It gets the default export and invokes it with settings.

```javascript
export function importServiceWorkerClient(settings) {
  return import(/* webpackChunkName: "service-worker-client" */ './client')
    // get the default export and invoke with settings
    .then(({ default: serviceWorkerClient }) => serviceWorkerClient(settings));
}
```

---

</SwmSnippet>

<SwmSnippet path="/src/client/service-worker/index.js" line="34">

---

## initializeServiceWorker

The `initializeServiceWorker` function is responsible for setting up the service worker. It checks if the service worker is available and enabled, and if so, it updates the registration and clears the cache. If the service worker is not enabled, it unregisters the service worker and clears the cache. In normal operations, it loads up the library and integrates with the service worker.

```javascript
export function initializeServiceWorker({
  serviceWorker,
  recoveryMode,
  scope,
  scriptUrl,
  webManifestUrl,
  offlineUrl,
  onError,
}) {
  // If the service worker is unavailable, we would not need
  // to call in the chunk since it is not supported in the given browser.
  if ('serviceWorker' in navigator === false) return Promise.resolve();

  // Before we load in the pwa chunk, we can make a few checks to avoid loading it, if not needed.
  return navigator.serviceWorker.getRegistration()
    .then((registration) => {
      // When the service Worker is not enabled (default)
      if (!serviceWorker) {
        // If by any chance a service worker is present, we clear the cache and remove it.
        if (registration) {
          return registration.unregister().then(clearCache).then(() => registration);
```

---

</SwmSnippet>

<SwmSnippet path="/src/client/service-worker/client.js" line="27">

---

## serviceWorkerClient

The `serviceWorkerClient` function listens for any messages that come in from the service worker and handles the registration of the service worker. It also preloads the offline shell and manifest into the cache.

```javascript
export default function serviceWorkerClient({
  scope,
  scriptUrl,
  webManifestUrl,
  offlineUrl,
  onError,
}) {
  // We listen for any messages that come in from the service worker
  on('message', [
    messageContext(),
    messenger({
      [ERROR_MESSAGE_ID_KEY]: onError,
    }),
  ]);

  on('register', () => {
    // to preload the the offline shell and manifest into the cache,
    // we only need to fetch them if they are missing since the
    // service worker will cache them
    match(offlineUrl).then((response) => {
      if (!response) fetch(offlineUrl);
```

---

</SwmSnippet>

# Service Worker Core Events

Service Worker Core Events

<SwmSnippet path="/src/client/service-worker/events/fetch.js" line="33">

---

## Fetch Event

The 'fetch' event is triggered whenever a network request is made. The service worker responds to this event by checking if the requested resource is in the cache. If it is, the cached resource is returned, otherwise, the resource is fetched from the network, cached for future use, and then returned. The 'fetch' event also handles caching of One App and Holocron module resources, which supports offline navigation.

```javascript
function createAppCachingMiddleware(oneAppVersion) {
  return function appResourceCachingMiddleware(event, context) {
    if (event.request.url.includes(oneAppVersion)) {
      const [baseAppUrl] = event.request.url.split(oneAppVersion);
      const meta = createResourceMetaData(
        event,
        ['app', `${baseAppUrl}${oneAppVersion}/`]
      );
      context.set('cacheName', meta.cacheName);
      context.set('request', event.request.clone());
      event.respondWith(fetchCacheResource(event, meta));
    }
  };
}

function createHolocronCachingMiddleware(holocronModuleMap) {
  const { clientCacheRevision, modules } = holocronModuleMap;
  const moduleNames = Object.keys(modules);
  const moduleEntries = moduleNames.map((name) => [name, modules[name].baseUrl]);

  return function holocronCachingMiddleware(event, context) {
```

---

</SwmSnippet>

## Install Event

The 'install' event is triggered when the service worker is being installed. This event is used to initialize the service worker and to ensure that the service worker takes control of the page as soon as it's installed, by calling `skipWaiting`.

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1vbmUtYXBwJTNBJTNBZ2lsYWRuYXZvdA==" repo-name="DEMO-one-app" doc-type="overview"><sup>Powered by [Swimm](/)</sup></SwmMeta>
