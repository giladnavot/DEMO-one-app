---
title: Introduction to Universal
---
Universal in the DEMO-one-app refers to a directory that contains shared code used across the application. It includes utilities, reducers, and routes that are used universally throughout the application. The `index.js` file in the `src/universal` directory exports these shared resources for use in other parts of the application.

<SwmSnippet path="/src/universal/index.js" line="17">

---

## Universal Directory Structure

The 'index.js' file in the 'Universal' directory exports the 'reducers' and 'createRoutes' functions. These functions can be imported into other parts of the application as needed.

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

The 'utils' subdirectory within the 'Universal' directory contains utility functions that are used throughout the application. These include 'hasChildRoutes.js' and 'transit.js'.

# Universal Functions

The Universal directory contains several important functions that are used throughout the application. The two main functions are createRoutes and reducers.

<SwmSnippet path="/src/universal/index.js" line="18">

---

## createRoutes

The `createRoutes` function is responsible for creating the routes for the application. It is exported from the Universal directory and can be used in any part of the application that requires routing.

```javascript
import createRoutes from './routes';
```

---

</SwmSnippet>

<SwmSnippet path="/src/universal/index.js" line="17">

---

## reducers

The `reducers` function is used to manage the state of the application. It is also exported from the Universal directory and can be used anywhere in the application where state management is required.

```javascript
import reducers from './reducers';
```

---

</SwmSnippet>

# Universal Routing

Understanding Universal Routing

<SwmSnippet path="/src/universal/routes.jsx" line="33">

---

## Root Module Route

This line defines the root module route. The `ModuleRoute` component from 'holocron-module-route' is used to define a route that corresponds to a specific Holocron module. The `moduleName` prop is used to specify the name of the module that should be loaded when this route is accessed. The `store` prop is used to provide the Redux store to the module. The `path` prop is used to specify the URL pattern that this route corresponds to. If the root module has child routes, the `path` prop is left undefined, which means this route will match any URL.

```javascript
    <ModuleRoute key="rootModule" moduleName={rootModuleName} store={store} path={rootHasChildRoutes ? undefined : '/'} />,
```

---

</SwmSnippet>

<SwmSnippet path="/src/universal/routes.jsx" line="34">

---

## 404 Route

This block of code defines a catch-all route that matches any URL not already matched by other routes. The `Route` component from '@americanexpress/one-app-router' is used to define this route. The `path` prop is set to '\*', which means this route will match any URL. The `component` prop is set to a function that returns 'Not found', which means this text will be displayed when this route is accessed. The `onEnter` and `onLeave` props are used to dispatch actions to the Redux store when this route is entered and left, respectively.

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
