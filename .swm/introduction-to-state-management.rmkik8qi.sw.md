---
title: Introduction to State Management
---
State Management in the DEMO-one-app refers to the way the application maintains, modifies, and watches over the state of the application. It is crucial for the functionality of the app as it allows components to share and manipulate data across the entire application. The state is managed using Redux, a predictable state container for JavaScript apps, and is combined with the use of 'ducks' - a modular way of organizing Redux-related code.

<SwmSnippet path="/src/universal/reducers.js" line="17">

---

# Redux Reducers

This is an example of how reducers are used in the DEMO-one-app. The `combineReducersWithBuildInitialState` function is used to combine the reducers from the `@americanexpress/one-app-ducks` and the local `config` reducer. This combined reducer is then used to create the Redux store, which holds the complete state tree of the app.

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
