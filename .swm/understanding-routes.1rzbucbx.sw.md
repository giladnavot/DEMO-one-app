---
title: Understanding Routes
---
Routes in the DEMO-one-app are defined using the `Route` and `ModuleRoute` components from `@americanexpress/one-app-router` and `holocron-module-route` respectively. They are used to map different URLs to specific React components. The `createRoutes` function is responsible for creating these routes. It uses the `rootModuleName` from the application's state to create a `ModuleRoute`. If the root module has child routes, the path is left undefined, otherwise it is set to '/'. A catch-all `Route` is also created to handle 404 errors.

<SwmSnippet path="/src/universal/routes.jsx" line="28">

---

## Defining Routes

The `createRoutes` function is used to define the routes for the application. It takes the application store as an argument and uses it to get the root module name. It then checks if the root module has child routes using the `hasChildRoutes` function. Two routes are defined - the root module route and a catch-all route for 404 errors.

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

## Route Properties

Each route is defined with a `key`, `path`, and optionally a `component`. The `key` is a unique identifier for the route. The `path` is the URL path for the route. The `component` is the React component that will be rendered when the route is accessed. If a `component` is not provided, the `ModuleRoute` component will render the module specified by the `moduleName` prop.

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

<SwmSnippet path="/src/universal/routes.jsx" line="38">

---

## Route Handlers

Routes can also have `onEnter` and `onLeave` handlers. The `onEnter` handler is called when the route is accessed, and the `onLeave` handler is called when the user navigates away from the route. In this case, the `onEnter` handler for the 404 route dispatches an `applicationError` action, and the `onLeave` handler dispatches a `clearError` action.

```javascript
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
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1vbmUtYXBwJTNBJTNBZ2lsYWRuYXZvdA==" repo-name="DEMO-one-app" doc-type="overview"><sup>Powered by [Swimm](/)</sup></SwmMeta>
