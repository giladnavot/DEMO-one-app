---
title: Getting Started with Reducers
---
Reducers in the DEMO-one-app are functions that determine changes to an application's state. They respond to actions sent to the store, modify the state accordingly, and return the new state.

In the provided code, `combineReducersWithBuildInitialState` is used to combine different reducing functions into a single reducer. This allows each reducing function to manage its own part of the global state.

The `globalReducers` from `@americanexpress/one-app-ducks` and `config` from `./ducks/config` are combined to create the root reducer for the application.

<SwmSnippet path="/src/universal/reducers.js" line="17">

---

# Reducer Implementation

In this file, we import the global reducers from `@americanexpress/one-app-ducks` and a local `config` reducer. These reducers are then combined using the `combineReducersWithBuildInitialState` function from `@americanexpress/vitruvius/immutable`. The combined reducer is then exported as the default export of the module.

```javascript
import globalReducers from '@americanexpress/one-app-ducks';
import combineReducersWithBuildInitialState from '@americanexpress/vitruvius/immutable';
import config from './ducks/config';

export default combineReducersWithBuildInitialState({
  ...globalReducers,
  config,
});
```

---

</SwmSnippet>

# Reducer Functions

Reducer Functions

<SwmSnippet path="/src/universal/reducers.js" line="21">

---

## combineReducersWithBuildInitialState

The `combineReducersWithBuildInitialState` function is a key function in the context of reducers. It is used to combine all the individual reducers into a single root reducer. This root reducer is then used to create the application's state. The function takes an object as an argument, where each key-value pair represents a slice of the state and the corresponding reducer function. In this case, it combines `globalReducers` and `config`.

```javascript
export default combineReducersWithBuildInitialState({
  ...globalReducers,
  config,
});
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1vbmUtYXBwJTNBJTNBZ2lsYWRuYXZvdA==" repo-name="DEMO-one-app" doc-type="overview"><sup>Powered by [Swimm](/)</sup></SwmMeta>
