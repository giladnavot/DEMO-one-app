---
title: Overview of Service Worker
---
A Service Worker in this repository is a script that the browser runs in the background, separate from the web page, opening the door to features that don't need a web page or user interaction. It's used for features like push notifications and background sync. In the context of this repository, the Service Worker is used to handle network requests and cache resources.

<SwmSnippet path="/src/client/service-worker/worker.js" line="26">

---

In this part of the code, the Service Worker is listening for 'install', 'activate', and 'fetch' events. If an error occurs during these events, the Service Worker sends a message to the main thread and unregisters itself.

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

<SwmSnippet path="/src/client/service-worker/index.js" line="34">

---

This function initializes the Service Worker. It checks if the Service Worker is enabled and if it's not, it clears the cache and removes the Service Worker. If the Service Worker is enabled, it checks if it's in recovery mode and updates the Service Worker if it is. If it's not in recovery mode, it loads the library and integrates with the Service Worker.

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

This function is the client of the Service Worker. It listens for messages from the Service Worker and registers the Service Worker.

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

<SwmSnippet path="/src/client/service-worker/index.js" line="34">

---

# Service Worker Registration

The `initializeServiceWorker` function is used to register and initialize the Service Worker. It checks if the Service Worker is enabled and if it is, it either clears the cache and removes the Service Worker if it is already registered or loads the library and integrates with the Service Worker if it is not.

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

# Service Worker Client

The `serviceWorkerClient` function is used to listen for messages from the Service Worker and handle registration of the Service Worker. It preloads the offline shell and manifest into the cache if they are missing since the Service Worker will cache them.

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

# Service Worker Events

The Service Worker listens for 'install', 'activate', and 'fetch' events and handles them using middleware functions. If there is any failure during setup, the Service Worker is immediately unregistered.

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

# Service Worker Endpoints

Service Worker Events

<SwmSnippet path="/src/client/service-worker/events/fetch.js" line="33">

---

## Fetch Event

The 'fetch' event is triggered whenever a network request is made. The Service Worker responds to this event by checking if the requested resource is available in the cache. If it is, the resource is served from the cache. If not, a network request is made to fetch the resource. The 'fetch' event also handles caching of One App and Holocron module resources, supporting offline navigation.

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

The 'install' event is triggered when the Service Worker is installed. This event is used to set up the Service Worker and prepare it for handling future requests. During the 'install' event, the Service Worker 'skips waiting', meaning it moves to the 'activate' phase without waiting for existing Service Workers to become redundant.

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1vbmUtYXBwJTNBJTNBZ2lsYWRuYXZvdA==" repo-name="DEMO-one-app" doc-type="overview"><sup>Powered by [Swimm](/)</sup></SwmMeta>
