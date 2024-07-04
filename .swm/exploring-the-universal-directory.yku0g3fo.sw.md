---
title: Exploring the 'Universal' Directory
---
Universal in the DEMO-one-app refers to a directory that contains the core functionalities of the application. It includes the main index file, reducers, routes, and utility functions. The 'universal' directory is organized into subdirectories such as 'ducks' and 'utils', each containing specific functionalities. 'Ducks' typically contains files related to the Redux state management, while 'utils' contains utility functions used across the application.

<SwmSnippet path="/src/universal/index.js" line="17">

---

## Universal Directory Structure

This is the entry point of the 'universal' directory. It exports the 'reducers' and 'createRoutes' functions, which are used throughout the application.

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

## Universal Utils

The 'universal/utils' directory contains utility functions that are used across the application. For example, 'hasChildRoutes.js' and 'transit.js'. These utilities can be imported and used wherever necessary in the codebase.

# Universal Endpoints

Universal Routes

<SwmSnippet path="/src/universal/routes.jsx" line="33">

---

## Root Module Route

The 'rootModule' route is the main entry point of the application. It uses the 'ModuleRoute' component from 'holocron-module-route' to dynamically load the root module based on the application's state. If the root module has child routes, the path is left undefined, allowing the child routes to be defined separately.

```javascript
    <ModuleRoute key="rootModule" moduleName={rootModuleName} store={store} path={rootHasChildRoutes ? undefined : '/'} />,
    <Route
```

---

</SwmSnippet>

<SwmSnippet path="/src/universal/routes.jsx" line="34">

---

## 404 Route

The '404' route is a catch-all route that matches any path not previously matched by other routes. When this route is entered, it dispatches an 'applicationError' action with a 404 status. When this route is left, it dispatches a 'clearError' action to clear the error state.

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
