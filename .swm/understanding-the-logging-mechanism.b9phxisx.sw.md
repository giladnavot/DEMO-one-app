---
title: Understanding the Logging Mechanism
---
Logging in DEMO-one-app is handled by a custom logger created using the Pino logging library. This logger is configured based on the application's environment and log format settings. It supports different configurations for development and production environments, and can also integrate with OpenTelemetry for distributed tracing. The logger provides various log levels such as debug, info, warn, and error, and it's used throughout the application to log important information and errors.

<SwmSnippet path="/src/server/utils/logging/logger.js" line="26">

---

## Logger Configuration

The `createLogger` function is responsible for creating and configuring the logger. It determines whether to use the production configuration based on the `NODE_ENV` environment variable and the `logFormat` command-line argument. It then uses the `getPinoConfig` and `getTransport` helper functions to configure the logger's settings and transport streams.

```javascript
export function createLogger() {
  const useProductionConfig = !!(argv.logFormat === 'machine' || process.env.NODE_ENV !== 'development');

  const getPinoConfig = () => {
    if (process.env.OTEL_EXPORTER_OTLP_LOGS_ENDPOINT) return deepmerge(baseConfig, otelConfig);
    if (useProductionConfig) return deepmerge(baseConfig, productionConfig);
    return baseConfig;
  };

  const getTransport = () => {
    const transportStreams = [];
    if (process.env.OTEL_EXPORTER_OTLP_LOGS_ENDPOINT) {
      transportStreams.push(createOtelTransport({
        grpc: true,
        console: process.env.NODE_ENV === 'development' && argv.logFormat === 'machine',
      }));
    }
    if (!useProductionConfig) {
      // eslint-disable-next-line global-require -- do not load development logger in production
      transportStreams.push(require('./config/development').default);
    }
```

---

</SwmSnippet>

<SwmSnippet path="/src/server/utils/logging/logger.js" line="35">

---

## Logger Transport

The `getTransport` function determines the transport streams for the logger. If the `OTEL_EXPORTER_OTLP_LOGS_ENDPOINT` environment variable is set, it adds an OpenTelemetry transport stream. If the application is not in production mode, it also adds a development transport stream. The function then returns the appropriate transport stream or a combination of streams using `multistream`.

```javascript
  const getTransport = () => {
    const transportStreams = [];
    if (process.env.OTEL_EXPORTER_OTLP_LOGS_ENDPOINT) {
      transportStreams.push(createOtelTransport({
        grpc: true,
        console: process.env.NODE_ENV === 'development' && argv.logFormat === 'machine',
      }));
    }
    if (!useProductionConfig) {
      // eslint-disable-next-line global-require -- do not load development logger in production
      transportStreams.push(require('./config/development').default);
    }

    if (transportStreams.length === 1) return transportStreams[0];
    if (transportStreams.length > 1) return multistream(transportStreams);
    return undefined;
  };
```

---

</SwmSnippet>

<SwmSnippet path="/src/server/utils/logging/config/base.js" line="29">

---

## Logger Usage

The `logMethod` function is an example of how the logger can be used. It takes an array of arguments and a logging method, and it logs the arguments using the provided method. If an error is included in the arguments, it is logged separately with its message.

```javascript
    logMethod(inputArgs, method) {
      if (inputArgs.length > 1) {
        let error;
        if (inputArgs[0] instanceof Error) error = inputArgs.shift();
        if (inputArgs[inputArgs.length - 1] instanceof Error) error = inputArgs.pop();
        if (error) {
          const message = util.format(...inputArgs);
          return method.call(this, { error, message });
        }
      }
      return method.apply(this, inputArgs);
    },
```

---

</SwmSnippet>

# Logging Functions

This section will cover the main functions involved in the logging process in the DEMO-one-app.

<SwmSnippet path="/src/server/utils/logging/logger.js" line="26">

---

## createLogger

The `createLogger` function is the main function for creating a logger. It determines the configuration to use based on environment variables and command line arguments, and sets up the transport streams for the logger.

```javascript
export function createLogger() {
  const useProductionConfig = !!(argv.logFormat === 'machine' || process.env.NODE_ENV !== 'development');

  const getPinoConfig = () => {
    if (process.env.OTEL_EXPORTER_OTLP_LOGS_ENDPOINT) return deepmerge(baseConfig, otelConfig);
    if (useProductionConfig) return deepmerge(baseConfig, productionConfig);
    return baseConfig;
  };

  const getTransport = () => {
    const transportStreams = [];
    if (process.env.OTEL_EXPORTER_OTLP_LOGS_ENDPOINT) {
      transportStreams.push(createOtelTransport({
        grpc: true,
        console: process.env.NODE_ENV === 'development' && argv.logFormat === 'machine',
      }));
    }
    if (!useProductionConfig) {
      // eslint-disable-next-line global-require -- do not load development logger in production
      transportStreams.push(require('./config/development').default);
    }
```

---

</SwmSnippet>

<SwmSnippet path="/src/server/utils/logging/config/otel.js" line="30">

---

## createOtelTransport

The `createOtelTransport` function is used to create a transport for OpenTelemetry logs. It can create a gRPC exporter, a console exporter, or both, based on the provided options.

```javascript
export function createOtelTransport({
  grpc: grpcExporter = true,
  console: consoleExporter = false,
} = {}) {
  if (!grpcExporter && !consoleExporter) {
    console.error('OTEL DISABLED: attempted to create OpenTelemetry transport without including a processor');
    return undefined;
  }

  let logRecordProcessorOptions = [];

  if (grpcExporter) {
    logRecordProcessorOptions.push({
      recordProcessorType: process.env.INTEGRATION_TEST === 'true' ? 'simple' : 'batch',
      exporterOptions: { protocol: 'grpc' },
    });
  }

  if (consoleExporter) {
    if (process.stdout.isTTY && !process.env.NO_COLOR) process.env.FORCE_COLOR = '1';
    logRecordProcessorOptions.push({
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1vbmUtYXBwJTNBJTNBZ2lsYWRuYXZvdA==" repo-name="DEMO-one-app" doc-type="overview"><sup>Powered by [Swimm](/)</sup></SwmMeta>
