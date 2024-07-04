---
title: Enhancing Redux with combineReducersWithBuildInitialState from Vitruvius
---
This document will cover the enhancement of Redux capabilities with 'combineReducersWithBuildInitialState' from the Vitruvius library. We'll cover:

1. What is 'combineReducersWithBuildInitialState'
2. How it is used in the codebase
3. Its functionality and purpose.

# What is 'combineReducersWithBuildInitialState'

'combineReducersWithBuildInitialState' is a function from the Vitruvius library. It is used to combine multiple reducer functions into a single reducer function. This combined reducer can then be used to create the Redux store.

<SwmSnippet path="/src/universal/reducers.js" line="17">

---

# How 'combineReducersWithBuildInitialState' is used in the codebase

In this file, 'combineReducersWithBuildInitialState' is imported from the Vitruvius library and used to combine 'globalReducers' and 'config' into a single reducer. This combined reducer is then exported for use in creating the Redux store.

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

# Functionality and purpose of 'combineReducersWithBuildInitialState'

'combineReducersWithBuildInitialState' enhances Redux capabilities by allowing for the combination of multiple reducer functions into a single reducer function. This is beneficial as it allows for the separation of concerns within the Redux store, with each reducer handling a specific slice of the state. Furthermore, it simplifies the creation of the Redux store as only a single reducer function needs to be passed to the 'createStore' function.

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1vbmUtYXBwJTNBJTNBZ2lsYWRuYXZvdA==" repo-name="DEMO-one-app" doc-type="follow-up"><sup>Powered by [Swimm](/)</sup></SwmMeta>
