---
title: Understanding State Management in Reducers
---
This document will cover the process of maintaining and updating state with reducers in the DEMO-one-app repository. We'll cover:

1. The role of reducers in state management
2. How initial state is built
3. How state is updated based on actions.

# Role of Reducers in State Management

Reducers are functions that take the current state and an action, and return a new state. They are crucial in state management in Redux-based applications like DEMO-one-app. They define how the application's state changes in response to actions sent to the store.

<SwmSnippet path="/prod-sample/sample-modules/frank-lloyd-root/0.0.2/src/components/FrankLloydRoot.jsx" line="60">

---

# Building Initial State

Here, the `reducer` function is defined with an initial state. The `buildInitialState` function is attached to the `reducer` and is used to build the initial state based on the request body.

```javascript
const reducer = (state = fromJS({})) => state;
reducer.buildInitialState = ({ req } = {}) => {
  if (req && req.body) {
```

---

</SwmSnippet>

<SwmSnippet path="/prod-sample/sample-modules/vitruvius-franklin/0.0.1/src/duck.js" line="21">

---

In this example, the `reducer` function is defined with an initial state that is built using the `buildInitialState` function.

```javascript
function reducer(state = buildInitialState()) {
  return state;
}
```

---

</SwmSnippet>

<SwmSnippet path="/src/universal/ducks/config.js" line="30">

---

# Updating State Based on Actions

Here, the `reducer` function updates the state based on the action type. If the action type is `SET_CONFIG`, it returns a new state with the updated configuration.

```javascript
export default function reducer(state = initialState, action) {
  if (action.type === SET_CONFIG) {
    return fromJS(action.config);
  }

  return state;
}
```

---

</SwmSnippet>

<SwmSnippet path="/prod-sample/sample-modules/ssr-frank/0.0.0/src/duck.js" line="34">

---

In this example, the `reducer` function updates the state based on different action types (`FAKE_REQUEST`, `FAKE_SUCCESS`, `FAKE_FAILURE`). Depending on the action type, different properties of the state are updated.

```javascript
function reducer(state = buildInitialState(), action = {}) {
  switch (action.type) {
    case FAKE_REQUEST:
      return state
        .set('isLoading', true)
        .set('isComplete', false);
    case FAKE_SUCCESS:
      return state
        .set('data', action.data)
        .set('isLoading', false)
        .set('isComplete', true);
    case FAKE_FAILURE:
      return state
        .set('error', action.error)
        .set('isLoading', false)
        .set('isComplete', true);
    default:
      return state;
  }
}
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1vbmUtYXBwJTNBJTNBZ2lsYWRuYXZvdA==" repo-name="DEMO-one-app" doc-type="follow-up"><sup>Powered by [Swimm](/)</sup></SwmMeta>
