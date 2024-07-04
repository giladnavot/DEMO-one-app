---
title: 'Structure and Functionality of one-app-ducks '
---
This document will cover the structure and functionality of the `one-app-ducks` package in the DEMO-one-app repository. We'll cover:

1. The purpose of the `one-app-ducks` package
2. How the `one-app-ducks` package is used in the codebase
3. The functionality and purpose of the `one-app-ducks` package.

# Purpose of the `one-app-ducks` package

The `one-app-ducks` package is a part of the One App ecosystem, which is a web application development framework that serves up a single page app built using React components. It uses a Node.js server and Holocron for code splitting. The `one-app-ducks` package is responsible for managing the state of the application using Redux, a predictable state container for JavaScript apps.

<SwmSnippet path="/src/server/utils/getI18nFileFromState.js" line="33">

---

# Usage of the `one-app-ducks` package in the codebase

The `getI18nFileFromState` function is an example of how the `one-app-ducks` package is used in the codebase. It retrieves the internationalization file based on the current state of the application, which is managed by Redux.

```javascript
function getI18nFileFromState(clientInitialState) {
  const activeLocale = clientInitialState.getIn(['intl', 'activeLocale']);

  if (i18nFiles.has(activeLocale)) {
    return i18nFiles.get(activeLocale);
  }

  // use the language without further refinement (ex: 'en' for 'en-SA')
  const localeArray = activeLocale.split('-');
  // adapted from one-app-ducks src/intl/index.js getLocalePack()
  while (localeArray.length > 0) {
    if (i18nFiles.has(localeArray.join('-'))) {
      return i18nFiles.get(localeArray.join('-'));
    }
    localeArray.pop();
  }

  return null;
}
```

---

</SwmSnippet>

<SwmSnippet path="/src/server/metrics/app-version.js" line="26">

---

# Functionality and purpose of the `one-app-ducks` package

The `one-app-ducks` package also provides functionality for tracking application metrics, as seen in the `app-version.js` file. It defines a metric for tracking the version info of One App.

```javascript
  name: versionNamespace('info'),
  labelNames: ['version', 'major', 'minor', 'patch', 'prerelease', 'build'],
  help: 'One App version info',
});
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1vbmUtYXBwJTNBJTNBZ2lsYWRuYXZvdA==" repo-name="DEMO-one-app" doc-type="follow-up"><sup>Powered by [Swimm](/)</sup></SwmMeta>
