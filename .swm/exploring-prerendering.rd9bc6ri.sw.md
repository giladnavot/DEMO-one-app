---
title: Exploring Prerendering
---
Prerendering in the DEMO-one-app is a technique used to improve the initial load performance of the application. It involves generating the HTML of a page server-side and sending that to the client, rather than having the client generate the HTML. This is particularly useful for content-heavy applications where the client-side rendering could be slow. In the DEMO-one-app, prerendering is handled by the `loadPrerenderScripts` function, which loads the necessary scripts for the initial state of the application.

<SwmSnippet path="/src/client/prerender.js" line="41">

---

## Prerendering in Action

Here is the `loadPrerenderScripts` function where prerendering is implemented. It checks the active locale in the initial state and loads the corresponding locale pack. If the `useNativeIntl` flag is set to true, it will resolve immediately without loading the locale pack.

```javascript
export function loadPrerenderScripts(initialState) {
  const locale = initialState && initialState.getIn(['intl', 'activeLocale']);
  return locale && !window?.useNativeIntl ? getLocalePack(locale) : Promise.resolve();
}
```

---

</SwmSnippet>

<SwmSnippet path="/__tests__/client/prerender.spec.js" line="106">

---

## Testing Prerendering

In the test suite for `loadPrerenderScripts`, we can see how the function behaves under different conditions. For example, it tests that the function resolves immediately if there is no active locale or if the `useNativeIntl` flag is set to true.

```javascript
describe('loadPrerenderScripts', () => {
  it('should load the correct locale pack', () => {
    const initialState = fromJS({ intl: { activeLocale: 'es-ES' } });

    return loadPrerenderScripts(initialState)
      .then((localePack) => {
        expect(localePack).toBe('es-ES');
        const { getLocalePack } = require('@americanexpress/one-app-ducks');
        expect(getLocalePack).toHaveBeenCalledWith('es-ES');
      });
  });

  it('should still resolve if there is no active locale', () => loadPrerenderScripts()
    .then((localePack) => {
      expect(localePack).toBeUndefined();
      const { getLocalePack } = require('@americanexpress/one-app-ducks');
      // need to call clearMock because jest.clearAllMocks() was removed
      // TODO: switch to clearAllMocks when it is added back to jest https://github.com/facebook/jest/issues/2134
      getLocalePack.mockClear();
      expect(getLocalePack).not.toHaveBeenCalled();
    })
```

---

</SwmSnippet>

# Prerender Functions

This section covers the main functions in the 'prerender.js' file.

<SwmSnippet path="/src/client/prerender.js" line="27">

---

## initializeClientStore

The `initializeClientStore` function is used to create a Redux store on the client side. It uses the 'createHolocronStore' function from the 'holocron' library to create the store. The initial state of the store is either derived from the global '**INITIAL_STATE**' variable or undefined if the variable is not set. The function also sets a six-second timeout for fetch requests on the client side.

```javascript
export function initializeClientStore() {
  // Six second timeout on client
  const enhancedFetch = compose(createTimeoutFetch(6e3))(fetch);

  const enhancer = createEnhancer();
  const initialState = global.__INITIAL_STATE__ !== undefined
    ? transit.fromJSON(global.__INITIAL_STATE__) : undefined;
  const store = createHolocronStore({
    reducer, initialState, enhancer, extraThunkArguments: { fetchClient: enhancedFetch },
  });

  return store;
}
```

---

</SwmSnippet>

<SwmSnippet path="/src/client/prerender.js" line="41">

---

## loadPrerenderScripts

The `loadPrerenderScripts` function is used to load locale-specific scripts during the prerendering process. It checks if the 'activeLocale' is set in the initial state and if the 'useNativeIntl' flag is not set. If both conditions are met, it calls the 'getLocalePack' function to load the locale pack for the active locale.

```javascript
export function loadPrerenderScripts(initialState) {
  const locale = initialState && initialState.getIn(['intl', 'activeLocale']);
  return locale && !window?.useNativeIntl ? getLocalePack(locale) : Promise.resolve();
}
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1vbmUtYXBwJTNBJTNBZ2lsYWRuYXZvdA==" repo-name="DEMO-one-app" doc-type="overview"><sup>Powered by [Swimm](/)</sup></SwmMeta>
