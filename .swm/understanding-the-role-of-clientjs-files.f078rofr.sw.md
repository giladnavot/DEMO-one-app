---
title: Understanding the Role of client.js Files
---
The `client.js` file in the `src/client` directory is the main entry point for the client-side of the application. It imports and initializes the client-side application by calling the `initClient` function from `initClient.js`. This function sets up the global store and kicks off rendering of the React components.

The `client.js` file in the `src/client/service-worker` directory is responsible for setting up the service worker client. It listens for messages from the service worker and handles registration of the service worker.

<SwmSnippet path="/src/client/client.js" line="27">

---

## Initialization of the Client

In `client.js`, the `initClient` function from `initClient.js` is imported and immediately called. This function is responsible for setting up the client-side application.

```javascript
import initClient from './initClient';

initClient();
```

---

</SwmSnippet>

<SwmSnippet path="/src/client/initClient.jsx" line="28">

---

## Setting Up the Client-Side Application

The `initClient` function sets up the global store, loads necessary scripts, and starts the rendering process. It also handles redirects and initializes the service worker.

```javascript
export default async function initClient() {
  try {
    setModuleMap(global.__CLIENT_HOLOCRON_MODULE_MAP__);
    setRequiredExternalsRegistry(global.__HOLOCRON_EXTERNALS__);
    moveHelmetScripts();

    const store = initializeClientStore();
    const history = browserHistory;
    const routes = createRoutes(store);

    await loadPrerenderScripts(store.getState());

    const { redirectLocation, renderProps } = await matchPromise({ history, routes });

    if (redirectLocation) {
      // FIXME: redirectLocation has pathname, query object, etc; need to format the URL better
      // TODO: would `browserHistory.push(redirectLocation);` and render below, but app stalls
      window.location.replace(redirectLocation.pathname);
      return;
    }

```

---

</SwmSnippet>

<SwmSnippet path="/src/client/service-worker/client.js" line="27">

---

## Service Worker Client

The `serviceWorkerClient` function in `service-worker/client.js` sets up the service worker, which is responsible for features like offline capability and resource caching.

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

# Client Initialization Functions

This section will cover the main functions used in the client-side initialization of the application.

<SwmSnippet path="/src/client/initClient.jsx" line="28">

---

## initClient function

The `initClient` function is the main entry point for initializing the client-side application. It sets up the module map, initializes the client store, creates the routes, and loads the service worker. It also handles rendering the application into the DOM and cleaning up server-side rendered stylesheets.

```javascript
export default async function initClient() {
  try {
    setModuleMap(global.__CLIENT_HOLOCRON_MODULE_MAP__);
    setRequiredExternalsRegistry(global.__HOLOCRON_EXTERNALS__);
    moveHelmetScripts();

    const store = initializeClientStore();
    const history = browserHistory;
    const routes = createRoutes(store);

    await loadPrerenderScripts(store.getState());

    const { redirectLocation, renderProps } = await matchPromise({ history, routes });

    if (redirectLocation) {
      // FIXME: redirectLocation has pathname, query object, etc; need to format the URL better
      // TODO: would `browserHistory.push(redirectLocation);` and render below, but app stalls
      window.location.replace(redirectLocation.pathname);
      return;
    }

```

---

</SwmSnippet>

<SwmSnippet path="/src/client/prerender.js" line="46">

---

## moveHelmetScripts function

The `moveHelmetScripts` function is responsible for moving scripts added by react-helmet from the body to the head of the document. This is done to ensure that these scripts are executed as soon as possible.

```javascript
export function moveHelmetScripts() {
  document.addEventListener('DOMContentLoaded', () => {
    const headHelmetScripts = [...document.head.querySelectorAll('script[data-react-helmet]')];
    const bodyHelmetScripts = [...document.body.querySelectorAll('script[data-react-helmet]')];

    bodyHelmetScripts.forEach((script) => script.remove());
    // If the helmet scripts exist in the head then we can assume that the body
    // scripts were already added by react-helmet
    if (headHelmetScripts.length === 0) {
      bodyHelmetScripts.forEach((script) => document.head.append(script));
    }
  });
}
```

---

</SwmSnippet>

<SwmSnippet path="/src/client/prerender.js" line="27">

---

## initializeClientStore function

The `initializeClientStore` function sets up the Redux store for the client-side application. It uses the initial state provided by the server and enhances the store with middleware and enhancers.

```javascript
export function initializeClientStore() {
  // Six second timeout on client
  const enhancedFetch = compose(createTimeoutFetch(6e3))(fetch);

  const enhancer = createEnhancer();
  const initialState = global.__INITIAL_STATE__ !== undefined
    ? transit.fromJSON(global.__INITIAL_STATE__) : undefined;
  const store = createHolocronStore({
    reducer, initialState, enhancer, extraThunkArguments: { fetchClient: enhancedFetch },
  });

  return store;
}
```

---

</SwmSnippet>

<SwmSnippet path="/src/universal/routes.jsx" line="28">

---

## createRoutes function

The `createRoutes` function is responsible for creating the routes for the application based on the configuration of the root module. It also handles 404 routes and dispatches actions to the store when a route is entered or left.

```javascript
const createRoutes = (store) => {
  const rootModuleName = store.getState().getIn(['config', 'rootModuleName']);
  const rootHasChildRoutes = hasChildRoutes(getModule(rootModuleName));

  return [
    <ModuleRoute key="rootModule" moduleName={rootModuleName} store={store} path={rootHasChildRoutes ? undefined : '/'} />,
    <Route
      key="404"
      path="*"
      component={() => 'Not found'}
      onEnter={({ location }) => {
        store.dispatch(applicationError(
          404,
          new Error('404: Not found'),
          { location }
        ));
      }}
      onLeave={() => {
        store.dispatch(clearError());
      }}
    />,
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1vbmUtYXBwJTNBJTNBZ2lsYWRuYXZvdA==" repo-name="DEMO-one-app" doc-type="overview"><sup>Powered by [Swimm](/)</sup></SwmMeta>
