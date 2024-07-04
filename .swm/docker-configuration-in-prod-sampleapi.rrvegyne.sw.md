---
title: Docker Configuration in prod-sample/api
---
This document provides a detailed walkthrough of how Docker is utilized in the `prod-sample/api` directory of the project.

<SwmSnippet path="/prod-sample/api/Dockerfile" line="1">

---

# Dockerfile Configuration

The Dockerfile begins by defining an argument `VERSION` which is used to specify the version of the Node.js image that will be used as the base image for the Docker build. The `FROM` directive is then used to pull this Node.js image.

```
ARG VERSION=lts

FROM node:$VERSION as builder
```

---

</SwmSnippet>

<SwmSnippet path="/prod-sample/api/Dockerfile" line="4">

---

The `MAINTAINER` directive is used to specify the team responsible for maintaining this Dockerfile, in this case, the One App Team.

```
MAINTAINER One App Team
```

---

</SwmSnippet>

<SwmSnippet path="/prod-sample/api/Dockerfile" line="6">

---

The `WORKDIR` directive sets the working directory in the Docker container to the root directory (`/`).

```
WORKDIR /
```

---

</SwmSnippet>

<SwmSnippet path="/prod-sample/api/Dockerfile" line="8">

---

The `EXPOSE` directive is used to inform Docker that the container will listen on the specified network ports at runtime. Here, ports 443 and 80 are exposed.

```
EXPOSE 443
EXPOSE 80
```

---

</SwmSnippet>

<SwmSnippet path="/prod-sample/api/Dockerfile" line="10">

---

The `ADD` directive is used to copy new files, directories or remote file URLs from `<src>` and add them to the filesystem of the container at the path `<dest>`. Here, several files including the server.js file, package.json file, and certificate files are added to the root directory of the container.

```
ADD api-cert.pem /api-cert.pem
ADD api-privkey.pem /api-privkey.pem
ADD server.js /server.js
ADD db.json /db.json
ADD package.json /package.json
```

---

</SwmSnippet>

<SwmSnippet path="/prod-sample/api/Dockerfile" line="15">

---

The `RUN` directive is used to execute any commands in a new layer on top of the current image and commit the results. Here, it's used to install the npm packages required for the application.

```
RUN npm install --registry=https://registry.npmjs.org
```

---

</SwmSnippet>

<SwmSnippet path="/prod-sample/api/Dockerfile" line="16">

---

The `ENTRYPOINT` directive allows you to configure a container that will run as an executable. Here, it's configured to run `npm start`. The `CMD` directive provides defaults for an executing container, but here it's left empty.

```
ENTRYPOINT ["npm", "start"]
CMD [""]
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1vbmUtYXBwJTNBJTNBZ2lsYWRuYXZvdA==" repo-name="DEMO-one-app" doc-type="general-build-tool"><sup>Powered by [Swimm](/)</sup></SwmMeta>
