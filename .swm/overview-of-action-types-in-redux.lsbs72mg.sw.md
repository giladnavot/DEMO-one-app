---
title: Overview of Action Types in Redux
---
Action Types in Redux are constants that represent the different types of actions that can be dispatched in the application. They are used to identify the type of action that is being performed and dictate how the state should change in response to that action. In the DEMO-one-app, action types are used within Redux reducers to determine how the application's state should be updated.

<SwmSnippet path="/src/universal/reducers.js" line="17">

---

# Defining Action Types

In this code snippet, we can see how the application imports and combines reducers. Each reducer corresponds to a specific action type. When an action is dispatched, the reducer corresponding to the action type is invoked.

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

# Using Action Types

Once the action types are defined, they can be used in action creators to dispatch actions, and in reducers to handle these actions. For example, in an action creator, you would return an object with a `type` property set to the action type, and any additional data in other properties. In a reducer, you would use a switch statement on the action type to determine how to update the state.

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1vbmUtYXBwJTNBJTNBZ2lsYWRuYXZvdA==" repo-name="DEMO-one-app" doc-type="overview"><sup>Powered by [Swimm](/)</sup></SwmMeta>
