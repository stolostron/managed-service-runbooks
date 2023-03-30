## ManagedClusterKubeAPIServerUnavailable

First, on hub cluster, check the condition `ManagedClusterConditionAvailable` of the ManagedCluster.

```bash
oc get managedcluster <managedcluster-name> -o=jsonpath='{.status.conditions[?(@.type=="ManagedClusterConditionAvailable")]}'
```

Currently, we have only one reason `ManagedClusterKubeAPIServerUnavailable` when the condition is `False`. And it’s message will contain the error returned by the managed cluster discovery client.

Next, switch to the management cluster, if the managed cluster is a management cluster, then check the `kube-apiserver` pod status and logs in the namespace `kueb-system`.

If the managed cluster is a hosted cluster, then check the `kube-apiserver` pod status and logs in the namespace `hosted_cluster_namespace`.
