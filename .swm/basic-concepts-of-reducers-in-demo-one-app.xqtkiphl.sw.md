---
title: Basic Concepts of Reducers in DEMO-one-app
---
Reducers in the DEMO-one-app are functions that determine changes to an application's state. They respond to actions dispatched from both the user interface and server. Reducers calculate a new state given the previous state and an action. They are pure functions, meaning they do not modify the input state but return a new state object.

In the provided code, the `combineReducersWithBuildInitialState` function is used to combine different reducing functions into a single reducer. This combined reducer can now handle all state updates. The `globalReducers` from the `one-app-ducks` package and the `config` reducer are combined here.

<SwmSnippet path="/src/universal/reducers.js" line="17">

---

# Reducer Implementation

In this file, we import the global reducers from `@americanexpress/one-app-ducks` and a local `config` reducer. We then combine these reducers using `combineReducersWithBuildInitialState` from `@americanexpress/vitruvius/immutable`. The combined reducer is then exported and can be used by the Redux store.

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

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1vbmUtYXBwJTNBJTNBZ2lsYWRuYXZvdA==" repo-name="DEMO-one-app" doc-type="overview"><sup>Powered by [Swimm](/)</sup></SwmMeta>
