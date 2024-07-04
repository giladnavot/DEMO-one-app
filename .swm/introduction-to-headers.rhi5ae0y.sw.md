---
title: Introduction to Headers
---
Headers in this repository refer to HTTP headers that are used in the server-side plugins. These headers are used to control the behavior of the client or provide information about the server or further request/response details. For example, security headers are added to enhance the security of the application, cache headers are used to control caching behavior, and custom headers like 'One-App-Version' are used to provide additional information about the application.

<SwmSnippet path="/src/server/plugins/addSecurityHeaders.js" line="31">

---

## Setting Headers

Here we see several headers being set in the server response. These headers control various security settings.

```javascript
    reply.header('vary', 'Accept-Encoding');
    reply.header('Strict-Transport-Security', 'max-age=63072000; includeSubDomains');
    reply.header('x-dns-prefetch-control', 'off');
    reply.header('x-download-options', 'noopen');
    reply.header('x-permitted-cross-domain-policies', 'none');
    reply.header('X-Content-Type-Options', 'nosniff');
```

---

</SwmSnippet>

<SwmSnippet path="/src/server/plugins/addSecurityHeaders.js" line="38">

---

## Conditional Headers

In this section, we see that headers can be set conditionally based on the request method and URL. This allows for fine-grained control over the response.

```javascript
    if (
      request.method.toLowerCase() !== 'get'
      || (request.method.toLowerCase() === 'get' && matchGetRoutes.includes(request.url))
    ) {
      reply.header('X-Frame-Options', 'DENY');
      reply.header('X-XSS-Protection', '1; mode=block');
      reply.header('Referrer-Policy', process.env.ONE_REFERRER_POLICY_OVERRIDE || 'same-origin');
    } else {
      reply.header('X-Frame-Options', 'SAMEORIGIN');
      reply.header('X-XSS-Protection', '0');
      reply.header('Referrer-Policy', process.env.ONE_REFERRER_POLICY_OVERRIDE || 'no-referrer');
    }
```

---

</SwmSnippet>

<SwmSnippet path="/src/server/plugins/forwardedHeaderParser.js" line="34">

---

## Parsing Headers

Headers can also be read from the request. In this example, the 'forwarded' header is parsed and added to the request object.

```javascript
  fastify.addHook('onRequest', async (request) => {
    if (request.headers.forwarded) {
      request.forwarded = parse(request.headers.forwarded);
    }
```

---

</SwmSnippet>

# Header Functions

This section discusses the functions related to headers in the DEMO-one-app repository.

<SwmSnippet path="/src/server/plugins/addSecurityHeaders.js" line="25">

---

## addSecurityHeaders

The `addSecurityHeaders` function is a Fastify plugin that adds security-related headers to the response. It sets headers such as 'Strict-Transport-Security', 'X-Content-Type-Options', and 'X-Frame-Options'. The function also uses the OpenTelemetry API for tracing.

```javascript
const addSecurityHeaders = (fastify, opts = {}, done) => {
  const matchGetRoutes = opts.matchGetRoutes || [];

  fastify.addHook('onRequest', async (request, reply) => {
    const { tracer } = request.openTelemetry();
    const span = tracer.startSpan('addSecurityHeaders');
    reply.header('vary', 'Accept-Encoding');
    reply.header('Strict-Transport-Security', 'max-age=63072000; includeSubDomains');
    reply.header('x-dns-prefetch-control', 'off');
    reply.header('x-download-options', 'noopen');
    reply.header('x-permitted-cross-domain-policies', 'none');
    reply.header('X-Content-Type-Options', 'nosniff');

    if (
      request.method.toLowerCase() !== 'get'
      || (request.method.toLowerCase() === 'get' && matchGetRoutes.includes(request.url))
    ) {
      reply.header('X-Frame-Options', 'DENY');
      reply.header('X-XSS-Protection', '1; mode=block');
      reply.header('Referrer-Policy', process.env.ONE_REFERRER_POLICY_OVERRIDE || 'same-origin');
    } else {
```

---

</SwmSnippet>

<SwmSnippet path="/src/server/plugins/forwardedHeaderParser.js" line="33">

---

## forwardedHeaderParser

The `forwardedHeaderParser` function is another Fastify plugin that parses the 'Forwarded' HTTP header and injects the parsed value into the request object. The parsing is done by the `parse` function.

```javascript
const forwardedHeaderParser = (fastify, _opts, done) => {
  fastify.addHook('onRequest', async (request) => {
    if (request.headers.forwarded) {
      request.forwarded = parse(request.headers.forwarded);
    }
  });

  done();
};
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1vbmUtYXBwJTNBJTNBZ2lsYWRuYXZvdA==" repo-name="DEMO-one-app" doc-type="overview"><sup>Powered by [Swimm](/)</sup></SwmMeta>
