---
title: Introduction to Enhancers
---
Enhancers in Redux are higher-order functions that compose a store creator to return a new, enhanced store creator. This is similar to how middleware adds extra functionality to the dispatch method of a Redux store. In the context of DEMO-one-app, the `createEnhancer` function is used to create a store enhancer that applies middleware to the store. This middleware can include any extra middleware passed in, as well as the 'redux-lifesaver' middleware. The resulting store enhancer can then be used to create a Redux store with the applied middleware.

<SwmSnippet path="/src/universal/enhancers.js" line="21">

---

# Creating an Enhancer

The `createEnhancer` function is used to create an enhancer. It takes an array of additional middleware as an argument. The function creates an array of middleware, which includes the additional middleware and a `lifesaver` middleware. The `lifesaver` middleware is configured with a dispatch limit and action types. The function then checks if the environment is development and if the code is running in the browser. If it is, it uses the Redux DevTools extension to compose the enhancers, otherwise, it uses the `applyMiddleware` function from Redux. The function then returns the composed enhancers.

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

<SwmSnippet path="/src/universal/enhancers.js" line="33">

---

# Using the Enhancer

The `storeEnhancers` variable is used to store the composed enhancers. If the environment is development and the code is running in the browser, the Redux DevTools extension is used to compose the enhancers. Otherwise, the `applyMiddleware` function from Redux is used. The composed enhancers are then returned by the `createEnhancer` function.

```javascript
  if (process.env.NODE_ENV === 'development' && global.BROWSER) {
    const composeEnhancers = window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__ || compose;
    storeEnhancers = composeEnhancers(applyMiddleware(...middleware));
  } else {
    storeEnhancers = applyMiddleware(...middleware);
  }
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1vbmUtYXBwJTNBJTNBZ2lsYWRuYXZvdA==" repo-name="DEMO-one-app" doc-type="overview"><sup>Powered by [Swimm](/)</sup></SwmMeta>
