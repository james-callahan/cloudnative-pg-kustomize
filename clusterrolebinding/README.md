This `ClusterRoleBinding` can be used to run PostgreSQL clusters in any kubernetes namespace without having to manually grant access on a per-namespace basis.

The reason *not* to use this is that it grants extensive sensitive permissions. It would be prudent to add `RoleBinding`s to the `ClusterRole` only in the namespaces where you want the operator to run.
However, note that secret access needs to be granted for now (see https://github.com/cloudnative-pg/cloudnative-pg/issues/5445).
