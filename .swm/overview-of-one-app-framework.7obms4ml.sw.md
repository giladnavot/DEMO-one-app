---
title: Overview of One App Framework
---
One App is a web application development framework that serves up a single page app built using React components. It leverages a Node.js server and Holocron for code splitting, which allows for efficient loading and updating of application modules. This framework is designed to facilitate the development of scalable and reliable web applications.

<SwmSnippet path="/src/universal/enhancers.js" line="17">

---

# Middleware in One App

This code snippet shows how middleware is used in the One App framework. Middleware is a way to extend Redux with custom functionality. Here, the `createEnhancer` function is used to apply middleware to the Redux store. The `redux-lifesaver` middleware is used to limit the number of actions that can be dispatched in a given time period, which can help prevent infinite loops or other performance issues.

```javascript
import { applyMiddleware, compose } from 'redux';
import lifesaver from 'redux-lifesaver';
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
```

---

</SwmSnippet>

# Function Overview

This section discusses the 'createEnhancer' function.

## createEnhancer

The `createEnhancer` function is a key part of the One App framework. It is used to create a Redux store enhancer with middleware. The function takes an array of extra middleware as an argument. It uses 'redux-lifesaver' middleware by default, which helps to prevent infinite loops in Redux. The function also integrates with Redux DevTools in development mode for easier debugging.

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1vbmUtYXBwJTNBJTNBZ2lsYWRuYXZvdA==" repo-name="DEMO-one-app" doc-type="overview"><sup>Powered by [Swimm](/)</sup></SwmMeta>
