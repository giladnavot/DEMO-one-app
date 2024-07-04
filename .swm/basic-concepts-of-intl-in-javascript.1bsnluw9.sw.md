---
title: Basic Concepts of Intl in JavaScript
---
Intl is a built-in object in JavaScript that provides a way to handle string comparison, number formatting, and date and time formatting in a language-sensitive manner. In the DEMO-one-app, the 'lean-intl' package is used as a polyfill for the Intl object to ensure consistent implementation across different browsers. This is particularly important as some browsers implement Intl inconsistently.

<SwmSnippet path="/src/client/polyfill/Intl.js" line="20">

---

# Intl Polyfill

Here, Intl is imported from 'lean-intl' and is set as a global object if the native Intl is not available in the window object. This ensures that the Intl object is available globally throughout the application for use in internationalization tasks.

```javascript
import Intl from 'lean-intl';

if (!(window && window.useNativeIntl)) {
  global.Intl = Intl;
  global.IntlPolyfill = Intl;
}
```

---

</SwmSnippet>

# The Intl object

The Intl object in JavaScript

<SwmSnippet path="/src/client/polyfill/Intl.js" line="20">

---

## The Intl object

The `Intl` object is imported from 'lean-intl' and is assigned to the global `Intl` object if `window.useNativeIntl` is not set. This ensures that the `Intl` object is always available globally and can be used for internationalization purposes throughout the application.

```javascript
import Intl from 'lean-intl';

if (!(window && window.useNativeIntl)) {
  global.Intl = Intl;
  global.IntlPolyfill = Intl;
}
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1vbmUtYXBwJTNBJTNBZ2lsYWRuYXZvdA==" repo-name="DEMO-one-app" doc-type="overview"><sup>Powered by [Swimm](/)</sup></SwmMeta>
