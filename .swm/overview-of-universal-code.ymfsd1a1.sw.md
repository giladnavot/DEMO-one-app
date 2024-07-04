---
title: Overview of Universal Code
---
In the DEMO-one-app repository, 'universal' refers to a directory that contains shared code used across the application. This includes utility functions, configuration settings, and route definitions. The code in this directory is designed to be environment agnostic, meaning it can run both on the server and the client side, hence the term 'universal'.

## Universal Directory

The 'universal' directory contains utility functions such as 'hasChildRoutes.js' and 'transit.js'. These utilities can be imported and used throughout the application.

<SwmSnippet path="/src/universal/index.js" line="17">

---

## Universal File

The 'universal' file exports a 'createRoutes' function and a 'reducers' object. These can be imported and used for managing routes and state in the application.

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

# Universal Routes

Understanding Routes in Universal

<SwmSnippet path="/src/universal/routes.jsx" line="33">

---

## ModuleRoute

The `ModuleRoute` component is a special type of route provided by the 'holocron-module-route' library. It is used to dynamically load a module based on the 'moduleName' prop. The 'store' prop is used to pass the Redux store to the module, and the 'path' prop is conditionally set based on whether the root module has child routes.

```javascript
    <ModuleRoute key="rootModule" moduleName={rootModuleName} store={store} path={rootHasChildRoutes ? undefined : '/'} />,
```

---

</SwmSnippet>

<SwmSnippet path="/src/universal/routes.jsx" line="34">

---

## 404 Route

This is a catch-all route that matches any path not previously matched by other routes. When a user navigates to a non-existent route, this route is activated, displaying a 'Not found' message and dispatching an 'applicationError' action to the Redux store. When the user navigates away from this route, a 'clearError' action is dispatched to clear the error state.

```javascript
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
