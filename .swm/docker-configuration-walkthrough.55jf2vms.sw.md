---
title: Docker Configuration Walkthrough
---
This document provides a detailed walkthrough of how Docker is used in the DEMO-one-app repository. It focuses on the Dockerfile configuration and explains each step involved in the process.

<SwmSnippet path="/Dockerfile" line="1">

---

# Dockerfile Configuration

The Dockerfile begins by specifying the base image for the build. Here, it's a Node.js image with a version specified by the `VERSION` argument. The `builder` alias is assigned to this stage of the multi-stage build process. The working directory is set to `/opt/build`.

```
ARG VERSION=lts
# Use the pre-baked fat node image only in the builder
# which includes build utils preinstalled (e.g. gcc, make, etc).
# This will result in faster and reliable One App docker image
# builds as we do not have to run apk installs for alpine.
FROM node:$VERSION as builder
```

---

</SwmSnippet>

<SwmSnippet path="/Dockerfile" line="8">

---

The global installation of npm version 9.9.3 is performed. Then, the application files are copied to the working directory in the Docker image.

```
RUN npm install -g npm@9.9.3 --registry=https://registry.npmjs.org
COPY --chown=node:node ./ /opt/build
```

---

</SwmSnippet>

<SwmSnippet path="/Dockerfile" line="10">

---

The dependencies are installed using `npm ci` command. A development build is created and copied to the `/opt/one-app/development` directory in the Docker image.

```
# npm ci does not run postinstall with root account
RUN NODE_ENV=development npm ci --build-from-source
# npm ci does not run postinstall with root account
# which is why there is a dev build
RUN NODE_ENV=development npm run build && \
    mkdir -p /opt/one-app/development && \
    chown node:node /opt/one-app/development && \
    cp -r /opt/build/. /opt/one-app/development
```

---

</SwmSnippet>

<SwmSnippet path="/Dockerfile" line="19">

---

A production build is created, and unnecessary dependencies are pruned. The build artifacts and necessary files are moved to the `/opt/one-app/production` directory in the Docker image.

```
RUN NODE_ENV=production npm run build && \
    NODE_ENV=production npm prune && \
    mkdir -p /opt/one-app/production && \
    chown node:node /opt/one-app/production && \
    mv /opt/build/LICENSE.txt /opt/one-app/production && \
    mv /opt/build/node_modules /opt/one-app/production && \
    mv /opt/build/scripts /opt/one-app/production && \
    mv /opt/build/package.json /opt/one-app/production && \
    mv /opt/build/lib /opt/one-app/production && \
    mv /opt/build/build /opt/one-app/production && \
    mv /opt/build/bundle.integrity.manifest.json /opt/one-app/production && \
    mv /opt/build/.build-meta.json /opt/one-app/production
```

---

</SwmSnippet>

<SwmSnippet path="/Dockerfile" line="33">

---

A new stage is started with the Node.js Alpine image. The `tini` utility is installed and set as the entrypoint to handle kernel signals.

```
FROM node:$VERSION-alpine as node-tini
RUN apk add --no-cache tini
ENTRYPOINT ["/sbin/tini", "--"]
```

---

</SwmSnippet>

<SwmSnippet path="/Dockerfile" line="39">

---

The development image is built from the `node-tini` image. It sets the `USER` environment variable, exposes several ports for local development servers, and sets the working directory to `/opt/one-app`. The `scripts/start.sh` script is set as the command to run when the container starts.

```
FROM node-tini as development
ARG USER
ENV USER ${USER:-node}
ENV NODE_ENV=development
# exposing these ports as they are default for all the local dev servers
# see src/server/config/env/runtime.js
EXPOSE 3000
EXPOSE 3001
EXPOSE 3002
EXPOSE 3005
WORKDIR /opt/one-app
RUN chown node:node /opt/one-app
USER $USER
CMD ["scripts/start.sh"]
COPY --from=builder --chown=node:node /opt/one-app/development ./
```

---

</SwmSnippet>

<SwmSnippet path="/Dockerfile" line="57">

---

The production image is built similarly to the development image, but it uses the production build artifacts from the `builder` stage.

```
FROM node-tini as production
ARG USER
ENV USER ${USER:-node}
ENV NODE_ENV=production
# exposing these ports as they are defaults for one app and the prom metrics server
# see src/server/config/env/runtime.js
EXPOSE 3000
EXPOSE 3005
WORKDIR /opt/one-app
USER $USER
CMD ["scripts/start.sh"]
COPY --from=builder --chown=node:node /opt/one-app/production ./
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1vbmUtYXBwJTNBJTNBZ2lsYWRuYXZvdA==" repo-name="DEMO-one-app" doc-type="general-build-tool"><sup>Powered by [Swimm](/)</sup></SwmMeta>
