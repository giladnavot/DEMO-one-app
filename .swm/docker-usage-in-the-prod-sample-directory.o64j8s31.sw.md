---
title: Docker Usage in the Prod-Sample Directory
---
This document provides a detailed walkthrough of how Docker is used in the prod-sample directory of the DEMO-one-app repository. It focuses on the Docker configuration files and their respective roles in building and running the application.

<SwmSnippet path="/prod-sample/api/Dockerfile" line="1">

---

# Dockerfile Configuration

The Dockerfile in the prod-sample/api directory sets up the environment for the API service. It starts with a Node.js base image and exposes ports 443 and 80. It also adds necessary files and installs dependencies using npm. The entrypoint is set to start the application using npm.

```
ARG VERSION=lts

FROM node:$VERSION as builder
MAINTAINER One App Team

WORKDIR /

EXPOSE 443
EXPOSE 80
ADD api-cert.pem /api-cert.pem
ADD api-privkey.pem /api-privkey.pem
ADD server.js /server.js
ADD db.json /db.json
ADD package.json /package.json
RUN npm install --registry=https://registry.npmjs.org
ENTRYPOINT ["npm", "start"]
CMD [""]
```

---

</SwmSnippet>

<SwmSnippet path="/prod-sample/docker-compose.yml" line="1">

---

# Docker Compose Configuration

The docker-compose.yml file in the prod-sample directory defines multiple services including one-app, fast-api, slow-api, extra-slow-api, nginx, selenium-chrome, and otel-collector. Each service has its own configuration including build context, ports, volumes, networks, and dependencies. The one-app service uses an environment file for configuration and a script for the entrypoint.

```yaml
version: '3'
networks:
  one-app-at-test-network:
services:
  one-app:
    build:
      context: ../
      args:
        - http_proxy
        - https_proxy
        - no_proxy
    image: 'one-app:at-test'
    expose:
      - '8443'
      - '3005'
    ports:
      - '8443:8443'
      - '3005:3005'
    volumes:
      - './one-app/one-app-cert.pem:/opt/cert.pem'
      - './one-app/one-app-privkey.pem:/opt/key.pem'
```

---

</SwmSnippet>

<SwmSnippet path="/prod-sample/docker-compose.override.yml" line="1">

---

# Docker Compose Override Configuration

The docker-compose.override.yml file in the prod-sample directory provides additional configuration for the one-app service. It specifies an environment file and a different entrypoint script.

```yaml
version: '3'

services:
  one-app:
    env_file: ./one-app/base.env
    entrypoint: scripts/start.sh

```

---

</SwmSnippet>

<SwmSnippet path="/prod-sample/docker-compose.debug.yml" line="1">

---

# Docker Compose Debug Configuration

The docker-compose.debug.yml file in the prod-sample directory provides configuration for debugging. It opens additional ports and sets a different entrypoint for the one-app service.

```yaml
version: '3'

services:
  one-app:
    ports:
      - "3000:3000"
      # node inspector
      - "9229:9229"
    env_file: ./one-app/debug.env
    entrypoint: scripts/start.sh --inspect=0.0.0.0

```

---

</SwmSnippet>

<SwmSnippet path="/__performance__/docker-compose.prod-sample.yml" line="1">

---

# Docker Compose for Performance Testing

The docker-compose.prod-sample.yml file in the **performance** directory extends the base docker-compose file to use the prod-sample one-app network for performance testing. It provides additional configuration for the prometheus and k6 services.

```yaml
version: '3.4'
# extend base docker-compose to use prod sample one app network
networks:
  prod-sample_one-app-at-test-network:
    external: true
services:
  prometheus:
    networks:
      - prod-sample_one-app-at-test-network
  k6:
    networks:
      - prod-sample_one-app-at-test-network
    environment:
      - TARGET_URL=https://one-app:8443/healthy-frank/ssr-frank
      - TARGET_BASE_URL=https://one-app:8443
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1vbmUtYXBwJTNBJTNBZ2lsYWRuYXZvdA==" repo-name="DEMO-one-app" doc-type="general-build-tool"><sup>Powered by [Swimm](/)</sup></SwmMeta>
