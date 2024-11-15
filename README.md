# 🛠 Actions
[![CI](https://github.com/GEWIS/actions/actions/workflows/validate-workflows.yml/badge.svg?branch=main)](https://github.com/GEWIS/actions/actions/workflows/ci.yml)
[![Latest Release](https://img.shields.io/github/v/tag/GEWIS/actions?label=Latest)](https://github.com/GEWIS/actions/tags)
![License](https://img.shields.io/github/license/GEWIS/actions)

Common and Reusable GitHub Actions for the GEWIS organization.

> [!NOTE]  
> All inputs for the workflows should be provided as strings. However, sometimes a certain structure is expected. These are noted in the "Options" columns.

## Lint and build
Workflow for linting, formatting, testing and building projects. Can be used for both `npm` and `yarn`.

```yaml
jobs:
  build-and-lint-yarn:
    uses: GEWIS/actions/.github/workflows/lint-and-build-npm.yml@v1.0.0
    with:
      node-version: '22.x'
      format: true

  build-and-lint-npm:
    uses: GEWIS/actions/.github/workflows/lint-and-build-yarn.yml@v1.0.0
    with:
      node-version: '20.x'
      test: true
```

### Inputs

| Input name        | Description                                                  | Required | Options                | Default value |
|-------------------|--------------------------------------------------------------|----------|------------------------|---------------|
| working-directory | The directory where the project is located.                  | &#x2610; |                        | `.`           |
| node-version      | The version of Node.js to use.                               | &#x2610; | `18.x`, `20.x`, `22.x` | `20.x`        |
| artifact-name     | The name of the artifact to use.                             | &#x2610; |                        |               |
| artifact-path     | The path where the artifact should be stored.                | &#x2610; |                        |               |
| prepare-command   | The command to run before building (and and after checkout). | &#x2610; |                        |               |
| cleanup-command   | The command to run after building.                           | &#x2610; |                        |               |
| lint              | Whether to lint the project.                                 | &#x2610; | `true`, `false`        | `true`        |
| format            | Whether to format the project.                               | &#x2610; | `true`, `false`        | `false`       |
| test              | Whether to run tests.                                        | &#x2610; | `true`, `false`        | `false`       |
| build             | Whether to build the project.                                | &#x2610; | `true`, `false`        | `true`        |

## Docker build
Workflow for building a docker image. Useful for, for example, testing if a container build succeeds on a develop branch.

```yaml
jobs:
  dockerize:
    uses: GEWIS/actions/.github/workflows/docker-build.yml@v1.0.0
    with:
      projects: '["."]'
```

### Inputs

| Input name | Description                                  | Required | Options | Default value |
|------------|----------------------------------------------|----------|---------|---------------|
| projects   | Comma-separated list of projects to release. | &#x2611; |         |               |

## Docker release
Workflow for building Docker images and releasing them. Can be used to push to GitHub and any other registry.

```yaml
jobs:
  build-and-lint:
    uses: GEWIS/actions/.github/workflows/docker-release.yml@v1.0.0
    with:
      projects: '["."]'
      docker-registry: 'abc.docker-registry.gewis.nl'
      docker-path: 'nc/aurora/core'
      version: '4.0.40'
      github-registry: 'true'
```

### Inputs

| Input name      | Description                                              | Required | Options         | Default value |
|-----------------|----------------------------------------------------------|----------|-----------------|---------------|
| projects        | Comma-separated list of projects to release.             | &#x2611; |                 |               |
| version         | Version of the Docker release.                           | &#x2611; |                 |               |
| docker-registry | The docker registry to push the build image to.          | &#x2611; |                 |               |
| github-registry | Whether to push the image to the GitHub registry.        | &#x2610; | `true`, `false` | `false`       |
| env-file        | Contents of the environment file to use during building. | &#x2610; |                 |               |
| docker-paths    | The docker namespace, project and image name to push to. | &#x2611; |                 |               |

### Secrets

| Secret name        | Secret value                                      | 
|--------------------|---------------------------------------------------|
| REGISTRY\_USERNAME | The username used to push to the custom registry. | 
| REGISTRY\_PASSWORD | The password used to push to the custom registry. |

## NPM release
Workflow for releasing a NPM package on GitHub.

```yaml
jobs:
  build-and-lint:
    uses: GEWIS/actions/.github/workflows/npm-release.yml@v1.0.0
    with:
      version: '4.0.40'
      lerna: 'true'
```

### Inputs

| Input name   | Description                             | Required | Options                | Default value |
|--------------|-----------------------------------------|----------|------------------------|---------------|
| node-version | The version of Node.js to use.          | &#x2610; | `18.x`, `20.x`, `22.x` | `20.x`        |
| version      | Version of the NPM release.             | &#x2611; |                        |               |
| lerna        | Whether the project is a lerna project. | &#x2610; | `true`, `false`        | `false`       |


## Versioning
Workflow for versioning a project using `semantic-release-action`.

```yaml
jobs:
  build-and-lint:
    uses: GEWIS/actions/.github/workflows/versioning.yml@v1.0.0
```

### Inputs

| Input name | Description                                          | Required | Options  | Default value |
|------------|------------------------------------------------------|----------|----------|---------------|
| dry-run    | Whether to run a dry run of the semantic versioning. | &#x2610; | `string` | `.`           |

### Outputs

| Output name  | Description                               | Options  | Default value |
|--------------|-------------------------------------------|----------|---------------|
| next-version | The next semantic version of the project. | `string` | ""            |

## Examples

Example workflows for building, versioning and releasing a project can be found in the [examples](./examples) directory.