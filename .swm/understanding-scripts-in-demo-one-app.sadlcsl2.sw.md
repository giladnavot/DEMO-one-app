---
title: Understanding Scripts in DEMO-one-app
---
Scripts in the DEMO-one-app repository are used to automate various tasks in the development process. They are written in JavaScript and are located in the 'scripts' directory. These scripts include utilities for running commands in modules, building modules, deploying modules, setting development endpoints, and more. There are also scripts for performance testing located in the '**performance**/scripts' directory. These scripts are used to simulate different types of load on the application for testing purposes.

<SwmSnippet path="/scripts/run-in-sample-modules.js" line="29">

---

# Running Commands in Modules

The `runBatchedModuleCommand` function is used to run a specified command in each module. It uses the `getModuleVersionPaths` function to get the paths to all modules, and then runs the command in each module using the `runCommandInModule` function.

```javascript
const runBatchedModuleCommand = async (command) => {
  const moduleVersionPaths = await getModuleVersionPaths();
  basicBatchedTask(
    moduleVersionPaths,
    (currentBatch) => Promise.all(currentBatch.map((modulePath) => {
      const { moduleName, moduleVersion, directory } = getModuleDetailsFromPath(modulePath);
      return runCommandInModule({
        moduleName,
        moduleVersion,
        directory,
        envVars: sanitizedEnvVars,
      }, command);
    }))
  );
};
```

---

</SwmSnippet>

<SwmSnippet path="/scripts/utils.js" line="58">

---

# Running Commands in a Module

The `runCommandInModule` function is used to run a specified command in a single module. It logs the start and end time of the command, and any errors that occur while the command is running.

```javascript
const runCommandInModule = async (
  {
    directory,
    moduleName,
    moduleVersion,
    envVars = {},
  },
  command
) => {
  console.time(`${moduleName}@${moduleVersion}`);
  console.log(`⚙️ Performing ${command} on ${moduleName}@${moduleVersion}...`);
  try {
    await promisifySpawn(command, {
      cwd: directory,
      shell: true,
      env: {
        ...envVars,
        NODE_ENV: 'development',
        NPM_CONFIG_PRODUCTION: false,
      },
    });
```

---

</SwmSnippet>

<SwmSnippet path="/scripts/set-dev-endpoints.js" line="1">

---

# Setting Development Endpoints

The `set-dev-endpoints.js` script is used to set the development endpoints for the application. It validates the endpoints file, creates a symbolic link to it in the `.dev/endpoints` directory, and logs any errors that occur.

```javascript
#!/usr/bin/env node

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

const path = require('node:path');
const fs = require('node:fs');
const os = require('node:os');
```

---

</SwmSnippet>

<SwmSnippet path="/scripts/build-sample-modules.js" line="1">

---

# Building Sample Modules

The `build-sample-modules.js` script is used to build the sample modules for the application. It installs the necessary dependencies, runs the build command, and updates the module map.

```javascript
#!/usr/bin/env node

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

const { existsSync, promises: fs } = require('node:fs');
const path = require('node:path');
const { argv } = require('yargs');
```

---

</SwmSnippet>

<SwmSnippet path="/scripts/deploy-prod-sample-module.js" line="1">

---

# Deploying Production Sample Module

The `deploy-prod-sample-module.js` script is used to deploy a production sample module. It builds the module, deploys it to the production sample CDN, and updates the module map.

```javascript
#!/usr/bin/env node

/*
 * Copyright 2020 American Express Travel Related Services Company, Inc.
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

const fs = require('node:fs/promises');
const path = require('node:path');
const { argv } = require('yargs');
```

---

</SwmSnippet>

# Script Functions

This section will cover the main functions used in the scripts of the DEMO-one-app project.

<SwmSnippet path="/scripts/run-in-sample-modules.js" line="29">

---

## runBatchedModuleCommand

The `runBatchedModuleCommand` function is used to run a command in a batch of modules. It first gets the module version paths, then runs the command in each module in the batch.

```javascript
const runBatchedModuleCommand = async (command) => {
  const moduleVersionPaths = await getModuleVersionPaths();
  basicBatchedTask(
    moduleVersionPaths,
    (currentBatch) => Promise.all(currentBatch.map((modulePath) => {
      const { moduleName, moduleVersion, directory } = getModuleDetailsFromPath(modulePath);
      return runCommandInModule({
        moduleName,
        moduleVersion,
        directory,
        envVars: sanitizedEnvVars,
      }, command);
    }))
  );
};
```

