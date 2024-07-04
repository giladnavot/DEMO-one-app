---
title: Introduction to Runtime Configuration
---
Runtime Configuration in DEMO-one-app refers to the setup and validation of environment variables that are used during the execution of the application. This configuration is defined in the `runTime` array in the `src/server/config/env/runTime.js` file. Each object in the array represents an environment variable with properties such as `name`, `normalize`, `valid`, `defaultValue`, and `validate` that define the behavior and constraints of the variable. The `preprocessEnvVar` function is then used to process each environment variable according to these definitions.

<SwmSnippet path="/src/server/config/env/runTime.js" line="40">

---

# Runtime Configuration Variables

This is the `runTime` array that contains the definitions for all the environment variables used for runtime configuration. Each object in the array represents a single environment variable, with properties for its name, normalization function, valid values, default value, and optional validation function.

```javascript
const runTime = [
  {
    name: 'NODE_ENV',
    normalize: (input) => input && input.toLowerCase(),
    valid: ['development', 'production'],
    defaultValue: 'production',
  },
  {
    name: 'ONE_DANGEROUSLY_DISABLE_CSP',
    normalize: (input) => input.toLowerCase(),
    validate: (input) => {
      if (input === 'true' && process.env.NODE_ENV !== 'development') {
        throw new Error('If you are trying to bypass CSP requirement, NODE_ENV must also be set to development.');
      }
      if (input === 'true' && process.env.NODE_ENV === 'development') {
        console.warn('ONE_DANGEROUSLY_DISABLE_CSP is true and NODE_ENV is set to development. Content-Security-Policy header will not be set.');
      }
    },
    valid: ['true', 'false'],
    defaultValue: 'false',
  },
```

---

</SwmSnippet>

<SwmSnippet path="/src/server/init.js" line="24">

---

# Using Runtime Configuration Variables

The runtime configuration variables are imported and used in the application's initialization script. They are processed by the `preprocessEnvVar` function, which applies the normalization and validation functions to the values of the environment variables.

```javascript
import './config/env/runTime';
```

---

</SwmSnippet>

# Runtime Configuration Functions

The Runtime Configuration consists of several functions that validate and normalize environment variables.

<SwmSnippet path="/src/server/config/env/runTime.js" line="18">

---

## isFetchableUrlInNode

The `isFetchableUrlInNode` function is used to validate if a given URL can be fetched in a Node.js environment. It is used in the validation of several environment variables such as 'HOLOCRON_MODULE_MAP_URL'.

```javascript
const isFetchableUrlInNode = require('@americanexpress/env-config-utils/isFetchableUrlInNode');
```

---

</SwmSnippet>

<SwmSnippet path="/src/server/config/env/runTime.js" line="19">

---

## isFetchableUrlInBrowser

The `isFetchableUrlInBrowser` function is used to validate if a given URL can be fetched in a browser environment. It is used in the validation of several environment variables such as 'ONE_CLIENT_REPORTING_URL' and 'ONE_CLIENT_CSP_REPORTING_URL'.

```javascript
const isFetchableUrlInBrowser = require('@americanexpress/env-config-utils/isFetchableUrlInBrowser');
```

---

</SwmSnippet>

<SwmSnippet path="/src/server/config/env/runTime.js" line="24">

---

## isPositiveIntegerIfDefined

The `isPositiveIntegerIfDefined` function is used to validate if a given input is a positive integer. It is used in the validation of several environment variables such as 'HOLOCRON_SERVER_MAX_MODULES_RETRY' and 'HOLOCRON_SERVER_MAX_SIM_MODULES_FETCH'.

```javascript
const isPositiveIntegerIfDefined = (input) => {
  if (input === undefined) {
    return;
  }

  const value = Number(input);

  if (value > 0 && value % 1 === 0) {
    return;
  }

  throw new Error(`Expected ${input} to be a positive integer`);
};
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1vbmUtYXBwJTNBJTNBZ2lsYWRuYXZvdA==" repo-name="DEMO-one-app" doc-type="overview"><sup>Powered by [Swimm](/)</sup></SwmMeta>
