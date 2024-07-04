---
title: Understanding Security Measures
---
Security in DEMO-one-app is implemented through various measures. One of these measures is the use of Content Security Policy (CSP), which is a security layer that helps detect and mitigate certain types of attacks, including Cross Site Scripting (XSS) and data injection attacks. This is evident in the `csp.js` file where the 'Content-Security-Policy' header is set. Additionally, Cross-Origin Resource Sharing (CORS) is conditionally allowed as seen in the `conditionallyAllowCors.js` file, providing another layer of security by controlling which domains can access resources.

<SwmSnippet path="/src/server/plugins/conditionallyAllowCors.js" line="5">

---

# CORS

CORS (Cross-Origin Resource Sharing) is a security measure that restricts how resources on a web page can be requested from another domain. This file contains the implementation of CORS in the application.

```javascript
 * you may not use this file except in compliance with the License.
```

---

</SwmSnippet>

<SwmSnippet path="/src/server/plugins/csp.js" line="5">

---

# Content Security Policy

Content Security Policy (CSP) is a security measure that helps to detect and mitigate certain types of attacks, like Cross Site Scripting (XSS) and data injection attacks. The CSP settings are defined and applied in this file.

```javascript
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
  const policy = {};
  headerValue
    .split(';')
    .map((s) => s.trim())
```

---

</SwmSnippet>

<SwmSnippet path="/src/server/plugins/addFrameOptionsHeader.js" line="5">

---

# Frame Options Header

The Frame Options Header is a security measure that indicates whether a browser should be allowed to render a page in a <frame>, <iframe>, <embed> or <object>. This file contains the implementation of the Frame Options Header in the application.

```javascript
 * you may not use this file except in compliance with the License.
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1vbmUtYXBwJTNBJTNBZ2lsYWRuYXZvdA==" repo-name="DEMO-one-app" doc-type="overview"><sup>Powered by [Swimm](/)</sup></SwmMeta>