---

</SwmSnippet>

<SwmSnippet path="/scripts/utils.js" line="136">

---

## getModuleVersionPaths

The `getModuleVersionPaths` function is used to get the paths of all module versions. It reads the sample module versions and pushes the path of each version to an array.

```javascript
const getModuleVersionPaths = async () => {
  const modulePaths = [];
  const sampleModuleVersions = await getSampleModuleVersions();
  Object.entries(sampleModuleVersions).forEach(([moduleName, versions]) => {
    versions.forEach((version) => {
      const pathToModuleVersion = path.resolve(sampleModulesDir, moduleName, version);
      modulePaths.push(pathToModuleVersion);
    });
  });
  return modulePaths;
};
```

---

</SwmSnippet>

<SwmSnippet path="/scripts/utils.js" line="155">

---

## basicBatchedTask

The `basicBatchedTask` function is used to perform a task in batches. It divides the list of items into batches and performs the task on each batch.

```javascript
const basicBatchedTask = async (list, task, batchSize = 5) => {
  const totalBatches = Math.max(Math.round(list.length / batchSize), 1);
  const batches = [];
  list.forEach((item, index) => {
    const batchNumber = index % totalBatches;
    batches[batchNumber] = [item, ...batches[batchNumber] ? batches[batchNumber] : []];
    return batches;
  }, []);
  let results = [];
  for (const currentBatch of batches) {
    const result = await task(currentBatch);
    results = [...results, ...result];
  }
  return results;
};
```

---

</SwmSnippet>

<SwmSnippet path="/scripts/utils.js" line="148">

---

## getModuleDetailsFromPath

The `getModuleDetailsFromPath` function is used to get the details of a module from its path. It returns an object containing the module name, module version, and directory.

```javascript
const getModuleDetailsFromPath = (pathToModule) => {
  const directory = path.resolve(pathToModule);
  const moduleVersion = path.basename(directory);
  const moduleName = path.basename(path.resolve(directory, '..'));
  return { moduleName, moduleVersion, directory };
};
```

---

</SwmSnippet>

# Script Functions

Understanding Script Functions

<SwmSnippet path="/scripts/build-one-app-docker-setup.js" line="38">

---

## buildApiImages

The `buildApiImages` function is responsible for building Docker images for the APIs. It uses Docker Compose to build the images for `fast-api`, `slow-api`, and `extra-slow-api`.

```javascript
const buildApiImages = async () => {
  console.time('Docker API Images Build');
  console.log('🛠  Building fast-api, slow-api, and extra-slow-api Docker images');
  try {
    await promisifySpawn(`docker compose build --build-arg VERSION=${nodeVersion} --no-cache --parallel fast-api slow-api extra-slow-api`, { shell: true, cwd: sampleProdDir, env: { ...sanitizedEnvVars } });
  } catch (error) {
    console.log('🚨 Docker API Images could not be built!\n');
    throw error;
  }
  console.log('✅ Docker API images built\n');
  console.timeEnd('Docker API Images Build');
};
```

---

</SwmSnippet>

<SwmSnippet path="/scripts/build-one-app-docker-setup.js" line="51">

---

## buildOneAppImage

The `buildOneAppImage` function is responsible for building the Docker image for One App. It uses Docker Compose to build the `one-app` image.

```javascript
const buildOneAppImage = async () => {
  console.time('Docker Build One App Image');
  console.log('🛠  Building one-app Docker image');
  try {
    await promisifySpawn(`docker compose build --build-arg VERSION=${nodeVersion} --no-cache one-app`, { shell: true, cwd: sampleProdDir, env: { ...sanitizedEnvVars } });
  } catch (error) {
    console.log('🚨 Docker could not build the one-app image!\n');
    throw error;
  }
  console.log('✅ Docker built One App Image \n');
  console.timeEnd('Docker Build One App Image');
};
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1vbmUtYXBwJTNBJTNBZ2lsYWRuYXZvdA==" repo-name="DEMO-one-app" doc-type="overview"><sup>Powered by [Swimm](/)</sup></SwmMeta>
