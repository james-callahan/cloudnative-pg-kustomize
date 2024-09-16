# cloudnative-pg-kustomize

This repository is [`kustomize`](https://kustomize.io/) manifests to install
and manage an installation of [cloudnative-pg](https://cloudnative-pg.io/).

## Usage

### Quick Start

```
kubectl -n cloudnative-pg apply -k github.com/james-callahan/cloudnative-pg
```

### Normal Usage

In your own gitops repository, create a `kustomization.yaml` that looks something like:

```
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization
namespace: cloudnative-pg
resources:
  - github.com/james-callahan/cloudnative-pg?ref=main
  - github.com/james-callahan/cloudnative-pg/clusterrolebinding?ref=main
```

Note that the `webhook` subapplication and top-level application both currently
hard-code a namespace of `cloudnative-pg`.
Hopefully this will one day be removed as an assumption.


## Permissions

For operating a cluster, there is a `ClusterRoleBinding` in the
`clusterrolebinding` folder.
However, if you instead want to take a route of least privilege, you'll want to
add a `RoleBinding` to the `cloudnative-pg:manager-edit` in your namespace to
the `manager` service account in the namespace where you deployed the operator.

Assuming you deploy to the `cloudnative-pg` namespace,
you'll want to add to your namespace:

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
 name: cloudnative-pg
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cloudnative-pg:manager-edit
subjects:
  - kind: ServiceAccount
    name: manager
    namespace: cloudnative-pg
```
