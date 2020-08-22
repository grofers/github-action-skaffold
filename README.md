# Skaffold Github Action

![License](https://img.shields.io/github/license/hiberbee/github-action-skaffold?style=flat-square)

![Docker Build](https://img.shields.io/docker/cloud/build/hiberb/skaffold?label=Docker%20Hub&style=flat-square)
![GitHub Workflow Status](https://img.shields.io/github/workflow/status/hiberbee/github-action-skaffold/Skaffold?label=Github%20Actions&style=flat-square)

![Docker Tag](https://img.shields.io/docker/v/hiberb/skaffold?label=hiberb%2Fskaffold&style=flat-square)
![Github Workflow](https://img.shields.io/github/v/tag/hiberbee/github-action-skaffold?label=hiberbee%2Fgithub-action-skaffold&style=flat-square)

Skaffold is a command line tool that facilitates continuous development for Kubernetes applications. You can iterate on your application source code locally then deploy to local or remote Kubernetes clusters. Skaffold handles the workflow for building, pushing and deploying your application. It also provides building blocks and describe customizations for a CI/CD pipeline.

This action allows you to execute skaffold commands as Github Action. Repository is self-testable, so you can refer to [Skaffold manifest](skaffold.yaml), [Container Structure tests](structure-test.yaml) and [Github workflow](.github/workflows/skaffold.yml)

See also [Github Action Minikube](https://github.com/hiberbee/github-action-minikube) for better development experience

## Installed versions

- skaffold 1.13.2
- container-structure-test 1.9.0

### Configuration

| Name | Description | Default |
| ---- | ----------- | ------- |
| `skaffold-version` | Set Skaffold version | 1.13.2 |
| `container-structure-test-version` | Set Container Structure Test version | 1.9.0 |

## Inputs

### Required

### Optional

| Name | Description | Default |
| ---- | ----------- | ------- |
| `build-image` | Set Skaffold profile name | n/a |
| `cache-artifacts` | Set to false to disable default caching of artifacts | true |
| `command` | Set Skaffold profile name | version |
| `default-repo` | Default repository value (overrides global config) | n/a |
| `filename` | Path or URL to the Skaffold config file | skaffold.yaml |
| `kube-context` | Deploy to this Kubernetes context | n/a |
| `kubeconfig` | Path to the kubeconfig file to use for CLI requests | n/a |
| `namespace` | Run deployments in the specified namespace | n/a |
| `profile` | Activate profiles by name | n/a |
| `tag` | Set Skaffold profile name | n/a |

## Outputs

### Example

```yaml
on:
  push:
    paths:
      - src/**
      - skaffold.yaml
      - Dockerfile
jobs:
  pipiline:
    runs-on: ubuntu-20.04
    steps:
      - name: Checkout sources
        uses: actions/checkout@v2

      - name: Setup Minikube
        uses: hiberbee/github-action-minikube@latest

      - name: Setup Helm
        uses: hiberbee/github-action-helm@latest

      - name: Authenticate with Docker registry
        run: echo $DOCKER_PASSWORD | docker login harbor.k8s.hiberbee.net -u $DOCKER_USERNAME --password-stdin
        env:
          DOCKER_USERNAME: ${{ secrets.DOCKER_USERNAME }}
          DOCKER_PASSWORD: ${{ secrets.DOCKER_PASSWORD }}

      - name: Run Skaffold pipeline
        uses: hiberbee/github-action-skaffold@latest
        with:
          command: run
          default-repo: harbor.k8s.hiberbee.net/library

      - name: Get Helm releases
        run: helm list

```
