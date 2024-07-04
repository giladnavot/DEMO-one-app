---
title: Basic Concepts of Enhancers
---
Enhancers in this codebase are higher-order functions that add additional functionality to the Redux store. They are used to modify the store's dispatch method, allowing developers to interact with dispatched actions before they reach the reducer. The `createEnhancer` function in `src/universal/enhancers.js` is an example of an enhancer. It takes an array of middleware as an argument and applies them to the store using Redux's `applyMiddleware` function. In development mode, it also integrates with the Redux DevTools extension for easier debugging.

<SwmSnippet path="/src/universal/enhancers.js" line="21">

---

# `createEnhancer` function

The `createEnhancer` function is used to create an enhancer. It takes an array of extra middleware as an argument and applies them to the store using Redux's `applyMiddleware` function. It also applies the `lifesaver` middleware, which limits the number of dispatched actions. If the application is running in development mode and in the browser, it also enables Redux DevTools.

```javascript
export default function createEnhancer(extraMiddleware = []) {
  const middleware = [
    ...extraMiddleware,
    lifesaver({
      dispatchLimit: 20,
      actionTypes: {
        [MODULE_LOADED]: { limitDuration: 0 },
      },
    }),
  ];

  let storeEnhancers;
  if (process.env.NODE_ENV === 'development' && global.BROWSER) {
    const composeEnhancers = window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__ || compose;
    storeEnhancers = composeEnhancers(applyMiddleware(...middleware));
  } else {
    storeEnhancers = applyMiddleware(...middleware);
  }
  return storeEnhancers;
}
```

---

</SwmSnippet>

<SwmSnippet path="/src/universal/enhancers.js" line="32">

---

# `storeEnhancers` variable

The `storeEnhancers` variable is used to store the enhancer created by the `createEnhancer` function. It is then returned by the function and can be used to create a Redux store with the applied middleware and optional Redux DevTools support.

```javascript
  let storeEnhancers;
  if (process.env.NODE_ENV === 'development' && global.BROWSER) {
    const composeEnhancers = window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__ || compose;
    storeEnhancers = composeEnhancers(applyMiddleware(...middleware));
  } else {
    storeEnhancers = applyMiddleware(...middleware);
  }
  return storeEnhancers;
```

---

</SwmSnippet>

# Enhancers in Redux

Understanding Enhancers

<SwmSnippet path="/src/universal/enhancers.js" line="21">

---

## createEnhancer

The `createEnhancer` function is used to create a Redux store enhancer. It takes an array of extra middleware as an argument and combines them with the `lifesaver` middleware. The `lifesaver` middleware is configured with a dispatch limit and a set of action types. The function then checks if the environment is development and if the code is running in the browser. If so, it uses the Redux DevTools extension compose function if available, otherwise, it uses the Redux `compose` function. The composed enhancers are then applied to the middleware to create the final store enhancer.

```javascript
export default function createEnhancer(extraMiddleware = []) {
  const middleware = [
    ...extraMiddleware,
    lifesaver({
      dispatchLimit: 20,
      actionTypes: {
        [MODULE_LOADED]: { limitDuration: 0 },
      },
    }),
  ];

  let storeEnhancers;
  if (process.env.NODE_ENV === 'development' && global.BROWSER) {
    const composeEnhancers = window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__ || compose;
    storeEnhancers = composeEnhancers(applyMiddleware(...middleware));
  } else {
    storeEnhancers = applyMiddleware(...middleware);
  }
  return storeEnhancers;
}
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1vbmUtYXBwJTNBJTNBZ2lsYWRuYXZvdA==" repo-name="DEMO-one-app" doc-type="overview"><sup>Powered by [Swimm](/)</sup></SwmMeta>
