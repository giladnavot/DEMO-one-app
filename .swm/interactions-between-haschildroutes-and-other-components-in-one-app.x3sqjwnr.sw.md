---
title: Interactions between hasChildRoutes and Other Components in One App
---
This document will cover the role and interaction of the `hasChildRoutes` utility within the One App project. We'll cover:

1. What is `hasChildRoutes` utility
2. How `hasChildRoutes` is used in the codebase
3. The functionality and purpose of `hasChildRoutes`

<SwmSnippet path="/src/universal/utils/hasChildRoutes.js" line="17">

---

# What is `hasChildRoutes` utility

`hasChildRoutes` is a utility function that checks if a module has child routes. It returns a boolean value based on the presence of child routes in the module.

```javascript
const hasChildRoutes = (module) => module && !!module.childRoutes;
```

---

</SwmSnippet>

<SwmSnippet path="/src/universal/routes.jsx" line="30">

---

# How `hasChildRoutes` is used in the codebase

`hasChildRoutes` is used in the `createRoutes` function to check if the root module has child routes. The result of this check is stored in the `rootHasChildRoutes` variable.

```javascript
  const rootHasChildRoutes = hasChildRoutes(getModule(rootModuleName));
```

---

</SwmSnippet>

<SwmSnippet path="/__tests__/universal/utils/hasChildRoutes.spec.js" line="19">

---

# The functionality and purpose of `hasChildRoutes`

The functionality of `hasChildRoutes` is tested in `hasChildRoutes.spec.js`. The tests check that `hasChildRoutes` correctly returns a boolean value based on whether the input module has child routes or not.

```javascript
it('hasChildRoutes returns false', () => {
  [undefined, null, '', [], {}].forEach((value) => {
    expect(hasChildRoutes(value)).toBeFalsy();
  });
});

it('hasChildRoutes returns true', () => {
  expect(hasChildRoutes({ childRoutes: [] })).toBe(true);
});
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1vbmUtYXBwJTNBJTNBZ2lsYWRuYXZvdA==" repo-name="DEMO-one-app" doc-type="follow-up"><sup>Powered by [Swimm](/)</sup></SwmMeta>
