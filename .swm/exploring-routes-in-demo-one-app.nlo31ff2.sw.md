---
title: Exploring Routes in DEMO-one-app
---
Routes in the DEMO-one-app are defined using the `createRoutes` function. This function uses the `ModuleRoute` and `Route` components from the `holocron-module-route` and `@americanexpress/one-app-router` libraries respectively. The `createRoutes` function generates an array of routes for the application, including a root module route and a 404 error route. The root module route is determined by the `rootModuleName` from the application's state, and its path is set based on whether it has child routes. The 404 error route is a catch-all route that displays a 'Not found' message for any undefined paths.

<SwmSnippet path="/src/universal/routes.jsx" line="28">

---

## Defining Routes

The `createRoutes` function is used to define the routes for the application. It uses the `ModuleRoute` and `Route` components to define the root route and a catch-all 404 route respectively.

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

<SwmSnippet path="/src/universal/routes.jsx" line="33">

---

## Route Props

The `key` prop is used to uniquely identify the route, the `path` prop defines the URL pattern for the route, and the `component` prop specifies the component to render when the route is matched. The `onEnter` and `onLeave` props are used to specify actions to perform when entering and leaving the route.

```javascript
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

<SwmSnippet path="/src/universal/utils/hasChildRoutes.js" line="17">

---

## Child Routes

The `hasChildRoutes` function is used to check if a module has child routes. This is used in the `createRoutes` function to determine if the root module has child routes.

```javascript
const hasChildRoutes = (module) => module && !!module.childRoutes;
```

---

</SwmSnippet>

# Routing Functions

The `createRoutes` function is a key part of the routing system in the application. It is responsible for creating the routes for the application based on the state of the store.

<SwmSnippet path="/src/universal/routes.jsx" line="28">

---

## createRoutes Function

The `createRoutes` function is responsible for creating the routes for the application. It takes the store as an argument and uses it to determine the root module name and whether it has child routes. It then returns an array of routes, including a `ModuleRoute` for the root module and a `Route` for handling 404 errors.

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

<SwmSnippet path="/src/universal/utils/hasChildRoutes.js" line="17">

---

## hasChildRoutes Function

The `hasChildRoutes` function is used within the `createRoutes` function to determine if the root module has child routes. It takes a module as an argument and returns a boolean indicating whether the module has child routes.

```javascript
const hasChildRoutes = (module) => module && !!module.childRoutes;
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1vbmUtYXBwJTNBJTNBZ2lsYWRuYXZvdA==" repo-name="DEMO-one-app" doc-type="overview"><sup>Powered by [Swimm](/)</sup></SwmMeta>
