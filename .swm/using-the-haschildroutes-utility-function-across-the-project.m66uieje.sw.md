---
title: Using the hasChildRoutes Utility Function Across the Project
---
This document will cover the utility function `hasChildRoutes` in the DEMO-one-app project. We'll cover:

1. What is `hasChildRoutes`?
2. How is `hasChildRoutes` used in the project?

<SwmSnippet path="/src/universal/utils/hasChildRoutes.js" line="17">

---

# What is `hasChildRoutes`?

`hasChildRoutes` is a utility function that checks if a given module has child routes. It takes a module as an argument and returns a boolean value indicating whether the module has child routes or not.

```javascript
const hasChildRoutes = (module) => module && !!module.childRoutes;
```

---

</SwmSnippet>

# How is `hasChildRoutes` used in the project?

`hasChildRoutes` is used in the project to determine if a module has child routes. This is useful in scenarios where the application needs to know if a module has any child routes for navigation or rendering purposes. For example, if a module has child routes, the application might render a nested navigation menu or handle routing differently.

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1vbmUtYXBwJTNBJTNBZ2lsYWRuYXZvdA==" repo-name="DEMO-one-app" doc-type="follow-up"><sup>Powered by [Swimm](/)</sup></SwmMeta>
