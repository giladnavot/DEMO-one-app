---
title: Overview of Service Worker Events
---
Service Worker Events in the DEMO-one-app are used to manage the lifecycle of service workers and handle fetch events. They are crucial for caching and retrieving resources, and for maintaining the offline functionality of the application.

The lifecycle events, such as 'install' and 'activate', are managed through middleware functions. These functions are exported from the 'lifecycle.js' file and are used to control when the service worker takes control of the page.

Fetch events are handled in the 'fetch.js' file. This file contains middleware functions that manage caching of the application shell and Holocron modules. These functions intercept network requests and serve the cached responses when available.

The 'utility.js' file contains helper functions that are used across the service worker events. These functions handle tasks such as creating metadata for resources, invalidating outdated cache entries, and fetching resources from the cache.

<SwmSnippet path="/src/client/service-worker/events/lifecycle.js" line="19">

---

# Lifecycle Events

The `createInstallMiddleware` and `createActivateMiddleware` functions handle the `install` and `activate` lifecycle events respectively. These events are crucial for the setup and activation of the service worker.

```javascript
export function createInstallMiddleware() {
  // `install` lifecycle event
  return skipWaiting();
}

export function createActivateMiddleware() {
  // `activate` lifecycle event
  return clientsClaim();
}
```

---

</SwmSnippet>

<SwmSnippet path="/src/client/service-worker/events/fetch.js" line="33">

---

# Fetch Event

The `createFetchMiddleware` function handles fetch events. It uses the `appShell`, `cacheRouter`, `createHolocronCachingMiddleware`, `createAppCachingMiddleware`, and `expiration` middleware to manage caching and network strategies.

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

<SwmSnippet path="/src/client/service-worker/events/utility.js" line="29">

---

# Utility Functions

The utility functions are used to manage resource metadata, mark resources for removal, invalidate cache resources, set cache resources, and fetch cache resources. These functions are used within the middleware functions to manage the caching of resources.

```javascript
export function getOneAppVersion() {
  return process.env.ONE_APP_BUILD_VERSION;
}

export function getHolocronModuleMap() {
  return JSON.parse(process.env.HOLOCRON_MODULE_MAP);
}

export function createResourceMetaData(event, resourceInfo) {
  const [name, baseUrl, revision] = resourceInfo;
  const [version] = baseUrl.replace(/\/$/, '').split('/').reverse();
  const { request } = event;

  let type = name === 'app' ? 'one-app' : 'modules';

  let path;
  let locale;
  if (moduleLocaleRegExp.test(request.url)) {
    type = 'lang-packs';
    [path, locale] = request.url.match(moduleLocaleRegExp);
  } else if (appLocaleRegExp.test(request.url)) {
```

---

</SwmSnippet>

# Service Worker Events

This section will cover the main functions of Service Worker Events: 'install', 'activate', and 'fetch'.

## Install Event

The 'install' event initializes the service worker and skips waiting during service worker installation. This is crucial for ensuring that the service worker is ready to control clients and handle functional events like 'fetch' and 'push' as soon as it's installed.

## Activate Event

The 'activate' event claims all the open window clients when the service worker is activated. This is important for ensuring that the latest service worker gets immediately controlled over all clients, which can be particularly useful when you need to apply updates to the service worker or any resources it serves.

## Fetch Event

The 'fetch' event supports caching One App and Holocron module resources in tandem to supporting offline navigation. This is essential for improving performance by only downloading resources that are necessary, and for providing offline functionality.

# Service Worker Events

Service Worker Events

<SwmSnippet path="/src/client/service-worker/events/fetch.js" line="33">

---

## Fetch Event

The 'fetch' event is used for caching One App and Holocron module resources, which supports offline navigation. It uses pseudo environment variables 'ONE_APP_BUILD_VERSION' and 'HOLOCRON_MODULE_MAP' to handle the caching process.

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

The 'install' event is used to initialize the service worker and skip waiting during service worker installation.

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1vbmUtYXBwJTNBJTNBZ2lsYWRuYXZvdA==" repo-name="DEMO-one-app" doc-type="overview"><sup>Powered by [Swimm](/)</sup></SwmMeta>
