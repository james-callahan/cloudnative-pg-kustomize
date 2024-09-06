# cloudnative-pg-kustomize

This repository is [`kustomize`](https://kustomize.io/) manifests to install and manage an installation of [cloudnative-pg](https://cloudnative-pg.io/).

## Permissions

For operating a cluster, there is a `ClusterRoleBinding` in the `clusterrolebinding` folder. However, if you instead want to take a route of least privilege, you'll want to add a `RoleBinding` to the `cloudnative-pg:manager-edit` in your namespace to the `manager` service account in the namespace where you deployed the operator.

e.g. if you deploy to the `cloudnative-pg` namespace, you'll want to add to your namespace:

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
