---
title: Role of globalReducers from @americanexpress/one-app-ducks
---
This document will cover the role and functionality of the `globalReducers` from `@americanexpress/one-app-ducks` in the DEMO-one-app repository. We'll cover:

1. What `globalReducers` is
2. How `globalReducers` is used in the codebase
3. The purpose and functionality of `globalReducers`.

# What is `globalReducers`

`globalReducers` is an object imported from the `@americanexpress/one-app-ducks` package. It contains a set of reducer functions that are used to manage the global state of the application.

<SwmSnippet path="/src/universal/reducers.js" line="17">

---

# How `globalReducers` is used in the codebase

In this file, `globalReducers` is imported and spread into the root reducer of the application. This means that all the reducer functions contained in `globalReducers` are included in the root reducer and can therefore manage the global state of the application.

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

<SwmSnippet path="/__tests__/universal/reducers.spec.js" line="19">

---

# The purpose and functionality of `globalReducers`

This test file demonstrates the functionality of `globalReducers`. It shows that the root reducer, which includes `globalReducers`, is used to create the Redux store. The state of this store includes the keys managed by `globalReducers`.

```javascript
import reducers from '../../src/universal/reducers';

describe('reducers', () => {
  it('should export a root reducer that can be used to create a store', () => {
    const store = createStore(reducers);
    const state = store.getState();
    // not testing global ducks keys because those should be tested in global ducks
    // TODO: all global reducers should come from global ducks (move api ducks, routing, config)
    expect(state.has('config')).toBe(true);
  });
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1vbmUtYXBwJTNBJTNBZ2lsYWRuYXZvdA==" repo-name="DEMO-one-app" doc-type="follow-up"><sup>Powered by [Swimm](/)</sup></SwmMeta>
