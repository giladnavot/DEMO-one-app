---
title: Understanding Server Configuration
---
Server Configuration in DEMO-one-app refers to the setup and parameters that the Node.js server uses to run the application. This includes environment variables, port settings, and other runtime configurations. These configurations are primarily managed in the 'src/server/config/env' directory, with 'argv.js' handling command line arguments and 'runTime.js' managing runtime environment configurations.

<SwmSnippet path="/src/server/config/env/argv.js" line="20">

---

## Command Line Arguments

This section of the code is responsible for parsing command line arguments using the 'yargs' library. It defines the options that can be passed to the application, such as 'use-middleware', 'root-module-name', 'module-map-url', 'use-host', 'log-format', and 'log-level'. It also checks for conflicts between command line arguments and environment variables.

```javascript
const rootModuleNameEnvVarValue = process.env.ONE_CLIENT_ROOT_MODULE_NAME;

if (process.env.NODE_ENV === 'development') {
  yargs
    .option('m', {
      alias: 'use-middleware',
      describe: 'Apply a custom middleware configuration for one-app-dev-proxy',
      type: 'boolean',
    })
    .option('root-module-name', {
      describe: 'Name of the module that serves as the entry point to your application.',
      type: 'string',
    })
    .option('module-map-url', {
      describe: 'Remote module map URL for one-app-dev-cdn to proxy',
      type: 'string',
    })
    .option('use-host', {
      describe: 'use req.headers.host instead of localhost for one-app-dev-cdn',
      default: false,
      type: 'boolean',
```

---

</SwmSnippet>

<SwmSnippet path="/src/server/config/env/runTime.js" line="40">

---

## Environment Variables

This section of the code is responsible for validating and normalizing environment variables. It defines a list of environment variables that the application uses, along with their default values, validation rules, and normalization functions. It also uses the 'preprocessEnvVar' function from the '@americanexpress/env-config-utils' library to process each environment variable.

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

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1vbmUtYXBwJTNBJTNBZ2lsYWRuYXZvdA==" repo-name="DEMO-one-app" doc-type="overview"><sup>Powered by [Swimm](/)</sup></SwmMeta>
