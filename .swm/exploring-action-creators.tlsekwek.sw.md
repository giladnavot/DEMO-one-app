---
title: Exploring Action Creators
---
Action Creators are functions that return an action object. These functions are used to abstract the process of creating an action object that has a specific type and payload. In the DEMO-one-app, Action Creators are used to manage the state of the application, by dispatching actions that represent user interactions or API calls.

<SwmSnippet path="/src/universal/reducers.js" line="17">

---

# Using Action Creators

In this code snippet, we can see how the application combines reducers to manage state. Action Creators would be used in the individual reducer files (like `config` imported from './ducks/config') to define the actions that can be dispatched to these reducers.

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
