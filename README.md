# 🛠 Actions

[![CI](https://github.com/GEWIS/actions/actions/workflows/validate-workflows.yml/badge.svg?branch=main)](https://github.com/GEWIS/actions/actions/workflows/ci.yml)
[![Latest Release](https://img.shields.io/github/v/tag/GEWIS/actions?label=Latest)](https://github.com/GEWIS/actions/tags)
![License](https://img.shields.io/github/license/GEWIS/actions)

Common and Reusable GitHub Actions for the GEWIS organization.

> [!NOTE]
> All inputs for the workflows should be provided as strings. However, sometimes a certain structure is expected. These are noted in the "Options" columns.

## Lint and build

Workflow for linting, formatting, testing and building projects. Implemented for both `npm/yarn` and `golang`.

```yaml
jobs:
  build-and-lint-yarn:
    uses: GEWIS/actions/.github/workflows/lint-and-build-yarn.yml@v1
    with:
      node-version: "22.x"
      format: true

  build-and-lint-npm:
    uses: GEWIS/actions/.github/workflows/lint-and-build-npm.yml@v1
    with:
      node-version: "20.x"
      test: true

  build-and-lint-go:
    uses: GEWIS/actions/.github/workflows/lint-and-build-go.yml@v1
    with:
      go-version: "1.24"
      test: true
```

### Inputs

| Input name        | Description                                              | Required | Options         | Default (npm/yarn) | Default (go) |
| ----------------- | -------------------------------------------------------- | -------- | --------------- | ------------------ | ------------ |
| working-directory | The directory where the project is located.              | &#x2610; |                 | `.`                | `.`          |
| node-version      | The version of Node.js to use.                           | &#x2610; | `20.x`, `22.x`  | `20.x`             | —            |
| go-version        | The version of Golang to use.                            | &#x2610; | `^1.17.0`       | —                  | `^1.24.0`    |
| artifact-name     | The name of the artifact to use.                         | &#x2610; |                 |                    |              |
| artifact-path     | The path where the artifact should be stored.            | &#x2610; |                 |                    |              |
| prepare-command   | The command to run before building (and after checkout). | &#x2610; |                 |                    |              |
| cleanup-command   | The command to run after building.                       | &#x2610; |                 |                    |              |
| lint              | Whether to lint the project.                             | &#x2610; | `true`, `false` | `true`             | `true`       |
| format            | Whether to format the project.                           | &#x2610; | `true`, `false` | `false`            | `true`       |
| test              | Whether to run tests.                                    | &#x2610; | `true`, `false` | `false`            | `true`       |
| build             | Whether to build the project.                            | &#x2610; | `true`, `false` | `true`             | `true`       |

## Docker build

Workflow for building a docker image. Useful for, for example, testing if a container build succeeds on a develop branch.

```yaml
jobs:
  dockerize:
    uses: GEWIS/actions/.github/workflows/docker-build.yml@v1
    with:
      projects: '["."]'
```

### Inputs

| Input name      | Description                                            | Required | Options | Default value |
| --------------- | ------------------------------------------------------ | -------- | ------- | ------------- |
| projects        | Comma-separated list of projects to release.           | &#x2611; |         |               |
| docker-registry | Docker registry for base images behind authentication. | &#x2610; |         |               |

### Secrets

| Secret name       | Secret value                               |
| ----------------- | ------------------------------------------ |
| REGISTRY_USERNAME | The username used for the custom registry. |
| REGISTRY_PASSWORD | The password used for the custom registry. |

## Docker release

Workflow for building Docker images and releasing them. Can be used to push to GitHub and any other registry.

```yaml
jobs:
  build-and-lint:
    uses: GEWIS/actions/.github/workflows/docker-release.yml@v1
    with:
      projects: '["."]'
      docker-registry: "abc.docker-registry.gewis.nl"
      docker-paths: '["nc/aurora/core"]'
      version: "4.0.40"
      github-registry: "true"
```

### Inputs

| Input name      | Description                                                              | Required | Options         | Default value |
| --------------- | ------------------------------------------------------------------------ | -------- | --------------- | ------------- |
| projects        | Comma-separated list of projects to release.                             | &#x2611; |                 |               |
| version         | Version of the Docker release.                                           | &#x2611; |                 |               |
| docker-paths    | The docker namespace, project and image name to push to.                 | &#x2611; |                 |               |
| docker-registry | The docker registry to push the build image to.                          | &#x2610; |                 |               |
| github-registry | Whether to push the image to the GitHub registry.                        | &#x2610; | `true`, `false` | `false`       |
| env-file        | Contents of the environment file to use during building.                 | &#x2610; |                 |               |
| build-contexts  | Comma-separated extra build contexts to pass to the docker build command | &#x2610; |                 |               |

### Secrets

| Secret name       | Secret value                                      |
| ----------------- | ------------------------------------------------- |
| REGISTRY_USERNAME | The username used to push to the custom registry. |
| REGISTRY_PASSWORD | The password used to push to the custom registry. |

## Docker release (GHCR only)

Workflow for building Docker images and releasing them to GitHub Container Registry only. Does not require registry secrets.

```yaml
jobs:
  release:
    uses: GEWIS/actions/.github/workflows/docker-release-ghcr.yml@v1
    with:
      projects: '["."]'
      docker-paths: '["gewis/aurora/core"]'
      version: "4.0.40"
```

### Inputs

| Input name   | Description                                                      | Required | Options | Default value |
| ------------ | ---------------------------------------------------------------- | -------- | ------- | ------------- |
| projects     | JSON array of project contexts to build, e.g. `["."]`.           | &#x2611; |         |               |
| version      | Version tag for the Docker release.                              | &#x2611; |         |               |
| docker-paths | JSON array of Docker paths matching projects.                    | &#x2611; |         |               |
| env-file     | Contents of the .env file to place in each project during build. | &#x2610; |         |               |

## NPM release

Workflow for releasing a NPM package on npmjs.com.

> [!NOTE]
> Make sure to include `prepublishOnly: 'yarn install --frozen-lockfile && yarn build'` or a `prepublishOnly: npm ci && npm run build` in your `package.json` to ensure the package is built before publishing. Since the workflow won't build by itself.

```yaml
jobs:
  build-and-lint:
    uses: GEWIS/actions/.github/workflows/npm-release.yml@v1
    with:
      version: "4.0.40"
      lerna: "true"
```

### Inputs

| Input name        | Description                               | Required | Options         | Default value |
| ----------------- | ----------------------------------------- | -------- | --------------- | ------------- |
| node-version      | The version of Node.js to use.            | &#x2610; | `20.x`, `22.x`  | `20.x`        |
| version           | Version of the NPM release.               | &#x2611; |                 |               |
| lerna             | Whether the project is a lerna project.   | &#x2610; | `true`, `false` | `false`       |
| working-directory | The subdirectory to run npm publish from. | &#x2610; |                 | `.`           |

### Secrets

| Secret name | Description                             | Required |
| ----------- | --------------------------------------- | -------- |
| NPM_TOKEN   | The token used to publish to npmjs.com. | &#x2611; |

## Versioning

Workflow for versioning a project using `semantic-release-action`.

```yaml
jobs:
  build-and-lint:
    uses: GEWIS/actions/.github/workflows/versioning.yml@v1
```

### Inputs

| Input name | Description                                          | Required | Options  | Default value |
| ---------- | ---------------------------------------------------- | -------- | -------- | ------------- |
| dry-run    | Whether to run a dry run of the semantic versioning. | &#x2610; | `string` |               |

### Outputs

| Output name  | Description                               | Options  | Default value |
| ------------ | ----------------------------------------- | -------- | ------------- |
| next-version | The next semantic version of the project. | `string` | ""            |

## Examples

Example workflows for building, versioning and releasing a project can be found in the [examples](./examples) directory.
