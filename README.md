# Flux2 - Presentation Repository

## Requirements

- [eksctl](https://eksctl.io/)
- [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html)
- [Flux CLI](https://fluxcd.io/docs/get-started/#install-the-flux-cli)

## Cluster Creation

```sh
eksctl create cluster -f cluster.yaml --profile YOUR_AWS_PROFILE
```

Add the context for `flux2-poc-cluster` or name you defined in `cluster.yaml`:

```sh
aws eks update-kubeconfig --name flux2-poc-cluster --profile YOUR_AWS_PROFILE
```

### Flux Installation

```sh
export GITHUB_USER=
export GITHUB_TOKEN=

flux bootstrap github \
  --owner=$GITHUB_USER \
  --repository=flux2 \
  --branch=main \
  --path=./cluster/ \
  --personal
```

This will:

1. Connect to github.com
1. Clone the repository: https://github.com/maxrodrigo/flux2
1. Generate components manifests if not exists.
1. Commit and push manifests.
1. Install components in "flux-system" namespace.
  - `source-controller`
  - `kustomize-controller`
  - `helm-controller`
  - `notification-controller`
1. Configure deploy key
1. Reconcile new resources in the cluster.

See [ways to structuring your repositories](https://fluxcd.io/docs/guides/repository-structure/) for further configuration.
