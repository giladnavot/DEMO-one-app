---
title: Diving into combineReducersWithBuildInitialState Function
---
This document will cover the functionality of the `combineReducersWithBuildInitialState` function in the DEMO-one-app repository. We'll cover:

1. The purpose of `combineReducersWithBuildInitialState`.
2. How `combineReducersWithBuildInitialState` is used in the codebase.
3. The relationship between `combineReducersWithBuildInitialState` and the `reducer` function.
4. The role of `buildInitialState` in the process.

# Purpose of combineReducersWithBuildInitialState

`combineReducersWithBuildInitialState` is a function imported from the `@americanexpress/vitruvius/immutable` package. It is used to combine multiple reducer functions into a single reducer function. This combined reducer can then be used to create the Redux store.

<SwmSnippet path="/src/universal/reducers.js" line="21">

---

# Usage of combineReducersWithBuildInitialState in the codebase

Here, `combineReducersWithBuildInitialState` is used to combine the `globalReducers` and `config` reducers into a single reducer. This combined reducer is then exported for use in the Redux store.

```javascript
export default combineReducersWithBuildInitialState({
  ...globalReducers,
  config,
});
```

---

</SwmSnippet>

# Relationship between combineReducersWithBuildInitialState and the reducer function

The `reducer` function is a key part of the Redux architecture. It describes how the application's state changes in response to actions sent to the store. In the context of `combineReducersWithBuildInitialState`, each reducer function passed to it should have a `buildInitialState` function attached to it. This function is responsible for building the initial state of that particular reducer.

<SwmSnippet path="/prod-sample/sample-modules/vitruvius-franklin/0.0.1/src/duck.js" line="19">

---

# Role of buildInitialState

Here, `buildInitialState` is defined as a function that takes an optional `data` argument and returns an Immutable.js object. This function is then used to set the initial state of the `reducer` function.

```javascript
const buildInitialState = (data = {}) => fromJS(data);

function reducer(state = buildInitialState()) {
  return state;
}
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1vbmUtYXBwJTNBJTNBZ2lsYWRuYXZvdA==" repo-name="DEMO-one-app" doc-type="follow-up"><sup>Powered by [Swimm](/)</sup></SwmMeta>
