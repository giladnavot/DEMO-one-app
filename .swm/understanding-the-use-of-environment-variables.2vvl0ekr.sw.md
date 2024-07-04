---
title: Understanding the Use of Environment Variables
---
Environment Variables in DEMO-one-app are used to configure various aspects of the application. They are read from the system's environment and can be used to set values that are needed by the app. For instance, `process.env.ONE_CLIENT_ROOT_MODULE_NAME` is used to specify the root module of the application. Similarly, `process.env.NODE_ENV` is used to determine the environment in which the application is running, such as 'development' or 'production'. These variables can be set in the system or through the command line when starting the application.

<SwmSnippet path="/src/server/config/env/argv.js" line="20">

---

# Accessing Environment Variables

Here, the `ONE_CLIENT_ROOT_MODULE_NAME` environment variable is accessed using `process.env`. This variable is used to specify the name of the module that serves as the entry point to the application.

```javascript
const rootModuleNameEnvVarValue = process.env.ONE_CLIENT_ROOT_MODULE_NAME;
```

---

</SwmSnippet>

<SwmSnippet path="/src/server/config/env/argv.js" line="22">

---

# Using Environment Variables for Conditional Logic

Environment variables can also be used to control the flow of the application. Here, the `NODE_ENV` environment variable is checked to determine if the application is running in development mode.

```javascript
if (process.env.NODE_ENV === 'development') {
```

---

</SwmSnippet>

<SwmSnippet path="/src/server/config/env/argv.js" line="81">

---

# Setting Default Values Based on Environment Variables

Environment variables can also be used to set default values for application settings. Here, the `log-format` option is set to 'friendly' if the application is running in development mode, and 'machine' otherwise.

```javascript
    default: process.env.NODE_ENV === 'development' ? 'friendly' : 'machine',
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1vbmUtYXBwJTNBJTNBZ2lsYWRuYXZvdA==" repo-name="DEMO-one-app" doc-type="overview"><sup>Powered by [Swimm](/)</sup></SwmMeta>
