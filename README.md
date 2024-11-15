# 🛠 Actions
[![CI](https://github.com/GEWIS/actions/actions/workflows/validate-workflows.yml/badge.svg?branch=main)](https://github.com/GEWIS/actions/actions/workflows/ci.yml)
[![Latest Release](https://img.shields.io/github/v/tag/GEWIS/actions?label=Latest)](https://github.com/GEWIS/actions/tags)
![License](https://img.shields.io/github/license/GEWIS/actions)

Common and Reusable GitHub Actions for the GEWIS organization.

## Docker build
Workflow for building a docker image.

### Example
```yaml
name: Docker Build

on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main

jobs:
  dockerize:
    uses: GEWIS/actions/.github/workflows/docker-build.yml@v0.0.2
    with:
      projects: '["."]'
```


## Semantic release
Workflow for building Docker images and releasing them. Includes versioning using `semantic-release-action`.

### Inputs

| Input name      | Description                                              | Required | Types/values | Default value |
|-----------------|----------------------------------------------------------|----------|--------------|---------------|
| projects        | A comma-separated list of projects to release.           | &#x2611; | `string`     | N/A           |
| docker-registry | The docker registry to push the build image to.          | &#x2611; | `string`     | N/A           |
| docker-paths    | The docker namespace, project and image name to push to. | &#x2611; | `string`     | N/A           |
| env_file        | Contents of the environment file to use during building. | &#x2610; | `string`     | N/A           |

### Secrets
| Secret name       | Secret value                               | 
|-------------------|--------------------------------------------|
| REGISTRY_USERNAME | The username used to push to the registry. | 
| REGISTRY_PASSWORD | The password used to push to the registry. |
### Example

```yaml
name: Semantic release

on:
  pull_request:
    branches: 
    - main

jobs:
  build-and-lint:
    uses: GEWIS/actions/.github/workflows/semantic-release.yml@v0.0.2
    with:
      projects: '["."]'
      docker_registry: 'abc.docker-registry.gewis.nl'
      docker_tag: 'nc/aurora/core'
```

## Lint and build
Workflow for linting, formatting, testing and building Typescript projects.

### Inputs

| Input name        | Description                                                  | Required | Types/values           | Default value |
|-------------------|--------------------------------------------------------------|----------|------------------------|---------------|
| working-directory | The directory where the TypeScript project is located.       | &#x2610; | `string`               | `.`           |
| node-version      | The version of Node.js to use.                               | &#x2610; | `18.x`, `20.x`, `22.x` | `20.x`        |
| package-manager   | The package manager to use.                                  | &#x2610; | `npm`, `yarn`          | `npm`         |
| artifact-name     | The name of the artifact to use.                             | &#x2610; | `string`               | N/A           |
| artifact-path     | The path where the artifact should be stored.                | &#x2610; | `string`               | N/A           |
| prepare-command   | The command to run before building (and and after checkout). | &#x2610; | `string`               | N/A           |
| cleanup-command   | The command to run after building.                           | &#x2610; | `string`               | N/A           |
| lint              | Whether to lint the project.                                 | &#x2610; | `boolean`              | `true`        |
| format            | Whether to format the project.                               | &#x2610; | `boolean`              | `false`       |
| test              | Whether to run tests.                                        | &#x2610; | `boolean`              | `false`       |
| build             | Whether to build the project.                                | &#x2610; | `boolean`              | `true`        |

### Example

```yaml
name: Lint and build

on:
  push:
    branches: 
    - develop
  pull_request:
    branches: 
    - develop

jobs:
  build-and-lint:
    uses: GEWIS/actions/.github/workflows/typescript-lint-and-build.yml@v0.0.2
    with:
      node-version: '22.x'
      package-manager: 'yarn'
      format: true
```

## Versioning
Workflow for versioning a project using `semantic-release-action`.

### Example
```yaml
name: Versioning

on:
  pull_request:
    branches: 
    - main

jobs:
  build-and-lint:
    uses: GEWIS/actions/.github/workflows/versioning.yml@v0.0.2
```

