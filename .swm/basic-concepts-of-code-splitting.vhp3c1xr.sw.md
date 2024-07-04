---
title: Basic Concepts of Code Splitting
---
Code Splitting is a feature that allows you to split your code into various bundles which can then be loaded on demand or in parallel. In the DEMO-one-app, this is achieved using Holocron. Holocron modules are self-contained, reusable pieces of code that can be loaded independently, thus enabling code splitting. This approach enhances the performance of the application by only loading the necessary code when it's needed.

<SwmSnippet path="/src/universal/enhancers.js" line="19">

---

## Using Code Splitting

In this file, the 'createEnhancer' function is defined. This function applies the middleware and enhancers to the Redux store. The 'MODULE_LOADED' action is dispatched when a module is loaded, which triggers the code splitting functionality.

```javascript
import { MODULE_LOADED } from 'holocron/ducks/constants';

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
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1vbmUtYXBwJTNBJTNBZ2lsYWRuYXZvdA==" repo-name="DEMO-one-app" doc-type="overview"><sup>Powered by [Swimm](/)</sup></SwmMeta>
