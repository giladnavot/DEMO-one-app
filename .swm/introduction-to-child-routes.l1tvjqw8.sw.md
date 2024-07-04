---
title: Introduction to Child Routes
---
Child Routes in the DEMO-one-app are sub-routes or nested routes within a parent route. They are used to create a hierarchical structure of routes in the application. This structure allows for more organized and modular code, as well as more complex user interfaces.

The function `hasChildRoutes` is used to check if a module has child routes. It takes a module as an argument and returns a boolean value indicating whether the module has child routes or not.

<SwmSnippet path="/src/universal/utils/hasChildRoutes.js" line="17">

---

## Checking for Child Routes

The `hasChildRoutes` function is used to check if a module has child routes. It takes a module as an argument and returns a boolean value indicating whether the module has a `childRoutes` property. This function is useful when you need to conditionally render components or perform actions based on the presence of child routes.

```javascript
const hasChildRoutes = (module) => module && !!module.childRoutes;
```

---

</SwmSnippet>

# Child Routes Function

Function Overview

<SwmSnippet path="/src/universal/utils/hasChildRoutes.js" line="17">

---

## hasChildRoutes

The `hasChildRoutes` function is a utility function that checks if a given module has child routes. It takes a module as an argument and returns a boolean value indicating whether the module has child routes or not. This function is crucial in the routing system of the application, as it helps in determining the structure of the application's routing tree.

```javascript
const hasChildRoutes = (module) => module && !!module.childRoutes;
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1vbmUtYXBwJTNBJTNBZ2lsYWRuYXZvdA==" repo-name="DEMO-one-app" doc-type="overview"><sup>Powered by [Swimm](/)</sup></SwmMeta>
