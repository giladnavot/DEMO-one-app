---
title: Getting Started with Server Plugins
---
Server Plugins in DEMO-one-app are modules that add specific functionalities to the server. They are used to extend the server's capabilities and handle various tasks such as health checks, content security policy (CSP), and request handling. For instance, the `healthCheck.js` plugin monitors the server's health status, while the `csp.js` plugin manages the content security policy. The `requestRaw.js` plugin, on the other hand, provides backward compatibility for request and response objects.

<SwmSnippet path="/src/server/plugins/healthCheck.js" line="32">

---

## Health Check Plugin

The health check plugin is used to monitor the health of the server. It provides a decorator for the server response to include a health report. The health report includes process stats, tick delay, and Holocron module health. If the server is not healthy, it responds with a 503 status code.

```javascript
const getProcessStats = (pid) => promisify((cb) => pidusage(pid, cb))();

export const getTickDelay = () => new Promise((resolve) => {
  const time = process.hrtime();
  setImmediate(() => { resolve(process.hrtime(time)); });
});

export const checkForRootModule = () => !!getModule(getClientStateConfig().rootModuleName);

export const verifyThresholds = ({
  process: { cpu, memory, tickDelay },
  holocron: { rootModuleExists },
}) => cpu <= 80
  && memory <= 1.4e9
  && tickDelay[0] < 1
  && rootModuleExists;

/**
 * Plugin that creates a reply decorator to respond with health report
 * @param {import('fastify').FastifyInstance} fastify instance
 * @param {object} _opts empty options object
```

---

</SwmSnippet>

<SwmSnippet path="/src/server/plugins/requestRaw.js" line="1">

---

## Request Raw Plugin

The request raw plugin is used for backward compatibility with ExpressJS. It adds properties to the request object that were available in ExpressJS, such as 'req' and 'res'. This allows developers to migrate from ExpressJS to Fastify without changing their application code.

```javascript
/*
 * Copyright 2024 American Express Travel Related Services Company, Inc.
 *
 * Licensed under the Apache License, Version 2.0 (the "License");
 * you may not use this file except in compliance with the License.
 * You may obtain a copy of the License at
 *
 * http://www.apache.org/licenses/LICENSE-2.0
 *
 * Unless required by applicable law or agreed to in writing, software
 * distributed under the License is distributed on an "AS IS" BASIS,
 * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express
 * or implied. See the License for the specific language governing
 * permissions and limitations under the License.
 */

import fp from 'fastify-plugin';

/**
 * Fastify Plugin for req/res exposure backward compatibility
 * @param {import('fastify').FastifyInstance} fastify Fastify instance
```

---

</SwmSnippet>

<SwmSnippet path="/src/server/plugins/csp.js" line="1">

---

## Content Security Policy (CSP) Plugin

The CSP plugin is used to enhance the security of the server. It adds a Content-Security-Policy header to the server response, which helps to prevent cross-site scripting (XSS) attacks. The policy can be updated dynamically based on the environment and request.

```javascript
/*
 * Copyright 2019 American Express Travel Related Services Company, Inc.
 *
 * Licensed under the Apache License, Version 2.0 (the "License");
 * you may not use this file except in compliance with the License.
 * You may obtain a copy of the License at
 *
 * http://www.apache.org/licenses/LICENSE-2.0
 *
 * Unless required by applicable law or agreed to in writing, software
 * distributed under the License is distributed on an "AS IS" BASIS,
 * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express
 * or implied. See the License for the specific language governing
 * permissions and limitations under the License.
 */

import fp from 'fastify-plugin';
import { v4 as uuidV4 } from 'uuid';
import { getIp } from '../utils/getIP';

function parsePolicy(headerValue) {
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1vbmUtYXBwJTNBJTNBZ2lsYWRuYXZvdA==" repo-name="DEMO-one-app" doc-type="overview"><sup>Powered by [Swimm](/)</sup></SwmMeta>
