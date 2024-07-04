---
title: Getting Started with the Client-Side Application
---
In the DEMO-one-app repository, 'client' refers to the client-side part of the application, which is responsible for rendering the user interface and managing the application state on the user's device. It is built using React and Redux, and it interacts with the server-side part of the application to fetch data and handle user actions. The client-side code is organized in a modular way, with each feature encapsulated in a separate module. The main entry point for the client-side application is the 'initClient' function, which sets up the global Redux store and kicks off the rendering process.

<SwmSnippet path="/src/client/client.js" line="27">

---

# Client Initialization

The client-side of the application is initialized by calling the `initClient` function. This function sets up the global store and kicks off rendering.

```javascript
import initClient from './initClient';

initClient();
```

---

</SwmSnippet>

<SwmSnippet path="/src/client/initClient.jsx" line="34">

---

# Client Store Initialization

The `initializeClientStore` function is used to set up the global Redux store for the client-side of the application. This store is used to manage the application state.

```javascript
    const store = initializeClientStore();
```

---

</SwmSnippet>

<SwmSnippet path="/src/client/initClient.jsx" line="64">

---

# Client Rendering

The `App` component is responsible for rendering the user interface of the application. It uses the Redux `Provider` to make the store available to all components in the component tree.

```javascript
    const App = () => (
      <Provider store={store}>
        <Router {...renderProps} />
      </Provider>
    );
```

---

</SwmSnippet>

<SwmSnippet path="/src/client/service-worker/client.js" line="27">

---

# Client Service Worker

The `serviceWorkerClient` function is used to register and set up the service worker for the client-side of the application. The service worker is used for features like offline support and push notifications.

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

# Service Worker Events

Service Worker Events

## Install Event

The `install` event is triggered when the service worker is being installed. It is used to set up the service worker and to cache the necessary resources.

## Fetch Event

The `fetch` event is triggered whenever a network request is made. It is used to handle the request and provide a response. This can involve fetching a resource from the network, retrieving it from the cache, or generating a response directly within the service worker.

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1vbmUtYXBwJTNBJTNBZ2lsYWRuYXZvdA==" repo-name="DEMO-one-app" doc-type="overview"><sup>Powered by [Swimm](/)</sup></SwmMeta>
